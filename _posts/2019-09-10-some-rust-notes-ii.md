---
layout: post
title: Some Rust notes II
categories: [Rust]
tags: [Rust, Notes]
fullview: false
---

1. A simple example of the syntax of specifying generic type parameters, trait bounds, and lifetimes: 
```rust
use std::fmt::Display;
fn longest_with_an_announcement<'a, T>(x: &'a str, y: &'a str, ann: T) -> &'a str
    where T: Display
{
    println!("Announcement! {}", ann);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```
2. We can put expected panic information in the `expected` parameter of `#[should_panic]`: 
```rust
#[should_panic(expected = "Guess value must be less than or equal to 100")]
```
3. `FnOnce` consumes the variables it captures from the closure’s environment. To consume the captured variables, the closure must take ownership of these variables and move them into the closure when it is defined. The Once part of the name represents the fact that the closure can’t take ownership of the same variables more than once, so it can be called only once.
`FnMut` can change the environment because it mutably borrows values.
`Fn` borrows values from the environment immutably.
4. An example of using a closure function as a element in a struct: 
```rust
struct Cacher<T>
    where T: Fn(u32) -> u32 
{
    calculation: T,
    value: HashMap<u32, u32>,
}
impl<T> Cacher<T>
    where T: Fn(u32) -> u32 
{
    fn new(calculation: T) -> Cacher<T> {
        Cacher {
            calculation,
            value: HashMap::new(),
        }
    }
    fn value(&mut self, arg: u32) -> u32 {
        match self.value.get(&arg) {
            Some(v)     => *v,
            None        => {
                let v = (self.calculation)(arg);
                self.value.insert(arg, v);
                v
            }
        }
    }
}
```
5. Using `sum` method for `iterator` will take ownership so that if this statement is called: 
```rust
let v1 = vec![1, 2, 3];
let v1_iter = v1.iter();
let total: i32 = v1_iter.sum();
```
we cannot use v1_iter after the call because of the reason stated above.
6. The default value for the opt-level setting for the dev profile is 0 and the one for the release profile is 3. Add these two lines in `Cargo.toml` will override the default optimization level for `dev` profile:
```
[profile.dev]
opt-level = 1
```
7. A `cons list` example written using `Box<T>`: 
```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}
use self::List::{Cons, Nil};
fn main() {
    let test = Cons(1, 
                    Box::new(Cons(2,
                    Box::new(Cons(3, 
                    Box::new(Nil))))));
}
```
8. An example of creating a simple smart pointer implementing `Deref` and `Drop` traits:
```rust
struct MyBox<T>(T);
impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}
impl<T> Deref for MyBox<T> {
    type Target = T;
    fn deref(&self) -> &T {
        &self.0
    }
}
impl<T> Drop for MyBox<T> {
    fn drop(&mut self) {
        println!("Dropping MyBox!");
    }
}
```
9. Comparing with regular reference, with `Rc` we can create things with shared ownership and do not need to specify lifetime parameters.
10. An example of combination of `Rc` and `RefCell`:
```rust
use std::rc::Rc;
use std::cell::RefCell;
#[derive(Debug)]
enum List {
    Cons(Rc<RefCell<i32>>, Rc<List>),
    Nil,
}
use crate::List::{Cons, Nil};
fn main() {
    let value = Rc::new(RefCell::new(5));
    let a = Rc::new(Cons(Rc::clone(&value), Rc::new(Nil)));
    let b = Cons(Rc::new(RefCell::new(6)), Rc::clone(&a));
    let c = Cons(Rc::new(RefCell::new(12)), Rc::clone(&a));
    *value.borrow_mut() += 10;
    println!("{:?}\n{:?}\n{:?}\n", a, b, c);
}
```
11. By using `Rc<T>` and `RefCell<T>`, it’s possible to create reference cycles which creates memory leaks because the reference count of each item in the cycle will never reach 0, and the values will never be dropped.
12. An example of using `Weak<T>` to avoid reference cycle and create a simple node structure:
```rust
struct Node {
    value: i32,
    parent: RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}
fn main() {
    let root = Rc::new(Node {
        value: 0,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![]),
    });

    let leaf = Rc::new(Node {
        value: 3,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![]),
    });
    println!(
        "leaf strong = {}, weak = {}",
        Rc::strong_count(&leaf),
        Rc::weak_count(&leaf),
    );
    println!(
        "root strong = {}, weak = {}",
        Rc::strong_count(&root),
        Rc::weak_count(&root),
    );
    println!("-------------------------------");
    {
        let branch = Rc::new(Node {
            value: 10,
            parent: RefCell::new(Rc::downgrade(&root)),
            children: RefCell::new(vec![Rc::clone(&leaf)]),
        });
        *leaf.parent.borrow_mut() = Rc::downgrade(&branch);
        root.children.borrow_mut().push(Rc::clone(&branch));
        println!(
            "root strong = {}, weak = {}",
            Rc::strong_count(&root),
            Rc::weak_count(&root),
        );
        println!(
            "branch strong = {}, weak = {}",
            Rc::strong_count(&branch),
            Rc::weak_count(&branch),
        );
        println!(
            "leaf strong = {}, weak = {}",
            Rc::strong_count(&leaf),
            Rc::weak_count(&leaf),
        );
        println!("-------------------------------");
    }
    println!("leaf parent = {:?}", leaf.parent.borrow().upgrade());
    println!(
        "leaf strong = {}, weak = {}",
        Rc::strong_count(&leaf),
        Rc::weak_count(&leaf),
    );
    println!(
        "root strong = {}, weak = {}",
        Rc::strong_count(&root),
        Rc::weak_count(&root),
    );
}
```
13. An example of using `Arc<T>` and `Mutex<T>` to share ownership between threads: 
```rust
fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];
    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();
            *num += 1;
        });
        handles.push(handle);
    }
    for handle in handles {
        handle.join().unwrap();
    }
    println!("{}", *counter.lock().unwrap());
}
```
14. This code: 
```rust
let a = 5;
{
    let a = 10;
    println!("{}", a);
}
println!("{}", a);
```
will print: 
```
10
5
```
15. Use concurrent safe types such as `Arc<T>` instead of `Rc<T>` in concurrent programs. However, types like `Rc<T>` have better performance so it's a trade-off we have to choose.
16. A trait is object safe if all the methods defined in the trait have the following properties: (1) The return type isn’t Self. (2) There are no generic type parameters.
17. An example of `match guard` combining with `|`:
```rust
let x = 4;
let y = false;
match x {
    4 | 5 | 6 if y => println!("yes"),
    _ => println!("no"),
}
```
17. Using `extern` to call external code in other languages:
```rust
extern "C" {
    fn abs(input: i32) -> i32;
}
fn main() {
    unsafe {
        println!("Absolute value of -3 according to C: {}", abs(-3));
    }
}
```
18. An example of using `extern` to create an interface that allows other languages to call Rust functions. (This usage of extern does not require unsafe):
```rust
#[no_mangle]
pub extern "C" fn call_from_c() {
    println!("Just called a Rust function from C!");
}
```
Note that `#[no_mangle]` annotation to tell the Rust compiler not to mangle the name of this function. Mangling is when a compiler changes the name we’ve given a function to a different name that contains more information for other parts of the compilation process to consume but is less human readable.
19. Values in a static variable have a fixed address in memory so that using the value will always access the same data. On the other hand, constants are allowed to duplicate their data whenever they’re used. Also, static variables can be mutable, although accessing and modifying mutable static variables is unsafe:
```rust
static mut COUNTER: u32 = 0;
fn add_to_count(inc: u32) {
    unsafe {
        COUNTER += inc;
    }
}
fn main() {
    add_to_count(3);
    unsafe {
        println!("COUNTER: {}", COUNTER);
    }
}
```
20. When having multiple methods with the same name defined in different traits or directly on a type:
```rust
trait Pilot {
    fn fly(&self);
}
trait Wizard {
    fn fly(&self);
}
struct Human;
impl Pilot for Human {
    fn fly(&self) {
        println!("This is your captain speaking.");
    }
}
impl Wizard for Human {
    fn fly(&self) {
        println!("Up!");
    }
}
impl Human {
    fn fly(&self) {
        println!("*waving arms furiously*");
    }
}
```
When we call the `fly` method on a `Human` without other information, the one directly defined in `Human` will be called. If we want to explicitly specify a `fly` method, we can do as below:
```rust
fn main() {
    let person = Human;
    Pilot::fly(&person);
    Wizard::fly(&person);
    person.fly();
}
```
However, for `associated functions` which does not have a `self` parameter so that Rust cannot figure out which implementation of a trait to use. In this case, we need to use `fully qualified syntax`:
```rust
trait Animal {
    fn baby_name() -> String;
}
struct Dog;
impl Dog {
    fn baby_name() -> String {
        String::from("Spot")
    }
}
impl Animal for Dog {
    fn baby_name() -> String {
        String::from("puppy")
    }
}
fn main() {
    println!("A baby dog is called a {}", Dog::baby_name());
    println!("A baby dog is called a {}", <Dog as Animal>::baby_name());
}
```
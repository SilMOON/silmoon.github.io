---
layout: post
title: Some Rust notes II
categories: [Rust]
tags: [Rust, Notes]
fullview: true
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
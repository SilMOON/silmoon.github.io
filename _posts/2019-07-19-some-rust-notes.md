---
layout: post
title: Some Rust notes
categories: [Rust]
tags: [Tips]
fullview: true
---

1. io::stdin().read_line(&xx) appends new value on the target which is "xx" in this case.
2. A tuple can have values with different types in it while an array must have the same type.
3. A tuple with explictly type annotations example:
```rust
let x: (i32, f64, u8) = (500, 6.4, 1);
```
4. An array in Rust have a fixed length while a Vector can grow or shrink its size.
5. An array with explictly type and elements' number annotations example:
```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```
6. If [6, 6, 6, 6, 6] is the desirable array, we can declare it in this way:
```rust
let a = [6; 5];
```
7. A good way to create a new instance of a struct that uses most of an old instance’s values but changes some is using update syntax:
```rust
let user2 = User {
    email: String::from("user2@example.com"),
    username: String::from("user2name"),
    ..user1
};
```
8. An example of using derived traits of custom structs:
```rust
#[derive(Debug)]
println!("{:?}",user1);
println!("{:#?}",user1);
```
9. If a struct tuple is defined, when a function want to call an instance of this struct, other struct tuple (even with the same form) cannot be used:
```rust
struct Color (u32,u32,u32);
struct Point (u32,u32,u32);
```
In this case, if a function want to call an instance of "Color", we cannot put an instance of "Point" in it even they have the same form.
10. An example of a struct and its method implementation:
```rust
struct Rectangle {
    width: u32,
    height: u32,
}
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }

    //method with multiple parameters
    fn can_hold(&self, other:&Rectangle) -> bool {
        self.height > other.height && self.width > other.width
    }

    //associated function
    fn square(size:u32) -> Rectangle {
        Rectangle {width:size, height:size}
    }
}
```
When we want to use them:
```rust
rect1.can_hold(&rect2)
let sq = Rectangle::square(20);
```
11. Option<T> and T are different types so that we cannot add an i8 and an Option<i8>.
12. If we use None rather than Some, we need to tell Rust what type of Option<T> we have:
```rust
let absent_number: Option<i32> = None;
```
13. Using match in order to check elements in enum or check Option<T>:
```rust
match seven {
    Some(7) => println!("{:?}", seven),
    _ => (),
}
```
The `_` pattern will match any value. The `()` is just the unit value, so nothing will happen in the _ case.
However, the example above can be written in `if let` which is simpler to read:
```rust
if let Some(7) = seven {
    println!("{:?}",seven);
}
```
Also, `else` can be used with `if let`:
```rust
if let Some(7) = seven {
    println!("{:?}",seven);
} else {
    println!("This is a else statement.");
}
```
14. Absolute path starts from a crate root by using a crate name or a literal `crate`:
```rust
crate::front_house::hosting::add_to_waitlist();
```
Relative path starts from the current module and uses `self`, `super`, or an identifier in the current module:
```rust
front_house::hosting::add_to_waitlist();
self::front_house::hosting::add_to_waitlist();
```
btw when we use `use` keyword for this one, we need to use `self` (but this usage might not be necessary in the future):
```rust
use self::front_of_house::hosting;
```
15. Use nested paths to bring the same items into scope in one line:
```rust
use std::{cmp::Ordering, io};
```
When there are two `use` statements where one is a subpath of the other, instead of using:
```rust
use std::io;
use std::io::Write;
```
we can use:
```rust
use std::io::{self, Write};
```
16. When using `pub` to a struct, we need to also use `pub` to fields we want to make it public:
```rust
pub struct Breakfast {
    pub toast : String,
    //fruit is still private as its default
    fruit : String,
}
```
but for an enum we only need to use `pub` to the enum itself:
```rust
pub enum Appetizer {
    //both Soup and Salad are now public
    Soup,
    Salad,
}
```
17. Usually we `use` the full path for structs and enums: 
```rust
use std::collections::HashMap;
fn main() {
    let mut map = HashMap::new();
    map.insert(1, 2);
}
```
when multiple of them have the same name we want to use their parent path in order to avoid conflicts:
```rust
use std::fmt;
use std::io;
fn function1() -> fmt::Result {}
fn function2() -> io::Result<()> {}
```
but we can still use the full path if we use `as` keyword to give them aliases:
```rust
use std::fmt::Result;
use std::io::Result as IoResult;
fn function1() -> Result {}
fn function2() -> IoResult<()> {}
```
and usually when we want to use functions from outside, we `use` their parent path.
18. We can separate modules into different files and still `use` the same path when we use them. But we need to declare them first in their parent path level by level until it comes to lib.rs. In this example ,the files structure is like this:
|-src
  -lib.rs
  -front_of_house.rs
  |-front_of_house
    -hosting.rs
lib.rs:
```rust
mod front_of_house;
pub use crate::front_of_house::hosting;
pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```
front_of_house.rs:
```rust
pub mod hosting;
```
hosting.rs:
```rust
pub fn add_to_waitlist() {}
```
---
layout: post
title: Some Rust note
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
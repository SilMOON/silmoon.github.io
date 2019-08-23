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
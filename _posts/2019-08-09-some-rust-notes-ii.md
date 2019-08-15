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
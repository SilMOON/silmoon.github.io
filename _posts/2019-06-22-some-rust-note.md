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
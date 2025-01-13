---
title: "Mastering Closures in Rust: The Idiomatic Way"
date: 2024-01-13
description: "Learn how to use closures effectively and idiomatically in Rust with practical examples."
categories: ["Programming"]
author: "adriel artiza"
---

Closures are a powerful feature in Rust, providing a concise way to capture and use variables from their surrounding scope. In this post, we’ll explore closures and how to use them in an idiomatic Rust programming style.

## What Are Closures?

A **closure** is an anonymous function that can capture variables from its enclosing scope. Closures in Rust are flexible and can adapt to different levels of functionality:

- **Fn:** Borrow variables from the environment immutably.
- **FnMut:** Borrow variables mutably.
- **FnOnce:** Take ownership of the variables they capture.

Here’s a simple example:

```rust
let greet = |name: &str| {
    println!("Hello, {}!", name);
};
greet("World");

```
**Capturing Variables by Reference**
```rust
let x = 10;
let add_to_x = |n: i32| n + x;

println!("{}", add_to_x(5)); // Outputs: 15

```

**Capturing Variables Mutably**
```rust
let mut counter = 0;
let mut increment = || {
    counter += 1;
    println!("Counter: {}", counter);
};

increment(); // Outputs: Counter: 1
increment(); // Outputs: Counter: 2

```

**Capturing Variables by Value**
```rust
let names = vec!["Alice", "Bob", "Charlie"];
let consume_names = || {
    println!("{:?}", names); // Takes ownership
};

// consume_names can only be called once
consume_names();
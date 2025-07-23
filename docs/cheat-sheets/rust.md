# Rust Cheat Sheet

A minimal, senior-style reference for when I eventually get serious with Rust.  
Strictly typed, memory-safe, and zero-cost abstractions — even when I don’t fully get it yet.

---

## Project Structure

```bash
cargo new my_project      # creates src/main.rs
cargo build               # compile
cargo run                 # build & run
cargo check               # type check only
cargo test                # run tests
```

File layout:
```
src/
  main.rs        // binary entry point
  lib.rs         // optional library module
Cargo.toml       // dependency & metadata
```

---

## Variables & Types

```rust
let x = 42;           // immutable by default
let mut y = 5;        // mutable
const PI: f64 = 3.14; // compile-time const

let s: &str = "hello";            // string slice
let name: String = String::from("hi");  // owned heap-allocated string

let t: (i32, bool) = (42, true);
let [a, b, c]: [i32; 3] = [1, 2, 3];
```

---

## Functions

```rust
fn square(x: i32) -> i32 {
    x * x
}

fn unit_function() {
    println!("no return");
}
```

Closures:

```rust
let add = |a, b| a + b;
let captured = |x: i32| x + y;
```

---

## Control Flow

```rust
if n > 0 {
    println!("positive");
} else if n == 0 {
    println!("zero");
} else {
    println!("negative");
}

let result = if cond { 1 } else { 0 };

match x {
    1 => println!("one"),
    2 | 3 => println!("two or three"),
    _ => println!("something else"),
}

while i < 10 { ... }
for i in 0..5 { ... }
loop { break; }
```

---

## Ownership, Borrowing & Lifetimes

- `T` – owned
- `&T` – shared reference (borrow)
- `&mut T` – mutable reference (borrow)
- Lifetimes: explicit when needed (`<'a>`), but inferred when possible

```rust
fn borrow(x: &i32) {
    println!("{}", x);
}

fn borrow_mut(x: &mut i32) {
    *x += 1;
}
```

Can only have:
- one mutable reference **OR**
- multiple immutable references

at the same time.

---

## Structs & Enums

```rust
struct Point {
    x: f64,
    y: f64,
}

enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

Associated functions:

```rust
impl Point {
    fn new(x: f64, y: f64) -> Self {
        Self { x, y }
    }
}
```

---

## Pattern Matching & Destructuring

```rust
let (a, b) = (1, 2);

match maybe_value {
    Some(x) => println!("{}", x),
    None => println!("Nothing"),
}

if let Some(x) = opt {
    println!("Found: {}", x);
}
```

---

## Traits & Generics

```rust
trait Speak {
    fn speak(&self);
}

impl Speak for Dog {
    fn speak(&self) {
        println!("Woof");
    }
}

fn print_all<T: Display>(items: &[T]) {
    for i in items {
        println!("{}", i);
    }
}
```

Trait bounds via `where`:

```rust
fn log<T>(item: T)
where
    T: Debug + Clone,
{
    println!("{:?}", item);
}
```

---

## Modules & Visibility

```rust
mod mymod {
    pub fn hello() { ... }
}

use crate::mymod::hello;
```

Public API must be explicitly exposed with `pub`.

---

## Error Handling

```rust
fn might_fail() -> Result<i32, String> {
    if success {
        Ok(42)
    } else {
        Err("something broke".into())
    }
}

let result = might_fail()?;
```

- `Result<T, E>` is the standard way to model recoverable errors.
- Use `?` to propagate.

---

## Smart Pointers

- `Box<T>`: heap allocation
- `Rc<T>`: shared ownership
- `RefCell<T>`: interior mutability
- `Arc<T>`: atomic Rc for threads

---

## Macros

```rust
println!("Hello, {}", name);
dbg!(x);
vec![1, 2, 3];
```

Custom macros use `macro_rules!`.

---

## Concurrency

```rust
use std::thread;

let handle = thread::spawn(|| {
    // do work
});

handle.join().unwrap();
```

- Use `Mutex<T>`, `Arc<T>` for shared state.
- Prefer message-passing via `channel()` when possible.

---

## Crates & Cargo

In `Cargo.toml`:

```toml
[dependencies]
regex = "1.9"
serde = { version = "1.0", features = ["derive"] }
```

Then:

```rust
use regex::Regex;
```

---

## Best Practices

- Use `Option<T>` and `Result<T, E>` instead of `null`
- Avoid cloning unless necessary — understand ownership
- `unwrap()` only in examples or panic-safe places
- Use `cargo fmt` and `clippy` for linting
- Avoid premature lifetimes unless compiler demands them
- Use `?` and `match` for clean error flow

---

---
source: hackernews
title: Rue: Higher level than Rust, lower level than Go
url: https://rue-lang.dev/
date: 2025-12-22
---

[Rue](https://rue-lang.dev)

* [Home](https://rue-lang.dev)* [Get Started](https://rue-lang.dev/getting-started)* [Specification](https://rue-lang.dev/spec)* [Blog](https://rue-lang.dev/blog)* [GitHub](https://github.com/rue-language/rue)* [Bluesky](https://bsky.app/profile/rue-lang.dev)

# Rue

Higher level than Rust, lower level than Go

[Get Started](https://rue-lang.dev/getting-started)[Read the Spec](/spec/)

## Memory Safe

No garbage collector, no manual memory management. A work in progress, though.

## Simple Syntax

Familiar syntax inspired by various programming languages. If you know one, you'll feel at home with Rue.

## Fast Compilation

Direct compilation to native code.

## Hello, Rue

```
// It's a classic for a reason
fn fib(n: i32) -> i32 {
    if n <= 1 {
        n
    } else {
        fib(n - 1) + fib(n - 2)
    }
}

fn main() -> i32 {
    // Print the first 20 Fibonacci numbers
    let mut i = 0;
    while i < 20 {
        @dbg(fib(i));
        i = i + 1;
    }

    // Return fib(10) = 55
    fib(10)
}
```

© 2025 The Rue Authors. Licensed under MIT/Apache-2.0.

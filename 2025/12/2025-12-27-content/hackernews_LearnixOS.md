---
source: hackernews
title: LearnixOS
url: https://www.learnix-os.com
date: 2025-12-27
---

## Keyboard shortcuts

Press `←` or `→` to navigate between chapters

Press `S` or `/` to search in the book

Press `?` to show this help

Press `Esc` to hide this help

[ ]

* Auto
* Light
* Rust
* Coal
* Navy
* Ayu

# The LearnixOS Book

# [The Learnix Operating System](#the-learnix-operating-system)

*"If you can't explain it simply, you don't understand it well enough." - Albert Einstein*

---

Hello there![1](#footnote-1)

In this book we are going to write and learn about operating systems together!

We are going to implement an entire POSIX compliant OS in Rust and not use ANY[2](#footnote-2) external libraries. All of the thought process, code and implementations will be explained and documented here as well as in this [repo](https://github.com/sagi21805/LearnixOS) which all the code snippets are from.

> *Note*: ALL the syntax highlighting of the Rust code is custom and create by me! If you see and bug, please write in the comments or submit an [issue](https://github.com/sagi21805/mdbook-rust-highlight).

## [Base Knowledge](#base-knowledge)

This book will be technical, and will assume a little bit of a programming knowledge background, but not necessarily in rust

If you are not coming from a low level programming knowledge that's fine!

Just make sure you know this stuff or learn it as you read. Also if in any place on this book I take some things for granted, please, open an issue [here](https://github.com/sagi21805/LearnixOS-Book) and let me know so I could explain it better.

Some of the base knowledge that you would need to have:

* Some assembly knowledge. (just understand simple movs, and arithmetic operations, at a very basic level[3](#footnote-3))
* Some knowledge on memory. (what's a pointer, what's an address)
* A knowledge in rust is not *that* important, but knowing at least one programming language is. I myself have some more learning in Rust, and in this book I will also explain some great features that it has!
* A lot of motivation to learn and understand because it is a complex subject.

## [Roadmap of this book](#roadmap-of-this-book)

1. Compiling a stand alone binary
2. Boot loading, Debugging, stages and some legacy stuff
3. Important cpu modes and instructions
4. Paging, writing our own *malloc*
5. Utilizing the Interrupt Descriptor Table
6. File systems and Disk Drivers
7. Thinking in terms of processes
8. Writing a shell
9. Running our first program! (Which off course will be Doom)
10. To be continued (Hopefully virtualization section and loading a vm of other OS)

---

1. Definitely not a star wars reference [↩](#fr-1-1)
2. Only libraries that remove boilerplate code will be used (And obviously be explained). [↩](#fr-2-1)
3. This is only relevant to the starting stages and some optimizations, and probably a day of learning will be enough [↩](#fr-3-1)

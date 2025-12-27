---
source: hackernews
title: Show HN: AutoLISP interpreter in Rust/WASM – a CAD workflow invented 33 yrs ago
url: https://acadlisp.de/noscript.html
date: 2025-12-27
---

# acadlisp: AutoLISP Interpreter in Rust/WebAssembly

[Try the Interactive Demo](https://acadlisp.de/)

## What is acadlisp?

**acadlisp** is an AutoLISP interpreter written in Rust and compiled to WebAssembly.
It runs AutoLISP code directly in the browser - no AutoCAD installation required.

## The Story: Schematic Generator 1991

In 1991, a small electrical company in Bavaria, Germany faced a challenge:
every machine installation required custom electrical schematics - a time-consuming manual process.

The solution: **AutoLISP**, the programming language built into AutoCAD.
I invented a workflow using CSV files, templates, and LISP code to automate schematic generation.
Define components in spreadsheets, feed them through templates, generate complete technical drawings automatically.

I've never met anyone else who used this approach. Now I've built an interpreter in Rust/WASM
so this workflow can live on in the browser - partly nostalgia, partly preservation before
this knowledge disappears entirely.

## LISP as Early AI

LISP (List Processing) was developed in 1958 by John McCarthy and was for decades
*the* language of Artificial Intelligence research. What makes LISP special?

* **Homoiconicity:** Code and data share the same structure (lists)
* **Self-modification:** Programs can write and modify themselves
* **Symbolic processing:** Manipulation of symbols, not just numbers

In the 1991 schematic generator, the code actually wrote itself:
inserting a component could trigger more components, templates generated templates.

## Technical Details

* **Language:** Rust
* **Target:** WebAssembly (WASM)
* **Output formats:** SVG, DXF (AutoCAD R12/AC1009)
* **Supported AutoLISP functions:** defun, setq, if, while, cond, +, -, \*, /, sin, cos, sqrt, car, cdr, list, nth, command, princ, and more

## Example Code

```
; Draw a rectangle
(defun draw-box (x y w h)
  (command "LINE" (list x y) (list (+ x w) y) "")
  (command "LINE" (list (+ x w) y) (list (+ x w) (+ y h)) "")
  (command "LINE" (list (+ x w) (+ y h)) (list x (+ y h)) "")
  (command "LINE" (list x (+ y h)) (list x y) ""))

(draw-box 10 10 100 50)
```

## Links

* [Interactive Demo (requires JavaScript)](https://acadlisp.de/)
* [Source code on GitHub](https://github.com/acadlisp/acadlisp)

© 2024 acadlisp.de - AutoLISP Interpreter in Rust/WebAssembly

---
source: hackernews
title: Dafny: Verification-Aware Programming Language
url: https://dafny.org/
date: 2025-12-17
---

# The Dafny Programming and Verification Language

* [Install](./latest/Installation)
  (or just use the VS Code [extension](https://marketplace.visualstudio.com/items?itemName=dafny-lang.ide-vscode))
* [Zulip channel](https://dafny.zulipchat.com/) to ask questions about Dafny
* [Reference Manual and User Guide](./latest/DafnyRef/DafnyRef)
* [Resources for Users](./latest/toc)
* [Blog](./blog)
* [YouTube channel](https://www.youtube.com/%40DafnyVideos)
* [Contribute on GitHub](https://github.com/dafny-lang/dafny)
* [Documentation snapshots](./Snapshots)
* [*Program Proofs*, by Rustan Leino, MIT Press](https://mitpress.mit.edu/9780262546232/program-proofs)

![The Dafny logo, showing the word Dafny in blue next to wavy black and blue lines.](images/dafny-logo.jpg)
**Dafny** is a verification-aware programming language that has native support for recording specifications
and is equipped with a static program verifier.
By blending sophisticated automated reasoning with familiar programming idioms and tools,
Dafny empowers developers to write provably correct code (w.r.t. specifications).
It also compiles Dafny code to familiar development environments such as C#, Java, JavaScript, Go and Python (with more to come) so Dafny can integrate with your existing workflow.
Dafny makes rigorous verification an integral part of development,
thus reducing costly late-stage bugs that may be missed by testing.

In addition to a verification engine to check implementation against specifications,
the Dafny ecosystem includes several compilers,
plugins for common software development IDEs,
a LSP-based Language Server,
a code formatter,
a reference manual,
tutorials,
power user tips,
books,
the experiences of professors teaching Dafny,
and the accumulating expertise of industrial projects using Dafny.

Dafny has support for common programming concepts such as

* mathematical and bounded integers and reals, bit-vectors, classes, iterators, arrays, tuples, generic types, refinement and inheritance,
* [inductive datatypes](./latest/DafnyRef/DafnyRef#sec-inductive-datatypes) that can have methods and are suitable for pattern matching,
* [lazily unbounded datatypes](./latest/DafnyRef/DafnyRef#sec-co-inductive-datatypes),
* [subset types](./latest/DafnyRef/DafnyRef#sec-subset-types), such as for bounded integers,
* [lambda expressions](./latest/DafnyRef/DafnyRef#sec-lambda-expressions) and functional programming idioms,
* and [immutable and mutable data structures](./latest/DafnyRef/DafnyRef#sec-collection-types).

Dafny also offers an extensive toolbox for mathematical proofs about software, including

* [bounded and unbounded quantifiers](./latest/DafnyRef/DafnyRef#sec-quantifier-domains),
* [calculational proofs](./latest/DafnyRef/DafnyRef#sec-calc-statement) and the ability to use and prove lemmas,
* [pre- and post-conditions, termination conditions, loop invariants, and read/write specifications](./latest/DafnyRef/DafnyRef#sec-specification-clauses).

![A snippet of code shown in the VSCode IDE showing an implementation of the Dutch national flag problem written in Dafny.  IDE extensions are showing successes and failures in verification reported by the Dafny Language Server running in real time.](images/Demo.png)

Dafny running in Visual Studio Code

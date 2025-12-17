---
source: hackernews
title: Ty: A fast Python type checker and LSP
url: https://astral.sh/blog/ty
date: 2025-12-17
---

* Products
* Docs
* [Blog](/blog)
* [Company](/about)

[Get Started](https://github.com/astral-sh)

G

[Back to blog](/blog)

December
16,
2025

# ty: An extremely fast Python type checker and LSP

![Charlie Marsh](data:image/gif;base64...)![Charlie Marsh](/_next/image?url=https%3A%2F%2Fastralcms.wpengine.com%2Fwp-content%2Fuploads%2F2023%2F03%2Fcharlie-marsh.jpg&w=3840&q=75)

##### Charlie Marsh

[@charliermarsh](https://x.com/charliermarsh)

**TL;DR:** [ty](https://github.com/astral-sh/ty) is an **extremely fast Python type checker and
language server**, written in Rust, and designed as an alternative to tools like mypy, Pyright, and
Pylance.

Today, we're announcing the Beta release of [ty](https://github.com/astral-sh/ty). We now use ty
exclusively in our own projects and are ready to recommend it to motivated users for production use.

---

At Astral, we build high-performance developer tools for the Python ecosystem. We're best known for
[uv](https://github.com/astral-sh/uv), our Python package manager, and
[Ruff](https://github.com/astral-sh/ruff), our linter and formatter.

Today, we're announcing the Beta release of the next tool in the Astral toolchain: **ty, an
extremely fast Python type checker and language server**, written in Rust.

0s20s40styPyreflyPyrightmypy5.32s19.62s45.66s2.19s

Type checking the [home-assistant](https://github.com/home-assistant/core) project on the command-line, without caching ([M4](https://github.com/astral-sh/ruff/blob/7f7485d608d2da19a0632a1238f2d4be551f612f/scripts/ty_benchmark/README.md)).

ty was designed from the ground up to power a language server. The entire ty architecture is built
around "incrementality", enabling us to selectively re-run only the necessary computations when a
user (e.g.) edits a file or modifies an individual function. This makes live updates extremely fast
in the context of an editor or long-lived process.

0s1s2styPyrightPyrefly370.5ms2.60s4.5ms

Re-computing diagnostics in the language server after editing a file in the [PyTorch](https://github.com/pytorch/pytorch) project ([M4](https://github.com/astral-sh/ruff/blob/7f7485d608d2da19a0632a1238f2d4be551f612f/scripts/ty_benchmark/README.md)).

You can install ty today with `uv tool install ty@latest`, or via our
[VS Code extension](https://marketplace.visualstudio.com/items?itemName=astral-sh.ty).

Like Ruff and uv, ty's implementation was grounded in some of our core product principles:

1. **An obsessive focus on performance.** Without caching, ty is consistently between 10x and 60x
   faster than mypy and Pyright. When run in an editor, the gap is even more dramatic. As an
   example, after editing a load-bearing file in the PyTorch repository, ty recomputes diagnostics
   in 4.7ms: 80x faster than Pyright (386ms) and 500x faster than Pyrefly (2.38 seconds). ty is very
   fast!
2. **Correct, pragmatic, and ergonomic.** With features like
   [first-class intersection types](https://docs.astral.sh/ty/features/type-system/#intersection-types),
   [advanced type narrowing](https://docs.astral.sh/ty/features/type-system/#top-and-bottom-materializations),
   and
   [sophisticated reachability analysis](https://docs.astral.sh/ty/features/type-system/#reachability-based-on-types),
   ty pushes forward the state of the art in Python type checking, providing more accurate feedback
   and [avoiding assumptions](https://docs.astral.sh/ty/features/type-system/#gradual-guarantee)
   about user intent that often lead to false positives. Our goal with ty is not only to build a
   faster type checker; we want to build a better type checker, and one that balances correctness
   with a deep focus on the end-user experience.
3. **Built in the open.** ty was built by our core team alongside dozens of active contributors
   under the MIT license, and the same goes for our
   [editor extensions](https://marketplace.visualstudio.com/items?itemName=astral-sh.ty). You can
   run ty anywhere that you write Python (including in the [browser](https://play.ty.dev/)).

Even compared to other Rust-based language servers like Pyrefly, ty can run orders of magnitude
faster when performing incremental updates on large projects.

[Your browser does not support the video tag.](/static/MP4/TyPyTorch.mp4)

Editing a central file in the [PyTorch](https://github.com/pytorch/pytorch) repository with [ty](https://github.com/astral-sh/ty) (left) and [Pyrefly](https://github.com/facebook/pyrefly) (right). ty's incremental architecture is designed to make live updates extremely fast.

ty also includes a
[best-in-class diagnostic system](https://docs.astral.sh/ty/features/diagnostics/), inspired by the
Rust compiler's own world-class error messages. A single ty diagnostic can pull in context from
multiple files at once to explain not only what's wrong, but why (and, often, how to fix it).

![](data:image/svg+xml...)![ty diagnostic showing an invalid assignment error to a TypedDict key with reference to the item declaration](data:image/gif;base64...)![ty diagnostic showing an invalid assignment error to a TypedDict key with reference to the item declaration](/_next/image?url=%2Fstatic%2FPNG%2FTyDiagnostic3Light.png&w=3840&q=75)

![](data:image/svg+xml...)![ty diagnostic showing an invalid assignment error to a TypedDict key with reference to the item declaration](data:image/gif;base64...)![ty diagnostic showing an invalid assignment error to a TypedDict key with reference to the item declaration](/_next/image?url=%2Fstatic%2FPNG%2FTyDiagnostic3Dark.png&w=3840&q=75)

When assigning an invalid value to a dictionary key, [ty](https://github.com/astral-sh/ty) surfaces both the type mismatch at the assignment site and the corresponding item declaration.

Diagnostic output is the primary user interface for a type checker; we prioritized our diagnostic
system from the start (with both humans and agents in mind) and view it as a first-class feature in
ty.

![](data:image/svg+xml...)![ty diagnostic showing an unresolved import error for tomllib module with reference to Python version configuration](data:image/gif;base64...)![ty diagnostic showing an unresolved import error for tomllib module with reference to Python version configuration](/_next/image?url=%2Fstatic%2FPNG%2FTyDiagnostic2Light.png&w=3840&q=75)

![](data:image/svg+xml...)![ty diagnostic showing an unresolved import error for tomllib module with reference to Python version configuration](data:image/gif;base64...)![ty diagnostic showing an unresolved import error for tomllib module with reference to Python version configuration](/_next/image?url=%2Fstatic%2FPNG%2FTyDiagnostic2Dark.png&w=3840&q=75)

When importing an unresolved module, [ty](https://github.com/astral-sh/ty) surfaces both the unresolved import at the import site and the corresponding Python version configuration.

If you use VS Code, Cursor, or a similar editor, we recommend installing the
[ty VS Code extension](https://marketplace.visualstudio.com/items?itemName=astral-sh.ty). The ty
language server supports [all the capabilities](https://docs.astral.sh/ty/features/language-server/)
that you'd expect for a modern language server (Go to Definition, Symbol Rename, Auto-Complete,
Auto-Import, Semantic Syntax Highlighting, Inlay Hints, etc.), and runs in any editor that
implements the [Language Server Protocol](https://microsoft.github.io/language-server-protocol/).

Following the Beta release, our immediate priority is supporting early adopters. From there, we're
working towards a Stable release next year, with the gap between the
[Beta](https://github.com/astral-sh/ty/milestone/2) and
[Stable](https://github.com/astral-sh/ty/milestone/4) milestones largely focusing on: (1) stability
and bug fixes, (2) completing the long tail of features in the
[Python typing specification](https://github.com/astral-sh/ty/issues/1889), and (3) first-class
support for popular third-party libraries like [Pydantic](https://pypi.org/project/pydantic/) and
[Django](https://pypi.org/project/Django/).

On a longer time horizon, though, ty will power semantic capabilities across the Astral toolchain:
dead code elimination, unused dependency detection, SemVer-compatible upgrade enforcement, CVE
reachability analysis, type-aware linting, and more (including some that are too ambitious to say
out loud just yet).

We want to make Python the most productive programming ecosystem on Earth. Just as with
[Ruff](https://github.com/astral-sh/ruff) and [uv](https://github.com/astral-sh/uv), our commitment
from here is that ty will get significantly better every week by working closely with our users.
Thank you for building with us.

### Acknowledgements [#](#acknowledgements)

ty is the most sophisticated product we've built, and its design and implementation have surfaced
some of the hardest technical problems we've seen at Astral. Working on ty requires a deep
understanding of type theory, Python runtime semantics, and how the Python ecosystem actually uses
Python.

I'd like to thank all those that contributed directly to the development of ty, including:
[Douglas Creager](https://github.com/dcreager), [Alex Waygood](https://github.com/AlexWaygood),
[David Peter](https://github.com/sharkdp), [Micha Reiser](https://github.com/MichaReiser),
[Andrew Gallant](https://github.com/BurntSushi), [Aria Desires](https://github.com/Gankra),
[Carl Meyer](https://github.com/carljm), [Zanie Blue](https://github.com/zanieb),
[Ibraheem Ahmed](https://github.com/ibraheemdev),
[Dhruv Manilawala](https://github.com/dhruvmanila), [Jack O'Connor](https://github.com/oconnor663),
[Zsolt Dollenstein](https://github.com/zsol), [Shunsuke Shibayama](https://github.com/mtshiba),
[Matthew Mckee](https://github.com/MatthewMckee4), [Brent Westbrook](https://github.com/ntBre),
[UnboundVariable](https://github.com/UnboundVariable),
[Shaygan Hooshyari](https://github.com/Glyphack), [Justin Chapman](https://github.com/thejchap),
[InSync](https://github.com/InSyncWithFoo), [Bhuminjay Soni](https://github.com/Bhuminjay-Soni),
[Abhijeet Prasad Bodas](https://github.com/abhijeetbodas2001),
[Rasmus Nygren](https://github.com/RasmusNygren), [lipefree](https://github.com/lipefree),
[Eric Mark Martin](https://github.com/ericmarkmartin), [Tomer Bin](https://github.com/TomerBin),
[Luca Chiodini](https://github.com/lucach), [Brandt Bucher](https://github.com/brandtbucher),
[Dylan Wilson](https://github.com/dylwil3), [Eric Jolibois](https://github.com/tyralla),
[Felix Scherz](https://github.com/felixscherz), [Leandro Braga](https://github.com/leandrobbraga),
[Renkai Ge](https://github.com/nickkuang), [Sumana Harihareswara](https://github.com/brainwane),
[Takayuki Maeda](https://github.com/TaKO8Ki), [Max Mynter](https://github.com/maxmynter),
[med1844](https://github.com/med1844), [William Woodruff](https://github.com/woodruffw),
[Chandra Kiran G](https://github.com/kiran-4444), [DetachHead](https://github.com/DetachHead),
[Emil Sadek](https://github.com/esadek), [Jo](https://github.com/j178),
[Joren Hammudoglu](https://github.com/jorenham), [Mahmoud Saada](https://github.com/mahmoud),
[Manuel Mendez](https://github.com/mmlb), [Mark Z. Ding](https://github.com/markzding),
[Simon Lamon](https://github.com/silamon), [Suneet Tipirneni](https://github.com/suneettipirneni),
[Francesco Giacometti](https://github.com/fgiacome),
[Adam Aaronson](https://github.com/adamaaronson), [Alperen Keleş](https://github.com/alpaylan),
[charliecloudberry](https://github.com/charliecloudberry),
[Dan Parizher](https://github.com/danparizher), [Daniel Hollas](https://github.com/danielhollas),
[David Sherret](https://github.com/dsherret), [Dmitry](https://github.com/mdqst),
[Eric Botti](https://github.com/ercbot), [Erudit Morina](https://github.com/eruditmorina),
[François-Guillaume Fernandez](https://github.com/frgfm),
[Fabrizio Damicelli](https://github.com/fabridamicelli),
[Guillaume-Fgt](https://github.com/Guillaume-Fgt), [Hugo van Kemenade](https://github.com/hugovk),
[Josiah Kane](https://github.com/JosiahKane), [Loïc Riegel](https://github.com/LoicRiegel),
[Ramil Aleskerov](https://github.com/Mathemmagician), [Samuel Rigaud](https://github.com/s-rigaud),
[Soof Golan](https://github.com/soof-golan), [Usul-Dev](https://github.com/Usul-Dev),
[decorator-factory](https://github.com/decorator-factory), [omahs](https://github.com/omahs), and
[wangxiaolei](https://github.com/fatelei).

We'd also like to thank the [Salsa](https://github.com/salsa-rs/salsa) team (especially
[Niko Matsakis](https://github.com/nikomatsakis), [David Barsky](https://github.com/davidbarsky),
and [Lukas Wirth](https://github.com/veykril)) for their support and collaboration; the
[Elixir](https://github.com/elixir-lang/elixir) team (especially
[José Valim](https://github.com/josevalim), [Giuseppe Castagna](https://www.irif.fr/~gc/), and
[Guillaume Duboc](https://gldubc.github.io/)), whose work strongly influenced our approach to
gradual types and intersections; and a few members of the broader Python typing community:
[Eric Traut](https://github.com/erictraut), [Jelle Zijlstra](https://github.com/JelleZijlstra),
[Jia Chen](https://github.com/grievejia), [Sam Goldman](https://github.com/samwgoldman),
[Shantanu Jain](https://github.com/hauntsaninja), and [Steven Troxler](https://github.com/stroxler).

Finally, on a personal level, I'd like to highlight the core team
([Alex](https://github.com/AlexWaygood), [Andrew](https://github.com/BurntSushi),
[Aria](https://github.com/Gankra), [Carl](https://github.com/carljm),
[David](https://github.com/sharkdp), [Dhruv](https://github.com/dhruvmanila),
[Doug](https://github.com/dcreager), [Ibraheem](https://github.com/ibraheemdev),
[Jack](https://github.com/oconnor663), and [Micha](https://github.com/MichaReiser)), who created ty
from nothing and pushed it to be great from Day 1.

![](data:image/svg+xml...)![Background pattern](data:image/gif;base64...)![Background pattern](/_next/static/media/cta-top.8497e9c2.png)

![](data:image/svg+xml...)![Background pattern](data:image/gif;base64...)![Background pattern](/_next/static/media/cta-top-light.b111ae50.png)

## Supercharge your Python tooling

[Get Started](https://github.com/astral-sh)[Browse Docs](https://docs.astral.sh)

![](data:image/svg+xml...)![Background pattern](data:image/gif;base64...)![Background pattern](/_next/static/media/cta-bottom.2e2e4b06.png)

![](data:image/svg+xml...)![Background pattern](data:image/gif;base64...)![Background pattern](/_next/static/media/cta-bottom-light.3495aab7.png)

* [Ruff](https://docs.astral.sh/ruff/)
* [uv](https://docs.astral.sh/uv/)
* [ty](https://docs.astral.sh/ty/)
* [pyxBeta](/pyx)
* [GitHub](https://github.com/astral-sh)
* [Company](/about)
* [Hiring](https://jobs.ashbyhq.com/astral)
* [Blog](/blog)

[Privacy Policy](/legal/privacy)

© 2025 Astral Software Inc.

* [![](data:image/svg+xml...)![](data:image/gif;base64...)![](https://astralcms.wpengine.com/wp-content/uploads/2023/03/discord.svg)](https://discord.com/invite/c9MhzV8aU5)
* [![](data:image/svg+xml...)![](data:image/gif;base64...)![](https://astralwp.wpengine.com/wp-content/uploads/2025/08/X-2.svg)](https://x.com/astral_sh)
* [![](data:image/svg+xml...)![](data:image/gif;base64...)![](https://astralcms.wpengine.com/wp-content/uploads/2023/03/github.svg)](https://github.com/astral-sh)

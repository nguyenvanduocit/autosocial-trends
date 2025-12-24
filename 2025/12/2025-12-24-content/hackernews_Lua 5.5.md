---
source: hackernews
title: Lua 5.5
url: https://lua.org/versions.html#5.5
date: 2025-12-24
---

# [![Lua](images/logo.png)](home.html) Version history

[5.5](#5.5)
·
[5.4](#5.4)
·
[5.3](#5.3)
·
[5.2](#5.2)
·
[5.1](#5.1)
·
[5.0](#5.0)
·
[4.0](#4.0)
·
[3.2](#3.2)
·
[3.1](#3.1)
·
[3.0](#3.0)
·
[2.5](#2.5)
·
[2.4](#2.4)
·
[2.2](#2.2)
·
[2.1](#2.1)
·
[1.1](#1.1)
·
[1.0](#1.0)

Here is a chronology of the versions of Lua.
[The evolution of Lua](doc/hopl.pdf)
is documented in a
[paper](docs.html#papers)
presented at
[HOPL III,
the Third ACM SIGPLAN History of Programming Languages Conference](https://en.wikipedia.org/wiki/History_of_Programming_Languages#HOPL_III),
in 2007.
See also its
[continuation](doc/cola.pdf) published in 2025 in a
[journal](https://doi.org/10.1016/j.cola.2025.101326).
The source code and documentation for all releases of Lua are available in the
[download area](ftp/).

![](images/timeline.png)

## Numbering scheme

The releases of Lua are numbered *x.y.z*,
where
*x* is the series, *x.y* is the version, and *z* is the release.

Different releases of the same version correspond to
[bug fixes](bugs.html).
Different releases of the same version have the same
[reference manual](manual/),
the same virtual machine, and are binary compatible (ABI compatible).

Different versions are really different.
The API is likely to be a little different
(but with compatibility switches),
and there is no ABI compatibility:
applications that embed Lua and C libraries for Lua must be recompiled.
The virtual machine is also very likely to be different in a new version:
Lua programs that have been precompiled for one version will not load in a different version.

## Lua 5.5

Lua 5.5.0 was released on 22 Dec 2025.
Its
[main new features](manual/5.5/readme.html#changes)
are
declarations for global variables,
more compact arrays,
new generational mode for garbage collection,
and
major garbage collections done incrementally.

## Lua 5.4

Lua 5.4
was released on 29 Jun 2020.
Its
[main new features](manual/5.4/readme.html#changes)
are
a new generational mode for garbage collection and
const and to-be-closed variables.

The current release is
[Lua 5.4.8](ftp/lua-5.4.8.tar.gz),
released on 4 Jun 2025.

## Lua 5.3

Lua 5.3
was released on 12 Jan 2015.
Its
[main new features](manual/5.3/readme.html#changes)
were
integers,
bitwise operators,
a basic utf-8 library,
and support for both 64-bit and 32-bit platforms.

The last release was
[Lua 5.3.6](ftp/lua-5.3.6.tar.gz),
released on 25 Sep 2020.
There will be no further releases of Lua 5.3.

## Lua 5.2

Lua 5.2
was released on 16 Dec 2011.
Its
[main new features](manual/5.2/readme.html#changes)
were
yieldable pcall and metamethods,
new lexical scheme for globals,
ephemeron tables,
new library for bitwise operations,
light C functions,
emergency garbage collector,
goto statement,
and
finalizers for tables.

The last release was
[Lua 5.2.4](ftp/lua-5.2.4.tar.gz),
released on 7 Mar 2015.
There will be no further releases of Lua 5.2.

## Lua 5.1

Lua 5.1
was released on 21 Feb 2006.
Its main new features were
a new module system,
incremental garbage collection,
new mechanism for varargs,
new syntax for long strings and comments,
mod and length operators,
metatables for all types,
new configuration scheme via luaconf.h,
and a
fully reentrant parser.

The last release was
[Lua 5.1.5](ftp/lua-5.1.5.tar.gz),
released on 17 Feb 2012.
There will be no further releases of Lua 5.1.

## Lua 5.0

Lua 5.0
was released on 11 Apr 2003.
Its main new features were
collaborative multithreading via Lua coroutines,
full lexical scoping instead of upvalues,
and
metatables instead of tags and tag methods.
Lua 5.0 also introduced
booleans,
proper tail calls,
and weak tables.
Other features were
better support for packages,
new API for loading Lua chunks,
new error handling protocol,
better error messages,
and much more.
Lua 5.0 was the first version to be released under the
[new license](license.html).

The last release was
[Lua 5.0.3](ftp/lua-5.0.3.tar.gz),
released on 26 Jun 2006.
There will be no further releases of Lua 5.0.

## Lua 4.0

Lua 4.0
was released on 6 Nov 2000.
Its main new features were
multiples states,
a new API,
"for" statements,
and full speed execution with full debug information.
Also,
Lua 4.0 no longer had built-in functions:
all functions in the standard library are written using the official API.

The last release was
[Lua 4.0.1](ftp/lua-4.0.1.tar.gz),
released on 4 Jul 2002.
There will be no further releases of Lua 4.0.

## Lua 3.2

Lua 3.2
was released on 8 Jul 1999.
Its main new features were
a debug library
and
new table functions.

The last release was
[Lua 3.2.2](ftp/lua-3.2.2.tar.gz),
released on 22 Feb 2000.
There will be no further releases of Lua 3.2.

## Lua 3.1

[Lua 3.1](ftp/lua-3.1.tar.gz)
was released on 11 Jul 1998.
Its main new features were
anonymous functions and
function closures via "upvalues".
([Lua 5.0](#5.0) brought full lexical scoping and dropped upvalues.)
This brought a flavor of functional programming to Lua.
There was also support for multiple global contexts;
however, the API was not fully reentrant
(this had to wait until [Lua 4.0](#4.0)).
Lua 3.1 also saw a
major code re-organization and clean-up,
with much reduced module interdependencies.
Lua 3.1 also adopted double precision for the internal representation of numbers.

## Lua 3.0

[Lua 3.0](ftp/lua-3.0.tar.gz)
was released on 1 Jul 1997.
Its main new feature was
tag methods as a powerful replacement for fallbacks.
Lua 3.0 also introduced auxlib,
a library for helping writing Lua libraries in C,
and support for conditional compilation
(dropped in [Lua 4.0](#4.0)).

## Lua 2.5

[Lua 2.5](ftp/lua-2.5.tar.gz)
was released on 19 Nov 1996.
Its main new features were
pattern matching and vararg functions.

## Lua 2.4

[Lua 2.4](ftp/lua-2.4.tar.gz)
was released on 14 May 1996.
Its main new features were
the external compiler *luac*,
an extended debug interface with hooks,
and the "getglobal" fallback.

## Lua 2.3

Lua 2.3 was never released publicly; it only existed as a beta version.

## Lua 2.2

[Lua 2.2](ftp/lua-2.2.tar.gz)
was released on 28 Nov 1995.
Its main new features were
long strings,
the debug interface,
better stack tracebacks,
extended syntax for function definition,
garbage collection of functions,
and support for pipes.

## Lua 2.1

[Lua 2.1](ftp/lua-2.1.tar.gz)
was released on 7 Feb 1995.
Its main new features were
extensible semantics via fallbacks
and support for object-oriented programming.
It was described in a
[journal paper](spe.html)
which was awarded the
II Compaq Award for Research and Development in Computer Science in 1997.
Starting with Lua 2.1,
Lua became freely available for all purposes,
including commercial uses.

## Lua 1.1

[Lua 1.1](ftp/lua-1.1.tar.gz)
was released on 8 Jul 1994.
It was the
[first public release of Lua](https://compilers.iecc.com/comparch/article/94-07-051) and was described in a
[conference paper](semish94.html).
Lua 1.1 already featured
powerful data description constructs,
simple syntax,
and a bytecode virtual machine.
Lua 1.1 was freely available for academic purposes;
commercial uses had to be negotiated, but none ever were.

## Lua 1.0

[Lua 1.0](ftp/lua-1.0.tar.gz)
was never released publicly.
It was up and running on
[28 Jul 1993](http://lua-users.org/lists/lua-l/2023-07/msg00130.html)
and most probably a couple of months before that.

Last update:
Mon Dec 22 14:21:22 UTC 2025

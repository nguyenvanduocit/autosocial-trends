---
source: hackernews
title: Common Lisp, ASDF, and Quicklisp: packaging explained
url: https://cdegroot.com/programming/commonlisp/2025/11/26/cl-ql-asdf.html
date: 2025-12-11
---

[cdegroot.com](/)

[Code](https://sr.ht/~cdegroot)
[Bio](https://ca.linkedin.com/in/cdegroot)
[About](/about/)
 [![RSS Feed](/img/rss.png)](/feed.xml)

# Common Lisp, ASDF, and Quicklisp: packaging explained

Nov 26, 2025

If there is one thing that confuses newcomers to Common Lisp, it is the interplay of built-in
CL functionality, add-ons like [Quicklisp](https://www.quicklisp.org/beta/) and [ASDF](https://asdf.common-lisp.dev/), and what all the words mean.

Common Lisp is old, and its inspiration is even older. It was developed when there was
zero consensus on how file systems worked, operating systems were more incompatible than you
can probably imagine, and that age shows. It pinned down terminology way before
other languages got to the same point, and, as it happens so often, the late arrivals
decided that they needed different words and these words stuck.

So let’s do a bit of a deep dive and see how all the bits and pieces work and
why they are there. All examples are using SBCL and might be SBCL-specific. Check your
Lisp’s manual if you use something else. Also, I’m (still) linking to the old LispWorks-provided
HyperSpec as I’m not sure that the newer versions are fully done yet.

## Common Lisp

Common Lisp comes with just the bare essentials to work with files. It has to, as that single
specification had to work on microcomputers, mainframes, and all sorts of minicomputers. Even
today with essentially just two branches of the operating system family alive, the difference
are big between Unix derivatives with a single hierarchy (and one of them, macOS, by default with
a case-insensitive interpretation) and MS-DOS derivaties with drive letters and backslashes
but also the option to have network-style paths with double backslashes. So Common Lisp has a
somewhat odd system of “namestrings” (plain strings) and “pathnames” (weird strings). It is
not super important and the spec has details, the tl&dr is that sometimes you will see
a special reader macro `#P"/foo/bar"` instead of just `"/foo/bar"` and the docs will tell
you which of these two is acceptable as an argument for what function. I just wanted to get
that out of the way first. They HyperSpec has [all the details](https://www.lispworks.com/documentation/HyperSpec/Body/19_ca.htm), of course.

### Loading code from files.

With files out of the way, next up is [`LOAD`](https://www.lispworks.com/documentation/HyperSpec/Body/f_load.htm#load). It loads a file “into the Lisp environment” (which
means your running image), but exactly how the file is named and whether it will load a source
file or a compiled file is system-dependent. So

```
(load "foo")
```

can load `foo.lisp` or `foo.fasl` or maybe even `foo.obj` if a Lisp implementation compiles to
C object files. If it is a source file, it’ll evaluate all the forms and do some system-specific
thing with them. The end result is that, well, everything in the file will now be ready for you
to use. So if we have:

```
(defun hello ()
  (print "Hello, world!"))

(print "Done loading!")
```

and we open SBCL:

```
CL-USER(1): (load "test")

"Done loading"
T
CL-USER(2): (hello)

"Hello, world!"
"Hello, world!"
```

Nothing too surprising there. In case we want to speed up loading, we can [compile](https://www.lispworks.com/documentation/HyperSpec/Body/f_cmp_fi.htm#compile-file) the file:

```
CL-USER(7): (compile-file "test")

; compiling file "/home/cees/tmp/test.lisp" (written 26 NOV 2025 09:03:19 PM):

; wrote /home/cees/tmp/test.fasl
; compilation finished in 0:00:00.004
#P"/home/cees/tmp/test.fasl"
NIL
NIL
```

and the next time we ask to load `"test"`, the FASL (“fast load”) file should be loaded. It is purely a time-saver
as the FASL file has been pre-parsed into your Lisp’s in-memory format so can be loaded very
quickly (bypassing `READ` with all its bells and whistles). FASL files are implementation dependent and more often than not even version dependent. This
is pretty much everything that the standard has to say about getting code into the system, and as you
can see, it’s not much.

There is also `PROVIDE` and `REQUIRE`, which operate on something
that the standard calls *modules* (and which are kept in a variable called `*modules*`) but the
standard designates this as deprecated so let’s skip it. Just know it is still lingering there. Don’t
use it (not even when packages “helpfully” wrap it).

### Packages

That `CL-USER` in the prompt is the name of the *package* that you are in. Here is a pretty
bad choice of naming, and an endless source of confusion. A package is a namespace, nothing else, and
[the spec says so much](https://www.lispworks.com/documentation/HyperSpec/Body/11_aa.htm):

> A package establishes a mapping from names to symbols.

These days, we associate the concept of “package” probably with more than that. A bundle of software,
with files and maybe some metadata, a *thing* you can download from somewhere, most likely. But in
Common Lisp, it’s just a tool to map symbol *names* (strings in your source code) to *symbols* (internal addresses in memory). It’s a pretty versatile facility and you should read the docs
on `DEFPACKAGE`. It’s is quite powerful, as it can `:use` other packages, it can shadow symbols,
and whatnot, but at the end of the day, *all* that happens is that when you type:

```
(hello)
```

The REPL will use the current package (in [`*package*`](https://www.lispworks.com/documentation/HyperSpec/Body/v_pkg.htm#STpackageST)) to translate `hello` to whatever function
is in memory, which should exist in the current package (here `COMMON-LISP-USER`, commonly aliased to
`CL-USER`) or in any packages it inherits from (“uses”). You can explicitly tell Lisp to look into
another package (`my-package:hello` is a different function) and even ignore
that package’s explicit list of exported symbols by using a double colon (but don’t make a habit
out of prying into other packages, it breaks modularity). There are a ton of details, but
what counts is that a Common Lisp package is *just* an in-memory namespace thing, a bunch of
connected lookup tables that help the parser map the strings in the files you load to the
correct items inside your running image.

Nothing more, nothing less.

### Systems

Common Lisp documentation often talks about *systems* in a general way like it is an intrinsic part
of the language. However, the standard is vague. In the chapter on “System construction” it deals
with [loading](https://www.lispworks.com/documentation/HyperSpec/Body/24_aa.htm)—the little bit of functionality we already discussed—and [“features”](https://www.lispworks.com/documentation/HyperSpec/Body/24_ab.htm), which are
essentially just flags that are used by the `#+` and `#-` reader macros to make bits of code that
is loaded conditional on the presence of features.

That is all the standard has to say about systems. You can load files and you can make compilation of these
files conditional on feature flags.

### So, where does that leave us?

In a sense, this is all you need. I mean, you can take someone else’s files and `LOAD` them, and
they can be made somewhat portable by using features and saying `#+sbcl` (this code only to be
compiled on SBCL) or `#-linux` (do not compile this on Linux), and the files can organize themselves
by using `DEFPACKAGE` and friends to separate the code into namespaces so everybody can write
code using names like `HELLO` and not step on each other’s toes.

Still, that Common Lisp “system” thing… it’s a bit vague and maybe there’s a hook there to
build something more?

## Another System Definition Facility

Some Common Lisp implementations come with a `DEFSYSTEM`, but that is not portable. There
were early (we’re in 1989-ish now) attempts to have a common version, [`MK:DEFSYSTEM`](https://gitlab.common-lisp.net/mantoniotti/mk-defsystem/-/blob/master/defsystem.lisp), which
still works and is used by some projects. At the turn of a century, another version of `DEFSYSTEM`
was created under the name `ASDF`, which modernized things and quickly turned into the de facto
standard. It can do a lot of things and has extensive docs on [its website](https://asdf.common-lisp.dev/), but we’ll focus here on the essentials.

So, what *is* a system? Well, a library? A, err, *package*? Well, it should be named a package and if
Common Lisp were born a couple of decades later it might have been called a package, but we have
already seen that that name has been given to something closer to what we would probably call “module”
today. “System” it is, then, and ASDF “defines” them.

Still, the closest analogy of a system is a package or a library: a bunch of Lisp code that together
defines some functionality. It’s not a perfect comparison, because a lot of Lisp libraries contain multiple systems: at
the very least, it is customary to have your code define separate systems for regular code and for
test code, and often more systems are defined for, say, optional or contributed code. In any case,
it is not intrinsic to Common Lisp, though, so ASDF strictly adds functionality:

* It allows you to *define* a system (`ASDF:DEFSYSTEM`). That’s the core function: you
  tell it that you have a system with a certain name, and description, and all sorts of
  metadata; and most importantly, what source files are part of the system.
* It allows you to define *dependencies* between systems in your `DEFSYSTEM`.
* It allows you to load such systems wholesale. Instead of the individual files, or a
  developer’s homebrew loading script, you can now work on a higher level and load a system
  by name.

A system still is not a “real” Common Lisp thing: all that it does with respect to the
standard is a bunch of `LOAD` and `COMPILE-FILE` calls. It will keep metadata in memory
about systems that are loaded, but under the hood, loading code is all it does. It comes
with [extensive documentation](https://asdf.common-lisp.dev/asdf.html) and can do a lot of things like additional compilation steps, manage
test runs, etcetera, but if you squint, it just loads code.

An ASDF file, with the extension `.asd`, is also just a Lisp source. The only special thing about
it is that the extension signals to ASDF that it is the file to look for when ASDF is searching
for systems, the one that has the system
definition in a given constellation of source files and directories.

It is important to realize that a “system” and a package are entirely different things:
one is an entity in an add-on tool, the other is intrinsic to Common Lisp’s namespacing. They
can have the same name and often enough, they have the same name (your ASDF system “foo” will
likely define a package “FOO” and it is helpful if that lives in a Git repository called “foo”
which has a file named “foo.asd”) but they are different things living in, well, different
namespaces and should not be confused with each other. One is intrinsic, the other an optional
(but widely used) add-on and they are fully orthogonal things.

### Where does ASDF gets its systems from?

Well, we have a “standard”, albeit a *de facto* one, to bundle Lisp code and describe how
to load it. But if you say “this system here is called FOO and is dependent on BAR”, how
does ASDF find BAR? The answer is very simple: it looks in predefined locations *on your local
disk* (and nowhere else!). There are two predefined locations, one older and one currently
preferred:

1. `~/common-lisp`, the old one;
2. `~/.local/share/common-lisp/source`, the XDG-compliant currently preferred one. Use this.

That’s all. You can extend that list by a very flexible but somewhat complicated mechanism
called “source registries”, extensively [documented](https://asdf.common-lisp.dev/asdf.html#Controlling-where-ASDF-searches-for-systems),
but essentially, the process looks like:

* You refer to a system called `foo`;
* ASDF will look for `foo.asd` under the configured directories;
* If found, it will load that file, and the `DEFSYSTEM` in there will do the rest.

This process recurses, so when system `foo` depends on system `bar` then the process will repeat, until
everything is loaded or an error occurs. All the systems will be found, defined (in your image/memory),
and loaded in the right order (depth-first so that dependencies are loaded, their packages defined and
functions and macros and variables ready for use, before dependents are).

### So, where does that leave us?

We upgraded from “here are a couple of Lisp files, good luck!” to “here is a library with
dependencies”. Good progress. All you need to do now is download the library (as a Zip file
or a tarball), unpack it under `~/.local/share/common-lisp/source`, and load if with
`ASDF:LOAD-SYSTEM`. Of course, the system may declare dependencies so you may get an error
message. Easy enough, hunt for the dependency on the Net, download and unpack *that*, try again, find the next one.

Not perfect, but, well, progress?

## Quicklisp enters the stage

It’s still a bit primitive, though. I mean, when coders were sending each other QIC tapes this
may have been sufficient, but then someone went and had to invent the Internet and now we
just push data over the information superhighway. We should be able to do better, not?
Like “Perl in 1995” better, even?

Just ilke ASDF is an optional add-on to what Common Lisp provides, Quicklisp is an optional
add-on to what ASDF offers. Essentially, it does two things:

1. It adds a new directory to the places where ASDF can find systems;
2. It offers some functions to download a system from “wherever”, which includes “the Internet”.

It hook into ASDF’s dependency resolution so that if there
are more dependencies needed, Quicklisp will go and fetch them as well.

Tadaa: problem solved! We can just open SBCL and say

```
(ql:quickload "foo")
```

and admire a scrolling list of systems being downloaded, unpacked, loaded, analyzed for dependencies,
dependencies being loaded, and so on.

### So, where does that leave us?

We have all the functionality, but there’s one final issue: it is “always on”. In a lot of
other languages, if you start your REPL (say, in Python or in Ruby or in Elixir) in a
certain directory, that carries significance. The language runtime will look for a special
project file, probably, and set up search paths so that they work for that project. Common Lisp
has no such concept, not even after you load ASDF and Quicklisp. So if you have a directory `~/my-code/my-awesome-lisp-project` with
a `my-awesome-lisp-project.asd` in there…. Neither Quicklisp nor ASDF is going to bother
about the current directory and magically find your system.

You *must* play with their rules. Luckily, the rules are simple: go to
`~/.local/share/common-lisp/source` and drop symlinks in there to your projects so that ASDF
can find them. That also means that it
doesn’t matter where you start `sbcl` or Sly or SLIME from, your code will always be found. And
when you then load your system with `QL:QUICKLOAD`, its dependencies will automatically be pulled
in (`ASDF:LOAD-SYSTEM` will still operate locally. It will, of course, use dependencies that
Quicklisp found and downloaded in previous runs).

## Final tips

### Read the source, Luke

```
$ git clone https://gitlab.common-lisp.net/asdf/asdf.git
$ git clone https://github.com/quicklisp/quicklisp-client
$ guix shell cloc -- cloc quicklisp-client asdf
     363 text files.
     264 unique files.
     104 files ignored.

github.com/AlDanial/cloc v 2.06  T=0.13 s (1984.8 files/s, 332115.5 lines/s)
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
Lisp                           219           3981           3889          31548
Markdown                         3            305              0           1173
HTML                             1             11             50            767
Bourne Shell                    10             40            120            526
Text                            18            105              0            357
make                             4             88             61            312
CSS                              1             60              8            236
YAML                             3             28             51            202
Perl                             2             22              8            117
DOS Batch                        2             23             13             74
C                                1              0              0              1
-------------------------------------------------------------------------------
SUM:                           264           4663           4200          35313
-------------------------------------------------------------------------------
```

It’s not *that* much code, and 7400 lines of that is the `UIOP` package that ASDF
includes. UIOP is a package that is very useful in its own right as it is full of
utilities that help you make your code less implementation-dependent, but it won’t
teach you much about ASDF. So 25KLOC, tops. Without tests and contrib and whatnot, each
package is around 5000 lines of well-written Lisp and worth learning. It’s helped
me more than once to understand especially ASDF-VM to just open the code and figure
out what *exactly* is going on.

### KISS: Use package-inferred-system and a single source tree.

Put your Lisp code in directories under, I dunno, say `~/Code/CL`. Symlink that directory to
`~/.local/share/common-lisp/source` and ASDF will be able to find all your
systems. I’ve done some magic using GUIX Home and Stow and whatnot and had to dig
around into how things worked, not recommended. If you have dependencies that are not
in Quicklisp (or [Ultralisp](https://ultralisp.org/), which is worth adding), then check
them out in a central spot (I use `~/OpenSource`) and symlink it into `~/quicklisp/local-projects`.
That way, all your dependency management is in one spot, the Quicklisp directory, whether you
download them or Quicklisp did the job.

Read about ASDF’s
[package-inferred-system](https://asdf.common-lisp.dev/asdf/The-package_002dinferred_002dsystem-extension.html)
and use it. It’ll keep you from having to spend much time writing `.asd` files.
As the docs say, [ASDF itself uses
it](https://gitlab.common-lisp.net/asdf/asdf/-/blob/master/asdf.asd?ref_type=heads#L90)
and since switching to it, there’s no going back for me. In a nutshell, every file is
now expected to be a package *and* a system, same name, so that bit of confusion
goes away. One of my project repos (my main monorepo as of lately, I’m slowly
moving all my other code to it) has a very short ASDF definition:

```
#-asdf3.1 (error "CA.BERKSOFT requires ASDF 3.1 or later.")
(asdf:defsystem "ca.berksoft"
  :class :package-inferred-system)
```

It also has some necessary `REGISTER-SYSTEM-PACKAGES` calls to register Coalton packages. Sometimes you
have dependencies that don’t work well with this scheme and this is the work-around, a small drawback
that is dwarved by the advantages. But essentially, these three lines are it.

With that setup, a library to calculate the color temperature of an RGB color, say, lives in `l/gfx/color-temperature.lisp` and starts with:

```
(uiop:define-package :ca.berksoft/l/gfx/color-temperature
  (:use :cl :infix-math :try)
  (:export :temp->rgb))
```

Note that I use the UIOP version of `defpackage`. It’s a good habit to use the UIOP versions of
functions where possible; it’ll increase portability and more often than not, the UIOP functions
clean up confusion or shortcomings of the standard.

And that is all. ASDF, when I instruct it to load the *system* “ca.berksoft/l/gfx/color-temperature”, will stumble upon the top level `.asd` file, and then will start interpreting the rest (“l/gfx/color-temperature”) as
a relative path under its package-inferred-system functionality. It finds that file, registers it as an ASDF system and loads it, which creates the Common Lisp package. Very simple, very clean. Give it
a try.

Questions? Jump on Libera IRC and join the `#commonlisp` channel, I usually keep a close eye on
it. You can also DM me on [Mastodon](https://mstdn.ca/%40cdegroot) or drop me a mail.

## cdegroot.com

* (c)2014-2016 Cees de Groot
* casedeg@gmail.com

* [https://mstdn.ca/@cdegroot](https://mstdn.ca/%40cdegroot)

Mostly technical musings by Cees de Groot. Expect Elixir, Agile, devops, and whatnot.

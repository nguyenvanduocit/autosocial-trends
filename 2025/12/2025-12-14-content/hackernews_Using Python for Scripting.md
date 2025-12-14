---
source: hackernews
title: Using Python for Scripting
url: https://hypirion.com/musings/use-python-for-scripting
date: 2025-12-14
---

# hyPiRion

* [Home](/)
* [Posts](/musings)
* [About](/about-me)

# Use Python for Scripting!

posted 06 Dec 2025

👋 – seems like you want to read stuff in Norwegian. Good news! This
is available over at [Kodemaker’s
blog](https://www.kodemaker.no/blogg/2025-12-03-bruk-python-for-skripting/)
in Norwegian.

We all like to use the simplest tool that solves our problem, but it’s
not always optimal in the long run, nor when you want to run it on
multiple machines.

Say, for example, that you have a shell script that builds your project.
You want to ensure you can call the script from different folders, so
the first thing you want to do is to find the root of the project and
make that the working directory:

```
SCRIPT_PATH="$(readlink -f "$0")"
PROJECT_ROOT="$(dirname "${SCRIPT_PATH}")"
cd "${PROJECT_ROOT}"
```

After that, because you have [a bug](https://apenwarr.ca/log/20181113)
with some build artefacts being stale *and* cached, you want to remove
all the artefacts you’ve built before. That ensures we don’t have any
old crap left from an earlier build:

```
find build gen -type f \( -name '*.o' -o -name '*.a' \) -print0 \
  | xargs -0 -r rm
```

Of course, we want a build signature for the release, so we put the
build date and git commit into a version file:

```
BUILD_DATE="$(date -d 'now' +%F)"

cp version.template build/version.txt

sed -i "s/@VERSION@/${COMMIT_TAG:-dev}/" build/version.txt
sed -i "s/@BUILD_DATE@/${BUILD_DATE}/" build/version.txt
```

Finally, we build the project with whatever build command(s) the
project relies on.

There are several issues here. For me, this script will work great.
But for a Mac user, all these steps will fail! The calls to
`readlink`, `find`, `xargs`, `date` and `sed` all depend on
functionality that’s provided by the GNU versions (Linux) of these
programs, and they don’t exist or don’t work the same way in the BSD
versions (Mac). Similarly, there are a bunch of arguments that work on
Mac and not on Linux.

There are ways around all of these problems, but they aren’t elegant.
And if you’re only familiar with either Mac or Linux, you’ll often be
surprised when the tool doesn’t work on the other OS. This is probably
most annoying for Mac users who want to modify a script running on a
CI machine, but I can confirm that it’s not fun to start on a new
project where your first task is to fix the development script to make
it work on Linux.

## Python to the Rescue

As the title suggests, Python 3 (hereafter just Python) is a very good
alternative to complicated shell scripts. There are four reasons for
that:

1. Python is installed on pretty much every machine
2. Many developers are familiar with Python (to some extent)
3. Python has a big and standardised standard library
4. Python code is easier to read after the fact

### Well-known and Already Installed

The most important argument in my eyes is that Python 3 is installed
on basically every machine out there.

In addition, we all “know” Python. Maybe we haven’t used it in big
projects, but we have all used it at some point – or something that
looks like it. That makes it much easier to get into.

### Standardised Standard Library

As long as you stick to the standard library, Python will work the
same on all the machines you run your script on. The reference
implementation, CPython, is the one that 99.8% of developers have
installed and are using. The other 0.2% know what they’re doing, but
I’d still be surprised if they experience differences: All the
implementations I know of work hard to have semantically equivalent
behaviour to the reference implementation.

The only thing you need to be aware of is which version of Python
you’re relying on. In practice, only relatively new packages may cause
issues; if some machines are using an older Python version, some
methods or packages may not be available. For common usage in
scripts, that doesn’t happen often.

And backwards compatibility is something Python takes seriously – as
all big languages should. [PEP 387](https://peps.python.org/pep-0387)
gives guidance on how breaking changes are introduced, and that
seldom happens. But if something’s not deprecated, your code will run
just fine for at least 5 years without any issues.

Of course, you want to know ahead of time if you’re using code that is
deprecated and may go away in the future. If you add

```
import warnings
warnings.simplefilter("default", DeprecationWarning)
```

at the top of your script, you’ll get deprecation warnings for
functions you run in your code, and usually some good tips on how to
migrate away.

As an example, `datetime.utcnow()` was deprecated in version 3.12,
and the warning you get if you turn on `DeprecationWarning` is that
you should replace it with `datetime.now(timezone.utc)` instead. Note
that this is likely a “soft deprecation” that will never disappear
from the standard library. But if the maintainers do decide to remove
it, then PEP 387 states it’ll (most likely) happen in 2028 at the
earliest.

### Big Standard Library

Together with the fact that the standard library is there and won’t go
away, the biggest reason to use Python is that it is big. You can do
all the actions I mentioned above with the standard library, and a ton
of other things as well.

Need to read/write JSON or XML? No problem. Need to fetch some data
from the Internet? There’s an HTTP client built in[1](#fn:curl-vs-fetch).
You have access to good time types, good data structures, even a
sqlite3 package if you need it.

Aside from specific build tools like `protoc`, `openapi` or the
compiler for the languages/frameworks you use, it’s hard to think of
something you can easily do in a shell script that you can’t also do
easily in Python.

### Easier to Read

I don’t like people saying “language X is easier to read than Y”,
especially if the languages have roughly the same semantics. Those
arguments/comments are usually a long-winded way of saying “I can read
X better because I have used it more often”.

But there’s a bit more weight behind claiming that Python is easier to
read than shell scripts. First and foremost, if you don’t use either
of them that often, it’s much harder to recall and understand the data
types and string operations in a shell script than in Python. For
example, here’s the way you capitalise a list of strings in Bash:

```
morning_greetings=('hi' 'hello' 'good morning')
energetic_morning_greetings=()

for s in "${morning_greetings[@]}"; do
  energetic_morning_greetings+=( "${s^^}!" )
done
```

If you don’t use Bash often, then `"${morning_greetings[@]}"` won’t
tell you much, and it’s not obvious why it’s written that way. When
you attempt to write it, it’s easy to forget the exact incantation,
and you will silently introduce bugs if you forget some of the parts.
If you forget the `[@]` part, you’ll only get back the first element.
And if you forget the double quotes, then “good morning” is turned
into two elements, not one. And what is `${s^^}` really doing?

By the way, don’t add a comma after the elements in the list, because

```
morning_greetings=('hi', 'hello', 'good morning')
```

is the list with the items `'hi,'`, `'hello,'` and `'good morning'`.

Finally, it should be noted that this doesn’t work in ZSH, and if
you’re using ZSH, you probably are a bit more of a terminal enthusiast
than the average person. And if the enthusiasts can’t use this in
their favourite shell, it’s less likely that they will use it in the
scripts they make.

As you can see, there are tons of mistakes you’re likely to make if
you’re not using Bash often.

The equivalent in Python is this:

```
morning_greetings = ['hi', 'hello', 'good morning']
energetic_morning_greetings = \
   [s.upper() + '!' for s in morning_greetings]
```

This isn’t necessarily easier to grasp the first time you see it: The
list comprehension `[x for y in z]` isn’t immediately understandable.
But when you know how it works, then `s.upper()` is easier to
understand than `${s^^}`.

That’s one of the reasons that makes it more accurate to say that
Python is easier to read: Methods have human-readable names in Python.
And while there’s a pattern for Bash’s string operations, it’s still
easier for a rusty developer to grasp what `s.removesuffix('.com')` is
doing, compared to `${s%.com}`… or what `len(morning_greetings)` is
doing compared to `${#morning_greetings[@]}`.

The other thing that gives greater weight to this argument is that
Python has a larger vocabulary, which means you don’t have to make as
many custom functions or use as many weird workarounds. Sure,
`morning_greetings.pop(1)` isn’t very easy to understand at first glance
(it removes the element at index 1), but after that, it makes sense.
But since Bash doesn’t have a similar method, you’re forced to do
things like this:

```
unset 'morning_greetings[1]'
arr=( "${morning_greetings[@]}" )
```

and that is certainly not easy to understand, and even worse, there’s
no docstring to look up to grok what it does.

## Don’t Throw the Baby Out with the Bathwater

All of these reasons to use Python come up when your script grows to a
certain size, or you’re doing things that are hard or unreadable in
Bash. And from experience, when the script’s already in Bash, you
won’t even consider rewriting it in Python.

As with all things, there’s obviously no need to port a straightforward
10-20 line Bash script. But if you struggle to comprehend what the
heck’s going on next time you’re editing the script, don’t forget that
Python’s a good alternative.

---

This was originally a post written in Norwegian, which you can
read [at
Kodemaker’s blog](https://www.kodemaker.no/blogg/2025-12-03-bruk-python-for-skripting/).

1. And you get rid of the `curl` vs. `wget` debate. [↩](#fnref:curl-vs-fetch)

Copyright 2025 Jean Niklas L'orange

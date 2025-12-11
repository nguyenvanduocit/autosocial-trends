---
source: hackernews
title: Factor 0.101 now available
url: https://re.factorcode.org/2025/12/factor-0-101-now-available.html
date: 2025-12-11
---

[Home](../../)
[Tags](../../tags)
[GitHub](https://github.com/mrjbq7/re-factor)

# Re: Factor

Factor: the language, the theory, and the practice.

## Factor 0.101 now available

Monday, December 8, 2025

[#release](../../tags/release.html)

*“Keep thy airspeed up, lest the earth come from below and smite thee.” -
William Kershner*

I’m very pleased to announce the release of [Factor](https://factorcode.org)
0.101!

| OS/CPU | Windows | Mac OS | Linux |
| --- | --- | --- | --- |
| x86 | [0.101](https://downloads.factorcode.org/releases/0.101/factor-windows-x86-32-0.101.zip) |  | [0.101](https://downloads.factorcode.org/releases/0.101/factor-linux-x86-32-0.101.tar.gz) |
| x86-64 | [0.101](https://downloads.factorcode.org/releases/0.101/factor-windows-x86-64-0.101.zip) | [0.101](https://downloads.factorcode.org/releases/0.101/factor-macos-x86-64-0.101.dmg) | [0.101](https://downloads.factorcode.org/releases/0.101/factor-linux-x86-64-0.101.tar.gz) |

**Source code**: [0.101](https://downloads.factorcode.org/releases/0.101/factor-src-0.101.zip)

This release is brought to you with almost 700 commits by the following individuals:

> Aleksander Sabak, Andy Kluger, Cat Stevens, Dmitry Matveyev, Doug Coleman,
> Giftpflanze, John Benediktsson, Jon Harper, Jonas Bernouli, Leo Mehraban, Mike
> Stevenson, Nicholas Chandoke, Niklas Larsson, Rebecca Kelly, Samuel Tardieu,
> Stefan Schmiedl, [@Bruno-366](https://github.com/Bruno-366),
> [@bobisageek](https://github.com/bobisageek),
> [@coltsingleactionarmyocelot](https://github.com/coltsingleactionarmyocelot),
> [@inivekin](https://github.com/inivekin),
> [@knottio](https://github.com/knottio), [@timor](https://github.com/timor)

Besides some bug fixes and library improvements, I want to highlight the following changes:

* Moved the UI to render buttons and scrollbars rather than using images, which
  allows easier theming.
* Fixed [HiDPI](../../2025/09/hidpi.html) scaling on Linux and
  Windows, although it currently doesn’t update the window settings when
  switching between screens with different scaling factors.
* Update to Unicode 17.0.0.
* Plugin support for the [Neovim editor](https://re.factorcode.org/2025/08/neovim.html).

Some possible backwards compatibility issues:

* The argument order to `ltake` was swapped to be more consistent with words like `head`.
* The `environment` vocabulary on Windows now supports disambiguating `f` and `""` (empty) values
* The `misc/atom` folder was removed in favor of the [factor/atom-language-factor](https://github.com/factor/atom-language-factor) repo.
* The `misc/Factor.tmbundle` folder was removed in favor of the [factor/factor.tmbundle](https://github.com/factor/factor.tmbundle) repo.
* The `misc/vim` folder was removed in favor of the [factor/factor.vim](https://github.com/factor/factor.vim) repo.
* The `http` vocabulary `request` tuple had a slot rename from `post-data` to `data`.
* The `furnace.asides` vocabulary had a slot rename from `post-data` to `data`, and might require running `ALTER TABLE asides RENAME COLUMN "post-data" TO data;`.
* The `html.streams` vocabulary was renamed to `io.streams.html`
* The `pdf.streams` vocabulary was renamed to `io.streams.pdf`

### What is Factor

Factor is a [concatenative](https://www.concatenative.org/), stack-based
programming language with [high-level
features](https://concatenative.org/wiki/view/Factor/Features/The%20language)
including dynamic types, extensible syntax, macros, and garbage
collection. On a practical side, Factor has a [full-featured
library](https://docs.factorcode.org/content/article-vocab-index.html),
supports many different platforms, and has been extensively documented.

The implementation is [fully
compiled](https://concatenative.org/wiki/view/Factor/Optimizing%20compiler)
for performance, while still supporting [interactive
development](https://concatenative.org/wiki/view/Factor/Interactive%20development).
Factor applications are portable between all common platforms. Factor
can [deploy stand-alone
applications](https://concatenative.org/wiki/view/Factor/Deployment) on
all platforms. Full source code for the Factor project is available
under a BSD license.

### New libraries:

* [base92](https://docs.factorcode.org/content/vocab-base92.html): adding support for Base92 encoding/decoding
* [bitcask](https://docs.factorcode.org/content/vocab-bitcask.html): implementing the [Bitcask key/value database](../../2025/06/bitcask.html)
* [bluesky](https://docs.factorcode.org/content/vocab-bluesky.html): adding support for the BlueSky protocol
* [calendar.holidays.world](https://docs.factorcode.org/content/vocab-calendar.holidays.world.html): adding some new holidays including [World Emoji Day](../../2025/07/world-emoji-day.html)
* [classes.enumeration](https://docs.factorcode.org/content/vocab-classes.enumeration.html): adding *enumeration classes* and new `ENUMERATION:` syntax word
* [colors.oklab](https://docs.factorcode.org/content/vocab-colors.oklab.html): adding support for [OKLAB color space](https://en.wikipedia.org/wiki/Oklab_color_space)
* [colors.oklch](https://docs.factorcode.org/content/vocab-colors.oklch.html): adding support for [OKLCH color space](https://en.wikipedia.org/wiki/Oklab_color_space)
* [colors.wavelength](https://docs.factorcode.org/content/vocab-colors.wavelength.html): adding `wavelength>rgba`
* [combinators.syntax](https://docs.factorcode.org/content/vocab-combinators.syntax.html): adding experimental *combinator syntax* words `@[`, `*[`, and `&[`, and short-circuiting `n&&[`, `n||[`, `&&[` and `||[`
* [continuations.extras](https://docs.factorcode.org/content/vocab-continuations.extras.html): adding `with-datastacks` and `datastack-states`
* [dotenv](https://docs.factorcode.org/content/vocab-dotenv.html): implementing support for [Dotenv](../../2025/06/dotenv.html) files
* [edn](https://docs.factorcode.org/content/vocab-edn.html): implementing support for [Extensible Data Notation](../../2025/10/extensible-data-notation.html)
* [editors.cursor](https://docs.factorcode.org/content/vocab-editors.cursor.html): adding support for the [Cursor](https://www.cursor.com) editor
* [editors.rider](https://docs.factorcode.org/content/vocab-editors.rider.html): adding support for the [JetBrains Rider](https://www.jetbrains.com/rider/) editor
* [gitignore](https://docs.factorcode.org/content/vocab-gitignore.html): parser for `.gitignore` files
* [http.json](https://docs.factorcode.org/content/vocab-http.json.html): promoted `json.http` and added some useful words
* [io.streams.farkup](https://docs.factorcode.org/content/vocab-io.streams.farkup.html): a [Farkup](https://concatenative.org/wiki/view/Farkup) formatted stream protocol
* [io.streams.markdowns](https://docs.factorcode.org/content/vocab-io.streams.markdown.html): a [Markdown](https://en.wikipedia.org/wiki/Markdown) formatted stream protocol
* [locals.lazy](https://docs.factorcode.org/content/vocab-locals.lazy.html): prototype of [emit syntax](../../2024/10/emit.html)
* [monadics](https://docs.factorcode.org/content/vocab-monadics.html): alternative vocabulary for using Haskell-style monads, applicatives, and functors
* [multibase](https://docs.factorcode.org/content/vocab-multibase.html): implementation of [Multibase](https://github.com/multiformats/multibase)
* [pickle](https://docs.factorcode.org/content/vocab-pickle.html): support for the [Pickle](../../2025/08/pickle.html) serialization format
* [persistent.hashtables.identity](https://docs.factorcode.org/content/vocab-persistent.hashtables.identity.html): support an [identity-hashcode](https://docs.factorcode.org/content/word-identity-hashcode%2Ckernel.html) version of persisent hashtables
* [raylib.live-coding](https://docs.factorcode.org/content/vocab-raylib.live-coding.html): demo of a vocabulary to do “live coding” of Raylib programs
* [rdap](https://docs.factorcode.org/content/vocab-rdap.html): support for the [Registration Data Access Protocol](../../2025/03/rdap.html)
* [reverse](https://docs.factorcode.org/content/vocab-reverse.html): implementation of the [std::flip](../../2025/09/std-flip.html)
* [slides.cli](https://docs.factorcode.org/content/vocab-slides.cli.html): simple text-based command-line interface for slides
* [tools.highlight](https://docs.factorcode.org/content/vocab-tools.highlight.html): command-line syntax-highlighting tool
* [tools.random](https://docs.factorcode.org/content/vocab-tools.random.html): command-line random generator tool
* [ui.pens.rounded](https://docs.factorcode.org/content/vocab-ui.pens.rounded.html): adding rounded corner [pen](https://docs.factorcode.org/content/vocab-ui.pens.html)
* [ui.pens.theme](https://docs.factorcode.org/content/vocab-ui.pens.theme.html): experimental themed [pen](https://docs.factorcode.org/content/vocab-ui.pens.html)
* [ui.tools.theme](https://docs.factorcode.org/content/vocab-ui.tools.theme.html): some words for updating [UI developer tools](https://docs.factorcode.org/content/article-ui-tools.html) themes

### Improved libraries:

* [alien.syntax](https://docs.factorcode.org/content/vocab-alien.syntax.html): added `C-LIBRARY:` syntax word
* [assocs.extras](https://docs.factorcode.org/content/vocab-assocs.extras.html): added `nzip` and `nunzip`, `map-zip` and `map-unzip` macros
* [base32](https://docs.factorcode.org/content/vocab-base32.html): adding the [human-oriented Base32](http://philzimmermann.com/docs/human-oriented-base-32-encoding.txt) encoding via `zbase32>` and `>zbase32`
* [base64](https://docs.factorcode.org/content/vocab-base64.html): minor performance improvement
* [benchmark](https://docs.factorcode.org/content/vocab-benchmark.html): adding more benchmarks
* [bootstrap.assembler](https://docs.factorcode.org/content/vocab-bootstrap.assembler.html): fixes for ARM-64
* [brainfuck](https://docs.factorcode.org/content/vocab-brainfuck.html): added `BRAINFUCK:` syntax word and `interpret-brainfuck`
* [bson](https://docs.factorcode.org/content/vocab-bson.html): use [linked-assocs](https://docs.factorcode.org/content/article-linked-assocs.html) to preserve order
* [cache](https://docs.factorcode.org/content/vocab-cache.html): implement `M\ cache-assoc delete-at`
* [calendar](https://docs.factorcode.org/content/vocab-calendar.html): adding `year<`, `year<=`, `year>`, `year>=` words
* [calendar.format](https://docs.factorcode.org/content/vocab-calendar.format.html): parse human-readable and elapsed-time output back into duration objects
* [cbor](https://docs.factorcode.org/content/vocab-cbor.html): use [linked-assocs](https://docs.factorcode.org/content/article-linked-assocs.html) to preserve order
* [classes.mixin](https://docs.factorcode.org/content/vocab-classes.mixin.html): added `definer` implementation
* [classes.singleton](https://docs.factorcode.org/content/vocab-classes.singleton.html): added `definer` implementation
* [classes.tuple](https://docs.factorcode.org/content/vocab-classes.tuple.html): added `tuple>slots`, rename `tuple>array` to `pack-tuple` and `>tuple` to `unpack-tuple`.
* [classes.union](https://docs.factorcode.org/content/vocab-classes.union.html): added `definer` implementation
* [checksums.sha](https://docs.factorcode.org/content/vocab-checksums.sha.html): some 20-40% performance improvements
* [command-line](https://docs.factorcode.org/content/vocab-command-line.html): allow passing script name of `-` to use stdin
* [command-line.parser](https://docs.factorcode.org/content/vocab-command-line.parser.html): support for [Argument Parser Commands](../../2025/07/argument-parser-commands.html)
* [command-line.startup](https://docs.factorcode.org/content/vocab-command-line.startup.html): document `-q` quiet mode flag
* [concurrency.combinators](https://docs.factorcode.org/content/vocab-concurrency.combinators.html): faster `parallel-map` and `parallel-assoc-map` using a [count-down latch](https://docs.factorcode.org/content/article-concurrency.count-downs.html)
* [concurrency.promises](https://docs.factorcode.org/content/vocab-concurrency.promises.html): 5-7% performance improvement
* [continuations](https://docs.factorcode.org/content/vocab-continuations.html): improve docs and fix stack effect for `ifcc`
* [countries](https://docs.factorcode.org/content/vocab-countries.html): adding `CQ` country code for [Sark](https://en.wikipedia.org/wiki/Sark)
* [cpu.architecture](https://docs.factorcode.org/content/vocab-cpu.architecture.html): fix `*-branch` stack effects
* [cpu.arm](https://docs.factorcode.org/content/vocab-cpu.arm.html): fixes for ARM-64
* [crontab](https://docs.factorcode.org/content/vocab-crontab.html): added `parse-crontab` which ignores blank lines and comments
* [db](https://docs.factorcode.org/content/vocab-db.html): making `query-each` row-polymorphic
* [delegate.protocols](https://docs.factorcode.org/content/vocab-delegate.protocols.html): adding `keys` and `values` to `assoc-protocol`
* [discord](https://docs.factorcode.org/content/vocab-discord.html): better support for network disconnects, added a configurable retry interval
* [discord.chatgpt-bot](https://docs.factorcode.org/content/vocab-discord.chatgpt-bot.html): some fixes for [LM Studio](https://lmstudio.ai/)
* [editors](https://docs.factorcode.org/content/vocab-editors.html): make the *editor restart* nicer looking
* [editors.focus](https://docs.factorcode.org/content/vocab-editors.focus.html): support *open-file-to-line-number* on newer releases, support Linux and Window
* [editors.zed](https://docs.factorcode.org/content/vocab-editors.zed.html): support use of [Zed](https://zed.dev/) on Linux
* [endian](https://docs.factorcode.org/content/vocab-endian.html): faster endian conversions of c-ptr-like objects
* [environment](https://docs.factorcode.org/content/vocab-environment.html): adding `os-env?`
* [eval](https://docs.factorcode.org/content/vocab-eval.html): move datastack and error messages to stderr
* [fonts](https://docs.factorcode.org/content/vocab-fonts.html): make `<font>` take a name, easier defaults
* [furnace.asides](https://docs.factorcode.org/content/vocab-furnace.asides.html): rename `post-data` slot on `aside` tuples to `data`
* [generalizations](https://docs.factorcode.org/content/vocab-generalizations.html): moved some *dip* words to [shuffle](https://docs.factorcode.org/content/vocab-shuffle.html)
* [help.tour](https://docs.factorcode.org/content/vocab-help.tour.html): fix some typos/grammar
* [html.templates.chloe](https://docs.factorcode.org/content/vocab-html.template.chloe.html): improve use of `CDATA` tags for unescaping output
* [http](https://docs.factorcode.org/content/vocab-http.html): rename `post-data` slot on `request` tuples to `data`
* [http.json](https://docs.factorcode.org/content/vocab-http.json.html): adding `http-json` that doesn’t return the response object
* [http.websockets](https://docs.factorcode.org/content/vocab-http.websockets.html): making `read-websocket-loop` row-polymorphic
* [ini-file](https://docs.factorcode.org/content/vocab-ini-file.html): adding `ini>file`, `file>ini`, and use `LH{ }` to preserve configuration order
* [io.encodings.detect](https://docs.factorcode.org/content/vocab-io.encodings.detect.html): adding `utf7` detection
* [io.encodings.utf8](https://docs.factorcode.org/content/vocab-io.encodings.utf8.html): adding `utf8-bom` to handle optional BOM
* [io.random](https://docs.factorcode.org/content/vocab-io.random.html): speed up `random-line` and `random-lines`
* [io.streams.ansi](https://docs.factorcode.org/content/vocab-io.streams.ansi.html): adding documentation and tests, support dim foreground on terminals that support it
* [io.streams.escape-codes](https://docs.factorcode.org/content/vocab-io.streams.escape-codes.html): adding documentation and tests
* [ip-parser](https://docs.factorcode.org/content/vocab-ip-parser.html): adding IPV4 and IPV6 network words
* [kernel](https://docs.factorcode.org/content/vocab-kernel.html): adding `until*`, fix docs for `and*` and `or*`
* [linked-sets](https://docs.factorcode.org/content/vocab-linked-sets.html): adding `LS{` syntax word
* [lists.lazy](https://docs.factorcode.org/content/vocab-lists.lazy.html): changed the argument order in `ltake`
* [macho](https://docs.factorcode.org/content/vocab-macho.html): support a few more link edit commands
* [make](https://docs.factorcode.org/content/vocab-make.html): adding `,%` for a `push-at` variant
* [mason.release.tidy](https://docs.factorcode.org/content/vocab-mason.release.tidy.html): cleanup a few more git artifacts
* [math.combinatorics](https://docs.factorcode.org/content/vocab-math.combinatorics.html): adding *counting* words
* [math.distances](https://docs.factorcode.org/content/vocab-math.distances.html): adding `jaro-distance` and `jaro-winkler-distance`
* [math.extras](https://docs.factorcode.org/content/vocab-math.extras.html): added `all-removals`, support [Recamán’s sequence](../../2025/05/recamans-sequence.html), and [Tribonacci Numbers](../../2025/07/tribonacci-numbers.html)
* [math.factorials](https://docs.factorcode.org/content/vocab-math.factorials.html): added `subfactorial`
* [math.functions](https://docs.factorcode.org/content/vocab-math.functions.html): added “closest to zero” modulus
* [math.parser](https://docs.factorcode.org/content/vocab-math.parser.html): improve ratio parsing for consistency
* [math.primes](https://docs.factorcode.org/content/vocab-math.primes.html): make `prime?` safe from non-integer inputs
* [math.runge-kutta](https://docs.factorcode.org/content/vocab-math.runge-kutta.html): make generalized improvements to the [Runge-Kutta](https://en.wikipedia.org/wiki/Runge%E2%80%93Kutta_methods) solver
* [math.similarity](https://docs.factorcode.org/content/vocab-math.similarity.html): adding `jaro-similarity`, `jaro-winkler-similarity`, and `trigram-similarity`
* [math.text.english](https://docs.factorcode.org/content/vocab-math.text.english.html): fix issue with very large and very small floats
* [metar](https://docs.factorcode.org/content/vocab-metar.html): updated the abbreviations glossary
* [mime.types](https://docs.factorcode.org/content/vocab-mime.types.html): updating `mime.types` file
* [msgpack](https://docs.factorcode.org/content/vocab-msgpack.html): use [linked-assocs](https://docs.factorcode.org/content/article-linked-assocs.html) to preserve order
* [qw](https://docs.factorcode.org/content/vocab-qw.html): adding `qw:` syntax
* [path-finding](https://docs.factorcode.org/content/vocab-path-finding.html): added `find-path*`
* [peg.parsers](https://docs.factorcode.org/content/vocab-peg.parsers.html): faster `list-of` and `list-of-many`
* [progress-bars.models](https://docs.factorcode.org/content/vocab-progress-bars.models.html): added `with-progress-display`, `map-with-progress-bar`, `each-with-progress-bar`, and `reduce-with-progress-bar`
* [raylib](https://docs.factorcode.org/content/vocab-raylib.html): adding `trace-log` and `set-trace-log-level`, updated to Raylib 5.5
* [readline-listener](https://docs.factorcode.org/content/vocab-readline-listener.html): store history across sessions, support color on terminals that support it
* [robohash](https://docs.factorcode.org/content/vocab-robohash.html): support for `"set4"`, `"set5"`, and `"set6"` types
* [sequences](https://docs.factorcode.org/content/vocab-sequences.html): rename `midpoint@` to `midpoint`, faster `each-from` and `map-reduce` on slices
* [sequences.extras](https://docs.factorcode.org/content/vocab-sequences.extras.html): adding `find-nth`, `find-nth-last`, `subseq-indices`, `deep-nth`, `deep-nth-of`, `2none?`, `filter-errors`, `reject-errors`, `all-same?`, `adjacent-differences`, and `partial-sum`.
* [sequences.generalizations](https://docs.factorcode.org/content/vocab-sequences.generalizations.html): fix `?firstn` and `?lastn` for string inputs, removed `(nsequence)` which duplicates `set-firstn-unsafe`
* [sequences.prefixed](https://docs.factorcode.org/content/vocab-sequences.prefixed.html): swap order of `<prefixed>` arguments to match `prefix`
* [sequences.repeating](https://docs.factorcode.org/content/vocab-sequences.repeating.html): adding `<cycles-from>` and `cycle-from`
* [sequences.snipped](https://docs.factorcode.org/content/vocab-sequences.snipped.html): fixed out-of-bounds issues
* [scryfall](https://docs.factorcode.org/content/vocab-scryfall.html): update for duskmourn
* [shuffle](https://docs.factorcode.org/content/vocab-shuffle.html): improve stack-checking of `shuffle(` syntax, added `SHUFFLE:` syntax, `nreverse`
* [sorting](https://docs.factorcode.org/content/vocab-sorting.html): fix `sort-with` to apply the quot with access to the stack below
* [sorting.human](https://docs.factorcode.org/content/vocab-sorting.human.html): implement [human sorting improved](../../2025/03/human-sorting-improved.html)
* [system-info.macos](https://docs.factorcode.org/content/vocab-system-info.macos.html): adding “Tahoe” code-name for [macOS 26](https://www.apple.com/newsroom/2025/06/macos-tahoe-26-makes-the-mac-more-capable-productive-and-intelligent-than-ever/)
* [terminfo](https://docs.factorcode.org/content/vocab-terminfo.html): add words for querying specific output capabilities
* [threads](https://docs.factorcode.org/content/vocab-threads.html): define a generalized `linked-thread` which used to be for `concurrency.mailboxes` only
* [toml](https://docs.factorcode.org/content/vocab-toml.html): use [linked-assocs](https://docs.factorcode.org/content/article-linked-assocs.html) to preserve order, adding `>toml` and `write-toml`
* [tools.annotations](https://docs.factorcode.org/content/vocab-tools.annotations.html): adding `<WATCH ... WATCH>` syntax
* [tools.deploy](https://docs.factorcode.org/content/vocab-tools.deploy.html): adding a command-line interface for deploy options
* [tools.deploy.backend](https://docs.factorcode.org/content/vocab-tools.deploy.backend.html): fix boot image location in system-wide installations
* [tools.deploy.unix](https://docs.factorcode.org/content/vocab-tools.deploy.unix.html): change binary name to append `.out` to fix conflict with vocab resources
* [tools.directory-to-file](https://docs.factorcode.org/content/vocab-tools.directory-to-file.html): better test file metrics, print filename for editing
* [tools.memory](https://docs.factorcode.org/content/vocab-tools.memory.html): adding `heap-stats-of` arbitrary sequence of instances, and `total-size` size of everything pointed to by an object
* [txon](https://docs.factorcode.org/content/vocab-txon.html): use [linked-assocs](https://docs.factorcode.org/content/article-linked-assocs.html) to preserve order
* [ui](https://docs.factorcode.org/content/vocab-ui.html): adding `adjust-font-size`
* [ui.gadgets.buttons](https://docs.factorcode.org/content/vocab-ui.gadgets.buttons.html): stop using images and respect theme colors
* [ui.gadgets.sliders](https://docs.factorcode.org/content/vocab-ui.gadgets.sliders.html): stop using images and respect theme colors
* [ui.theme.base16](https://docs.factorcode.org/content/vocab-ui.theme.base16.html): adding a lot more (270!) [Base16 Themes](../../2024/10/base16-themes.html)
* [ui.tools](https://docs.factorcode.org/content/vocab-ui.tools.html): adding font-sizing keyboard shortcuts
* [ui.tools.browser](https://docs.factorcode.org/content/vocab-ui.tools.browser.html): more responsive font sizing
* [ui.tools.listener](https://docs.factorcode.org/content/vocab-ui.tools.listener.html): more responsive font sizing, adding some [UI listener styling](https://docs.factorcode.org/content/article-ui-listener-style.html)
* [ui.tools.listener.completion](https://docs.factorcode.org/content/vocab-ui.tools.listener.completion.html): allow spaces in history search popup
* [unicode](https://docs.factorcode.org/content/vocab-unicode.html): update to [Unicode 17.0.0](https://www.unicode.org/versions/Unicode17.0.0/)
* [webapps.planet](https://docs.factorcode.org/content/vocab-webapps.planet.html): improve CSS for `video` tags
* [words](https://docs.factorcode.org/content/vocab-words.html): adding `define-temp-syntax`
* [zoneinfo](https://docs.factorcode.org/content/vocab-zoneinfo.html): update to version 2025b

### Removed libraries

* `ui.theme.images`

### VM Improvements:

* More work on ARM64 backend (fix set-callstack, fix generic dispatch)

[« DNS LOC Records](../../2025/12/dns-loc-records.html)
[zxcvbn »](../../2025/12/zxcvbn.html)

© [John Benediktsson](https://github.com/mrjbq7) 2008-2025

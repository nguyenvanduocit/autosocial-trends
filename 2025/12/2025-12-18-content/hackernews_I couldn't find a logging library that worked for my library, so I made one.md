---
source: hackernews
title: I couldn't find a logging library that worked for my library, so I made one
url: https://hackers.pub/@hongminhee/2025/logtape-fedify-case-study
date: 2025-12-18
---

[![Hackers' Pub](/logo-dark.svg?__frsh_c=27f624c7fbf9db071d91ad5d403116df8f11de7e)](/)

| Syntax | Description | Examples |
| --- | --- | --- |
| `"` keyword `"` | Finds the string within quotes, including spaces. Case-insensitive. (Escape quotes inside with `\"`) | `"Hackers' Pub"` |
| `from:` handle | Finds content written by the specified user. | `from:hongminhee` `from:hongminhee@hollo.social` |
| `lang:` ISO 639-1 | Finds content written in the specified language. | `lang:en` |
| `#` tag | Finds content with the specified tag. Case-insensitive. | `#HackersPub` |
| condition  condition | Finds content that satisfies both conditions on either side of the space (logical AND). | `"Hackers' Pub" lang:en` |
| condition `OR` condition | Finds content that satisfies at least one of the conditions on either side of the OR operator (logical OR). | `#HackersPub OR "Hackers' Pub" lang:en` |
| `(` condition `)` | Combines the operators within the parentheses first. | `(#HackersPub OR "Hackers' Pub" OR "Hackers Pub") lang:en` |

[Sign in](/sign)

# I couldn't find a logging library that worked for my library, so I made one

[![](https://media.hackers.pub/avatars/7df26108-523c-4104-906b-e568d0792e5d.jpg)](/%40hongminhee)

[洪 民憙 (Hong Minhee)](/%40hongminhee) @hongminhee@hackers.pub

12/12/2025, 3:41:25 PM

Table of Contents

1. [The problem: existing loggers assume you're building an app](#019b1339-5cf0-78c5-92b1-b8c548e95179--the-problem-existing-loggers-assume-youre-building-an-app)
2. [The solution: hierarchical categories with zero default output](#019b1339-5cf0-78c5-92b1-b8c548e95179--the-solution-hierarchical-categories-with-zero-default-output)
3. [Request tracing with implicit contexts](#019b1339-5cf0-78c5-92b1-b8c548e95179--request-tracing-with-implicit-contexts)
4. [What users actually see](#019b1339-5cf0-78c5-92b1-b8c548e95179--what-users-actually-see)
5. [Lessons learned](#019b1339-5cf0-78c5-92b1-b8c548e95179--lessons-learned)
6. [Try it yourself](#019b1339-5cf0-78c5-92b1-b8c548e95179--try-it-yourself)

**Read in other languages** → [한국어](/%40hongminhee/2025/logtape-fedify-case-study/ko) · [한국어(대한민국)](/%40hongminhee/2025/logtape-fedify-case-study/ko-KR)

When I started building [Fedify](https://fedify.dev/), an ActivityPub server framework, I ran into a problem that surprised me: I couldn't figure out how to add logging.

Not because logging is hard—there are dozens of mature logging libraries for JavaScript. The problem was that they're primarily designed for *applications*, not for libraries that want to stay unobtrusive.

I wrote about this [a few months ago](https://hackers.pub/%40hongminhee/2025/logtape-for-libraries), and the response was modest—some interest, some skepticism, and quite a bit of debate about whether the post was AI-generated. I'll be honest: English isn't my first language, so I use LLMs to polish my writing. But the ideas and technical content are mine.

Several readers wanted to see a real-world example rather than theory.

## The problem: existing loggers assume you're building an app

Fedify helps developers build federated social applications using the ActivityPub protocol. If you've ever worked with federation, you know debugging can be painful. When an activity fails to deliver, you need to answer questions like:

* Did the HTTP request actually go out?
* Was the signature generated correctly?
* Did the remote server reject it? Why?
* Was there a problem parsing the response?

These questions span multiple subsystems: HTTP handling, cryptographic signatures, JSON-LD processing, queue management, and more. Without good logging, debugging turns into guesswork.

But here's the dilemma I faced as a library author: if I add verbose logging to help with debugging, I risk annoying users who don't want their console cluttered with Fedify's internal chatter. If I stay silent, users struggle to diagnose issues.

I looked at the existing options. With winston or Pino, I would have to either:

* Configure a logger inside Fedify (imposing my choices on users), or
* Ask users to pass a logger instance to Fedify (adding boilerplate)

There's also `debug`, which is designed for this use case. But it doesn't give you structured, level-based logs that ops teams expect—and it relies on environment variables, which some runtimes like Deno restrict by default for security reasons.

None of these felt right. So I built [LogTape](https://logtape.org/)—a logging library designed from the ground up for library authors. And Fedify became its first real user.

## The solution: hierarchical categories with zero default output

The key insight was simple: a library should be able to log without producing any output unless the *application* developer explicitly enables it.

Fedify uses LogTape's hierarchical category system to give users fine-grained control over what they see. Here's how the categories are organized:

| Category | What it logs |
| --- | --- |
| `["fedify"]` | Everything from the library |
| `["fedify", "federation", "inbox"]` | Incoming activities |
| `["fedify", "federation", "outbox"]` | Outgoing activities |
| `["fedify", "federation", "http"]` | HTTP requests and responses |
| `["fedify", "sig", "http"]` | HTTP Signature operations |
| `["fedify", "sig", "ld"]` | Linked Data Signature operations |
| `["fedify", "sig", "key"]` | Key generation and retrieval |
| `["fedify", "runtime", "docloader"]` | JSON-LD document loading |
| `["fedify", "webfinger", "lookup"]` | WebFinger resource lookups |

…and about a dozen more. Each category corresponds to a distinct subsystem.

This means a user can configure logging like this:

```
await configure({
  sinks: { console: getConsoleSink() },
  loggers: [
    // Show errors from all of Fedify
    { category: "fedify", sinks: ["console"], lowestLevel: "error" },
    // But show debug info for inbox processing specifically
    { category: ["fedify", "federation", "inbox"], sinks: ["console"], lowestLevel: "debug" },
  ],
});
```

When something goes wrong with incoming activities, they get detailed logs for that subsystem while keeping everything else quiet. No code changes required—just configuration.

## Request tracing with implicit contexts

The hierarchical categories solved the filtering problem, but there was another challenge: correlating logs across async boundaries.

In a federated system, a single user action might trigger a cascade of operations: fetch a remote actor, verify their signature, process the activity, fan out to followers, and so on. When something fails, you need to correlate all the log entries for that specific request.

Fedify uses LogTape's implicit context feature to automatically tag every log entry with a `requestId`:

```
await configure({
  sinks: {
    file: getFileSink("fedify.jsonl", { formatter: jsonLinesFormatter })
  },
  loggers: [
    { category: "fedify", sinks: ["file"], lowestLevel: "info" },
  ],
  contextLocalStorage: new AsyncLocalStorage(),  // Enables implicit contexts
});
```

With this configuration, every log entry automatically includes a `requestId` property. When you need to debug a specific request, you can filter your logs:

```
jq 'select(.properties.requestId == "abc-123")' fedify.jsonl
```

And you'll see every log entry from that request—across all subsystems, all in order. No manual correlation needed.

The `requestId` is derived from standard headers when available (`X-Request-Id`, `Traceparent`, etc.), so it integrates naturally with existing observability infrastructure.

## What users actually see

So what does all this configuration actually mean for someone using Fedify?

If a Fedify user doesn't configure LogTape at all, they see nothing. No warnings about missing configuration, no default output, and minimal performance overhead—the logging calls are essentially no-ops.

For basic visibility, they can enable error-level logging for all of Fedify with three lines of configuration. When debugging a specific issue, they can enable debug-level logging for just the relevant subsystem.

And if they're running in production with serious observability requirements, they can pipe structured JSON logs to their monitoring system with request correlation built in.

The same library code supports all these scenarios—whether the user is running on Node.js, Deno, Bun, or edge functions, without extra polyfills or shims. The user decides what they need.

## Lessons learned

Building Fedify with LogTape taught me a few things:

**Design your categories early.** The hierarchical structure should reflect how users will actually want to filter logs. I organized Fedify's categories around subsystems that users might need to debug independently.

**Use structured logging.** Properties like `requestId`, `activityId`, and `actorId` are far more useful than string interpolation when you need to analyze logs programmatically.

**Implicit contexts turned out to be more useful than I expected.** Being able to correlate logs across async boundaries without passing context manually made debugging distributed operations much easier. When a user reports that activity delivery failed, I can give them a single `jq` command to extract everything relevant.

**Trust your users.** Some library authors worry about exposing too much internal detail through logs. I've found the opposite—users appreciate being able to see what's happening when they need to. The key is making it opt-in.

## Try it yourself

If you're building a library and struggling with the logging question—how much to log, how to give users control, how to avoid being noisy—I'd encourage you to look at how Fedify does it.

The [Fedify logging documentation](https://fedify.dev/manual/log) explains everything in detail. And if you want to understand the philosophy behind LogTape's design, my [earlier post](https://hackers.pub/%40hongminhee/2025/logtape-for-libraries) covers that.

[LogTape](https://logtape.org/) isn't trying to replace winston or Pino for application developers who are happy with those tools. It fills a different gap: logging for libraries that want to stay out of the way until users need them. If that's what you're looking for, it might be a better fit than the usual app-centric loggers.

[5](https://hackers.pub/%40hongminhee/2025/logtape-fedify-case-study/reactions "Reactions")

❤️5🎉0😂0😲0🤔0😢0👀0

[0](https://hackers.pub/%40hongminhee/2025/logtape-fedify-case-study/reactions "Reaction statistics")

[0](https://hackers.pub/%40hongminhee/2025/logtape-fedify-case-study "Replies")3[1](https://hackers.pub/%40hongminhee/2025/logtape-fedify-case-study/quotes "Quotes")

# No comments

If you have a fediverse account, you can comment on this article from your own instance. Search https://hackers.pub/ap/articles/019b1339-5cf0-78c5-92b1-b8c548e95179 on your instance and reply to it.

[Code of conduct](/coc) · The source code of this website is available on [GitHub repository](https://github.com/hackers-pub/hackerspub) under the [AGPL 3.0](https://www.gnu.org/licenses/agpl-3.0.html) license.

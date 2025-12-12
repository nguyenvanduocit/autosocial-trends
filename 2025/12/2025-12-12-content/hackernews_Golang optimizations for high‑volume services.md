---
source: hackernews
title: Golang optimizations for high‑volume services
url: https://packagemain.tech/p/golang-optimizations-for-highvolume
date: 2025-12-12
---

[![packagemain.tech](https://substackcdn.com/image/fetch/$s_!ya8w!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F42332f2e-7531-44b1-920c-bba7831fcdbe_777x777.png)](/)

# [![packagemain.tech](https://substackcdn.com/image/fetch/$s_!SmSF!,e_trim:10:white/e_trim:10:transparent/h_72,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcc9c835b-8ed4-4104-bf6a-992aec86a321_950x206.png)](/)

SubscribeSign in

# Golang optimizations for high‑volume services

### Lessons from a Postgres → Elasticsearch pipeline

[![Julien Singler's avatar](https://substackcdn.com/image/fetch/$s_!N5CR!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F39dd31c9-04f3-4ffa-bad4-970764368793_512x512.jpeg)](https://substack.com/%40snickers54)

[Julien Singler](https://substack.com/%40snickers54)

Dec 08, 2025

8

3

Share

[![](https://substackcdn.com/image/fetch/$s_!HiPm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb56a59d5-f31c-424d-a3fa-0eff50005175_1024x1024.png)](https://substackcdn.com/image/fetch/%24s_%21HiPm%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A//substack-post-media.s3.amazonaws.com/public/images/b56a59d5-f31c-424d-a3fa-0eff50005175_1024x1024.png)

## Intro

Building services that sit on top of a Postgres replication slot and continuously stream data into Elasticsearch is a great way to get low‑latency search without hammering your primary database with ad‑hoc queries. But as soon as traffic ramps up, these services become a stress test for Go’s memory allocator, garbage collector, and JSON stack.

This post walks through optimizations applied to a real-world service that:

* Connects to a Postgres replication slot
* Transforms and enriches the change events
* Uses Elasticsearch’s bulk indexer to index and delete documents

The constraints: the service cannot stop reading from the replication slot for long (or Postgres disk will grow), and it cannot buffer unbounded data in memory (or Go’s heap will). The goal is to keep latency and memory stable under sustained high volume.

## The core problem: unbounded streams under tight constraints

Replication slots are relentless: as long as your primary receives writes, the slot will keep producing changes. If your consumer slows down, Postgres has to retain more WAL segments, increasing disk usage on the database server. If your consumer tries to “just buffer more” in memory, the heap will balloon and garbage collection will kick in more frequently, stealing CPU from useful work.

In this setup, you typically have three competing forces:

* Backpressure from Elasticsearch bulk indexing
* The continuous stream of changes from the replication slot
* The Go runtime’s allocator and garbage collector trying to keep up with allocations in your hot path

The design work is about turning this into a stable flow: limit in-flight work, keep memory usage predictable, and reduce per-message overhead.

## JSON performance: why switch from encoding/json to jsoniter

One of the earliest hot spots in services like this is JSON encoding/decoding for documents going into Elasticsearch. The standard library’s `encoding/json` is correct and convenient, but it trades some performance for safety and reflection-based flexibility.

High-volume services often switch to `jsoniter` (github.com/json-iterator/go) for a few reasons:

* Faster encoding/decoding for common patterns
* Less reflection overhead when configured with codegen or field caching
* Better throughput when serializing large batches of similar structs

The wins are most visible when:

* You are serializing many small documents at high frequency (typical for bulk indexing)
* You can keep your types stable and avoid excessive use of `interface{}` and maps
* You care about reducing allocations in the JSON path and shaving microseconds off each object

However, replacing `encoding/json` is never just a drop-in optimization; it changes behavior in subtle ways, especially around nulls and omitted fields.

### Jsoniter compatibility

Jsoniter provides different configurations, one of them is the `ConfigCompatibleWithStandardLibrary which attempt to be generating a really similar payload than the standard library.`

There is, however, some edge cases.

Here one I went through:

* Jsoniter doesn’t seems to work well with the json tag “omitzero” and libraries like guregu/null.v4. Prefer using “omitempty” to have the same behavior because jsoniter will check the .Valid() method.

All in all, this is a drop in replacement, but adding some tests to make sure you don’t have side effects in complex systems will make a big difference.

## Controlling allocations with sync.Pool

Once the JSON serialization hot path is reasonably efficient, memory allocations often become the next bottleneck. Every replication event that flows through your service may involve:

* Allocating a struct to represent the change
* Allocating buffers for JSON encoding
* Allocating intermediate slices and maps during transformations

Under sustained load, this can produce a flood of short-lived objects. The garbage collector has to scan and reclaim them, and that work shows up as CPU usage and latency spikes.

`sync.Pool` is a practical tool for these patterns:

* It lets you reuse objects (structs, buffers, small slices) across requests without manual “object lifecycle” tracking
* Objects in the pool are eligible for garbage collection if they are not in use, so the pool does not create a permanent memory leak
* For hot types (e.g., your “replication event” struct or reusable `[]byte` buffers for JSON encoding), pooling can significantly reduce the number of allocations per event

Good use cases in this pipeline include:

* Reusing buffers for building bulk requests (e.g., `bytes.Buffer` or `[]byte`)
* Reusing small structs that hold metadata for each change event
* Reusing temporary scratch space used during transformations

Some practical guidelines:

* Only pool objects that are frequently allocated and easy to reset to a zero state
* Add helper methods like `Reset()` so the code path that returns an object to the pool always leaves it in a clean state
* Avoid pooling objects that embed context, locks, or anything with complex lifecycle or ownership semantics

Used carefully, `sync.Pool` can cut heap allocations dramatically in a high-throughput Go service, which brings GC frequency and pause times down.

## Garbage collection tuning and experimental GCs

Even after optimizing allocations, GC behavior remains critical in long-lived services under high load.

Starting with go1.25, you can enable an experimental GC at build time that promise better performances. https://go.dev/blog/greenteagc

* Reduce GC-induced latency spikes in services that care more about throughput and tail latency than about absolute minimal memory usage
* Provide more even performance under bursts by scheduling GC work more smoothly over time

In a pipeline that must keep up with a replication slot and a bulk indexer, that trade-off is often desirable:

* Slightly higher steady-state memory usage is acceptable if it avoids GC pauses that temporarily slow down ingestion
* Less erratic latency helps keep Elasticsearch batches flowing and prevents backpressure from building up

However, tuning or switching GC behavior should be the last step, not the first. It works best when:

* Allocations in hot paths have already been reduced with pooling, pre-allocation, and careful data structures
* JSON and other serialization work has been profiled and streamlined
* The service has clear SLOs around memory and latency, and you are comfortable trading one for the other within defined bounds

GC tweaks can then be used to shift the balance slightly rather than to compensate for fundamentally inefficient code.

---

## Putting it together: a stable, high‑volume change pipeline

When all of these optimizations come together, the architecture looks like this:

* A controlled number of goroutines read from the replication slot and push events through a bounded internal queue
* Each event passes through transformation and enrichment code that avoids unnecessary allocations and uses `sync.Pool` for reusable buffers and structs
* JSON serialization uses a faster encoder like `jsoniter`
* A bulk indexer sends batched operations to Elasticsearch with size and concurrency tuned to avoid large in-memory batches but still keep the cluster saturated
* GC behavior is tuned only after profiling, to smooth out remaining latency spikes without blowing out memory usage

The result is a Go service that can keep up with a constant stream of database changes, avoid unbounded buffering, and make efficient use of CPU and memory—all while staying within the operational constraints of Postgres and Elasticsearch.

8

3

Share

Previous

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![amnonbc's avatar](https://substackcdn.com/image/fetch/$s_!Lszd!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5d4c2ff-70c8-4029-a1dc-62cf58be4857_144x144.png)](https://substack.com/profile/15047520-amnonbc?utm_source=comment)

[amnonbc](https://substack.com/profile/15047520-amnonbc?utm_source=substack-feed-item)

[1d](https://packagemain.tech/p/golang-optimizations-for-highvolume/comment/186322550 "Dec 10, 2025, 4:46 PM")

Liked by Alex Pliutau

Any reason you suggest jsoniter, but not encoding/json/v2?

Have you tried porting to the new v2 api with its zero alloc encoding?

Were the savings this produced inadequate?

Expand full comment

ReplyShare

[2 replies](https://packagemain.tech/p/golang-optimizations-for-highvolume/comment/186322550)

[2 more comments...](https://packagemain.tech/p/golang-optimizations-for-highvolume/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2025 Aliaksandr Pliutau · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture

This site requires JavaScript to run correctly. Please [turn on JavaScript](https://enable-javascript.com/) or unblock scripts

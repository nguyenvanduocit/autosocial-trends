---
source: hackernews
title: Microservices should form a polytree
url: https://bytesauna.com/post/microservices
date: 2025-12-13
---

[![Byte Sauna Logo](/TEXT_LOGO.png)](/)

![](/polytree.webp)

# Microservices should form a polytree

December 8, 2025

Microservices are a treacherous path. On paper, they look wonderful: small, focused components; independently deployable units; clean, well-defined interfaces. What could go wrong?

Plenty.

Teams often discover that a system that began as a tidy collection of services has quietly evolved into an Armageddon of impossible-to-trace failures and a painfully slow development experience. Microservices are easy to get wrong and a great way to cause infinite pain. They introduce complexity that has the unfortunate tendency to sneak up on you: you add a call here, expose a tiny API there, share a bit of logic “to stay DRY,” and before you know it, the whole thing is a mess.

To avoid this situation in advance, I propose the following principle: **microservices should form a polytree.**

This is an intentionally strict criterion. Simple enough to enable fast decision-making. The fact is that most teams do not have the time or appetite for endless architecture debates. They just need a straightforward check base for go/no-go decisions on.

## What is a polytree

A **polytree** is a specific type of graph. Stated in precise terms, it's a directed acyclic graph whose underlying undirected graph is a tree. So, for example, this is a polytree:

![A polytree](/trees/polytree.png)

In order to check that this graph satisfies the definition of a polytree, notice that there are no directed cycles. Also the underlying undirected graph is connected and acyclic, i.e. a tree:

![The undirected graph of a polytree](/trees/polytree_tree.png)

What I mean by microservices should form a polytree is that the *dependence structure* between the services should be a polytree. So for example, in the case of the above graph, `n1` may call `n3`, but `n3` may **not** call `n1`.

This rules out painful situations I'll highlight next.

## What is *not* a polytree

For this post, it's important to highlight why a graph may fail to be a polytree, and to explain why excluding such structure is a good idea.

### Counterexample #1: A directed cycle

One great way to mess up a microsevice architecture is to create a cyclical dependency structure such as this one:

![A directed cycle](/trees/cycle.png)

Theoretically, this exposes you to endless issues:

* **State becomes delocalized.** When services in a cycle all read and write pieces of the same conceptual state, no one service has clear ownership. This makes development and debugging potentially difficult.
* **Failures may echo around the cycle**, as each service triggers the next.
* **Resource usage can spiral**, as services retry or hold open connections while waiting for responses that depend on their own work.

**Fix:** Cycles are a symptom of tangled responsibilities. Perhaps the services `n1`, `n2` and `n3` could be a single one. Sometimes functionality should be rearranged so that they form a tree instead.

### Counterexample #2: An undirected cycle

A sneakier one is a dependency structure, where there is no directed cycle, but the underlying undirected graph is cyclical.

![An undirected cycle](/trees/undirected_cycle.png)

![An undirected cycle](/trees/undirected_cycle_undirected.png)

Even without a directed cycle this kind of structure can still cause trouble. Although the architecture may appear clean when examined only through the direction of service calls the deeper dependency network reveals a loop that reduces fault tolerance increases brittleness and makes both debugging and scaling significantly more difficult.

**Fix:** This is, again, a sign of tangled responsibilities. Two ideas to consider are should `n2` and `n3` be a single service *or* should `n4` actually be two different services.

## Why polytrees are nice

A polytree structure enforces a clear, hierarchical flow of responsibility. This brings several practical benefits:

* **Clear ownership of state.** Each piece of data or behavior has a single, unambiguous home. When something breaks, you know exactly where to look.
* **Predictable failure modes.** A failure at a node can only hit nodes below, and it must terminate after a finite number of steps.
* **Simpler reasoning and onboarding.** New engineers can understand the system by starting at the leaves and working upward. No complex back-and-forth call chains to unravel.
* **Independent evolution.** With no hidden cycles, teams can change, version, or redeploy services with far less risk of surprising breakage.

A polytree’s practical advantages flow directly from its mathematical structure. Because a polytree is a directed acyclic graph whose edges impose a partial order, every node has a well-defined position in a hierarchy. That same partial order also enables a clean mental model for engineers as reasoning can proceed inductively from leaves upward, with each component depending only on a finite, well-bounded set of predecessors. The shape of the structure does most of the conceptual work.

The acyclicity of polytrees also ensures that all flows terminate: whether we are talking about failure propagation, data updates, or event handling, everything moves according to the partial order and must reach a leaf or a root after a finite number of steps. This rules out feedback loops, oscillations, and other emergent behaviors that make systems unpredictable and fragile. As a result, teams can evolve different branches of the system independently, knowing that changes remain confined to their downstream cones and cannot sneak back indirectly. The mathematical properties (no cycles, unique paths, and guaranteed termination) translate almost one-to-one into operational clarity, predictable behavior, and ease of evolution.

![My photo](/me.webp)

## Article by

Matias Heikkilä

Let's talkSubscribe

[⬅️ Back to all posts](/post)

© 2025 ByteSauna. All rights reserved.

[Privacy policy](/privacy)

Cookie settings

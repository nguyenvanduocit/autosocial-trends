---
source: hackernews
title: Letta Code
url: https://www.letta.com/blog/letta-code
date: 2025-12-17
---

[Research

![](https://cdn.prod.website-files.com/66b5f39db6c9510dc8cbe834/68ecd074ce970e73cb212219_giphy3-ezgif.com-crop.gif)](/research)

![](https://cdn.prod.website-files.com/66b5f39db6c9510dc8cbe834/68ecd074ce970e73cb212219_giphy3-ezgif.com-crop.gif)

Product

[Developer Platform

Build and deploy stateful agents with advanced long-term memory](/developer-platform)[Letta Code

Use our CLI to connect remote agents to your local computer to automate work](https://docs.letta.com/letta-code)[Learning SDK

Run specialized memory agents to learn about anything and integrate with any AI framework](https://github.com/letta-ai/learning-sdk)![](https://cdn.prod.website-files.com/66b5f39db6c9510dc8cbe834/68ecd074ce970e73cb212219_giphy3-ezgif.com-crop.gif)

![](https://cdn.prod.website-files.com/66b5f39db6c9510dc8cbe834/68ecd074ce970e73cb212219_giphy3-ezgif.com-crop.gif)

Resources

[Blog

Learn about product and research updates](/blog)[Customer Stories

Read about Letta in production](/case-studies)[Demos

See Letta in action](/demos)[Model Leaderboard

Understand which LLMs work best](https://leaderboard.letta.com)[Developer Community

Join the Letta community on Discord](https://discord.gg/letta)![](https://cdn.prod.website-files.com/66b5f39db6c9510dc8cbe834/68ecd074ce970e73cb212219_giphy3-ezgif.com-crop.gif)

![](https://cdn.prod.website-files.com/66b5f39db6c9510dc8cbe834/68ecd074ce970e73cb212219_giphy3-ezgif.com-crop.gif)

Company

[About us

Learn about our mission and team](/about-us)[Careers

Join our team to work on open AI](https://jobs.ashbyhq.com/letta)Contact us

Get in touch![](https://cdn.prod.website-files.com/66b5f39db6c9510dc8cbe834/68ecd074ce970e73cb212219_giphy3-ezgif.com-crop.gif)

[Pricing

![](https://cdn.prod.website-files.com/66b5f39db6c9510dc8cbe834/68ecd074ce970e73cb212219_giphy3-ezgif.com-crop.gif)](/pricing)

[Letta Cloud](https://app.letta.com)

Create, deploy, and manage your agents at scale with Letta Cloud. Build production applications backed by agent microservices with REST APIs.

[Developer docs](https://docs.letta.com/)

Letta adds memory to your LLM services to give them advanced reasoning capabilities and transparent long-term memory (powered by MemGPT).

Light

Dark

[11.3K](https://github.com/cpacker/MemGPT)

Menu

Close

[19K](https://github.com/letta-ai/letta)[Docs](https://docs.letta.com)[Sign In](https://app.letta.com)

Light

Dark

Product

# Letta Code: A Memory-First Coding Agent

December 16, 2025

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/69419caea50a5208e60e88d9_letta-code-blog-thumb.png)

Letta Code is a memory-first coding agent, designed for working with agents that learn over time. When working with coding agents today, interactions happen in independent sessions. Letta Code is built around long-lived agents that persist across sessions and improve with use. Rather than working in independent sessions, each session is tied to a persisted agent that learns. Letta Code is also the #1 model-agnostic OSS harness on TerminalBench, and achieves comparable performance to harnesses built by LLM providers ([Claude Code](https://code.claude.com/docs/en/overview), [Gemini CLI](https://github.com/google-gemini/gemini-cli), [Codex CLI](https://openai.com/codex/)) on their own models.

## Continual Learning & Memory for Coding Agents

Agents today accumulate valuable experience: they receive the user’s preferences and feedback, review significant parts of code, and observe the outcomes of taking actions like running scripts or commands. Yet today this experience is largely wasted. Letta agents learn from experience through [agentic](https://arxiv.org/pdf/2310.08560) [context engineering](https://arxiv.org/abs/2510.04618), [long-term memory](https://www.letta.com/blog/agent-memory), and [skill learning](https://www.letta.com/blog/skill-learning). The more you work with an agent, the more context and memory it accumulates, and the better it becomes.

### **Memory Initialization**

When you get started with Letta Code, you can run an `/init` command to encourage your agent to learn about your existing project. This will trigger your agent to run deep research on your local codebase, forming memories and rewriting its system prompt (through [memory blocks](https://www.letta.com/blog/memory-blocks)) as it learns.

‍

[](https://iasi9yhacrkpgiie.public.blob.vercel-storage.com/letta-code-init.mp4)

‍

Your agent will continue to learn automatically, but you can also explicitly trigger your agent to reflect and learn with the `/remember` command.

### **Skill Learning**

Many tasks that we work on with coding agents are repeated or follow similar patterns - for example API patterns or running DB migrations. Once you’ve worked with an agent to coach it through a complex task, you can trigger it to learn a skill from its experience, so the agent itself or other agents can reference the skill for similar tasks in the future. [Skill learning](https://www.letta.com/blog/skill-learning) can dramatically improve performance on future similar tasks, as we showed with recent results on TerminalBench.

On our team, some skills that agents have contributed (with the help of human engineers) are:

* Generating DB migrations on schema changes
* Creating PostHog dashboards with the PostHog CLI
* Best practices for API changes

Since skills are simply `.md` files, they can be managed in git repositories for versioning - or even used by other coding agents that support skills.

### **Persisted State**

Agents can also lookup past conversations (or even conversations of other agents) through the [Letta API](https://docs.letta.com/api). The builtin `/search` command allows you to easily search through messages, so you can find the agent you worked on something with. The Letta API supports vector, full-text, and hybrid search over [messages](https://docs.letta.com/api/resources/messages/methods/search) and [available tools](https://docs.letta.com/api/resources/tools/methods/search).

## Letta Code is the #1 model-agnostic OSS coding harness

Letta Code adds statefulness and learning to coding agents, but is the #1 model-agnostic, OSS harness on [Terminal-Bench](https://www.tbench.ai/). Letta Code’s performance is comparable to provider-specific harnesses (Gemini CLI, Claude Code, Codex) across model providers, and significantly outperforms the previous leading model-agnostic harness, Terminus 2.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/69419fd8e48a3dd80df1c6e5_letta-code-chart-4x.png)

This means that even without memory, you can expect Letta Code agents to work just as well with a frontier model as they would with a specific harness built by the model provider.

## Getting Started with Letta Code

To try out Letta Code, you can install it with `npm install -g @letta-ai/letta-code` or [install from source](https://github.com/letta-ai/letta-code) (see the full [documentation](https://docs.letta.com/letta-code)).

Letta Code can be used with the [Letta Developer Platform](https://app.letta.com/), or with a self-hosted Letta server.

[Back](/trash/old-blog)

Twitter/XLinkedIn

[## Company

Company announcements, partnerships](/blog-categories/company)

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/686b5e988ad87717a55fca3f_letta_graphs_2x.webp)

Jul 7, 2025

Agent Memory: How to Build Agents that Learn and Remember

Traditional LLMs operate in a stateless paradigm—each interaction exists in isolation, with no knowledge carried forward from previous conversations. Agent memory solves this problem.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/6866c72419989f4527ad72d4_letta_CPU.OS%402x.png)

Jul 3, 2025

Anatomy of a Context Window: A Guide to Context Engineering

As AI agents become more sophisticated, understanding how to design and manage their context windows (via context engineering) has become crucial for developers.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/6824ff9e8b9b8cfa0b38d37d_memory-h-l4.png)

May 14, 2025

Memory Blocks: The Key to Agentic Context Management

Memory blocks offer an elegant abstraction for context window management. By structuring the context into discrete, functional units, we can give LLM agents more consistent, usable memory.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/67aafd6c88a6c68cca350b59_letta_books%402x.webp)

Feb 13, 2025

RAG is not Agent Memory

Although RAG provides a way to connect LLMs and agents to more data than what can fit into context, traditional RAG is insufficient for building agent memory.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/67a44735ae8c2311297490c9_letta_memory_2x.webp)

Feb 6, 2025

Stateful Agents: The Missing Link in LLM Intelligence

Introducing “stateful agents”: AI systems that maintain persistent memory and actually learn during deployment, not just during training.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/6735971c9bed4dbe4fd98d55_BlogPost_01.webp)

Nov 14, 2024

The AI agents stack

Understanding the AI agents stack landscape.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/6732732bb6386c6228ad1ae9_letta_dlai_grey.webp)

Nov 7, 2024

New course on Letta with DeepLearning.AI

DeepLearning.AI has released a new course on agent memory in collaboration with Letta.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/67314733cc0d8719cd76879f_letta_intro.webp)

Sep 23, 2024

Announcing Letta

We are excited to publicly announce Letta.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/67314a470f07af58bc65b179_letta_memgpt.webp)

Sep 23, 2024

MemGPT is now part of Letta

The MemGPT open source project is now part of Letta.

[## Product

Release notes, feature announcements](/blog-categories/product)

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/692dff0f8c7ecba7ca2db3fe_scale%20growing%402x.png)

Dec 1, 2025

Programmatic Tool Calling with any LLM

The Letta API now supports programmatic tool calling for any LLM model, enabling agents to generate their own workflows.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/68f9cce19e13b738da57f275_evals2x.png)

Oct 23, 2025

Letta Evals: Evaluating Agents that Learn

Introducing Letta Evals: an open-source evaluation framework for systematically testing stateful agents.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/68edb440587079f73297d6f0_agentic-loop.webp)

Oct 14, 2025

Rearchitecting Letta’s Agent Loop: Lessons from ReAct, MemGPT, & Claude Code

Introducing Letta's new agent architecture, optimized for frontier reasoning models.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/68dc3e4f0f4b902c1443470f_tool_usage.webp)

Sep 30, 2025

Introducing Claude Sonnet 4.5 and the memory omni-tool in Letta

Letta agents can now take full advantage of Sonnet 4.5’s advanced memory tool capabilities to dynamically manage their own memory blocks.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/6881a27d66b2ac405309303b_image.png)

Jul 24, 2025

Introducing Letta Filesystem

Today we're announcing Letta Filesystem, which provides an interface for agents to organize and reference content from documents like PDFs, transcripts, documentation, and more.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/68012229d2774402e0e526ae_letta_fern_2x.webp)

Apr 17, 2025

Announcing Letta Client SDKs for Python and TypeScript

We've releasing new client SDKs (support for TypeScript and Python) and upgraded developer documentation

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/67ed897786b9e1a53bd585e1_agentfile_blog_small.webp)

Apr 2, 2025

Agent File

Introducing Agent File (.af): An open file format for serializing stateful agents with persistent memory and behavior.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/678685ffc445fbad06362394_ade_thumbnail.webp)

Jan 15, 2025

Introducing the Agent Development Environment

Introducing the Letta Agent Development Environment (ADE): Agents as Context + Tools

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/675ca410bbe509b8e72e2140_letta_064_release.webp)

Dec 13, 2024

Letta v0.6.4 release

Letta v0.6.4 adds Python 3.13 support and an official TypeScript SDK.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/6731407133e684213a75bcfe_letta_v052.webp)

Nov 6, 2024

Letta v0.5.2 release

Letta v0.5.2 adds tool rules, which allows you to constrain the behavior of your Letta agents similar to graphs.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/67313f239f7f093166906290_letta_v051.webp)

Oct 23, 2024

Letta v0.5.1 release

Letta v0.5.1 adds support for auto-loading entire external tool libraries into your Letta server.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/67313cb6ab1b61326e745837_letta_v05.webp)

Oct 14, 2024

Letta v0.5 release

Letta v0.5 adds dynamic model (LLM) listings across multiple providers.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/67313680e8dcc90d962a4439_letta_v041.webp)

Oct 3, 2024

Letta v0.4.1 release

Letta v0.4.1 adds support for Composio, LangChain, and CrewAI tools.

[## Research

Sleep-time compute, anatomy of a context window](/blog-categories/research)

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/6939e1df2872ece8e8298a83_sound_waves%402x.png)

Dec 11, 2025

Continual Learning in Token Space

At Letta, we believe that learning in token space is the key to building AI agents that truly improve over time. Our interest in this problem is driven by a simple observation: agents that can carry their memories across model generations will outlast any single foundation model.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/692f4687c4226272753e94b9_speed%402x.png)

Dec 2, 2025

Skill Learning: Bringing Continual Learning to CLI Agents

Today we’re releasing Skill Learning, a way to dynamically learn skills through experience. With Skill Learning, agents can use their past experience to actually improve, rather than degrade, over time.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/690e3f466f9e5a266a7823d7_tracing%20and%20understanding%402x.png)

Nov 7, 2025

Can Any Model Use Skills? Adding Skills to Context-Bench

Today we're releasing Skill Use, a new evaluation suite inside of Context-Bench that measures how well models discover and load relevant skills from a library to complete tasks.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/6903098c91ecf5051a3ba64a_tokens%20words%402x.png)

Oct 30, 2025

Context-Bench: Benchmarking LLMs on Agentic Context Engineering

We are open-sourcing Context-Bench, which evaluates how well language models can chain file operations, trace entity relationships, and manage multi-step information retrieval in long-horizon tasks.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/68edd9502e6ba397447c9ee3_recovery.webp)

Aug 27, 2025

Introducing Recovery-Bench: Evaluating LLMs' Ability to Recover from Mistakes

We're excited to announce Recovery-Bench, a benchmark and evaluation method for measuring how well agents can recover from errors and corrupted states.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/689ad5b144245a7bee0bb80a_evaluation%402x.png)

Aug 12, 2025

Benchmarking AI Agent Memory: Is a Filesystem All You Need?

Letta Filesystem scores 74.0% of the LoCoMo benchmark by simply storing conversational histories in a file, beating out specialized memory tool libraries.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/68924a831b743fff4eaadc89_terminalbench-thumb.png)

Aug 5, 2025

Building the #1 open source terminal-use agent using Letta

We built the #1 open-source agent for terminal use, achieving 42.5% overall score on Terminal-Bench ranking 4th overall and 2nd among agents using Claude 4 Sonnet.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/6838bc001f142182a0d48fbb_letta-leaderboard-thumb.webp)

May 29, 2025

Letta Leaderboard: Benchmarking LLMs on Agentic Memory

We're excited to announce the Letta Leaderboard, a comprehensive benchmark suite that evaluates how effectively LLMs manage agentic memory.

![](https://cdn.prod.website-files.com/66bb3d1f468f0f3848a20a84/68065943b27ea6238e9d427e_sleeptime_header_wide.webp)

Apr 21, 2025

Sleep-time Compute

Sleep-time compute is a new way to scale AI capabilities: letting models "think" during downtime. Instead of sitting idle between tasks, AI agents can now use their "sleep" time to process information and form new connections by rewriting their memory state.

in this article

This is some text inside of a div block.

### Product

[What is Letta](/#product)

[Customers](/case-studies)[Research](/research)[News](/blog)

### DEVELOPERS

[GitHub

11.9K](https://github.com/letta-ai/letta)[Documentation](http://docs.letta.com/)[Community](https://discord.com/invite/letta)[Demos](/demos)

### Company

[About us](/about-us)[Open positions](/about-us#joinus)[Privacy policy](/privacy-policy)[Terms of service](/terms-of-service)

### Newsletter

Thank you, you are subscribed

Oops! Something went wrong while submitting the form.

Follow Letta

Follow Letta

Follow Letta

Follow Letta

Follow Letta

Follow Letta

[GitHub](https://github.com/letta-ai/letta)[Discord](https://discord.gg/letta)[Twitter/X](https://x.com/letta_ai)[Bluesky](https://bsky.app/profile/letta.com)Instagram[YouTube](https://www.youtube.com/%40letta-ai)[LinkedIn](https://www.linkedin.com/company/letta-ai)

---
source: hackernews
title: Get an AI code review in 10 seconds
url: https://oldmanrahul.com/2025/12/19/ai-code-review-trick/
date: 2025-12-22
---

[![O](/images/oldmanrahul_menu_icon_39.jpg)ldmanrahul](/ "You are loved. Keep moving forward.")

* [X](https://x.com/oldmanrahul "X")
* [Subscribe](/subscribe/ "Subscribe")

# Get an AI code review in 10 seconds

December 19, 2025

Here’s a trick I don’t see enough people using:

> Add `.diff` to the end of any PR URL and copy&paste into a LLM

You can get an instant feedback on any GitHub PR.

**No Copilot Enterprise. No browser extensions. No special tooling.**

That’s it.

## Example

1. PR Link: `https://github.com/RahulPrabha/oldmanrahul.com/pull/11`
2. Add `.diff` to the end: `https://github.com/RahulPrabha/oldmanrahul.com/pull/11.diff`
3. Copy the raw diff
4. Paste it into Claude, ChatGPT, or any LLM (Maybe add a short instuction like: `please review.`)

## So no more human reviewers?

This isn’t a replacement for a real code review by a peer. But it’s a great way to get a first pass in short order.

Before you ping a teammate, run your PR through an LLM. You’ll catch obvious issues, get suggestions for edge cases you missed, and show up to the real review with cleaner code.

It’ll shorten your cycle times and be [a courtesy to others.](https://simonwillison.net/2025/Dec/18/code-proven-to-work/)

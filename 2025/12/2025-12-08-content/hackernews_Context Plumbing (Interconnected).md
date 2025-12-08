---
source: hackernews
title: Context Plumbing (Interconnected)
url: https://interconnected.org/home/2025/11/28/plumbing
date: 2025-12-08
---

# [Interconnected](/home/)

##### A blog by Matt Webb

* [About](/)
* [Archive](#archive)
* [Work](https://www.actsnotfacts.com)

##### Subscribe for $0

* [Email](https://buttondown.com/genmon)
* [RSS feed](https://interconnected.org/home/feed)
* [(What is a feed?)](https://aboutfeeds.com)

##### Unoffice Hours

* [Book a call](https://calendly.com/mwie/30min)
* [(What is this?)](/home/2020/09/24/unoffice_hours)

##### a.k.a. genmon

* [Bluesky](https://bsky.app/profile/genmon.org)
* [X/Twitter](https://x.com/genmon)
* [Insta](https://www.instagram.com/genmon/)
* [Mastodon](https://mastodon.social/%40genmon)
* [LinkedIn](https://www.linkedin.com/in/genmon/)

##### Building the AI clock

* [Check out Poem/1](https://poem.town)

## Context plumbing

##### 18.05, Saturday 29 Nov 2025 [Link to this post](/home/2025/11/28/plumbing)

These past few weeks I’ve been deep in code and doing what I think about as **context plumbing.**

I’ve been building an AI system and that’s what it feels like.

Let me unpack.

---

**Intent**

Loosely AI interfaces are about intent and context.

Intent is the user’s goal, big or small, explicit or implicit.

Uniquely for computers, AI can understand intent and respond in a really human way. This is a new capability! Like the user can type "I want to buy a camera" or point at a keylight and subvocalise "I’ve got a call in 20 minutes" or hit a button labeled "remove clouds" and *job done.*

Companies care about this because computers that are closer to intent tend to win.

e.g. the smartphone displaced the desktop. On a phone, you see something and then you touch it directly. With a desktop that intent is mediated through a pointer – you see something on-screen but to interact you tell your arm to move the mouse that moves the pointer. Although it doesn’t seem like much your monkey brain doesn’t like it.

So the same applies to user interfaces in general: picking commands from menus or navigating and collating web pages to plan a holiday or remembering how the control panel on your HVAC works. All of that is bureaucracy. Figuring out the sequence for yourself is *administrative burden* between intent and result.

Now as an AI company, you can overcome that burden. And you want to be present at the very millisecond and in the very location where the user’s intent - desire - arises. You don’t want the user to have the burden of even taking a phone out of their pocket, or having to formulate an unconscious intent into words. Being closest to the origin of intent will crowd out their competitor companies.

That explains the push for devices like AI-enabled glasses or lanyards or mics or cameras that read your body language.

This is why I think [the future of interfaces is Do What I Mean](/home/2025/08/29/dwim): it’s not just a new capability enabled by AI, there’s a whole attentional economics imperative to it.

---

**Context**

What makes an AI able to handle intent really, really well is **context.**

Sure there’s the world knowledge in the large language model itself, which it gets from vast amounts of training data.

But let’s say an AI agent is taking some user intent and hill-climbing towards that goal using a sequence of tool calls (which is how agents work) then it’s going to do way better when the prompt is filled with all kinds of useful context:

For example:

* Background knowledge from sources like Wikipedia or Google about what others have done in this situation.
* Documentation about the tools the agent will use to satisfy the intent.
* The user’s context such as what they’ve done before, the time of day, etc.
* Tacit knowledge and [common ground](/home/2022/05/09/npcs) shared between the user and the AI, i.e. what we’re all assuming we’re here to do.
* The shared “whiteboard”: the document we’re working on.
* For the agent itself, session context: whether this task is a subtask of a larger goal, what’s worked before and what hasn’t, and so on.

This has given rise to the idea of [context engineering](https://blog.langchain.com/the-rise-of-context-engineering/) (LangChain blog):

> Context engineering is building dynamic systems to provide the right information and tools in the right format such that the LLM can plausibly accomplish the task.

btw access to context also explains some behaviour of the big AI companies:

If you want to best answer user intent, then you need to be where the user context is, and that’s why being on a lanyard with an always-on camera is preferred over a regular on-demand camera, and why an AI agent that lives in your email archive is going to be more effective than one that doesn’t. So they really wanna get in there, really cosy up.

(And what’s context at inference time is valuable training data if it’s recorded, so there’s that too.)

---

**Plumbing?**

What’s missing in the idea of context engineering is that context is dynamic. It changes, it is timely.

Context appears at disparate sources, by user activity or changes in the user’s environment: what they’re working on changes, emails appear, documents are edited, it’s no longer sunny outside, the available tools have been updated.

This context is not always where the AI runs (and the AI runs as close as possible to the point of user intent).

So the job of making an agent run really well is to move the context to where it needs to be.

Essentially copying data out of one database and putting it into another one – but as a continuous process.

You often don’t want your AI agent to have to look up context every single time it answers intent. That’s slow. If you want an agent to act quickly then you have to plan ahead: build pipes that flow potential context from where it is created to where it’s going to be used.

How can that happen continuously behind the scenes without wasting bandwidth or cycles or the data going stale?

So I’ve been thinking of AI system technical architecture as plumbing the sources and sinks of context.

---

In the old days of Web 2.0 the go-to technical architecture was a “CRUD” app: a web app wrapping a database where you would have entities and operations to create, read, update, and delete (these are also the HTTP verbs).

This was also the user experience, so the user entity would have a webpage (a profile) and the object entity, say a photo, would have a webpage, and then dynamic webpages would index the entities in different ways (a stream or a feed). And you could decompose webapps like this; the technology and the user understanding aligned.

With AI systems, you want the user to have an intuition about what context is available to it. The plumbing of context flow isn’t just what is technically possible or efficient, but what matches user expectation.

---

Anyway.

I am aware this is getting - for you, dear reader - impossibly abstract.

But for me, I’m building the platform I’ve been trying to build for the last 2 years only this time it’s working.

I’m building on Cloudflare and I have context flowing between all kinds of entities and AI agents and sub-agents running where they need to run, and none of it feels tangled or confusing because it is plumbed just right.

And I wanted to make a note about that even if I can’t talk specifically, yet, about what it is.

---

##### Follow-up posts:

* [My mental model of the AI race](/home/2025/12/05/training)
  (5 Dec 2025)

##### Auto-calculated kinda related posts:

* [Observations on Siri, Apple Intelligence, and hiding in plain sight](/home/2024/06/11/siri)
  (11 Jun 2024)
* [Mapping the landscape of gen-AI product user experience](/home/2024/07/19/ai-landscape)
  (19 Jul 2024)
* [Let me recruit AI teammates into Figma](/home/2022/10/26/teammates)
  (26 Oct 2022)
* [Extending AI chat with Model Context Protocol (and why it matters)](/home/2025/02/11/mcp)
  (11 Feb 2025)
* [Multiplayer AI chat and conversational turn-taking: sharing what we learnt](/home/2025/05/23/turntaking)
  (23 May 2025)

If you enjoyed this post, please consider sharing it by email or on social media. [Here’s the link.](/home/2025/11/28/plumbing) Thanks, —Matt.

## Most recent posts

* [My mental model of the AI race](/home/2025/12/05/training)
  5 Dec 2025
* Context plumbing
  29 Nov 2025
  (This post)
* [Spinning up a new thing: Inanimate](/home/2025/11/19/inanimate)
  19 Nov 2025
* [3 books with Samuel Arbesman](/home/2025/11/14/arbesman)
  14 Nov 2025
* [Oedipus is about the act of figuring out what Oedipus is about](/home/2025/11/07/oedipus)
  7 Nov 2025
* [Filtered for wobbly tables and other facts](/home/2025/10/30/filtered)
  30 Oct 2025
* [Some wholesome media](/home/2025/10/23/wholesome)
  24 Oct 2025
* [I love the smell of autopoiesis in the morning](/home/2025/10/15/3dp)
  15 Oct 2025
* [Cyborgs vs rooms, two visions for the future of computing](/home/2025/10/13/dichotomy)
  13 Oct 2025
* [Parables involving gross bodily functions](/home/2025/10/10/parables)
  10 Oct 2025
* [The Campaign for Vertical Television](/home/2025/10/01/vertical)
  1 Oct 2025
* [I like AI slop and I cannot lie](/home/2025/09/26/slop)
  26 Sep 2025

Continue reading:
[All in 2025](/home/2025)

streak

New posts for 297 consecutive weeks

(see: [blogging tips](/home/2020/09/10/streak))

New? Start here:
[Best of 2023](/home/2023/12/22/top-posts)
(also
[2022](/home/2022/12/21/top_posts),
[2021](/home/2021/12/23/top_posts),
[2020](/home/2020/12/17/top_posts))

Or explore the archives:
[On this day](/home/on-this-day)

---

## Archive

* [2025](/home/2025)
  58 posts
* [2024](/home/2024)
  60 posts
* [2023](/home/2023)
  68 posts
* [2022](/home/2022)
  96 posts
* [2021](/home/2021)
  128 posts
* [2020](/home/2020)
  118 posts
* [2019](/home/2019)
  23 posts
* [2018](/home/2018)
  47 posts
* [2017](/home/2017)
  22 posts
* [2016](/home/2016)
  48 posts
* [2015](/home/2015)
  88 posts
* [2014](/home/2014)
  30 posts
* [2013](/home/2013)
  6 posts
* [2012](/home/2012)
  27 posts
* [2011](/home/2011)
  76 posts
* [2010](/home/2010)
  2 posts
* [2009](/home/2009)
  2 posts
* [2008](/home/2008)
  59 posts
* [2007](/home/2007)
  20 posts

---

Since February 2000. Copyright © 2025 Matt Webb.

p.s. here’s [my blogroll](/home/blogroll) and the [colophon](/home/2024/10/28/colophon).

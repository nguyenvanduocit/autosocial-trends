---
source: hackernews
title: AI should only run as fast as we can catch up
url: https://higashi.blog/2025/12/07/ai-verification/
date: 2025-12-09
---

[ ]

[Steven (Dongze) Yue's](https://www.linkedin.com/in/steven-dongze-yue/) personal blog.

[Home](https://higashi.blog/)
[About](https://higashi.blog/about/)
[中文博客](https://higashi.blog/index_cn/)
Currently v1.1.0

© 2025.

### [Higashi.blog](/ "Home") Random notes on cryptography and engineering

# AI should only run as fast as we can catch up

07 Dec 2025

## AI should only run as fast as we can catch up.

### The story of Daniel and Eric

Recently I have spoke with two of my friends who all had fun playing with AI.

Last month, I met with Eric, a fearless PM at a medium size startup who recently got into vibe coding with Gemini.

> After getting familiarized with Gemini, Eric was genuinely amazed by how AI quickly turns prompt into playable web applications. It served great purpose as a first prototype to communicate ideas to designers and engineers. But Eric really wanted to skip those steps and directly ship it to prod. But he couldn’t really understand that Gemini actually built a single-page HTML file that merely looks like a working app. Sadly, one cannot build a reliable enterprise product out of this. And there is really no effective way for Eric to catch up on these technical details and outpace the engineering team himself.

Last week, I had coffee with Daniel, a senior staff engineer who recently grew fond of AI coding and found it to be the true force multiplier.

> Daniel was skeptical of AI at first, but lately he hasn’t wrote a single line of code for months already. What he does is just precisely prompt the AI to create new components in an existing framework (involving Kafka, postgres, AuthN/Z, and k8s infra stuff) and adhering to certain preexisting paradigms. He would just spot-check the correctness of AI’s work and quickly spin up local deployments to verify it’s indeed working. Later, he pushes the changes through code review process and lands those features. All without writing a single line of code and it’s production ready just as if he wrote them himself. To Daniel, building and shipping things fast and scalable is simpler than ever.

### Interpolating between the two stories

After speaking with Eric and Daniel, I suddenly feel that there is an overarching theme around the use of AI that we can probably interpolate out of the stories here. And after pondering for a weekend, I think I can attempt to describe it now: it’s the problem of **reliable engineering - how can we make AI work reliably**.

With the AI superpower, one can task it to do all crazy things on the internet with just typing a few lines of prompt. AI always thinks and learns faster than us, this is undeniable now. However, to make AI work actually useful (not only works, but reliable and trustworthy), we also need to catch up with what the AI does as quickly as possible.

It’s almost like - we need to send the AI off to learn and think as fast as possible, but we also need to catch up as soon as possible to make it all relevant. And the speed we catch up things is critical to whether AI can help us effectively do these tasks. For the case of Daniel, he can spot-check and basically just skim through AI’s work and know for sure it’s doing the right thing with a few simple tests steps to verify, hence his results are more reliable. Whereas for Eric, he needs to basically learn software development from the bottom up to comprehend what the AI has done, and that really doesn’t give him the edge to outpace engineering teams to ship features reliably by himself.

### Where AI exploded: fast verification, slow learning and creation

To generalize the problem again, I think for all the tasks we do, we can break them down into two parts: learning/creation and verification. Basically doing the task and checking if the task is done right. Interestingly, this gives us a good perspective to our relationship with AI on performing such tasks.

Effort wise, if **verification « learning/creation**, one can very effectively check AI’s work and be confident about its reliability.

If **verification ~= learning/creation**, one spends equal amount of time checking AI’s work. It’s not a big win, maybe AI becomes a good automation script to cut down some boilerplate.

If **verification » learning/creation**, one cannot be sure about AI’s work that easily, and we are in the vibe-land.

A very good example of the first category is image (and video) generation. Drawing/rendering a realistic looking image is a crazily hard task. Have you tried to make a slide look nicer? It will take me literally hours to center the text boxes to make it look “good”. However, you really just need to take a look at the output of Nano Banana and you can tell if it’s a good render or a bad one based on how you feel. The verification is literally **instantaneous** and **effortless** because it’s all encoded as feeling or vibes in your brain. “Does this look right?” probably can be answered in the span of milliseconds by your vision cortex. There is also no special knowledge required - **human beings have been evaluating visual images since birth**, hardwired into our instincts.

The significant cost asymmetry can greatly explain why AI image generation exploded. If we can look for similar scenarios, we can probably identify other “killer” use cases of AI as well.

### Verification debt: scarier than tech debt

However, if we go down into the bottom of the spectrum where verification becomes more intense - requiring domain knowledge, technical expertise, industry know-hows to tell if the AI is producing slop or not, we will enter this dark age of piling verification debt. More things are being created, but we are lagging behind to check if any of it actually works to our satisfaction.

If an organization keeps vibe-coding without catching up with verification, those tasks can quickly end up as “debts” that needs to be verified. When verification becomes the bottleneck, dangerous things can happen if we still want to move fast - we will risk ourselves running unverified code and having unexpected side effects that are yet to be validated. It can also apply to other fields - imagine asking AI to craft a new vaccine and you don’t want to wait for FDA to use it.

I’ve come across a few blog posts that talks about Verification Debt already. I think it’s genuinely a good problem for technical leaders to have in their mind in this era.

### Verification Engineering is the next Context Engineering

AI can only reliably run as fast as we check their work. It’s almost like a complexity theory claim. But I believe it needs to be the case to ensure we can harvest the exponential warp speed of AI but also remain robust and competent, as these technologies ultimate serve human beings, and us human beings need technology to be reliable and accountable, as we humans are already flaky enough ;)

This brings out the topic of Verification Engineering. I believe this can be a big thing after Context Engineering (which is the big thing after Prompt Engineering). By cleverly rearranging tasks and using nice abstractions and frameworks, we can make verification of AI performed tasks easier and use AI to ship more solid products the world. No more slop.

I can think of a few ideas to kickoff verification engineering:

* How to craft more technicall precise prompts to guide AI to surgically do things, rather than vibing it.
* How to train more capable technical stakeholders who can effectively verify and approve what AI has done.
* How to find more tasks that are relatively easy to verify but rather hard to create.
* How to push our theoretical boundaries of what things we can succinctly verify (complexity theory strikes again).

### Where next

I believe whoever figures out ways to effectively verify more complex tasks using human brains, can gain the most benefit out of the AI boom. Maybe we need to discard traditional programming languages and start programming in abstract graph-like dataflow representations where one can easily tell if a thing is done right or wrong despite its language or implementation details.

Maybe our future is like the one depicted in Severance - we look at computer screens with wiggly numbers and whatever “feels right” is the right thing to do. We can harvest these effortless low latency “feelings” that nature gives us to make AI do more powerful work.

## Related posts

* ### [I built a Coffee Beans Tracker that does OCR inventory management, with GPT and no code 26 Aug 2024](/2024/08/26/gpt-01/)
* ### [A Gentle Introduction of NTT - Part III: The Kyber Trick 15 Dec 2023](/2023/12/15/ntt-03/)
* ### [Announcing my new blog 31 Aug 2023](/2023/08/31/new-blog/)

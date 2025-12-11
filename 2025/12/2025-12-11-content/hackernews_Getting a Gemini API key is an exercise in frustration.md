---
source: hackernews
title: Getting a Gemini API key is an exercise in frustration
url: https://ankursethi.com/blog/gemini-api-key-frustration/
date: 2025-12-11
---

Reader, writer, computer gremlin. Afflicted with cats.

      Menu       Close

* [Home](/)
* [About](/about/)
* Email
* [RSS Feed](/feeds/index.xml)

## Newsletter

 Enter your email

## Color scheme

 System Light Dark

# [Getting a Gemini API key is an exercise in frustration](/blog/gemini-api-key-frustration/)

Published 10 Dec 2025 at 2:35PM IST

Last week, I started working on a new side-project. It’s a standard React app partly made up of run-of-the-mill CRUD views—a perfect fit for LLM-assisted programming. I reasoned that if I could get an LLM to quickly write the boring code for me, I’d have more time to focus on the interesting problems I wanted to solve.

I’ve pretty much settled on Claude Code as my coding assistant of choice, but I’d been hearing great things about Google’s Gemini 3 Pro. Despite my aversion to Google products, I decided to try it out on my new codebase.

I already had [Gemini CLI](https://github.com/google-gemini/gemini-cli) installed, but that only gave me access to Gemini 2.5 with rate limits. I wanted to try out Gemini 3 Pro, and I wanted to avoid being rate limited. I had some spare cash to burn on this experiment, so I went looking for ways to pay for a Gemini Pro plan, if such a thing existed.

Thus began my grand adventure in trying to give Google my money.

## What is a Gemini, really?

The name “Gemini” is so overloaded that it barely means anything. Based on the context, Gemini could refer to:

* The chatbot available at [gemini.google.com](https://gemini.google.com).
* The mobile app that lets you use the same Gemini chatbot on your [iPhone](https://apps.apple.com/us/app/google-gemini/id6477489729) or [Android](https://play.google.com/store/apps/details?id=com.google.android.apps.bard&hl=en_US).
* The [voice assistant](https://gemini.google/us/assistant/) on Android phones.
* The AI features [built into Google Workspace](https://workspace.google.com/resources/ai/), Firebase, Colab, BigQuery, and other Google products.
* Gemini CLI, an agentic coding tool for your terminal that works the same way as Claude Code or OpenAI Codex.
* The [Gemini Code Assist](https://codeassist.google) suite of products, which includes extensions for various IDEs, a GitHub app, and Gemini CLI.
* The [underlying LLM](https://en.wikipedia.org/wiki/Gemini_%28language_model%29) powering all these products.
* Probably three more products by the time I finish writing this blog post.

To make things even more confusing, Google has at least three different products just for agentic coding: Gemini Code Assist (Gemini CLI is a part of this suite of products), [Jules](https://jules.google), and [Antigravity](https://antigravity.google).

And then there’s a bunch of other GenAI stuff that is powered by Gemini but doesn’t have the word Gemini in the name: [Vertex AI Platform](https://cloud.google.com/vertex-ai?hl=en), [Google AI Studio](https://aistudio.google.com/api-keys), [NotebookLM](https://notebooklm.google), and who knows what else.

I just wanted to plug my credit card information into a form and get access to a coding assistant. Instead, I was dunked into an alphabet soup of products that all seemed to do similar things and, crucially, didn’t have any giant “Buy Now!” buttons for me to click.

In contrast, both Anthropic and OpenAI have two primary ways you can access their products: via their consumer offerings at [claude.ai](https://claude.ai) and [chatgpt.com](https://chatgpt.com) respectively, or via API credits that you can buy through their respective [developer](https://platform.claude.com/) [consoles](https://platform.openai.com/). In each case, there is a form field where you can plug in your credit card details, and a big, friendly “Buy Now!” button to click.

After half an hour of searching the web, I did the obvious thing and asked the free version of Gemini (the chatbot, not one of those other Geminis) what to do:

> How do I pay for the pro version of Gemini so i can use it in the terminal for writing code? I specifically want to use the Gemini 3 Pro model.

It thought for a suspiciously long time and told me that Gemini 3 Pro required a developer API key to use. Since the new model is still in preview, it’s not yet available on any of the consumer plans. When I asked follow up questions about pricing, it told me that “Something went wrong”. Which translates to: we broke something, but we won’t tell you how to fix it.

So I asked Claude for help. Between the two LLMs, I was able to figure out how to create an API key for the Gemini I wanted.

## Creating an API key is easy

Google AI Studio is supposed to be the all-in-one dashboard for Google’s generative AI models. This is where you can experiment with model parameters, manage API keys, view logs, and manage billing for your projects.

I logged into Google AI Studio and [created a new API key](https://aistudio.google.com/api-keys). This part was pretty straightforward: I followed the on-screen instructions and had a fresh new key housed under a project in a few seconds. I then verified that my key was working with Gemini CLI.

It worked! Now all that was left to do was to purchase some API credits. Back in Google AI Studio, I saw a link titled “Set up billing” next to my key. It looked promising, so I clicked it.

That’s where the fun *really* began.

## Google doesn’t want my money

The “Set up billing” link kicked me out of Google AI Studio and into Google Cloud Console, and my heart sank. Every time I’ve logged into Google Cloud Console or AWS, I’ve wasted hours upon hours reading outdated documentation, gazing in despair at graphs that make no sense, going around in circles from dashboard to dashboard, and feeling a strong desire to attain freedom from this mortal coil.

Turns out I can’t just put $100 into my Gemini account. Instead, I must first create a Billing Account. After I’ve done that, I must associate it with a project. *Then* I’m allowed to add a payment method to the Billing Account. And *then*, if I’m lucky, my API key will turn into a paid API key with Gemini Pro privileges.

So I did the thing. The whole song and dance. Including the mandatory two-factor OTP verification that every Indian credit card requires. At the end of the process, I was greeted with a popup telling me I had to verify my payment method before I’d be allowed to use it.

Wait. Didn’t I *just* verify my payment method? When I entered the OTP from my bank?

Nope, turns out Google hungers for more data. Who’d have thunk it?

To verify my payment method *for reals*, I had to send Google a picture of my government-issued ID and the credit card I’d just associated with my Billing Account. I had to ensure all the numbers on my credit card were redacted by manually placing black bars on top of them in an image editor, leaving only my name and the last four digits of the credit card number visible.

This felt unnecessarily intrusive. But by this point, I was too deep in the process to quit. I was invested. I needed my Gemini 3 Pro, and I was willing to pay any price.

The upload form for the government ID rejected my upload twice before it finally accepted it. It was the same exact ID every single time, just in different file formats. It wanted a PNG file. Not a JPG file, nor a PDF file, but a PNG file. Did the upload form mention that in the instructions? Of course not.

After jumping through all these hoops, I received an email from Google telling me that my verification will be completed in a few days.

A *few days*? Nothing to do but wait, I suppose.

## 403 Forbidden

At this point, I closed all my open Cloud Console tabs and went back to work. But when I was fifteen minutes into writing some code by hand like a Neanderthal, I received a second email from Google telling me that my verification was complete.

So for the tenth time that day, I navigated to AI Studio. For the tenth time I clicked “Set up billing” on the page listing my API keys. For the tenth time I was told that my project wasn’t associated with a billing account. For the tenth time I associated the project with my new billing account. And finally, after doing all of this, the “Quota tier” column on the page listing my API keys said “Tier 1” instead of “Set up billing”.

Wait, Tier 1? Did that mean there were other tiers? What were tiers, anyway? Was I already on the best tier? Or maybe I was on the worst one? Not important. The important part was that I had my API key and I’d managed to convince Google to charge me for it.

I went back to the Gemini CLI, ran the `/settings` command, and turned on the “Enable experimental features” option. I ran the `/models` command, which told me that Gemini 3 Pro was now available.

Success? Not yet.

When I tried sending a message to the LLM, it failed with this 403 error:

```
{
  "error": {
    "message": "{\n  \"error\": {\n    \"code\": 403,\n    \"message\": \"The caller does not have permission\",\n    \"status\":\"PERMISSION_DENIED\"\n  }\n}\n",
    "code": 403,
    "status": "Forbidden"
  }
}
```

Is that JSON inside a string inside JSON? Yes. Yes it is.

To figure out if my key was even working, I tried calling the Gemini API from JavaScript, reproducing the basic example from [Google’s own documentation](https://ai.google.dev/gemini-api/docs#javascript).

No dice. I ran into the exact same error.

I then tried talking to Gemini 3 Pro using the [Playground](https://aistudio.google.com/prompts/new_chat) inside Google AI Studio. It showed me a toast message saying `Failed to generate content. Please try again.` The chat transcript said `An internal error has occurred.`

At this point I gave up and walked away from my computer. It was already 8pm. I’d been trying to get things to work since 5pm. I needed to eat dinner, play *Clair Obscur*, and go to bed. I had no more time to waste and no more fucks to give.

## Your account is in good standing at this time

Just as I was getting into bed, I received an email from Google with this subject line:

> Your Google Cloud and APIs billing account XXXXXX-XXXXXX-XXXXXX is in good standing at this time.

With the message inside saying:

> Based on the information you provided and further analysis by Google, we have reinstated your billing account XXXXXX-XXXXXX-XXXXXX. Your account is in good standing, and you should now have full access to your account and related Project(s) and Service(s).

I have no idea what any of this means, but Gemini 3 Pro started working correctly after I received this email. It worked in the Playground, directly by calling the API from JavaScript, and with Gemini CLI.

Problem solved, I guess. Until Google mysteriously decides that my account is no longer in good standing.

## This was a waste of time

This was such a frustrating experience that I still haven’t tried using Gemini with my new codebase, nearly a week after I made all those sacrifices to the Gods of Billing Account.

I understand why the process for getting a Gemini API key is so convoluted. It’s designed for large organizations, not an individual developers trying to get work done; it serves the bureaucracy, not the people doing the work; it’s designed for maximum compliance with government regulations, not for efficiency or productivity.

Google doesn’t want my money unless I’m an organization that employs ten thousand people.

In contrast to Google, Anthropic and OpenAI are much smaller and much more nimble. They’re able to make the process of setting up a developer account quick and easy for those of us who just want to get things done. Unlike Google, they haven’t yet become complacent. They need to compete for developer mindshare if they are to survive a decade into the future. Maybe they’ll add the same level of bureaucracy to their processes as they become larger, but for now they’re fairly easy to deal with.

I’m still going to try using Gemini 3 Pro with Gemini CLI as my coding assistant, but I’ll probably cap the experiment to a month. Unless Gemini 3 Pro is a massive improvement over its competitors, I’ll stick to using tools built by organizations that want me as a customer.

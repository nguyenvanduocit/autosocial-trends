---
source: hackernews
title: Please just try HTMX
url: http://pleasejusttryhtmx.com/
date: 2025-12-19
---

# Please Just Fucking Try HTMX

*A measured-yet-opinionated plea from someone who's tired of watching you suffer*

Look. I'm not going to call you a [fucking moron](https://motherfuckingwebsite.com) every other sentence. That's been done. It's a whole genre now. And honestly? HTMX doesn't need me to scream at you to make its point.

The sweary web manifesto thing is fun—I've enjoyed reading them—but let's be real: yelling "[JUST USE HTML](https://justfuckingusehtml.com)" or "[JUST FUCKING USE REACT](https://justfuckingusereact.com)" hasn't actually changed anyone's stack. People nod, chuckle, and then go right back to fighting their raw JS or their webpack config.[1](#fn1)

So I'm going to try something different. I'll still swear (I'm not a fucking saint), but I'm also going to *show you something*, in the course of imploring you, for your own sanity and happiness, to at least please just *try* htmx.

## The False Choice

Right now, the shouters are offering you two options:

**Option A: "Just use HTML!"** And they're not wrong. HTML is shockingly capable. Forms work. Links work. The `<dialog>` element exists now. The web was built on this stuff and it's been chugging along since Tim Berners-Lee had hair. And a little *tasteful* CSS can go [a long motherfucking way](http://bettermotherfuckingwebsite.com).

But sometimes—and here's where it gets uncomfortable—you actually *do* need a button that updates part of a page without reloading the whole damn thing. You *do* need a search box that shows results as you type. You *do* need interactivity.

So you turn to:

**Option B: React (or Vue, or Svelte, or Angular if you're being punished for something).**

And suddenly you've got:

* A `package.json` with 847 dependencies
* A build step that takes 45 seconds (if the CI gods are merciful)
* State management debates polluting your pull requests
* Junior devs losing their minds over why `useEffect` runs twice
* A bundle size that would make a 56k modem weep

For what? A to-do list? A contact form? A dashboard that displays some numbers from a database?

This is the false choice: raw HTML's limitations *or* JavaScript framework purgatory.

There's a third option. I'm begging you, please just try it.

## HTMX: The Middle Path

What if I told you:

* **Any HTML element** can make an HTTP request
* The server just returns **HTML** (not JSON, actual HTML)
* That HTML gets **swapped into the page** wherever you want
* You write **zero JavaScript**
* The whole library is **~14kb gzipped**

That's HTMX. That's literally the whole thing.

Here's a button that makes a POST request and replaces itself with the response:

```
<button hx-post="/clicked" hx-swap="outerHTML">
    Click me
</button>
```

When you click it, HTMX POSTs to `/clicked`, and whatever HTML the server returns replaces the button. No `fetch()`. No `setState()`. No `npm install`. No fucking webpack config.

The server just returns HTML. Like it's 2004, except your users have fast internet and your server can actually handle it. It's the [hypermedia architecture](https://hypermedia.systems) the entire freaking web was designed for, but with modern conveniences.

## Don't Believe Me? Click Things.

This page uses HTMX. These demos actually work.

### Demo 1: Click a Button

This button makes a POST request and swaps in the response:

Click me (hx-post)

### Demo 2: Load More Content

This button fetches additional content and appends it below:

Here's some initial content.

Load more (hx-get)

### Demo 3: Live Search

Type something—results update as you type (debounced, of course):

*Results will appear here...*

**That's HTMX.** I didn't write JavaScript to make those work. I wrote HTML attributes. The "server" (mocked client-side for this demo, but the htmx code is real) returns HTML fragments, and HTMX swaps them in. The behavior is right there in the markup—you don't have to hunt through component files and state management code to understand what a button does. HTMX folks call this **"Locality of Behavior"** and once you have it, you'll miss it everywhere else.

## The Numbers

Anecdotes are nice. Data is better.

A company called **[Contexte](https://htmx.org/essays/a-real-world-react-to-htmx-port/)** rebuilt their production SaaS app from React to Django templates with HTMX. Here's what happened:

67%

less code

(21,500 → 7,200 lines)

96%

fewer JS dependencies

(255 → 9 packages)

88%

faster builds

(40s → 5s)

50-60%

faster page loads

(2-6s → 1-2s)

They deleted two-thirds of their codebase and the app got *better*. Every developer became "full-stack" because there wasn't a separate frontend to specialize in anymore.

Now, they note this was a content-focused app and not every project will see these exact numbers. Fair. But even if you got *half* these improvements, wouldn't that be worth a weekend of experimentation?

## For the Skeptics

"But what about complex client-side state management?"

You probably don't have complex client-side state. You have forms. You have lists. You have things that show up when you click other things. HTMX handles all of that.

If you're building Google Docs, sure, you need complex state management. But you're not building Google Docs. You're building a CRUD app that's convinced it's Google Docs.

"But the React ecosystem!"

The ecosystem is why your `node_modules` folder is 2GB. The ecosystem is why there are 14 ways to style a component and they all have tradeoffs. The ecosystem is why "which state management library" is somehow still a debate.

HTMX's ecosystem is: your server-side language of choice. That's it. That's the ecosystem.

"But SPAs feel faster!"

After the user downloads 2MB of JavaScript, waits for it to parse, waits for it to execute, waits for it to hydrate, waits for it to fetch data, waits for it to render... yes, then subsequent navigations feel snappy. Congratulations.

HTMX pages load fast the *first* time because you're not bootstrapping an application runtime. And subsequent requests are fast because you're only swapping the parts that changed.

"But I need [specific React feature]!"

Maybe you do. I'm not saying React is never the answer. I'm saying it's the answer to about 10% of the problems it's used for, and the costs of reaching for it reflexively are staggering.

Most teams don't fail because they picked the wrong framework. They fail because they picked *too much* framework. HTMX is a bet on simplicity, and simplicity tends to win over time.

## When NOT to Use HTMX

I'm not a zealot. HTMX isn't for everything.

* **Real-time collaborative editing** (Google Docs, Figma)
* **Heavy client-side computation** (video editors, CAD tools)
* **Offline-first applications** (though you can combine approaches)
* **Genuinely complex UI state** (not "my form has validation" complex—actually complex)

But be honest with yourself: is that what you're building?

Or are you building another dashboard, another admin panel, another e-commerce site, another blog, another SaaS app that's fundamentally just forms and tables and lists? Be honest. I won't tell anyone. We all have to pay the bills.

For that stuff, HTMX is embarrassingly good. Like, "why did we make it so complicated" good. Like, "oh god, we wasted so much time" good.

## So Just Try It

You've tried React. You've tried Vue. You've tried Angular and regretted it. You've tried whatever meta-framework is trending on Hacker News this week.

**Just try HTMX.** One weekend. Pick a side project. Pick that internal tool nobody cares about. Pick the thing you've been meaning to rebuild anyway.

Add one `<script>` tag. Write one `hx-get` attribute. Watch what happens.

If you hate it, you've lost a weekend. But you won't hate it. You'll wonder why you ever thought web development had to be so fucking complicated.

**Learn more:**
[htmx.org](https://htmx.org) — The official site and docs
[hypermedia.systems](https://hypermedia.systems) — The free book on hypermedia-driven apps

---

1 Honor obliges me to admit this is not literally true. [bettermotherfuckingwebsite.com](http://bettermotherfuckingwebsite.com) is a fucking pedagogical masterpiece and reshaped how I built my own site. But let's not spoil the bit... [↩](#fnref1)

This page is a single HTML file. It uses HTMX for the demos. There is no build step. There is no `package.json`. View source if you don't believe me—it's not minified, because why would it be?

Inspired by (and in joyful dialogue with) [motherfuckingwebsite.com](https://motherfuckingwebsite.com), [justfuckingusehtml.com](https://justfuckingusehtml.com), [bettermotherfuckingwebsite.com](http://bettermotherfuckingwebsite.com), and [justfuckingusereact.com](https://justfuckingusereact.com). Extremism in defense of developer experience is no vice! This site made by [me](https://x.com/alexisgallagher). Does this all sound a bit like shallow slop? Yup, please help [make it better](https://github.com/algal/pleasejusttryhtmx/pulls).

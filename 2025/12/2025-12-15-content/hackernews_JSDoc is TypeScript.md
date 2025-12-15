---
source: hackernews
title: JSDoc is TypeScript
url: https://culi.bearblog.dev/jsdoc-is-typescript/
date: 2025-12-15
---

[# culi.bear.blog](/)

[Home](/) [Blog](/blog/)

# JSDoc \*is\* TypeScript

*29 Nov, 2025*

In May of 2023 an [internal refactoring PR](https://github.com/sveltejs/svelte/pull/8569) from the Svelte repo made it to the front page of the Hacker News forums. The (superficially) controversial PR seemingly vindicated TypeScript skeptics/luddites (which at the time included figures like [Dan Abramov](https://overreacted.io/things-i-dont-know-as-of-2018/#:~:text=TypeScript%2E%20I%20understand%20the%20concept%20of%20types%20and%20can%20read%20annotations%20but%20I%E2%80%99ve%20never%20written%20it%2E%20The%20few%20times%20I%20tried%2C%20I%20ran%20into%20difficulties) of the React team). The [premier](https://survey.stackoverflow.co/2023/#section-admired-and-desired-web-frameworks-and-technologies) darling of web frameworks ostensibly rejecting the benefits of static typing was a big deal.[1](#fn-1) So Rich Harris felt compelled to [hop onto HN](https://news.ycombinator.com/item?id=35892250) and explain how this was "not a vindication of anti-Typescript positions".

Harris offered a considered take on why moving type declarations from .ts files to JSDoc comments in .js files was not an anti-TypeScript take. In fact, Harris asserted that Svelte's "commitment to TypeScript [was] stronger than ever."

This event heralded (and served as a citation for) a flood of "TypeScript VS JSDoc" blog posts and forum threads that offered the somewhat novel defense of JSDoc as "all the benefits of TypeScript without the build step" (albeit with a sometimes clunkier syntax).

But this is not the blog post lineage I hope to draw on with this post. Instead, I'd like to focus on a larger and more often misunderstood point. I take issue with the "vs" framing and offer a subtle but crucial substitution: JSDoc *is* TypeScript.

Some background:

## TypeScript is C#?

(No, but it explains much of TS's early development)

Back in the late aughts/early 10s, JavaScript was still mostly seen as an unserious language. The tooling for JavaScript development lacked autocomplete, rename symbol, type safety, etc. Microsoft developers were so allergic to it that they would write C# code and use a tool called [ScriptSharp](https://github.com/nikhilk/scriptsharp) to generate slightly-more-typesafe JavaScript code.

These are the origins of TypeScript.[2](#fn-2) It is, fundamentally, a build tool to make writing JS less crappy. In fact:

## TypeScript is IntelliSense!

Even if you don't write your code in .ts files, you're probably using TypeScript. That's because TypeScript is the IntelliSense engine. Even if you're not using VSCode, if your editor gives you code completion, parameter info, quick info, member lists, etc. while writing JS code, you are almost certainly running the TypeScript language service.

## TypeScript is JSDoc :)

It is also the TypeScript language service that is used to interpret JSDoc comments. That is why the TypeScript CHANGELOG often includes [notes about JSDoc features](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-5.html#the-jsdoc-import-tag).[3](#fn-3) It also the reason your JSDoc-related IntelliSense can be governed by a `tsconfig.json` file and you can run `tsc` on a project typed with JSDoc comments.

You are already using TypeScript.

## My Own Experience

I recently rewrote the front-end for an old project of mine completely typed with JSDoc comments and I wanted to share some of my takeaways:

1. Besides runtime features like enums, basically everything you can express in TypeScript you can express in JSDoc. Certain features like generics are much clunkier, forcing you to type the return in order to infer generic slots. But sometimes the clunkier syntax can actually encourage better TypeScript practices by pushing devs to rely more on type inference.
2. For packages typed with JSDoc, CTRL/CMD clicking on a function will take you to actual code rather than a type declarations file. I *much* prefer this experience as a dev.
3. TypeScript tooling is surprisingly reusable for JSDoc projects. This includes type generation libraries that take schemas (e.g. OpenApi or GraphQL) defined in your backend and generate corresponding types in your front-end. I found that most of these can be set up to generate types in JSDoc comments instead of TypeScript code.

Take it from a massive TypeScript nerd: JSDoc is not an anti-TypeScript position to take. It is the same powerful static analysis without the build step.

---

1. EDIT: This got [some discussion](https://news.ycombinator.com/item?id=46266102) on HN and I've learned that [webpack](https://github.com/webpack/webpack) has also [switched from .ts files to JSDoc](https://news.ycombinator.com/item?id=46267318)[↩](#fnref-1)
2. And the reason enums were, [regrettably](https://github.com/microsoft/TypeScript/issues/30590#issuecomment-476417128), added to the language[↩](#fnref-2)
3. A miserable sidenote: becoming a TypeScript expert means accepting that half of the TypeScript documentation is in the changelog. [E.g.](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-7.html#unique-symbol)[↩](#fnref-3)

[#potofhoney](/blog/?q=pot-of-honey)
[#jsdoc](/blog/?q=jsdoc)
[#typescript](/blog/?q=typescript)

Powered by [Bear ʕ•ᴥ•ʔ](https://bearblog.dev)

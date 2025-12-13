---
source: hackernews
title: OpenAI are quietly adopting skills, now available in ChatGPT and Codex CLI
url: https://simonwillison.net/2025/Dec/12/openai-skills/
date: 2025-12-13
---

# [Simon Willison’s Weblog](/)

[Subscribe](/about/#subscribe)

## OpenAI are quietly adopting skills, now available in ChatGPT and Codex CLI

12th December 2025

One of the things that most excited me about [Anthropic’s new Skills mechanism](https://simonwillison.net/2025/Oct/16/claude-skills/) back in October is how easy it looked for other platforms to implement. A skill is just a folder with a Markdown file and some optional extra resources and scripts, so any LLM tool with the ability to navigate and read from a filesystem should be capable of using them. It turns out OpenAI are doing exactly that, with skills support quietly showing up in both their Codex CLI tool and now also in ChatGPT itself.

#### Skills in ChatGPT

I learned about this [from Elias Judin](https://x.com/elias_judin/status/1999491647563006171) this morning. It turns out the Code Interpreter feature of ChatGPT now has a new `/home/oai/skills` folder which you can access simply by prompting:

> `Create a zip file of /home/oai/skills`

I [tried that myself](https://chatgpt.com/share/693c9645-caa4-8006-9302-0a9226ea7599) and got back [this zip file](https://static.simonwillison.net/static/cors-allow/2025/skills.zip). Here’s [a UI for exploring its content](https://tools.simonwillison.net/zip-wheel-explorer?url=https%3A%2F%2Fstatic.simonwillison.net%2Fstatic%2Fcors-allow%2F2025%2Fskills.zip) ([more about that tool](https://tools.simonwillison.net/colophon#zip-wheel-explorer.html)).

![Screenshot of file explorer. Files skills/docs/render_docsx.py and skills/docs/skill.md and skills/pdfs/ and skills/pdfs/skill.md - that last one is expanded and reads: # PDF reading, creation, and review guidance  ## Reading PDFs - Use pdftoppm -png $OUTDIR/$BASENAME.pdf $OUTDIR/$BASENAME to convert PDFs to PNGs. - Then open the PNGs and read the images. - pdfplumber is also installed and can be used to read PDFs. It can be used as a complementary tool to pdftoppm but not replacing it. - Only do python printing as a last resort because you will miss important details with text extraction (e.g. figures, tables, diagrams).  ## Primary tooling for creating PDFs - Generate PDFs programmatically with reportlab as the primary tool. In most cases, you should use reportlab to create PDFs. - If there are other packages you think are necessary for the task (eg. pypdf, pyMuPDF), you can use them but you may need topip install them first. - After each meaningful update—content additions, layout adjustments, or style changes—render the PDF to images to check layout fidelity:   - pdftoppm -png $INPUT_PDF $OUTPUT_PREFIX - Inspect every exported PNG before continuing work. If anything looks off, fix the source and re-run the render → inspect loop until the pages are clean.  ## Quality expectations - Maintain a polished, intentional visual design: consistent typography, spacing, margins, color palette, and clear section breaks across all pages. - Avoid major rendering issues—no clipped text, overlapping elements, black squares, broken tables, or unreadable glyphs. The rendered pages should look like a curated document, not raw template output. - Charts, tables, diagrams, and images must be sharp, well-aligned, and properly labeled in the PNGs. Legends and axes should be readable without excessive zoom. - Text must be readable at normal viewing size; avoid walls of filler text or dense, unstructured bullet lists. Use whitespace to separate ideas. - Never use the U+2011 non-breaking hyphen or other unicode dashes as they will not be](https://static.simonwillison.net/static/2025/skills-explore.jpg)

So far they cover spreadsheets, docx and PDFs. Interestingly their chosen approach for PDFs and documents is to convert them to rendered per-page PNGs and then pass those through their vision-enabled GPT models, presumably to maintain information from layout and graphics that would be lost if they just ran text extraction.

Elias [shared copies in a GitHub repo](https://github.com/eliasjudin/oai-skills). They look very similar to Anthropic’s implementation of the same kind of idea, currently published in their [anthropics/skills](https://github.com/anthropics/skills/tree/main/skills) repository.

I tried it out by prompting:

> Create a PDF with a summary of the rimu tree situation right now and what it means for kakapo breeding season

Sure enough, GPT-5.2 Thinking started with:

> Reading skill.md for PDF creation guidelines

Then:

> Searching rimu mast and Kākāpō 2025 breeding status

It took [just over eleven minutes](https://chatgpt.com/share/693ca54b-f770-8006-904b-9f31a585180a) to produce [this PDF](https://static.simonwillison.net/static/cors-allow/2025/rimu_kakapo_breeding_brief.pdf), which was long enough that I had Claude Code for web [build me a custom PDF viewing tool](https://github.com/simonw/tools/pull/155) while I waited.

[Here’s ChatGPT’s PDF in that tool](https://tools.simonwillison.net/view-pdf?url=https%3A%2F%2Fstatic.simonwillison.net%2Fstatic%2Fcors-allow%2F2025%2Frimu_kakapo_breeding_brief.pdf).

![Screenshot of my tool. There is a URL at the top, a Load PDF button and pagination controls. Then the PDF itself is shown, which reads: Rimu mast status and what it means for the kākāpō breeding season Summary as of 12 December 2025 (Pacific/Auckland context) Kākāpō breeding is tightly linked to rimu (Dacrydium cupressinum) mast events: when rimu trees set and ripen large amounts of fruit, female kākāpō are much more likely to nest, and more chicks can be successfully raised. Current monitoring indicates an unusually strong rimu fruiting signal heading into the 2025/26 season, which sets the stage for a potentially large breeding year in 2026.^1,2 Key numbers at a glance Kākāpō population (official DOC count) 237 birds alive Breeding trigger (rimu fruiting)>10% of rimu branch tips bearing fruit Forecast rimu fruiting for 2026 (DOC monitoring) Around 50–60% fruiting across breeding islands¹Breeding-age females (DOC 2025 planning figure)About 87 females (potentially nearly all could nest)](https://static.simonwillison.net/static/2025/rimu.jpg)

(I am **very excited** about [Kākāpō breeding season this year](https://www.auckland.ac.nz/en/news/2025/12/03/bumper-breeding-season-for-kakapo-on-the-cards.html).)

The reason it took so long is that it was fastidious about looking at and tweaking its own work. I appreciated that at one point it tried rendering the PDF and noticed that the macrons in kākāpō were not supported by the chosen font, so it switched to something else:

![ChatGPT screenshot. Analyzed image. There's an image of a page of PDF with obvious black blocks on some of the letters in the heading. It then says: Fixing font issues with macrons. The page is showing black squares for words like "kākāpō," probably because Helvetica can't handle macrons. I'll switch to a font that supports them, such as DejaVu Sans or Noto Sans. I'll register both regular and bold fonts, then apply them to the document. I'll update the footer to note the issue with Helvetica. Time to rebuild the PDF!](https://static.simonwillison.net/static/2025/skills-macrons.jpg)

#### Skills in Codex CLI

Meanwhile, two weeks ago OpenAI’s open source Codex CLI tool landed a PR titled [feat: experimental support for skills.md](https://github.com/openai/codex/pull/7412). The most recent docs for that are in [docs/skills.md](https://github.com/openai/codex/blob/main/docs/skills.md).

The documentation suggests that any folder in `~/.codex/skills` will be treated as a skill.

I [used Claude Opus 4.5’s skill authoring skill](https://claude.ai/share/0a9b369b-f868-4065-91d1-fd646c5db3f4) to create [this skill for creating Datasette plugins](https://github.com/datasette/skill), then installed it into my Codex CLI skills folder like this:

```
git clone https://github.com/datasette/skill \
  ~/.codex/skills/datasette-plugin
```

You have to run Codex with the `--enable skills` option. I ran this:

```
cd /tmp
mkdir datasette-cowsay
cd datasette-cowsay
codex --enable skills -m gpt-5.2
```

Then prompted:

> `list skills`

And Codex replied:

> `- datasette-plugins — Writing Datasette plugins using Python + pluggy (file: /Users/simon/.codex/skills/datasette-plugin/SKILL.md)`
> `- Discovery — How to find/identify available skills (no SKILL.md path provided in the list)`

Then I said:

> `Write a Datasette plugin in this folder adding a /-/cowsay?text=hello page that displays a pre with cowsay from PyPI saying that text`

It worked perfectly! Here’s [the plugin code it wrote](https://github.com/simonw/datasette-cowsay) and here’s [a copy of the full Codex CLI transcript](http://gistpreview.github.io/?96ee928370b18eabc2e0fad9aaa46d4b), generated with my [terminal-to-html tool](https://simonwillison.net/2025/Oct/23/claude-code-for-web-video/).

You can try that out yourself if you have `uvx` installed like this:

```
uvx --with https://github.com/simonw/datasette-cowsay/archive/refs/heads/main.zip \
  datasette
```

Then visit:

```
http://127.0.0.1:8001/-/cowsay?text=This+is+pretty+fun
```

![Screenshot of that URL in Firefox, an ASCII art cow says This is pretty fun.](https://static.simonwillison.net/static/2025/cowsay-datasette.jpg)

#### Skills are a keeper

When I first wrote about skills in October I said [Claude Skills are awesome, maybe a bigger deal than MCP](https://simonwillison.net/2025/Oct/16/claude-skills/). The fact that it’s just turned December and OpenAI have already leaned into them in a big way reinforces to me that I called that one correctly.

Skills are based on a *very* light specification, if you could even call it that, but I still think it would be good for these to be formally documented somewhere. This could be a good initiative for the new [Agentic AI Foundation](https://aaif.io/) ([previously](https://simonwillison.net/2025/Dec/9/agentic-ai-foundation/)) to take on.

Posted [12th December 2025](/2025/Dec/12/) at 11:29 pm · Follow me on [Mastodon](https://fedi.simonwillison.net/%40simon), [Bluesky](https://bsky.app/profile/simonwillison.net), [Twitter](https://twitter.com/simonw) or [subscribe to my newsletter](https://simonwillison.net/about/#subscribe)

## More recent articles

* [GPT-5.2](/2025/Dec/11/gpt-52/) - 11th December 2025
* [Useful patterns for building HTML tools](/2025/Dec/10/html-tools/) - 10th December 2025

This is **OpenAI are quietly adopting skills, now available in ChatGPT and Codex CLI** by Simon Willison, posted on [12th December 2025](/2025/Dec/12/).

[pdf
39](/tags/pdf/)
[ai
1730](/tags/ai/)
[kakapo
2](/tags/kakapo/)
[openai
373](/tags/openai/)
[generative-ai
1528](/tags/generative-ai/)
[chatgpt
188](/tags/chatgpt/)
[llms
1494](/tags/llms/)
[ai-assisted-programming
285](/tags/ai-assisted-programming/)
[anthropic
211](/tags/anthropic/)
[gpt-5
28](/tags/gpt-5/)
[codex-cli
17](/tags/codex-cli/)
[skills
6](/tags/skills/)

**Previous:** [GPT-5.2](/2025/Dec/11/gpt-52/)

### Monthly briefing

Sponsor me for **$10/month** and get a curated email digest of the month's most important LLM developments.

Pay me to send you less!

[Sponsor & subscribe](https://github.com/sponsors/simonw/)

* [Colophon](/about/#about-site)
* ©
* [2002](/2002/)
* [2003](/2003/)
* [2004](/2004/)
* [2005](/2005/)
* [2006](/2006/)
* [2007](/2007/)
* [2008](/2008/)
* [2009](/2009/)
* [2010](/2010/)
* [2011](/2011/)
* [2012](/2012/)
* [2013](/2013/)
* [2014](/2014/)
* [2015](/2015/)
* [2016](/2016/)
* [2017](/2017/)
* [2018](/2018/)
* [2019](/2019/)
* [2020](/2020/)
* [2021](/2021/)
* [2022](/2022/)
* [2023](/2023/)
* [2024](/2024/)
* [2025](/2025/)

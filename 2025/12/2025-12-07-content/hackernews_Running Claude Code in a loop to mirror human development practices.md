---
source: hackernews
title: Running Claude Code in a loop to mirror human development practices
url: https://anandchowdhary.com/blog/2025/running-claude-code-in-a-loop
date: 2025-12-07
---

Anand Chowdhary

</blog>[/2025](/blog/2025)

![](https://raw.githubusercontent.com/AnandChowdhary/blog-images/refs/heads/main/384x256/2.png)

🤖🔄💻

# Running Claude Code in a loop

November 15, 2025

1,076 words

[View on GitHub](https://github.com/AnandChowdhary/blog/blob/main/blog/2025/running-claude-code-in-a-loop.md?utm_source=anandchowdhary.com)

This all started because I was contractually obligated to write unit tests for a codebase with hundreds of thousands of lines of code and go from 0% to 80%+ coverage in the next few weeks - seems like something Claude should do. So I built [Continuous Claude](https://github.com/AnandChowdhary/continuous-claude), a CLI tool to run Claude Code in a loop that maintains a persistent context across multiple iterations.

Current AI coding tools tend to halt after completing a task once they think the job is done and they don’t really have an opportunity for self-criticism or further improvement. And this one-shot pattern then makes it difficult to tackle larger projects. So in contrast to running Claude Code “as is” (which provides help in isolated bursts), what you want is to run Claude code for a long period of time without exhausting the context window.

Turns out, it’s as simple as just running Claude Code in a continuous loop - but drawing inspiration from CI/CD practices and persistent agents - you can take it a step further by running it on a schedule or through triggers and connecting it to your GitHub pull requests workflow. And by persisting relevant context and results from one iteration to the next, this process ensures that knowledge gained in earlier steps is not lost, which is currently not possible in stateless AI queries and something you have to slap on top by setting up markdown files to store progress and context engineer accordingly.

## While + git + persistence

The first version of this idea was a simple while loop:

```
while true; do
  claude --dangerously-skip-permissions "Increase test coverage [...] write notes for the next developer in TASKS.md, [etc.]"
  sleep 1
done
```

to which my friend [Namanyay](https://nmn.gl) of Giga AI said “genius and hilarious”. I spent all of Saturday building the rest of the tooling. Now, the Bash script acts as the conductor, repeatedly invoking Claude Code with the appropriate prompts and handling the surrounding tooling. For each iteration, the script:

1. Creates a new branch and runs Claude Code to generate a commit
2. Pushes changes and creates a pull request using GitHub’s CLI
3. Monitors CI checks and reviews via `gh pr checks`
4. Merges on success or discards on failure
5. Pulls the updated main branch, cleans up, and repeats

When an iteration fails, it closes the PR and discards the work. This is wasteful, but with knowledge of test failures, the next attempt can try something different. Because it piggybacks on GitHub’s existing workflows, you get code review and preview environments without additional work - if your repo requires code owner approval or specific CI checks, it will respect those constraints.

## Context continuity

A shared markdown file serves as external memory where Claude records what it has done and what should be done next. Without specific prompting instructions, it would create verbose logs that harm more than help - the intent is to keep notes as a clean handoff package between runs. So the key instruction to the model is: “This is part of a continuous development loop… you don’t need to complete the entire goal in one iteration, just make meaningful progress on one thing, then leave clear notes for the next iteration… think of it as a relay race where you’re passing the baton.”

Here’s an actual production example: the previous iteration ended with “Note: tried adding tests to X but failed on edge case, need to handle null input in function Y” and the very next Claude invocation saw that and prioritized addressing it. A single small file reduces context drift, where it might forget earlier reasoning and go in circles.

What’s fascinating is how the markdown file enables self-improvement. A simple “increase coverage” from the user becomes “run coverage, find files with low coverage, do one at a time” as the system teaches itself through iteration and keeps track of its own progress.

## Continuous AI

My friends at GitHub Next have been exploring this idea in their project [Continuous AI](https://githubnext.com/projects/continuous-ai/) and I shared Continuous Claude with them.

One compelling idea from the team was running specialized agents simultaneously - one for development, another for tests, a third for refactoring. While this could divide and conquer complex tasks more efficiently, it possibly introduces coordination challenges. I’m trying a similar approach for adding tests in different parts of a monorepository at the same time.

The [agentics project](https://github.com/githubnext/agentics) combines an explicit research phase with pre-build steps to ensure the software is restored before agentic work begins. “The fault-tolerance of Agent in a Loop is really important. If things go wrong it just hits the resource limits and tries again. Or the user just throws the generated PR away if it’s not helpful. It’s so much better than having a frustrated user trying to guide an agent that’s gone down a wrong path,” said GitHub Next Principal Researcher [Don Syme](https://github.com/dsyme).

It reminded me of a concept in economics/mathematics called “radiation of probabilities” (I know, pretty far afield, but bear with me) and here, each agent run is like a random particle - not analyzed individually, but the general direction emerges from the distribution. Each run can even be thought of as idempotent: if GitHub Actions kills the process after six hours, you only lose some dirty files that the next agent will pick up anyway. All you care about is that it’s moving in the right direction in general, for example increasing test coverage, rather than what an individual agent does. This wasteful-but-effective approach becomes viable as token costs approach zero, similar to Cursor’s multiple agents.

## Dependabot on steroids

Tools like Dependabot handle dependency updates, but Continuous Claude can also fix post-update breaking changes using release notes. You could run a GitHub Actions workflow every morning that checks for updates and continuously fixes issues until all tests pass.

Large refactoring tasks become manageable: breaking a monolith into modules, modernizing callbacks to async/await, or updating to new style guidelines. It could perform a series of 20 pull requests over a weekend, each doing part of the refactor with full CI validation. There’s a whole class of tasks that are too mundane for humans but still require attention to avoid breaking the build.

The model mirrors human development practices. Claude Code handles the grunt work, but humans remain in the loop through familiar mechanisms like PR reviews. Download the CLI from GitHub to get started: [AnandChowdhary/continuous-claude](https://github.com/AnandChowdhary/continuous-claude)

[Accidentally founding Koj](/blog/2025/accidentally-founding-koj)

[2024 will be the Year of Gratitude](/blog/2024/year-of-gratitude)

Anand Chowdhary

[Made with  by Anand Chowdhary](https://madewithloveinindia.org/?utm_source=anandchowdhary.com)

Anand Chowdhary is a technology entrepreneur, engineer, and designer from New Delhi, India, based in Utrecht, the Netherlands and San Francisco, California.

[Home](/)[About](/about)[Archive](/archive)[Life](/life)[Blog](/blog)[Notes](/notes)[Events](/events)[Open source](/open-source)[Projects](/projects)[Mentoring](/mentoring)[Press](/press)[Videos](/videos)[Themes](/themes)[Versions](/versions)[Contact](/contact)[X (formerly Twitter)](https://x.com/AnandChowdhary?utm_source=anandchowdhary.com)

[2007](/archive/2007)[2008](/archive/2008)[2009](/archive/2009)[2010](/archive/2010)[2011](/archive/2011)[2012](/archive/2012)[2013](/archive/2013)[2014](/archive/2014)[2015](/archive/2015)[2016](/archive/2016)[2017](/archive/2017)[2018](/archive/2018)[2019](/archive/2019)[2020](/archive/2020)[2021](/archive/2021)[2022](/archive/2022)[2023](/archive/2023)[2024](/archive/2024)[2025](/archive/2025)

---
source: hackernews
title: Skills Officially Comes to Codex
url: https://developers.openai.com/codex/skills/
date: 2025-12-21
---

[![OpenAI Developers](/OpenAI_Developers.svg)](/)

[Resources](/resources)

[Codex](/codex)

[ChatGPT](/chatgpt)

[Apps SDK

Build apps to extend ChatGPT](/apps-sdk)[Agentic Commerce

Build commerce flows in ChatGPT](/commerce)

[Blog](/blog)

## Search the Codex docs

Close

Primary navigation

Codex

Resources   Codex   ChatGPT   Blog

* [Home](/resources)
* [Changelog](/changelog)

### Categories

* [Code](/resources/code)
* [Cookbooks](/resources/cookbooks)
* [Guides](/resources/guides)
* [Videos](/resources/videos)

### Topics

* [Agents](/resources/agents)
* [Audio & Voice](/resources/audio)
* [Image generation](/resources/imagegen)
* [Video generation](/resources/videogen)
* [Tools](/resources/tools)
* [Computer use](/resources/cua)
* [Fine-tuning](/resources/fine-tuning)
* [Scaling](/resources/scaling)

### Getting Started

* [Home](/codex)
* [Quickstart](/codex/quickstart)
* [Pricing](/codex/pricing)
* Concepts
  + [Tasks & Prompts](/codex/concepts)
  + [Models](/codex/models)
  + [Sandboxing](/codex/sandbox)
  + [Build AI-Native Teams](/codex/guides/build-ai-native-engineering-team)

### Using Codex

* IDE Extension
  + [Overview](/codex/ide)
  + [Features](/codex/ide/features)
* CLI
  + [Overview](/codex/cli)
  + [Features](/codex/cli/features)
  + [Command Line Options](/codex/cli/reference)
  + [Slash commands](/codex/guides/slash-commands)
* Web
  + [Overview](/codex/cloud)
  + [Environments](/codex/cloud/environments)
  + [Internet Access](/codex/cloud/internet-access)
* Integrations
  + [GitHub](/codex/integrations/github)
  + [Slack](/codex/integrations/slack)
  + [Linear](/codex/integrations/linear)

### Configuration

* [Config File](/codex/local-config)
* [Instructions](/codex/guides/agents-md)
* [MCP](/codex/mcp)
* [Skills](/codex/skills)

### Deployment & Admin

* [Administration](/codex/enterprise)
* [Authentication](/codex/guides/api-key)
* [Security](/codex/security)
* [Windows](/codex/windows)

### Automation

* [Codex SDK](/codex/sdk)
* [MCP Server](/codex/guides/agents-sdk)
* [GitHub Action](/codex/github-action)

### Releases

* [Changelog](/codex/changelog)

* [Apps SDK](/apps-sdk)
* [Agentic Commerce](/commerce)

* [All posts](/blog)

### Recent

* [What makes a great ChatGPT app](/blog/what-makes-a-great-chatgpt-app)
* [Using Codex for education at Dagster Labs](/blog/codex-for-documentation-dagster)
* [How Codex ran OpenAI DevDay 2025](/blog/codex-at-devday)
* [Why we built the Responses API](/blog/responses-api)
* [Developer notes on the Realtime API](/blog/realtime-api)

Search
⌘
K

### Getting Started

* [Home](/codex)
* [Quickstart](/codex/quickstart)
* [Pricing](/codex/pricing)
* Concepts
  + [Tasks & Prompts](/codex/concepts)
  + [Models](/codex/models)
  + [Sandboxing](/codex/sandbox)
  + [Build AI-Native Teams](/codex/guides/build-ai-native-engineering-team)

### Using Codex

* IDE Extension
  + [Overview](/codex/ide)
  + [Features](/codex/ide/features)
* CLI
  + [Overview](/codex/cli)
  + [Features](/codex/cli/features)
  + [Command Line Options](/codex/cli/reference)
  + [Slash commands](/codex/guides/slash-commands)
* Web
  + [Overview](/codex/cloud)
  + [Environments](/codex/cloud/environments)
  + [Internet Access](/codex/cloud/internet-access)
* Integrations
  + [GitHub](/codex/integrations/github)
  + [Slack](/codex/integrations/slack)
  + [Linear](/codex/integrations/linear)

### Configuration

* [Config File](/codex/local-config)
* [Instructions](/codex/guides/agents-md)
* [MCP](/codex/mcp)
* [Skills](/codex/skills)

### Deployment & Admin

* [Administration](/codex/enterprise)
* [Authentication](/codex/guides/api-key)
* [Security](/codex/security)
* [Windows](/codex/windows)

### Automation

* [Codex SDK](/codex/sdk)
* [MCP Server](/codex/guides/agents-sdk)
* [GitHub Action](/codex/github-action)

### Releases

* [Changelog](/codex/changelog)

Copy PageMore page actions

Copy PageMore page actions

# Agent Skills

Give Codex new capabilities and expertise

Agent Skills let you extend Codex with task-specific capabilities. A skill packages instructions, resources, and optional scripts so Codex can perform a specific workflow reliably. You can share skills across teams or the community, and they build on the [open Agent Skills standard](http://agentskills.io).

Skills are available in both the Codex CLI and IDE extensions.

## What are Agent Skills

A skill captures a capability expressed through markdown instructions inside a `SKILL.md` file accompanied by optional scripts, resources, and assets that Codex uses to perform a specific task.

* my-skill/

  + SKILL.md    Required: instructions + metadata
  + scripts/    Optional: executable code
  + references/    Optional: documentation
  + assets/    Optional: templates, resources

Skills use **progressive disclosure** to manage context efficiently. At startup, Codex loads the name and description of each available skill. Codex can then activate and use a skill in two ways:

1. **Explicit invocation:** You can include skills directly as part of your prompt. To select one, run the `/skills` slash command, or start typing `$` to mention a skill. (Codex web and iOS don’t support explicit invocation yet, but you can still prompt Codex to use any skill checked into the repo.)

![](/images/codex/skills/skills-selector-cli-light.webp)

![](/images/codex/skills/skills-selector-ide-light.webp)![](/images/codex/skills/skills-selector-ide-dark.webp)

2. **Implicit invocation:** Codex can decide to use an available skill when the user’s task matches the skill’s description.

In either method, Codex reads the full instructions of the invoked skills and any extra references checked into the skill.

## Where to save skills

Codex loads skills from these locations. A skill’s location defines its scope.

When Codex loads available skills from these locations, it overwrites skills with the same name from a scope of lower precedence. The list below shows skill scopes and locations in order of precedence (high to low):

| Skill Scope | Location | Suggested Use |
| --- | --- | --- |
| `REPO` | `$CWD/.codex/skills`   Current Working Directory: where you launch Codex. | If in a repository or code environment, teams can check in skills most relevant to a working folder here. For instance, skills only relevant to a microservice or a code module. |
| `REPO` | `$CWD/../.codex/skills`   A folder above CWD when you launch Codex inside a git repository. | If in a repository with nested folders, organizations can check in skills most relevant to a shared area in a parent folder. |
| `REPO` | `$REPO_ROOT/.codex/skills`   The top-most root folder when you launch Codex inside a git repository. | If in a repository with nested folders, organizations can check in skills that are relevant to everyone using the repository. These serve as root skills that any subfolder in the repository can overwrite. |
| `USER` | `$CODEX_HOME/skills`   (Mac/Linux default: `~/.codex/skills`)   Any skills checked into the user’s personal folder. | Use to curate skills relevant to a user that apply to any repository the user may work in. |
| `ADMIN` | `/etc/codex/skills`   Any skills checked into the machine or container in a shared, system location. | Use for SDK scripts, automation, and for checking in default admin skills available to each user on the machine. |
| `SYSTEM` | Bundled with Codex. | Useful skills relevant to a broad audience such as the skill-creator and plan skills. Available to everyone when they start Codex and can be overwritten by any layer above. |

## Create a skill

To create a new skill, use the built-in `$skill-creator` skill inside Codex. Describe what you want your skill to do, and Codex will start bootstrapping your skill. If you combine it with the `$plan` skill, Codex will first create a plan for your skill.

You can also create a skill manually by creating a folder with a `SKILL.md` file inside a valid skill location. A `SKILL.md` must contain a `name` and `description` to help Codex select the skill:

```
---
name: skill-name
description: Description that helps Codex select the skill
metadata:
  short-description: Optional user-facing description
---

Skill instructions for the Codex agent to follow when using this skill.
```

Codex skills build on the [Agent Skills specification](https://agentskills.io/specification). Check out the documentation to learn more.

## Install new skills

To expand on the list of built-in skills, you can download skills from a [curated set of skills on GitHub](https://github.com/openai/skills) using the `$skill-installer` skill:

```
$skill-installer linear
```

You can also prompt the installer to download skills from other repositories.

## Skill examples

### Plan a new feature

Codex ships with a built-in `$plan` skill that’s great to have Codex research and create a plan to build a new feature or solve a complex problem.

### Access Linear context for Codex tasks

```
$skill-installer linear
```

[](https://cdn.openai.com/codex/docs/linear-example.mp4)

### Have Codex access Notion for more context

```
$skill-installer notion-spec-to-implementation
```

[](https://cdn.openai.com/codex/docs/notion-spec-example.mp4)

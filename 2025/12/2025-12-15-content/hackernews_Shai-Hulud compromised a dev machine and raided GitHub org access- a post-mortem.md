---
source: hackernews
title: Shai-Hulud compromised a dev machine and raided GitHub org access: a post-mortem
url: https://trigger.dev/blog/shai-hulud-postmortem
date: 2025-12-15
---

[Trigger.dev logo](/)

* [How it works](/#how-it-works)
* [Product](/product)

  ### Product

  [All features](/product)

  [#### AI Agents

  Build and deploy invincible AI agents.](/product/ai-agents)[#### Trigger.dev Realtime

  Connect your frontend app to your tasks.](/product/realtime)[#### Concurrency & queues

  Control how many tasks run at once.](/product/concurrency-and-queues)[#### Scheduled tasks

  Durable cron schedules without timeouts.](/product/scheduled-tasks)[#### Observability & monitoring

  Real-time monitoring and tracing of tasks.](/product/observability-and-monitoring)[#### Roadmap

  See what we're building next.](https://feedback.trigger.dev/roadmap)
* [Changelog](/changelog)

  ### Latest changelogs

  [All changelog entries](/changelog)

  [![Deployments with native builds](/changelog/native-builds/native-builds.jpg)

  #### Deployments with native builds

  December 8](/changelog/deployments-with-native-builds)[![Higher concurrency limits and lower prices](/changelog/concurrency-plan-increases/concurrency-plan-increases.jpg?1)

  #### Higher concurrency limits and lower prices

  November 27](/changelog/concurrency-plan-increases)[![Prisma 6 & 7 support](/changelog/prisma-7-integration/prisma-7-integration.jpg)

  #### Prisma 6 & 7 support

  November 19](/changelog/prisma-7-integration)
* [Blog](/blog)

  ### Latest blog posts

  [All blog posts](/blog)

  [![How GovSignals is solving government procurement using Trigger.dev](/customers/govsignals/govsignals-customer-story-thumbnail.jpg)

  #### How GovSignals is solving government procurement using Trigger.dev

  December 11](/customers/govsignals-customer-story)[![OTel incident post-mortem](/blog/clickhouse-too-many-parts-postmortem/otel-post-mortem-dec-1-2025-thumbnail.jpg)

  #### OTel incident post-mortem

  December 2](/blog/clickhouse-too-many-parts-postmortem)[![How we got hit by Shai-Hulud: A complete post-mortem](/blog/shai-hulud-postmortem/shai-hulud-postmortem-thumbnail.jpg)

  #### How we got hit by Shai-Hulud: A complete post-mortem

  November 28](/blog/shai-hulud-postmortem)
* [Docs](https://trigger.dev/docs)

  ### Documentation

  [Quick start](https://trigger.dev/docs/quick-start)[Writing tasks](https://trigger.dev/docs/tasks/overview)[Official MCP server](https://trigger.dev/docs/mcp-introduction)[Guides & examples](https://trigger.dev/docs/guides/introduction)[Trigger.dev Realtime](https://trigger.dev/docs/realtime)[React Hooks](https://trigger.dev/docs/frontend/react-hooks/overview)[Scheduled tasks (cron)](https://trigger.dev/docs/tasks-scheduled)[Self-hosting](https://trigger.dev/docs/self-hosting/overview)

  ### Guides

  [View docs](https://trigger.dev/docs/introduction)

  [How to build AI agents](https://trigger.dev/docs/guides/ai-agents/overview)[Triggering Python scripts](https://trigger.dev/docs/config/extensions/pythonExtension)[Get started with Node.js](https://trigger.dev/docs/guides/frameworks/nodejs)[Get started with Next.js](https://trigger.dev/docs/guides/frameworks/nextjs)[Get started with Bun](https://trigger.dev/docs/guides/frameworks/bun)[Get started with Remix](https://trigger.dev/docs/guides/frameworks/remix)[Using Supabase & Trigger.dev](https://trigger.dev/docs/guides/frameworks/supabase-guides-overview)[Trigger tasks from webhooks](https://trigger.dev/docs/guides/frameworks/webhooks-guides-overview)
* [Pricing](/pricing)

* [Star 13.0k](https://github.com/triggerdotdev/trigger.dev)
* [Login](https://cloud.trigger.dev)
* [Get started](https://cloud.trigger.dev)

[Post-mortem ·

November 28, 2025

View all blog posts](/blog)

# How we got hit by Shai-Hulud: A complete post-mortem

![Eric Allam](/blog/authors/eric.png)

[Eric
Allam](https://x.com/maverickdotdev)

CTO, Trigger.dev

![Image for How we got hit by Shai-Hulud: A complete post-mortem](/blog/shai-hulud-postmortem/shai-hulud-postmortem.png)

On November 25th, 2025, we were on a routine Slack huddle debugging a production issue when we noticed something strange: a PR in one of our internal repos was suddenly closed, showed zero changes, and had a single commit from... Linus Torvalds?

The commit message was just "init."

Within seconds, our #git Slack channel exploded with notifications. Dozens of force-pushes. PRs closing across multiple repositories. All attributed to one of our engineers.

![Nick's initial alert](/blog/shai-hulud-postmortem/screenshots/nicks-alert.png)

We had been compromised by [Shai-Hulud 2.0](https://socket.dev/blog/shai-hulud-strikes-again-v2), a sophisticated npm supply chain worm that compromised over 500 packages, affected 25,000+ repositories, and spread across the JavaScript ecosystem. We weren't alone: [PostHog](https://posthog.com/blog/nov-24-shai-hulud-attack-post-mortem), Zapier, AsyncAPI, Postman, and ENS were among those hit.

This is the complete story of what happened, how we responded, and what we've changed to prevent this from happening again.

> No Trigger.dev packages were ever compromised. The `@trigger.dev/*` packages and `trigger.dev` CLI were never infected with Shai-Hulud malware. This incident involved one of our engineers installing a compromised package on their development machine, which led to credential theft and unauthorized access to our GitHub organization. Our published packages remained safe throughout.

---

## The Attack Timeline

| Time (UTC) | Event |
| --- | --- |
| Nov 24, 04:11 | Malicious packages go live |
| Nov 24, ~20:27 | Engineer compromised |
| Nov 24, 22:36 | First attacker activity |
| Nov 25, 02:56-05:32 | Overnight reconnaissance |
| Nov 25, 09:08-15:08 | Legitimate engineer work (from Germany) |
| Nov 25, 09:10-09:17 | Attacker monitors engineer activity |
| Nov 25, 15:17-15:27 | Final recon |
| Nov 25, 15:27-15:37 | Destructive attack |
| Nov 25, ~15:32 | Detection |
| Nov 25, ~15:36 | Access revoked |
| Nov 25, 16:35 | AWS session blocked |
| Nov 25, 22:35 | All branches restored |
| Nov 26, 20:16 | GitHub App key rotated |

---

## The compromise

On the evening of November 24th, around 20:27 UTC (9:27 PM local time in Germany), one of our engineers was experimenting with a new project. They ran a command that triggered `pnpm install`. At that moment, somewhere in the dependency tree, a malicious package executed.

We don't know exactly which package delivered the payload. The engineer was experimenting at the time and may have deleted the project directory as part of cleanup. By the time we investigated, we couldn't trace back to the specific package. The engineer checked their shell history and they'd only run install commands in our main trigger repo, cloud repo, and one experimental project.

This is one of the frustrating realities of these attacks: once the malware runs, identifying the source becomes extremely difficult. The package doesn't announce itself. The `pnpm install` completes successfully. Everything looks normal.

What we do know is that the Shai-Hulud malware ran a `preinstall` script that:

1. Downloaded and executed [TruffleHog](https://github.com/trufflesecurity/trufflehog), a legitimate security tool repurposed for credential theft
2. Scanned the engineer's machine for secrets: GitHub tokens, AWS credentials, npm tokens, environment variables
3. Exfiltrated everything it found

When the engineer later recovered files from their compromised laptop (booted in recovery mode), they found the telltale signs:

![TruffleHog artifacts found on compromised machine](/blog/shai-hulud-postmortem/screenshots/trufflehog.jpg)

The `.trufflehog-cache` directory and `trufflehog_3.91.1_darwin_amd64.tar.gz` file found on the compromised machine. The `extract` directory was empty, likely cleaned up by the malware to cover its tracks.

---

## 17 hours of reconnaissance

The attacker had access to our engineer's GitHub account for 17 hours before doing anything visible. According to our GitHub audit logs, they operated methodically.

Just over two hours after the initial compromise, the attacker validated their stolen credentials and began mass cloning:

| Time (UTC) | Location | Activity |
| --- | --- | --- |
| 22:36:50 | US | First attacker access, mass cloning begins |
| 22:36-22:39 | US | 73 repositories cloned |
| 22:48-22:50 | US | ~70 more repositories cloned (second wave) |
| 22:55-22:56 | US | ~90 repositories cloned (third wave) |
| 22:59-23:04 | US | ~70 repositories cloned (fourth wave) |
| 23:32:59 | India | Attacker switches to India-based infrastructure |
| 23:32-23:37 | India | 73 repositories cloned |
| 23:34-23:35 | US + India | Simultaneous cloning from both locations |

The simultaneous activity from US and India confirmed we were dealing with a single attacker using multiple VPNs or servers, not separate actors.

While our engineer slept in Germany, the attacker continued their reconnaissance. More cloning at 02:56-02:59 UTC (middle of the night in Germany), sporadic activity until 05:32 UTC. Total repos cloned: 669 (527 from US infrastructure, 142 from India).

Here's where it gets unsettling. Our engineer woke up and started their normal workday:

| Time (UTC) | Actor | Activity |
| --- | --- | --- |
| 09:08:27 | Engineer | Triggers workflow on cloud repo (from Germany) |
| 09:10-09:17 | Attacker | Git fetches from US, watching the engineer |
| 09:08-15:08 | Engineer | Normal PR reviews, CI workflows (from Germany) |

The attacker was monitoring our engineer's activity while they worked, unaware they were compromised.

During this period, the attacker created repositories with random string names to store stolen credentials, a known Shai-Hulud pattern:

* `github.com/[username]/xfjqb74uysxcni5ztn`
* `github.com/[username]/ls4uzkvwnt0qckjq27`
* `github.com/[username]/uxa7vo9og0rzts362c`

They also created three repos marked with "Sha1-Hulud: The Second Coming" as a calling card. These repositories were empty by the time we examined them, but based on the documented Shai-Hulud behavior, they likely contained triple base64-encoded credentials.

---

## 10 minutes of destruction

At 15:27 UTC on November 25th, the attacker switched from reconnaissance to destruction.

The attack began on our `cloud` repo from India-based infrastructure:

| Time (UTC) | Event | Repo | Details |
| --- | --- | --- | --- |
| 15:27:35 | First force-push | triggerdotdev/cloud | Attack begins |
| 15:27:37 | PR closed | triggerdotdev/cloud | PR #300 closed |
| 15:27:44 | BLOCKED | triggerdotdev/cloud | Branch protection rejected force-push |
| 15:27:50 | PR closed | triggerdotdev/trigger.dev | PR #2707 closed |

The attack continued on our main repository:

| Time (UTC) | Event | Details |
| --- | --- | --- |
| 15:28:13 | PR closed | triggerdotdev/trigger.dev PR #2706 (release PR) |
| 15:30:51 | PR closed | triggerdotdev/trigger.dev PR #2451 |
| 15:31:10 | PR closed | triggerdotdev/trigger.dev PR #2382 |
| 15:31:16 | BLOCKED | Branch protection rejected force-push to trigger.dev |
| 15:31:31 | PR closed | triggerdotdev/trigger.dev PR #2482 |

At 15:32:43-46 UTC, 12 PRs on jsonhero-web were closed in 3 seconds. Clearly automated. PRs #47, #169, #176, #181, #189, #190, #194, #197, #204, #206, #208 all closed within a 3-second window.

Our critical infrastructure repository was targeted next:

| Time (UTC) | Event | Details |
| --- | --- | --- |
| 15:35:41 | PR closed | triggerdotdev/infra PR #233 |
| 15:35:45 | BLOCKED | Branch protection rejected force-push (India) |
| 15:35:48 | PR closed | triggerdotdev/infra PR #309 |
| 15:35:49 | BLOCKED | Branch protection rejected force-push (India) |

The final PR was closed on json-infer-types at 15:37:13 UTC.

---

## Detection and response

We got a lucky break. One of our team members was monitoring Slack when the flood of notifications started:

![Slack notifications showing the attack](/blog/shai-hulud-postmortem/screenshots/slack-notifications.png)

Our #git Slack channel during the attack. A wall of force-pushes, all with commit message "init."

Every malicious commit was authored as:

`Author: Linus Torvalds <[[email protected]](/cdn-cgi/l/email-protection)>

Message: init`

![What an attacked branch looked like](/blog/shai-hulud-postmortem/screenshots/attacked-branch.png)

An attacked branch: a single "init" commit attributed to Linus Torvalds, thousands of commits behind main.

We haven't found reports of other Shai-Hulud victims seeing this same "Linus Torvalds" vandalism pattern. The worm's documented behavior focuses on credential exfiltration and npm package propagation, not repository destruction. This destructive phase may have been unique to our attacker, or perhaps a manual follow-up action after the automated worm had done its credential harvesting.

Within 4 minutes of detection we identified the compromised account, removed them from the GitHub organization, and the attack stopped immediately.

Our internal Slack during those first minutes:

> "Urmmm guys? what's going on?"
>
> "add me to the call @here"
>
> "Nick could you double check Infisical for any machine identities"
>
> "can someone also check whether there are any reports of compromised packages in our CLI deps?"

Within the hour:

| Time (UTC) | Action |
| --- | --- |
| ~15:36 | Removed from GitHub organization |
| ~15:40 | Removed from Infisical (secrets manager) |
| ~15:45 | Removed from AWS IAM Identity Center |
| ~16:00 | Removed from Vercel and Cloudflare |
| 16:35 | AWS SSO sessions blocked via deny policy (sessions can't be revoked) |
| 16:45 | IAM user console login deleted |

---

## The damage

Repository clone actions: 669 (public and private), including infrastructure code, internal documentation, and engineering plans.

Branches force-pushed: 199 across 16 repositories

Pull requests closed: 42

Protected branch rejections: 4. Some of our repositories have main branch protection enabled, but we had not enabled it for all repositories at the time of the incident.

npm packages were **not** compromised. This is the difference between "our repos got vandalized" and "our packages got compromised."

Our engineer didn't have an npm publishing token on their machine, and even if they did we had already required 2FA for publishing to npm. Without that, Shai-Hulud would have published malicious versions of `@trigger.dev/sdk`, `@trigger.dev/core`, and others, potentially affecting thousands of downstream users.

Production databases or any AWS resources were **not** accessed. Our AWS CloudTrail audit showed only read operations from the compromised account:

| Event Type | Count | Service |
| --- | --- | --- |
| ListManagedNotificationEvents | ~40 | notifications |
| DescribeClusters | 8 | ECS |
| DescribeTasks | 4 | ECS |
| DescribeMetricFilters | 6 | CloudWatch |

These were confirmed to be legitimate operations by our engineer.

One nice surprise: AWS actually sent us a proactive alert about Shai-Hulud. They detected the malware's characteristic behavior (ListSecrets, GetSecretValue, BatchGetSecretValue API calls) on an old test account that hadn't been used in months, so we just deleted it. But kudos to AWS for the proactive detection and notification.

---

## The recovery

GitHub doesn't have server-side reflog. When someone force-pushes, that history is gone from GitHub's servers.

But we found ways to recover.

Push events are retained for 90 days via the GitHub Events API. We wrote a script that fetched pre-attack commit SHAs:

`# Find pre-attack commit SHA from events

gh api repos/$REPO/events --paginate | \

jq -r '.[] | select(.type=="PushEvent") |

select(.payload.ref=="refs/heads/'$BRANCH'") |

.payload.before' | head -1`

Public repository forks still contained original commits. We used these to verify and restore branches.

Developers who hadn't run `git fetch --prune` (all of us?) still had old SHAs in their local reflog.

Within 7 hours, all 199 branches were restored.

---

## GitHub app private key exposure

During the investigation, our engineer was going through files recovered from the compromised laptop and discovered something concerning: the private key for our GitHub App was in the trash folder.

When you create a private key in the GitHub App settings, GitHub automatically downloads it. The engineer had created a key at some point, and while the active file had been deleted, it was still in the trash, potentially accessible to TruffleHog.

Our GitHub App has the following permissions on customer repositories:

| Permission | Access Level | Risk |
| --- | --- | --- |
| contents | read/write | Could read/write repository contents |
| pull\_requests | read/write | Could read/create/modify PRs |
| deployments | read/write | Could create/trigger deployments |
| checks | read/write | Could create/modify check runs |
| commit\_statuses | read/write | Could mark commits as passing/failing |
| metadata | read | Could read repository metadata |

To generate valid access tokens, an attacker would need both the private key (potentially compromised) and the installation ID for a specific customer (stored in our database which was not compromised, not on the compromised machine).

We immediately rotated the key:

| Time (UTC) | Action |
| --- | --- |
| Nov 26, 18:51 | Private key discovered in trash folder |
| Nov 26, 19:54 | New key deployed to test environment |
| Nov 26, 20:16 | New key deployed to production |

We found no evidence of unauthorized access to any customer repositories. The attacker would have needed installation IDs from our database to generate tokens, and our database was not compromised as previously mentioned.

However, we cannot completely rule out the possibility. An attacker with the private key could theoretically have called the GitHub API to enumerate all installations. We've contacted GitHub Support to request additional access logs. We've also analyzed the webhook payloads to our GitHub app, looking for suspicious push or PR activity from connected installations & repositories. We haven't found any evidence of unauthorized activity in these webhook payloads.

We've sent out an email to potentially effected customers to notify them of the incident with detailed instructions on how to check if they were affected. Please check your email for more details if you've used our GitHub app.

---

## Technical deep-dive: how Shai-Hulud works

For those interested in the technical details, here's what we learned about the malware from [Socket's analysis](https://socket.dev/blog/shai-hulud-strikes-again-v2) and our own investigation.

When npm runs the `preinstall` script, it executes `setup_bun.js`:

1. Detects OS/architecture
2. Downloads or locates the [Bun](https://bun.sh) runtime
3. Caches Bun in `~/.cache`
4. Spawns a detached Bun process running `bun_environment.js` with output suppressed
5. Returns immediately so `npm install` completes successfully with no warnings

The malware runs in the background while you think everything is fine.

The payload uses TruffleHog to scan `$HOME` for GitHub tokens (from env vars, gh CLI config, git credential helpers), AWS/GCP/Azure credentials, npm tokens from `.npmrc`, environment variables containing anything that looks like a secret, and GitHub Actions secrets (if running in CI).

Stolen credentials are uploaded to a newly-created GitHub repo with a random name. The data is triple base64-encoded to evade GitHub's secret scanning.

Files created:

* `contents.json` (system info and GitHub credentials)
* `environment.json` (all environment variables)
* `cloud.json` (cloud provider credentials)
* `truffleSecrets.json` (filesystem secrets from TruffleHog)
* `actionsSecrets.json` (GitHub Actions secrets if any)

If an npm publishing token is found, the malware validates the token against the npm registry, fetches packages maintained by that account, downloads each package, patches it with the malware, bumps the version, and re-publishes, infecting more packages.

This is how the worm spread through the npm ecosystem, starting from [PostHog's compromised CI](https://posthog.com/blog/nov-24-shai-hulud-attack-post-mortem) on November 24th at 4:11 AM UTC. Our engineer was infected roughly 16 hours after the malicious packages went live.

If no credentials are found to exfiltrate or propagate, the malware attempts to delete the victim's entire home directory. Scorched earth.

File artifacts to look for: `setup_bun.js`, `bun_environment.js`, `cloud.json`, `contents.json`, `environment.json`, `truffleSecrets.json`, `actionsSecrets.json`, `.trufflehog-cache/` directory.

Malware file hashes (SHA1):

* `bun_environment.js`: `d60ec97eea19fffb4809bc35b91033b52490ca11`
* `bun_environment.js`: `3d7570d14d34b0ba137d502f042b27b0f37a59fa`
* `setup_bun.js`: `d1829b4708126dcc7bea7437c04d1f10eacd4a16`

We've published a [detection script](https://gist.github.com/ericallam/e2ae497d99a21d500fd9fbb01e1bfa02) that checks for Shai-Hulud indicators.

---

## What we've changed

We disabled npm scripts globally:

`npm config set ignore-scripts true --location=global`

This prevents `preinstall`, `postinstall`, and other lifecycle scripts from running. It's aggressive and some packages will break, but it's the only reliable protection against this class of attack.

We upgraded to pnpm 10. This was significant effort (had to migrate through pnpm 9 first), but pnpm 10 brings critical security improvements. Scripts are ignored by default. You can explicitly whitelist packages that need to run scripts via `pnpm.onlyBuiltDependencies`. And the `minimumReleaseAge` setting prevents installing packages published recently.

`# pnpm-workspace.yaml

minimumReleaseAge: 4320 # 3 days in minutes

preferOffline: true`

To whitelist packages that legitimately need build scripts:

`pnpm approve-builds`

This prompts you to select which packages to allow (like `esbuild`, `prisma`, `sharp`).

For your global pnpm config:

`pnpm config set minimumReleaseAge 4320

pnpm config set --json minimumReleaseAgeExclude '["@trigger.dev/*", "trigger.dev"]'`

We switched npm publishing to OIDC. No more long-lived npm tokens anywhere. Publishing now uses [npm's trusted publishers](https://docs.npmjs.com/trusted-publishers) with GitHub Actions OIDC. Even if an attacker compromises a developer machine, they can't publish packages because there are no credentials to steal. Publishing only happens through CI with short-lived, scoped tokens.

We enabled branch protection on all repositories. Not just critical repos or just OSS repos. Every repository with meaningful code now has branch protection enabled.

We've adopted [Granted](https://docs.commonfate.io/granted/introduction) for AWS SSO. Granted encrypts SSO session tokens on the client side, unlike the AWS CLI which stores them in plaintext.

Based on [PostHog's analysis](https://posthog.com/blog/nov-24-shai-hulud-attack-post-mortem) of how they were initially compromised (via `pull_request_target`), we've reviewed our GitHub Actions workflows. We now require approval for external contributor workflow runs on all our repositories (previous policy was only for public repositories).

---

## Lessons for other teams

The ability for packages to run arbitrary code during installation is the attack surface. Until npm fundamentally changes, add this to your `~/.npmrc`:

`ignore-scripts=true`

Yes, some things will break. Whitelist them explicitly. The inconvenience is worth it.

pnpm 10 ignores scripts by default and lets you set a minimum age for packages:

`pnpm config set minimumReleaseAge 4320 # 3 days`

Newly published packages can't be installed for 3 days, giving time for malicious packages to be detected.

Branch protection takes 30 seconds to enable. It prevents attackers from pushing to a main branch, potentially executing malicious GitHub action workflows.

Long-lived npm tokens on developer machines are a liability. Use [trusted publishers](https://docs.npmjs.com/trusted-publishers) with OIDC instead.

If you don't need a credential on your local machine, don't have it there. Publishing should happen through CI only.

Our #git Slack channel is noisy. That noise saved us.

---

## A note on the human side

One of the hardest parts of this incident was that it happened to a person.

> "Sorry for all the trouble guys, terrible experience"

Our compromised engineer felt terrible, even though they did absolutely nothing wrong. It could have happened to any team member.

Running `npm install` is not negligence. Installing dependencies is not a security failure. The security failure is in an ecosystem that allows packages to run arbitrary code silently.

They also discovered that the attacker had made their GitHub account star hundreds of random repositories during the compromise. Someone even emailed us: "hey you starred my repo but I think it was because you were hacked, maybe remove the star?"

---

## Summary

| Metric | Value |
| --- | --- |
| Time from compromise to first attacker activity | ~2 hours |
| Time attacker had access before destructive action | ~17 hours |
| Duration of destructive attack | ~10 minutes (15:27-15:37 UTC) |
| Time from first malicious push to detection | ~5 minutes |
| Time from detection to access revocation | ~4 minutes |
| Time to full branch recovery | ~7 hours |
| Repository clone actions by attacker | 669 |
| Repositories force-pushed | 16 |
| Branches affected | 199 |
| Pull requests closed | 42 |
| Protected branch rejections | 4 |

---

## Resources

About the Attack:

* [Socket.dev: Shai-Hulud Strikes Again V2](https://socket.dev/blog/shai-hulud-strikes-again-v2) - Technical deep-dive into the malware
* [PostHog Post-Mortem](https://posthog.com/blog/nov-24-shai-hulud-attack-post-mortem) - Another company's experience with Shai-Hulud
* [Wiz Blog: Shai-Hulud 2.0 Supply Chain Attack](https://www.wiz.io/blog/shai-hulud-2-0-ongoing-supply-chain-attack)
* [The Hacker News Coverage](https://thehackernews.com/2025/11/second-sha1-hulud-wave-affects-25000.html)
* [Endor Labs Analysis](https://www.endorlabs.com/learn/shai-hulud-2-malware-campaign-targets-github-and-cloud-credentials-using-bun-runtime)
* [HelixGuard Advisory](https://helixguard.ai/blog/malicious-sha1hulud-2025-11-24) (referenced in AWS alert)

Mitigation Resources:

* [npm Trusted Publishers](https://docs.npmjs.com/trusted-publishers) - OIDC-based publishing
* [pnpm onlyBuiltDependencies](https://pnpm.io/package_json#pnpmonlybuiltdependencies) - Whitelist packages allowed to run scripts
* [pnpm minimumReleaseAge](https://pnpm.io/settings#minimumreleaseage) - Delay installation of new packages
* [Granted](https://docs.commonfate.io/granted/introduction) - AWS SSO credential management

---

Have questions about this incident? Reach out on [Twitter/X](https://twitter.com/triggerdotdev) or [Discord](https://trigger.dev/discord).

* [On this page](/blog/shai-hulud-postmortem)

+ [The Attack Timeline](/blog/shai-hulud-postmortem#the-attack-timeline)
+ [The compromise](/blog/shai-hulud-postmortem#the-compromise)
+ [17 hours of reconnaissance](/blog/shai-hulud-postmortem#17-hours-of-reconnaissance)
+ [10 minutes of destruction](/blog/shai-hulud-postmortem#10-minutes-of-destruction)
+ [Detection and response](/blog/shai-hulud-postmortem#detection-and-response)
+ [The damage](/blog/shai-hulud-postmortem#the-damage)
+ [The recovery](/blog/shai-hulud-postmortem#the-recovery)
+ [GitHub app private key exposure](/blog/shai-hulud-postmortem#github-app-private-key-exposure)
+ [Technical deep-dive: how Shai-Hulud works](/blog/shai-hulud-postmortem#technical-deep-dive-how-shai-hulud-works)
+ [What we've changed](/blog/shai-hulud-postmortem#what-weve-changed)
+ [Lessons for other teams](/blog/shai-hulud-postmortem#lessons-for-other-teams)
+ [A note on the human side](/blog/shai-hulud-postmortem#a-note-on-the-human-side)
+ [Summary](/blog/shai-hulud-postmortem#summary)
+ [Resources](/blog/shai-hulud-postmortem#resources)

Share this article

[![How GovSignals is solving government procurement using Trigger.dev](/customers/govsignals/govsignals-customer-story-thumbnail.jpg)

### How GovSignals is solving government procurement using Trigger.dev

![Conner Aldrich](/customers/govsignals/conner-aldrich.png?1)

Customer story

•

December 11](/customers/govsignals-customer-story)

[![Accelerating global logistics workflows using AI copilots](/customers/pallet/pallet-customer-story-thumbnail.jpg)

### Accelerating global logistics workflows using AI copilots

![Karl Kaiser](/customers/pallet/karl-kaiser.png)

Customer story

•

November 13](/customers/pallet-customer-story)

[![How we built a kanban-style triage agent for managing coding agents](/customers/capy/capy-customer-story-thumbnail.jpg?1)

### How we built a kanban-style triage agent for managing coding agents

![Justin Sun](/testimonials/capy/justin-sun.png)

Customer story

•

October 7](/customers/capy-customer-story)

[![Our roadmap for the next 3 months and beyond](/blog/next-3-months/next-3-months-thumbnail.jpg)

### Our roadmap for the next 3 months and beyond

![James Ritchie](/blog/authors/james.jpg)

Article

•

September 4](/blog/our-roadmap-for-the-next-3-months)

[![Powering HeroUI Chat's complex deployment pipeline with Trigger.dev](/customers/hero-ui/hero-ui-customer-story-thumbnail.jpg?1)

### Powering HeroUI Chat's complex deployment pipeline with Trigger.dev

![Junior Garcia](/customers/hero-ui/junior-garcia.png)

Customer story

•

August 29](/customers/hero-ui-customer-story)

[![Official MCP server & agent rules](/launchweeks/launchweek2/launchweek-2-official-mcp-server.jpg)

### Official MCP server & agent rules

![Eric Allam](/blog/authors/eric.png)

Launch week

•

August 22](/launchweek/2/official-mcp-server)

### Ready to start building?

Build and deploy your first task in 3 minutes.

[Get started now](https://cloud.trigger.dev)

[Trigger.dev logo](#top)

#### Docs

[Introduction](https://trigger.dev/docs/introduction "Introduction")[Quick start](https://trigger.dev/docs/quick-start "Quick start")[Official MCP server](https://trigger.dev/docs/mcp-introduction "Official MCP server")[Tasks](https://trigger.dev/docs/tasks/overview "Tasks")[Examples](https://trigger.dev/docs/guides/introduction "Examples")[AI agents](https://trigger.dev/docs/guides/ai-agents/overview "AI agents")[Self-hosting](https://trigger.dev/docs/self-hosting/overview "Self-hosting")

#### Developers

[Changelog](/changelog "Changelog")[Blog](/blog "Blog")[Contributing](https://github.com/triggerdotdev/trigger.dev/blob/main/CONTRIBUTING.md "Contributing")[Open source](https://github.com/triggerdotdev/trigger.dev?tab=Apache-2.0-1-ov-file#readme "Open source")[OSS friends](/oss-friends "OSS friends")[Testimonials](/#testimonials "Testimonials")[Customers](/customers "Customers")

#### Product

[Pricing](/pricing "Pricing")[Product](/product "Product")[Roadmap](https://feedback.trigger.dev/roadmap "Roadmap")[Feature requests](https://feedback.trigger.dev/ "Feature requests")[FAQs](/pricing#faqs "FAQs")[GitHub](https://github.com/triggerdotdev/trigger.dev "GitHub")[Uptime status](https://status.trigger.dev/ "Uptime status")

#### Company

[Contact](/contact "Contact")[Careers](/careers "Careers")[Terms](/legal "Terms")[Privacy](/legal/privacy "Privacy")[Security](/security "Security")[GDPR](https://security.trigger.dev "GDPR")[SOC2](https://security.trigger.dev "SOC2")

© 2025 API Hero Ltd.

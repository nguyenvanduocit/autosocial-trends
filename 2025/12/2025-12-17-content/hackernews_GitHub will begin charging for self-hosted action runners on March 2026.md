---
source: hackernews
title: GitHub will begin charging for self-hosted action runners on March 2026
url: https://github.blog/changelog/2025-12-16-coming-soon-simpler-pricing-and-a-better-experience-for-github-actions/
date: 2025-12-17
---

[Skip to content](#start-of-content)
[Skip to sidebar](#sidebar)

/
[Blog](https://github.blog/)

* [Changelog](https://github.blog/changelog/)
* [Docs](https://docs.github.com/)
* [Customer stories](https://github.com/customer-stories)

[Try GitHub Copilot](https://github.com/features/copilot?utm_source=blog-tap-nav&utm_medium=blog&utm_campaign=universe25)
[See what's new](https://github.com/events/universe/recap?utm_source=k2k-blog-tap-nav&utm_medium=blog&utm_campaign=universe25)

Search

* [Changelog](https://github.blog/changelog/)
* [Docs](https://docs.github.com/)
* [Customer stories](https://github.com/customer-stories)

[See what's new](https://github.com/events/universe/recap?utm_source=k2k-blog-tap-nav&utm_medium=blog&utm_campaign=universe25)
[Try GitHub Copilot](https://github.com/features/copilot?utm_source=blog-tap-nav&utm_medium=blog&utm_campaign=universe25)

[Back to changelog](https://github.blog/changelog/)

Release

December 16, 2025 •
4 minute read

# Coming soon: Simpler pricing and a better experience for GitHub Actions

![](https://github.blog/wp-content/themes/github-2021-child/assets/img/featured-v3-new-releases.svg)

On **January 1, 2026**, GitHub will **reduce the price of GitHub-hosted runners by up to 39%** [depending on the machine type used](https://docs.github.com/billing/reference/actions-runner-pricing). The free usage minute quotas will remain the same.

On **March 1, 2026,** GitHub will introduce a new **$0.002 per minute GitHub Actions cloud platform charge** that will apply to self-hosted runner usage. Any usage subject to this charge will count toward the minutes included in your plan, as explained in [our GitHub Actions billing documentation](https://docs.github.com/billing/concepts/product-billing/github-actions#free-use-of-github-actions).

Runner usage in public repositories will **remain free.** There will be no changes in price structure for GitHub Enterprise Server customers.

### [Deeper investment in the Actions self-hosted experience](#deeper-investment-in-the-actions-self-hosted-experience)

We are increasing our investment into our self-hosted experience to ensure that we can provide autoscaling for scenarios beyond just Linux containers. This will include new approaches to scaling, new platform support, Windows support, and more as we move through the next 12 months.

For more details about the product investments we’re making in Actions, please visit our [Executive Insights page](https://resources.github.com/actions/2026-pricing-changes-for-github-actions).

### [FAQ](#faq)

**Why am I being charged to use my own hardware?**
Historically, self-hosted runner customers were able to leverage much of GitHub Actions’ infrastructure and services at no cost. This meant that the cost of maintaining and evolving these essential services was largely being subsidized by the prices set for GitHub-hosted runners. By updating our pricing, we’re aligning costs more closely with usage and the value delivered to every Actions user, while fueling further innovation and investment across the platform. The vast majority of users, especially individuals and small teams, will see no price increase.

You can see the [Actions pricing calculator](https://github.com/pricing/calculator#actions) to estimate your future costs.

**What are the new GitHub-hosted runner rates?**
See the GitHub Actions [runner pricing reference](https://docs.github.com/billing/reference/actions-runner-pricing) for the updated rates that will go into effect on January 1, 2026. These listed rates include the new $0.002 per-minute Actions cloud platform charge.

**Why is .002/minute the right price for self-hosted runners on cloud?**
We determined per-minute was deemed the most fair and accurate by our users, and compared to other self-hosted CI solutions in the market. We believe this is a sustainable option that will not deeply impact our lightly- nor heavily-active customers, while still delivering fast, flexible workloads for the best end user experience.

**Which job execution scenarios for GitHub Actions are affected by this pricing change?**

* Jobs that run in private repositories and use standard GitHub-hosted or self-hosted runners
* Any jobs running on larger GitHub-hosted runners

Standard GitHub-hosted or self-hosted runner usage on public repositories will remain free. GitHub Enterprise Server pricing is not impacted by this change.

**When will this pricing change take effect?**

The price decrease for GitHub-hosted runners will take effect on January 1, 2026. The new charge for self-hosted runners will apply beginning on March 1, 2026. The price changes will impact all customers on these dates.

**Will the free usage quota available in my plan change?**
Beginning March 1, 2026, self-hosted runners will be included within your free usage quota, and will consume available usage based on list price the same way that Linux, Windows, and MacOS standard runners work today.

**Will self-hosted runner usage consume from my free usage minutes?**
Yes, billable self-hosted runner usage will be able to consume minutes from the free quota associated with your plan.

**How does this pricing change affect customers on GitHub Enterprise Server?**

This pricing change does not affect customers using GitHub Enterprise Server. Customers running Actions jobs on self-hosted runners on GitHub Enterprise Server may continue to host, manage, troubleshoot and use Actions on and in conjunction with their implementation free of charge.

**Can I bill my self-hosted runner usage on private repositories through Azure?**

Yes, as long as you have an active Azure subscription ID associated with your GitHub Enterprise or Organization(s).

**What is the overall impact of this change to GitHub customers?**

96% of customers will see no change to their bill. Of the 4% of Actions users impacted by this change, 85% of this cohort will see their Actions bill decrease and the remaining 15% who are impacted across all face a median increase around $13.

**Did GitHub consider how this impacts individual developers, not just Enterprise scale customers of GitHub?**
From our individual users (free & Pro plans) of those who used GitHub Actions in the last month in private repos only 0.09% would end up with a price increase, with a median increase of under $2 a month. Note that this impact is after these users have made use of their included minutes in their plans today, entitling them to over 33 hours of included GitHub compute, and this has no impact on their free use of public repos. A further 2.8% of this total user base will see a decrease in their monthly cost as a result of these changes. The rest are unimpacted by this change.

**How can I figure out what my new monthly cost for Actions looks like?**

GitHub Actions provides [detailed usage reports for the current and prior year](https://docs.github.com/billing/how-tos/products/view-productlicense-use). You can use this prior usage alongside the [rate changes](https://docs.github.com/billing/reference/actions-runner-pricing) that will be introduced in January and March to estimate cost under the new pricing structure. We have created a [Python script](https://docs.github.com/billing/tutorials/estimate-actions-costs) to help you leverage [full usage reports](https://docs.github.com/billing/how-tos/products/view-productlicense-use#downloading-usage-reports) to calculate your expected cost after the price updates.

We have also updated our [Actions pricing calculator](https://github.com/pricing/calculator#actions), making it easier to estimate your future costs, particularly if your historical usage is limited or not representative of expected future usage.

## [Recommended resources](#recommended-resources)

* See the [GitHub Actions runner pricing documentation](https://docs.github.com/billing/reference/actions-runner-pricing) for the new GitHub-hosted runner rates effective January 1, 2026.
* For more details on upcoming GitHub Actions releases, see the [GitHub public roadmap](https://github.com/orgs/github/projects/4247).
* For help estimating your expected Actions usage cost, use the newly updated [Actions pricing calculator](https://github.com/pricing/calculator#actions).
* To see your current or historical Actions usage, see our documentation for [viewing and downloading detailed usage reports](https://docs.github.com/billing/how-tos/products/view-productlicense-use).
* If you are interested in moving existing self-hosted runner usage to GitHub-hosted runners, see the [SHR to GHR migration guide](http://docs.github.com/actions/tutorials/migrate-to-github-runners) in our documentation.

[actions](https://github.blog/changelog/2025/?label=actions)

Share
Copied
Shared

[Back to changelog](https://github.blog/changelog/)

## Related Posts

### Dec.12 Improvement

[Better diagnostics for VNET injected runners and required self-hosted runner upgrades](https://github.blog/changelog/2025-12-12-better-diagnostics-for-vnet-injected-runners-and-required-self-hosted-runner-upgrades)

[actions](https://github.blog/changelog/2025/?label=actions)

### Dec.04 Improvement

[Actions workflow dispatch workflows now support 25 inputs](https://github.blog/changelog/2025-12-04-actions-workflow-dispatch-workflows-now-support-25-inputs)

[actions](https://github.blog/changelog/2025/?label=actions)

### Nov.25 Improvement

[Code scanning default setup bypasses GitHub Actions policy blocks](https://github.blog/changelog/2025-11-25-code-scanning-default-setup-bypasses-github-actions-policy-blocks)

[actions](https://github.blog/changelog/2025/?label=actions)
[application security](https://github.blog/changelog/2025/?label=application-security)
[platform governance](https://github.blog/changelog/2025/?label=platform-governance)

...
+2

### Nov.20 Improvement

[GitHub Actions cache size can now exceed 10 GB per repository](https://github.blog/changelog/2025-11-20-github-actions-cache-size-can-now-exceed-10-gb-per-repository)

[actions](https://github.blog/changelog/2025/?label=actions)

### Nov.13 Improvement

[New GitHub Actions OIDC token claims](https://github.blog/changelog/2025-11-13-github-actions-oidc-token-claims-now-include-check_run_id)

[actions](https://github.blog/changelog/2025/?label=actions)

### Nov.07 Improvement

[Actions pull\_request\_target and environment branch protections changes](https://github.blog/changelog/2025-11-07-actions-pull_request_target-and-environment-branch-protections-changes)

[actions](https://github.blog/changelog/2025/?label=actions)

### Nov.06 Improvement

[New releases for GitHub Actions – November 2025](https://github.blog/changelog/2025-11-06-new-releases-for-github-actions-november-2025)

[actions](https://github.blog/changelog/2025/?label=actions)

### Oct.28 Retired

[Upcoming deprecation of CodeQL Action v3](https://github.blog/changelog/2025-10-28-upcoming-deprecation-of-codeql-action-v3)

[actions](https://github.blog/changelog/2025/?label=actions)
[application security](https://github.blog/changelog/2025/?label=application-security)

...
+1

### Oct.28 Improvement

[Copilot coding agent now supports self-hosted runners](https://github.blog/changelog/2025-10-28-copilot-coding-agent-now-supports-self-hosted-runners)

[actions](https://github.blog/changelog/2025/?label=actions)
[copilot](https://github.blog/changelog/2025/?label=copilot)
[universe25](https://github.blog/changelog/2025/?label=universe25)

...
+2

## Subscribe to our developer newsletter

Discover tips, technical guides, and best practices in our biweekly newsletter just for devs.

Enter your email\*

Subscribe

By submitting, I agree to let GitHub and its affiliates use my information for personalized communications, targeted advertising, and campaign effectiveness. See the [GitHub Privacy Statement](https://github.com/site/privacy) for more details.

[Back to top](#start-of-content)

## Site-wide Links

### Product

* [Features](https://github.com/features)
* [Security](https://github.com/security)
* [Enterprise](https://github.com/enterprise)
* [Customer Stories](https://github.com/customer-stories?type=enterprise)
* [Pricing](https://github.com/pricing)
* [Resources](https://resources.github.com/)

### Platform

* [Developer API](https://developer.github.com/)
* [Partners](https://partner.github.com/)
* [Atom](https://atom.io/)
* [Electron](https://www.electronjs.org/)
* [GitHub Desktop](https://desktop.github.com/)

### Support

* [Docs](https://docs.github.com/)
* [Community Forum](https://github.community/)
* [Training](https://services.github.com/)
* [Status](https://www.githubstatus.com/)
* [Contact](https://support.github.com/)

### Company

* [About](https://github.com/about)
* [Blog](https://github.blog/)
* [Careers](https://github.com/about/careers)
* [Press](https://github.com/about/press)
* [Shop](https://shop.github.com/)

* © 2025 GitHub, Inc.
* [Terms](https://docs.github.com/en/github/site-policy/github-terms-of-service)
* [Privacy](https://docs.github.com/en/github/site-policy/github-privacy-statement)
* Manage Cookies
* Do not share my personal information

* [LinkedIn icon

  GitHub on LinkedIn](https://www.linkedin.com/company/github)
* [Instagram icon

  GitHub on Instagram](https://www.instagram.com/github/)
* [YouTube icon

  GitHub on YouTube](https://www.youtube.com/github)
* [X icon

  GitHub on X](https://twitter.com/github)
* [TikTok icon

  GitHub on TikTok](https://www.tiktok.com/%40github)
* [Twitch icon

  GitHub on Twitch](https://www.twitch.tv/github)
* [GitHub icon

  GitHub’s organization on GitHub](https://github.com/github)

---
source: hackernews
title: We architected an edge caching layer to eliminate cold starts
url: https://www.mintlify.com/blog/page-speed-improvements
date: 2025-12-16
---

Resources

Explore

[Startups

Built for fast-moving teams](/startups)[Enterprise

Scalable for large organizations](/enterprise)[Switch

Seamless migration tools](/switch)

Company

[Careers

Join our growing team](/careers)[Wall of Love

Customer testimonials](/wall-of-love)[Guides

Guide to technical writing](/guides)

Documentation

Guides

[Getting Started

Deploy in minutes](https://mintlify.com/docs)[Components

Customizable components library](https://mintlify.com/docs/components)

Developers

[API Reference

Build integrations and custom workflows](https://mintlify.com/docs/api-reference)[Changelog

Learn what's new](https://mintlify.com/docs/changelog)

[Customers](/customers)[Blog](/blog)[Pricing](/pricing)

[Contact sales](/contact/sales)[Start for free](https://dashboard.mintlify.com/signup)

* [Resources](/startups)
* [Documentation](https://mintlify.com/docs)
* [Customers](/customers)
* [Blog](/blog)
* [Pricing](/pricing)

[Contact sales](/contact/sales)[Start for free](https://dashboard.mintlify.com/signup)

[All articles](/blog)

Engineering/7 minutes read

# How we eliminated cold starts for 72M monthly page views with edge caching

December 8, 2025

NK

Nick Khami

Engineering

#### Share this article

---

![ How we eliminated cold starts for 72M monthly page views with edge caching](/_next/image?url=%2Fimages%2Ffeatured%2Fpage-speed-improvements.webp&w=1920&q=75)

Mintlify powers documentation for tens of thousands of developer sites, serving 72 million monthly page views. Every pageload matters when millions of developers and AI agents depend on your platform for technical information.

We had a problem. Nearly one in four visitors experienced slow [cold starts](https://aws.amazon.com/blogs/compute/understanding-and-remediating-cold-starts-an-aws-lambda-perspective/) when accessing documentation pages. Our existing Next.js ISR caching solution could not keep up with deployment velocity that kept climbing as our engineering team grew.

We ship code updates multiple times per day and each deployment invalidated the entire cache across all customer sites. This post walks through how we architected a custom edge caching layer to decouple deployments from cache invalidation, bringing our cache hit rate from 76% to effectively 100%.

## [Solution](#solution)

We achieved our goal of fully eliminating cold starts and used a veritable smorgasbord of Cloudflare products to get there.

![Cloudflare Architecture](/images/page-speed-improvements-cf-arch.png)

| Component | Purpose |
| --- | --- |
| Workers | docs-proxy handles requests; revalidation-worker consumes the queue |
| KV | Store deployment configs, version IDs, connected domains |
| Durable Objects | Global singleton coordination for revalidation locks |
| Queues | Async message processing for cache warming |
| CDN Cache | Edge caching with custom cache keys via fetch with cf options |
| Zones/DNS | Route traffic to workers |

We could have built a similar system on any hyperscaler, but leaning on Cloudflare's CDN expertise, especially for configuring tiered cache, was a huge help.

It is important that you understand the difference between two key terms which I use throughout the following solution explanation.

* **Revalidations** are a *reactive* process triggered when we detect a version mismatch at request time (e.g., after we deploy new code)
* **Prewarming** is a *proactive* process triggered when customers update their documentation content, before any user requests it

Both ultimately warm the cache by fetching pages, but they differ in *when* and *why* they're triggered. More on this in sections 2 through 4 below.

### [1. The Proxy Layer](#1-the-proxy-layer)

We placed a Cloudflare Worker in front of all traffic to Mintlify hosted sites. It proxies every request and contains business logic for both updating and using the associated cache.
When a request comes in, the worker proceeds through the following steps.

1. Determines the deployment configuration for the requested host
2. Builds a unique `cache key` based on the path, deployment ID, and request type
3. Leverages Cloudflare's edge cache with a 15-day TTL for successful responses

Our `cache key` structure shown below. The `cachePrefix` roughly maps to the name of a particular customer, `deploymentId` identifies which Vercel deployment to proxy to, `path` is needed to know the correct page to fetch and then `contentType` functions such that we can store both `html` and `rsc` variants for every page.

```
`${cachePrefix}/${deploymentId}/${path}#${kind}:${contentType}`;
```

For example: `acme/dpl_abc123/getting-started:html` and `acme/dpl_abc123/getting-started:rsc`.

### [2. Automatic Version Detection and Revalidation](#2-automatic-version-detection-and-revalidation)

The most innovative aspect of our solution is automatic version mismatch detection.

When we deploy a new version of our Next.js client to production, Vercel sends a `deployment.succeeded` webhook. Our backend receives this and writes the new deployment ID to Cloudflare's KV.

```
KV.put('DEPLOY:{projectId}:id', deploymentId);
```

Then, when user requests come through the docs-proxy worker, it extracts version information from the origin response headers and compares it against the expected version in KV.

```
gotVersion = originResponse.headers['x-version'];
projectId = originResponse.headers['x-vercel-project-id'];

wantVersion = KV.get('DEPLOY:{projectId}:id');

shouldRevalidate = wantVersion != gotVersion;
```

When a version mismatch is detected, the worker automatically triggers revalidation in the background using `ctx.waitUntil()`. The user gets the previously cached stale version immediately. Meanwhile, cache warming of the new version happens asynchronously in the background.

We do not start serving the new version of pages until we have warmed all paths in the sitemap. Since, when you load a new version of any given page after an update, you have to make sure that all subsequent navigations also fetch that same version. If you were on `v2` and then randomly saw `v1` designs when navigating to a new page it would be jarring and worse than them loading slowly.

### [3. The Revalidation Coordinator](#3-the-revalidation-coordinator)

Our first concern when triggering revalidations for sites was that we were going to create a race condition where we had multiple updates in parallel for a given customer and start serving traffic for both new and old versions at the same time.

We decided to use Cloudflare's Durable Objects (`DO`) as a lock around the update process to prevent this. We execute the following steps during every attempted revalidation trigger.

1. Check the `DO` storage for any inflight updates, ignore the trigger if there is one
2. Write to the `DO` storage to track that we are starting an update and "lock"
3. Queue a message containing the `cachePrefix`, `deploymentId`, and host info for the revalidation worker to process
4. Wait for the revalidation worker to report completion, then "unlock" by deleting the `DO` state

We also added a failsafe where we automatically delete the DO's data and unlock in step 1 if it has been held for 30 minutes. We know from our analytics that no update should take that long and it is a safe timeout.

### [4. Revalidation Worker](#4-revalidation-worker)

Cloudflare Queues make it easy to [attach a worker that can consume and process messages](https://developers.cloudflare.com/queues/get-started/#4-create-your-consumer-worker), so we have a dedicated revalidation worker that handles both prewarming (proactive) and version revalidation (reactive). Using a queue to control the rate of cache warming requests was mission critical since without it, we'd cause a thundering herd that takes down our own databases.

Each queue message contains the full context for a deployment: `cachePrefix`, `deploymentId`, and either a list of `paths` or enough info to fetch them from our sitemap API. The worker then warms **all pages for that deployment** before reporting completion.

```
// Get paths from message or fetch from sitemap API
paths = message.paths ?? fetchSitemap(cachePrefix)

// Process in batches of 6 (Cloudflare's concurrent connection limit)
for batch in chunks(paths, 6):
  awaitAll(
    batch.map(path =>
      // Warm both HTML and RSC variants
      for variant in ["html", "rsc"]:
        cacheKey = "{cachePrefix}/{deploymentId}/{path}#{variant}"
        headers = { "X-Cache-Key": cacheKey }
        if variant == "rsc":
          headers["RSC"] = "1"
        fetchWithRetry(originUrl, headers)
    )
  )
```

Once all paths are warmed, the worker reads the current doc version from the coordinator's `DO` storage to ensure we're not overwriting a newer version with an older one. If the version is still valid, it updates the `DEPLOYMENT:{domain}` key in KV for all connected domains and notifies the coordinator that cache warming is complete. The coordinator only unlocks after receiving this completion signal.

### [5. Proactive Prewarming on Content Updates](#5-proactive-prewarming-on-content-updates)

Beyond reactive revalidation, we also proactively prewarm caches when customers update their documentation. After processing a docs update, our backend calls the Cloudflare Worker's admin API to trigger prewarming:

```
POST /admin/prewarm HTTP/1.1
Host: workerUrl
Content-Type: application/json

{
  "paths": ["/docs/intro", "/docs/quickstart", "..."],
  "cachePrefix": "acme/42",
  "deploymentId": "dpl_abc123",
  "isPrewarm": true
}
```

The admin endpoint accepts batch prewarm requests and queues them for processing. It also updates the doc version in the coordinator's `DO` to prevent older versions from overwriting newer cached content.

This two-pronged approach ensures caches stay warm through both:

* reactive **revalidation** system triggered when our code deployments create version mismatches
* proactive **prewarming** triggered when customers update their documentation content

## [Results](#results)

We have successfully moved our cache hit rate to effectively 100% based on monitoring logs from the Cloudflare proxy worker over the past 2 weeks. Our system solves for revalidations due to both documentation content updates and new codebase deployments in the following ways.

### [For code changes affecting sites (revalidation)](#for-code-changes-affecting-sites-revalidation)

1. Vercel webhook notifies our backend of the new deployment
2. Backend writes the new deployment ID to Cloudflare KV
3. The first user request detects the version mismatch
4. Revalidation triggers in the background
5. The coordinator ensures only one cache warming operation runs globally
6. All pages get cached at the edge with the new version

### [For customer docs updates (prewarming)](#for-customer-docs-updates-prewarming)

1. Update workflow completes processing
2. Backend proactively triggers prewarming via admin API
3. All pages are warmed before users even request them

Our system is also self-healing. If a revalidation fails, the next request will trigger it again. If a lock gets stuck, alarms clean it up automatically after 30 minutes. And because we cache at the edge with a 15-day TTL, even if the origin goes down, users still get fast responses from the cache. Improving reliability as well as speed!

## [Word to the Wise](#word-to-the-wise)

If you're running a dynamic site and chasing P99 latency at the origin, consider whether that's actually the right battle. We spent weeks trying to optimize ours (RSCs, multiple databases, signed S3 URLs) and the system was too complicated to debug meaningfully.

The breakthrough came when we stopped trying to make dynamic requests faster and instead made them not happen at all. Push your dynamic site towards being static wherever possible. Cache aggressively, prewarm proactively, and let the edge do what it's good at.

#### More blog posts to read

[![Inside our effort to improve the Mintlify assistant](/_next/image?url=%2Fimages%2Fassistant-improvements.png&w=1080&q=75)

Engineering

### Inside our effort to improve the Mintlify assistant

A data-driven look at improving the assistant, powered by ClickHouse and deeper feedback analysis.

December 12, 2025

PF

Patrick Foster

Software Engineer](/blog/assistant-improvements)[![Introducing the next step towards self-updating docs](/_next/image?url=%2Fimages%2Ffeatured%2Fagent-suggestions.png&w=1080&q=75)

Announcements

### Introducing the next step towards self-updating docs

The agent now monitors your codebase, proactively identifies documentation updates, and surfaces needed suggestions to your team.

December 8, 2025

HW

Han Wang

Co-Founder](/blog/autopilot)

NK

Nick Khami

Engineering

#### Share this article

### explore

* [Startups](/startups)
* [Enterprise](/enterprise)
* [Switch](/switch)
* [OSS Program](/oss-program)

### resources

* [Customers](/customers)
* [Blog](/blog)
* [Pricing](/pricing)
* [Guides](/guides)
* [Feature Requests](https://github.com/orgs/mintlify/discussions/categories/feature-requests)

### documentation

* [Getting Started](/docs)
* [API Reference](/docs/api-reference)
* [Components](/docs/components)
* [Changelog](/docs/changelog)

### company

* [Careers](/careers)
* [Wall of Love](/wall-of-love)

### legal

* [Privacy Policy](/legal/privacy)
* [Responsible Disclosure](/security/responsible-disclosure)
* [Terms Of Service](/legal/terms)
* [Security](https://security.mintlify.com)
* [DSR/DSAR](https://mintlify.typeform.com/to/Bxa77EKc)

[All systems normal](https://status.mintlify.com/)

© 2025 Mintlify, Inc.

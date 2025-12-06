---
source: hackernews
title: Cloudflare outage on December 5, 2025
url: https://blog.cloudflare.com/5-december-2025-outage/
date: 2025-12-06
---

Cloudflare outage on December 5, 2025

[Get Started Free](https://dash.cloudflare.com/sign-up)|[Contact Sales](https://www.cloudflare.com/plans/enterprise/contact/)|

▼

[![The Cloudflare Blog](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/69RwBidpiEHCDZ9rFVVk7T/092507edbed698420b89658e5a6d5105/CF_logo_stacked_blktype.png)](/)

## [The Cloudflare Blog](/)

Subscribe to receive notifications of new posts:

Subscribe

[![magnifier icon](/images/magnifier.svg)](/search/)![hamburger menu](/images/hamburger.svg)

[AI](/tag/ai/)

[Developers](/tag/developers/)

[Radar](/tag/cloudflare-radar/)

[Product News](/tag/product-news/)

[Security](/tag/security/)

[Policy & Legal](/tag/policy/)

[Zero Trust](/tag/zero-trust/)

[Speed & Reliability](/tag/speed-and-reliability/)

[Life at Cloudflare](/tag/life-at-cloudflare/)

[Partners](/tag/partners/)

[AI](/tag/ai/)

[Developers](/tag/developers/)

[Radar](/tag/cloudflare-radar/)

[Product News](/tag/product-news/)

[Security](/tag/security/)

[Policy & Legal](/tag/policy/)

[Zero Trust](/tag/zero-trust/)

[Speed & Reliability](/tag/speed-and-reliability/)

[Life at Cloudflare](/tag/life-at-cloudflare/)

[Partners](/tag/partners/)

[![magnifier icon](/images/magnifier.svg)](/search/)

# Cloudflare outage on December 5, 2025

2025-12-05

* [![Dane Knecht](https://blog.cloudflare.com/cdn-cgi/image/format=auto,dpr=3,width=64,height=64,gravity=face,fit=crop,zoom=0.5/https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2Vb2AhRye3notdj8Q605bw/15216da4779b7ef162916df8ea5ebcf3/Screenshot_2025-09-22_at_1.23.33%C3%A2__PM.png)](/author/dane-knecht/)

  [Dane Knecht](/author/dane-knecht/)

5 min read

This post is also available in [Português](/pt-br/5-december-2025-outage) and [Español (Latinoamérica)](/es-la/5-december-2025-outage).

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5NoiDo8eH6E6bTJYfEqoe9/60cbcc5d7d59e4c68a99a22026d221d8/image2.png)

On December 5, 2025, at 08:47 UTC (all times in this blog are UTC), a portion of Cloudflare’s network began experiencing significant failures. The incident was resolved at 09:12 (~25 minutes total impact), when all services were fully restored.

A subset of customers were impacted, accounting for approximately 28% of all HTTP traffic served by Cloudflare. Several factors needed to combine for an individual customer to be affected as described below.

The issue was not caused, directly or indirectly, by a cyber attack on Cloudflare’s systems or malicious activity of any kind. Instead, it was triggered by changes being made to our body parsing logic while attempting to detect and mitigate an industry-wide vulnerability [disclosed this week](https://blog.cloudflare.com/waf-rules-react-vulnerability/) in React Server Components.

Any outage of our systems is unacceptable, and we know we have let the Internet down again following the incident on November 18. We will be publishing details next week about the work we are doing to stop these types of incidents from occurring.

### What happened

The graph below shows HTTP 500 errors served by our network during the incident timeframe (red line at the bottom), compared to unaffected total Cloudflare traffic (green line at the top).

Cloudflare's Web Application Firewall (WAF) provides customers with protection against malicious payloads, allowing them to be detected and blocked. To do this, Cloudflare’s proxy buffers HTTP request body content in memory for analysis. Before today, the buffer size was set to 128KB.

As part of our ongoing work to protect customers who use React against a critical vulnerability, [CVE-2025-55182](https://nvd.nist.gov/vuln/detail/CVE-2025-55182), we started rolling out an increase to our buffer size to 1MB, the default limit allowed by Next.js applications, to make sure as many customers as possible were protected.

This first change was being rolled out using our gradual deployment system. During rollout, we noticed that our internal WAF testing tool did not support the increased buffer size. As this internal test tool was not needed at that time and had no effect on customer traffic, we made a second change to turn it off.

This second change of turning off our WAF testing tool was implemented using our global configuration system. This system does not perform gradual rollouts, but rather propagates changes within seconds to the entire fleet of servers in our network and is under review [following the outage we experienced on November 18](https://blog.cloudflare.com/18-november-2025-outage/).

Unfortunately, in our FL1 version of our proxy, under certain circumstances, the second change of turning off our WAF rule testing tool caused an error state that resulted in 500 HTTP error codes to be served from our network.

As soon as the change propagated to our network, code execution in our FL1 proxy reached a bug in our rules module which led to the following Lua exception:

```
[lua] Failed to run module rulesets callback late_routing: /usr/local/nginx-fl/lua/modules/init.lua:314: attempt to index field 'execute' (a nil value)
```

resulting in HTTP code 500 errors being issued.

The issue was identified shortly after the change was applied, and was reverted at 09:12, after which all traffic was served correctly.

Customers that have their web assets served by our older FL1 proxy **AND** had the Cloudflare Managed Ruleset deployed were impacted. All requests for websites in this state returned an HTTP 500 error, with the small exception of some test endpoints such as `/cdn-cgi/trace`.

Customers that did not have the configuration above applied were not impacted. Customer traffic served by our China network was also not impacted.

### The runtime error

Cloudflare’s rulesets system consists of sets of rules which are evaluated for each request entering our system. A rule consists of a filter, which selects some traffic, and an action which applies an effect to that traffic. Typical actions are “`block`”, “`log`”, or “`skip`”. Another type of action is “`execute`”, which is used to trigger evaluation of another ruleset.

Our internal logging system uses this feature to evaluate new rules before we make them available to the public. A top level ruleset will execute another ruleset containing test rules. It was these test rules that we were attempting to disable.

We have a killswitch subsystem as part of the rulesets system which is intended to allow a rule which is misbehaving to be disabled quickly. This killswitch system receives information from our global configuration system mentioned in the prior sections. We have used this killswitch system on a number of occasions in the past to mitigate incidents and have a well-defined Standard Operating Procedure, which was followed in this incident.

However, we have never before applied a killswitch to a rule with an action of “`execute`”. When the killswitch was applied, the code correctly skipped the evaluation of the execute action, and didn’t evaluate the sub-ruleset pointed to by it. However, an error was then encountered while processing the overall results of evaluating the ruleset:

```
if rule_result.action == "execute" then
  rule_result.execute.results = ruleset_results[tonumber(rule_result.execute.results_index)]
end
```

This code expects that, if the ruleset has action=”execute”, the “rule\_result.execute” object will exist. However, because the rule had been skipped, the rule\_result.execute object did not exist, and Lua returned an error due to attempting to look up a value in a nil value.

This is a straightforward error in the code, which had existed undetected for many years. This type of code error is prevented by languages with strong type systems. In our replacement for this code in our new FL2 proxy, which is written in Rust, the error did not occur.

### What about the changes being made after the incident on November 18, 2025?

We made an unrelated change that caused a similar, [longer availability incident](https://blog.cloudflare.com/18-november-2025-outage/) two weeks ago on November 18, 2025. In both cases, a deployment to help mitigate a security issue for our customers propagated to our entire network and led to errors for nearly all of our customer base.

We have spoken directly with hundreds of customers following that incident and shared our plans to make changes to prevent single updates from causing widespread impact like this. We believe these changes would have helped prevent the impact of today’s incident but, unfortunately, we have not finished deploying them yet.

We know it is disappointing that this work has not been completed yet. It remains our first priority across the organization. In particular, the projects outlined below should help contain the impact of these kinds of changes:

* **Enhanced Rollouts & Versioning**: Similar to how we slowly deploy software with strict health validation, data used for rapid threat response and general configuration needs to have the same safety and blast mitigation features. This includes health validation and quick rollback capabilities among other things.
* **Streamlined break glass capabilities:** Ensure that critical operations can still be achieved in the face of additional types of failures. This applies to internal services as well as all standard methods of interaction with the Cloudflare control plane used by all Cloudflare customers.
* **"Fail-Open" Error Handling:** As part of the resilience effort, we are replacing the incorrectly applied hard-fail logic across all critical Cloudflare data-plane components. If a configuration file is corrupt or out-of-range (e.g., exceeding feature caps), the system will log the error and default to a known-good state or pass traffic without scoring, rather than dropping requests. Some services will likely give the customer the option to fail open or closed in certain scenarios. This will include drift-prevention capabilities to ensure this is enforced continuously.

Before the end of next week we will publish a detailed breakdown of all the resiliency projects underway, including the ones listed above. While that work is underway, we are locking down all changes to our network in order to ensure we have better mitigation and rollback systems before we begin again.

These kinds of incidents, and how closely they are clustered together, are not acceptable for a network like ours. On behalf of the team at Cloudflare we want to apologize for the impact and pain this has caused again to our customers and the Internet as a whole.

### Timeline

|  |  |  |
| --- | --- | --- |
| Time (UTC) | Status | Description |
| 08:47 | INCIDENT start | Configuration change deployed and propagated to the network |
| 08:48 | Full impact | Change fully propagated |
| 08:50 | INCIDENT declared | Automated alerts |
| 09:11 | Change reverted | Configuration change reverted and propagation start |
| 09:12 | INCIDENT end | Revert fully propagated, all traffic restored |

Cloudflare's connectivity cloud protects [entire corporate networks](https://www.cloudflare.com/network-services/), helps customers build [Internet-scale applications efficiently](https://workers.cloudflare.com/), accelerates any [website or Internet application](https://www.cloudflare.com/performance/accelerate-internet-applications/), [wards off DDoS attacks](https://www.cloudflare.com/ddos/), keeps [hackers at bay](https://www.cloudflare.com/application-security/), and can help you on [your journey to Zero Trust](https://www.cloudflare.com/products/zero-trust/).

Visit [1.1.1.1](https://one.one.one.one/) from any device to get started with our free app that makes your Internet faster and safer.

To learn more about our mission to help build a better Internet, [start here](https://www.cloudflare.com/learning/what-is-cloudflare/). If you're looking for a new career direction, check out [our open positions](https://www.cloudflare.com/careers).

  [Outage](/tag/outage/)[Post Mortem](/tag/post-mortem/)

Follow on X

Dane Knecht|[@dok2001](https://x.com/%40dok2001)

Cloudflare|[@cloudflare](https://x.com/%40cloudflare)

Related posts

November 18, 2025 12:00 AM

[## Cloudflare outage on November 18, 2025](/18-november-2025-outage/)

Cloudflare suffered a service outage on November 18, 2025. The outage was triggered by a bug in generation logic for a Bot Management feature file causing many Cloudflare services to be affected.
...

By * [Matthew Prince](/author/matthew-prince/)

[Outage,](/tag/outage/) [Post Mortem,](/tag/post-mortem/) [Bot Management](/tag/bot-management/)

November 18, 2025 12:00 AM

[## Cloudflare outage on November 18, 2025](/18-november-2025-outage-uk-ua/)

Cloudflare suffered a service outage on November 18, 2025. The outage was triggered by a bug in generation logic for a Bot Management feature file causing many Cloudflare services to be affected.
...

By * [Matthew Prince](/author/matthew-prince/)

[Outage,](/tag/outage/) [Post Mortem,](/tag/post-mortem/) [Bot Management](/tag/bot-management/)

October 28, 2025 12:00 PM

[## Online outages: Q3 2025 Internet disruption summary](/q3-2025-internet-disruption-summary/)

In Q3 2025, we observed Internet disruptions around the world resulting from government directed shutdowns, power outages, cable cuts, a cyberattack, an earthquake, a fire, and technical problems, as well as several with unexplained causes....

By * [David Belson](/author/david-belson/)

[Radar,](/tag/cloudflare-radar/) [Internet Shutdown,](/tag/internet-shutdown/) [Internet Traffic,](/tag/internet-traffic/) [Outage,](/tag/outage/) [Consumer Services,](/tag/consumer-services/) [Internet Trends](/tag/internet-trends/)

September 30, 2025 10:05 AM

[## Nationwide Internet shutdown in Afghanistan extends localized disruptions](/nationwide-internet-shutdown-in-afghanistan/)

On September 29, 2025, Internet connectivity was completely shut down across Afghanistan, impacting business, education, finance, and government services....

By * [David Belson](/author/david-belson/)

[Radar,](/tag/cloudflare-radar/) [Internet Shutdown,](/tag/internet-shutdown/) [Internet Traffic,](/tag/internet-traffic/) [Outage](/tag/outage/)

* Getting Started
* [Free plans](https://www.cloudflare.com/plans/free/)
* [For enterprises](https://www.cloudflare.com/enterprise/)
* [Compare plans](https://www.cloudflare.com/plans/)
* [Get a recommendation](https://www.cloudflare.com/about-your-website/)
* [Request a demo](https://www.cloudflare.com/plans/enterprise/demo/)
* [Contact Sales](https://www.cloudflare.com/plans/enterprise/contact/)

* Resources
* [Learning Center](https://www.cloudflare.com/learning/)
* [Analyst reports](https://www.cloudflare.com/analysts/)
* [Cloudflare Radar](https://radar.cloudflare.com/)
* [Cloudflare TV](https://cloudflare.tv/)
* [Case Studies](https://www.cloudflare.com/case-studies/)
* [Webinars](https://www.cloudflare.com/resource-hub/?resourcetype=Webinar)
* [White Papers](https://www.cloudflare.com/resource-hub/?resourcetype=Whitepaper)
* [Developer docs](https://developers.cloudflare.com)
* [theNet](https://www.cloudflare.com/the-net/)

* Solutions
* [Connectivity cloud](https://www.cloudflare.com/connectivity-cloud/)
* [SSE and SASE services](https://www.cloudflare.com/zero-trust/)
* [Application services](https://www.cloudflare.com/application-services/)
* [Network services](https://www.cloudflare.com/network-services/)
* [Developer services](https://www.cloudflare.com/developer-platform/)

* Community
* [Community Hub](https://community.cloudflare.com)
* [Project Galileo](https://www.cloudflare.com/galileo/)
* [Athenian Project](https://www.cloudflare.com/athenian/)
* [Cloudflare for Campaigns](https://www.cloudflare.com/campaigns/)
* [Critical Infrastructure Defense Project](https://www.cloudflare.com/partners/technology-partners/cidp/)
* [Connect 2024](https://www.cloudflare.com/connect2024/)

* Support
* [Help center](https://support.cloudflare.com)
* [Cloudflare Status](https://www.cloudflarestatus.com)
* [Compliance](https://www.cloudflare.com/compliance/)
* [GDPR](https://www.cloudflare.com/gdpr/introduction/)
* [Trust & Safety](https://www.cloudflare.com/trust-hub/abuse-approach/)

* Company
* [About Cloudflare](https://www.cloudflare.com/about-overview/)
* [Our team](https://www.cloudflare.com/people/)
* [Investor relations](https://cloudflare.net/)
* [Press](https://www.cloudflare.com/press/)
* [Careers](https://www.cloudflare.com/careers/)
* [Diversity, equity & inclusion](https://www.cloudflare.com/diversity-equity-and-inclusion/)
* [Impact/ESG](https://www.cloudflare.com/impact/)
* [Network Map](https://www.cloudflare.com/network/)
* [Logos & press kit](https://www.cloudflare.com/press-kit/)
* [Become a partner](https://www.cloudflare.com/partners/)

[![facebook](https://www.cloudflare.com/img/footer/facebook.svg)](https://www.facebook.com/Cloudflare/)[![X](https://www.cloudflare.com/img/footer/twitter.svg)](https://x.com/Cloudflare)[![linkedin](https://www.cloudflare.com/img/footer/linkedin.svg)](https://www.linkedin.com/company/cloudflare)[![youtube](https://www.cloudflare.com/img/footer/youtube.svg)](https://www.youtube.com/cloudflare)[![instagram](https://www.cloudflare.com/img/footer/instagram.svg)](https://www.instagram.com/cloudflare)

© 2025 Cloudflare, Inc. | [Privacy Policy](https://www.cloudflare.com/privacypolicy/) | [Terms of Use](https://www.cloudflare.com/website-terms/) | [Report Security Issues](https://www.cloudflare.com/disclosure/) |![Privacy Options](/images/privacy-options.svg)[Cookie Preferences](#cookie-settings) | [Trademark](https://www.cloudflare.com/trademark/)

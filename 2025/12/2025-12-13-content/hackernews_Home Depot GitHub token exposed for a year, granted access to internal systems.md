---
source: hackernews
title: Home Depot GitHub token exposed for a year, granted access to internal systems
url: https://techcrunch.com/2025/12/12/home-depot-exposed-access-to-internal-systems-for-a-year-says-researcher/
date: 2025-12-13
---

[![](https://techcrunch.com/wp-content/uploads/2024/09/tc-lockup.svg) TechCrunch Desktop Logo](https://techcrunch.com)

[![](https://techcrunch.com/wp-content/uploads/2024/09/tc-logo-mobile.svg) TechCrunch Mobile Logo](https://techcrunch.com)

* [Latest](/latest/)
* [Startups](/category/startups/)
* [Venture](/category/venture/)
* [Apple](/tag/apple/)
* [Security](/category/security/)
* [AI](/category/artificial-intelligence/)
* [Apps](/category/apps/)

* [Events](/events/)
* [Podcasts](/podcasts/)
* [Newsletters](/newsletters/)

Search

Submit

Site Search Toggle

Mega Menu Toggle

### Topics

[Latest](/latest/)

[AI](/category/artificial-intelligence/)

[Amazon](/tag/amazon/)

[Apps](/category/apps/)

[Biotech & Health](/category/biotech-health/)

[Climate](/category/climate/)

[Cloud Computing](/tag/cloud-computing/)

[Commerce](/category/commerce/)

[Crypto](/category/cryptocurrency/)

[Enterprise](/category/enterprise/)

[EVs](/tag/evs/)

[Fintech](/category/fintech/)

[Fundraising](/category/fundraising/)

[Gadgets](/category/gadgets/)

[Gaming](/category/gaming/)

[Google](/tag/google/)

[Government & Policy](/category/government-policy/)

[Hardware](/category/hardware/)

[Instagram](/tag/instagram/)

[Layoffs](/tag/layoffs/)

[Media & Entertainment](/category/media-entertainment/)

[Meta](/tag/meta/)

[Microsoft](/tag/microsoft/)

[Privacy](/category/privacy/)

[Robotics](/category/robotics/)

[Security](/category/security/)

[Social](/category/social/)

[Space](/category/space/)

[Startups](/category/startups/)

[TikTok](/tag/tiktok/)

[Transportation](/category/transportation/)

[Venture](/category/venture/)

### More from TechCrunch

[Staff](/about-techcrunch/)

[Events](/events/)

[Startup Battlefield](/startup-battlefield/)

[StrictlyVC](https://strictlyvc.com/)

[Newsletters](/newsletters/)

[Podcasts](/podcasts/)

[Videos](/video/)

[Partner Content](/sponsored/)

[TechCrunch Brand Studio](/brand-studio/)

[Crunchboard](https://www.crunchboard.com/)

[Contact Us](/contact-us/)

![The Home Depot logo is displayed outside the home improvement retail store in Los Angeles, California, on February 21, 2025.](https://techcrunch.com/wp-content/uploads/2025/12/home-depot-2200450751.jpg?w=1024)

**Image Credits:**PATRICK T. FALLON / AFP / Getty Images

[Security](https://techcrunch.com/category/security/)

# Home Depot exposed access to internal systems for a year, says researcher

[Zack Whittaker](https://techcrunch.com/author/zack-whittaker/)

8:42 AM PST · December 12, 2025

A security researcher said Home Depot exposed access to its internal systems for a year after one of its employees published a private access token online, likely by mistake. The researcher found the exposed token and tried to privately alert Home Depot to its security lapse but was ignored for several weeks.

The exposure is now fixed after TechCrunch contacted company representatives last week.

Security researcher [Ben Zimmermann](https://benzimmermann.dev/) told TechCrunch that, in early November, he found a published GitHub access token belonging to a Home Depot employee, which was exposed sometime in early 2024.

When he tested the token, Zimmermann said that it granted access to hundreds of private Home Depot source code repositories hosted on GitHub and allowed the ability to modify their contents.

The researcher said the keys allowed access to Home Depot’s cloud infrastructure, including its order fulfillment and inventory management systems, and code development pipelines, among other systems. Home Depot has hosted much of its developer and engineering infrastructure on GitHub since 2015, according to a [customer profile on GitHub’s website](https://github.com/customer-stories/homedepot).

Zimmermann said he sent several emails to Home Depot but didn’t hear back.

Nor did he get a response from Home Depot’s chief information security officer, Chris Lanzilotta, after sending a message over LinkedIn.

Zimmermann told TechCrunch that he has disclosed several similar exposures in recent months to companies, which have thanked him for his findings.

“Home Depot is the only company that ignored me,” he said.

Given that Home Depot does not have a way to report security flaws, such as a vulnerability disclosure or bug bounty program, Zimmermann contacted TechCrunch in an effort to get the exposure fixed.

When reached by TechCrunch on December 5, Home Depot spokesperson George Lane acknowledged receipt of our email but did not respond to follow-up emails asking for comment. The exposed token is no longer online, and the researcher said the token’s access was revoked soon after our outreach.

We also asked Lane if Home Depot has the technical means, such as logs, to determine if anyone else used the token during the months it was left online to access any of Home Depot’s internal systems. We did not hear back.

Topics

[cybersecurity](https://techcrunch.com/tag/cybersecurity/), [data breach](https://techcrunch.com/tag/data-breach/), [Exclusive](https://techcrunch.com/tag/exclusive/), [GitHub](https://techcrunch.com/tag/github/), [home depot](https://techcrunch.com/tag/home-depot/), [Security](https://techcrunch.com/category/security/)

![Zack Whittaker](https://techcrunch.com/wp-content/uploads/2024/10/whittaker_headshot_disrupt2024.jpg?w=150)

Zack Whittaker

Security Editor

Zack Whittaker is the security editor at TechCrunch. He also authors the weekly cybersecurity newsletter, [this week in security](https://this.weekinsecurity.com/).

He can be reached via encrypted message at zackwhittaker.1337 on Signal. You can also contact him by email, or to verify outreach, at zack.whittaker@techcrunch.com.

[View Bio](https://techcrunch.com/author/zack-whittaker/)

![Event Logo](https://techcrunch.com/wp-content/uploads/2025/12/strictlyvc-wordmark-orange_horizontal_cropped.png)

Dates TBD

Locations TBA

Plan ahead for the 2026 StrictlyVC events. Hear straight-from-the-source candid insights in on-stage fireside sessions and meet the builders and backers shaping the industry. Join the waitlist to get first access to the lowest-priced tickets and important updates.

[Waitlist Now](https://techcrunch.com/events/strictlyvc-2026-events/)

## Most Popular

* ### [Google launched its deepest AI research agent yet — on the same day OpenAI dropped GPT-5.2](https://techcrunch.com/2025/12/11/google-launched-its-deepest-ai-research-agent-yet-on-the-same-day-openai-dropped-gpt-5-2/)

  + [Julie Bort](https://techcrunch.com/author/julie-bort/)
* ### [Disney hits Google with cease-and-desist claiming ‘massive’ copyright infringement](https://techcrunch.com/2025/12/11/disney-hits-google-with-cease-and-desist-claiming-massive-copyright-infringement/)

  + [Aisha Malik](https://techcrunch.com/author/aisha-malik/)
* ### [OpenAI fires back at Google with GPT-5.2 after ‘code red’ memo](https://techcrunch.com/2025/12/11/openai-fires-back-at-google-with-gpt-5-2-after-code-red-memo/)

  + [Rebecca Bellan](https://techcrunch.com/author/rebecca-bellan/)
* ### [Google debuts ‘Disco,’ a Gemini-powered tool for making web apps from browser tabs](https://techcrunch.com/2025/12/11/google-debuts-disco-a-gemini-powered-tool-for-making-web-apps-from-browser-tabs/)

  + [Sarah Perez](https://techcrunch.com/author/sarah-perez/)
* ### [Marco Rubio bans Calibri font at State Department for being too DEI](https://techcrunch.com/2025/12/10/marco-rubio-bans-calibri-font-at-state-department-for-being-too-dei/)

  + [Julie Bort](https://techcrunch.com/author/julie-bort/)
* ### [Claude Code is coming to Slack, and that’s a bigger deal than it sounds](https://techcrunch.com/2025/12/08/claude-code-is-coming-to-slack-and-thats-a-bigger-deal-than-it-sounds/)

  + [Rebecca Bellan](https://techcrunch.com/author/rebecca-bellan/)
* ### [Creator IShowSpeed sued for allegedly punching, choking viral humanoid Rizzbot](https://techcrunch.com/2025/12/06/creator-ishowspeed-sued-for-allegedly-punching-choking-viral-humanoid-rizzbot/)

  + [Dominic-Madori Davis](https://techcrunch.com/author/dominic-madori-davis/)

Loading the next article

Error loading the next article

[![TechCrunch Logo](https://techcrunch.com/wp-content/themes/tc-24/dist/svg/tc-logo.svg)](https://techcrunch.com)

* [X](https://twitter.com/techcrunch)
* [LinkedIn](https://www.linkedin.com/company/techcrunch)
* [Facebook](https://www.facebook.com/techcrunch)
* [Instagram](https://instagram.com/techcrunch)
* [youTube](https://www.youtube.com/user/techcrunch)
* [Mastodon](https://mstdn.social/%40TechCrunch)
* [Threads](https://www.threads.net/%40techcrunch)
* [Bluesky](https://bsky.app/profile/techcrunch.com)

* [TechCrunch](https://techcrunch.com/)
* [Staff](https://techcrunch.com/about-techcrunch/)
* [Contact Us](https://techcrunch.com/contact-us/)
* [Advertise](https://techcrunch.com/advertise/)
* [Crunchboard Jobs](https://www.crunchboard.com/)
* [Site Map](https://techcrunch.com/site-map/)

* [Terms of Service](https://techcrunch.com/terms-of-service/)
* [Privacy Policy](https://techcrunch.com/privacy-policy/)
* [RSS Terms of Use](https://techcrunch.com/rss-terms-of-use/)
* [Code of Conduct](https://techcrunch.com/code-of-conduct/)

* [AWS re:Invent](https://techcrunch.com/2025/12/01/aws-reinvent-2025-how-to-watch-and-follow-along-live/)
* [Apple AI](https://techcrunch.com/2025/12/01/apple-just-named-a-new-ai-chief-with-google-and-microsoft-expertise-as-john-giannandrea-steps-down/)
* [Zillow](https://techcrunch.com/2025/12/01/zillow-drops-climate-risk-scores-after-agents-complained-of-lost-sales/)
* [Clipbook](https://techcrunch.com/2025/12/01/how-ai-pr-startup-clipbook-won-mark-cubans-investment-from-a-cold-email/)
* [Nvidia](https://techcrunch.com/2025/12/01/nvidia-announces-new-open-ai-models-and-tools-for-autonomous-driving-research/)
* [Tech Layoffs](https://techcrunch.com/2025/02/28/tech-layoffs-2024-list/)
* [ChatGPT](https://techcrunch.com/2025/01/28/chatgpt-everything-to-know-about-the-ai-chatbot/)

© 2025 TechCrunch Media LLC.

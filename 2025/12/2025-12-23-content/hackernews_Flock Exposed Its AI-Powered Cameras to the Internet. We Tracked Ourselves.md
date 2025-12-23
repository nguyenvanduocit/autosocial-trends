---
source: hackernews
title: Flock Exposed Its AI-Powered Cameras to the Internet. We Tracked Ourselves
url: https://www.404media.co/flock-exposed-its-ai-powered-cameras-to-the-internet-we-tracked-ourselves/
date: 2025-12-23
---

###### Account

* [Log in](/signin/)
* [Subscribe](/signup/)

###### Navigation

* [Home](https://www.404media.co)

* [About](https://www.404media.co/about/)
* [RSS](https://www.404media.co/404-media-now-has-a-full-text-rss-feed/)
* [Support/FAQ](https://www.404media.co/faq/)
* [Podcast](https://www.404media.co/the-404-media-podcast/)
* [FOIA Forum Archive](https://www.404media.co/foia-forum-archive/)
* [Merch](https://404media.myshopify.com/)
* [Advertise](https://www.404media.co/advertise-with-404-media/)
* [Thanks](https://www.404media.co/thank-yous/)
* [Privacy](https://www.404media.co/privacy-policy/)

###### Follow us

[Twitter](https://x.com/404mediaco "Twitter")
[Bluesky](https://bsky.app/profile/404media.co "Bluesky")
[Mastodon](https://mastodon.social/%40404mediaco "Mastodon")
[Instagram](https://instagram.com/404mediaco "Instagram")
[TikTok](https://tiktok.com/%40404.media "TikTok")
[Facebook](https://www.facebook.com/404mediaco "Facebook")
[RSS](/rss "RSS")

[Sign in](/signin/)
[Subscribe](/signup/)

* [About](https://www.404media.co/about/)
* [RSS](https://www.404media.co/404-media-now-has-a-full-text-rss-feed/)
* [Support/FAQ](https://www.404media.co/faq/)
* [Podcast](https://www.404media.co/the-404-media-podcast/)
* [FOIA Forum Archive](https://www.404media.co/foia-forum-archive/)
* [Merch](https://404media.myshopify.com/)
* [Advertise](https://www.404media.co/advertise-with-404-media/)
* [Thanks](https://www.404media.co/thank-yous/)
* [Privacy](https://www.404media.co/privacy-policy/)

Advertisement

•

[Go ad free](#/portal/signup)

[Flock](/tag/flock/ "Flock")

# Flock Exposed Its AI-Powered Cameras to the Internet. We Tracked Ourselves

[![Jason Koebler](/content/images/size/w30/2023/08/404-jason-01-copy.jpeg)
Jason Koebler](/author/jason-koebler/ "Jason Koebler")

·

Dec 22, 2025
at 11:05 AM

Flock left at least 60 of its people-tracking Condor PTZ cameras live streaming and exposed to the open internet.

![Flock Exposed Its AI-Powered Cameras to the Internet. We Tracked Ourselves](https://www.404media.co/assets/images/img-placeholder-md.jpg?v=7e152b2c59)

I am standing on the corner of Harris Road and Young Street outside of the Crossroads Business Park in Bakersfield, California, looking up at a Flock surveillance camera bolted high above a traffic signal. On my phone, I am watching myself in real time as the camera records and livestreams me—without any password or login—to the open internet. I wander into the intersection, stare at the camera and wave. On the livestream, I can see myself clearly. Hundreds of miles away, my colleagues are remotely watching me too through the exposed feed.

Flock left livestreams and administrator control panels for at least 60 of its AI-enabled Condor cameras around the country exposed to the open internet, where anyone could watch them, download 30 days worth of video archive, and change settings, see log files, and run diagnostics.

![](https://www.404media.co/content/images/2025/12/CleanShot-2025-12-22-at-07.28.53@2x.png)

Unlike many of Flock’s cameras, which are designed to capture license plates as people drive by, Flock’s Condor cameras are pan-tilt-zoom (PTZ) cameras designed to record and track people, not vehicles. Condor cameras can be set to automatically zoom in on people’s faces as they walk through a parking lot, down a public street, or play on a playground, or they can be controlled manually, according to marketing material on Flock’s website. We watched Condor cameras zoom in on a woman walking her dog on a bike path in suburban Atlanta; a camera followed a man walking through a Macy’s parking lot in Bakersfield; surveil children swinging on a swingset at a playground; and film high-res video of people sitting at a stoplight in traffic. In one case, we were able to watch a man rollerblade down Brookhaven, Georgia’s Peachtree Creek Greenway bike path. The Flock camera zoomed in on him and tracked him as he rolled past. Minutes later, he showed up on another exposed camera livestream further down the bike path. The camera’s resolution was good enough that we were able to see that, when he stopped beneath one of the cameras, he was watching rollerblading videos on his phone.

[![](https://img.spacergif.org/v1/1288x720/0a/spacer.png)](https://www.404media.co/content/media/2025/12/1222--1-.mp4)

0:00

/0:16

1×

The exposure was initially discovered by YouTuber and technologist Benn Jordan and was shared with security researcher Jon “GainSec” Gaines, who [recently found numerous vulnerabilities](https://www.youtube.com/watch?v=uB0gr7Fh6lY&t=1387s&ref=404media.co) in several other models of Flock’s automated license plate reader (ALPR) cameras. They shared the details of what they found  with me, and I verified many of the details seen in the exposed portals by driving to Bakersfield to walk in front of two cameras there while I watched myself on the livestream. I also pulled Flock’s contracts with cities for Condor cameras, pulled details from company presentations about the technology, and geolocated a handful of the cameras to cities and towns across the United States. Jordan also filmed himself in front of several of the cameras on the Peachtree Creek Greenway bike path. Jordan said he and Gaines discovered many of the exposed cameras with Shodan, an internet of things search engine that researchers regularly use to identify improperly secured devices.

After finding links to the feed, “immediately, we were just without any username, without any password, we were just seeing everything from playgrounds to parking lots with people, Christmas shopping and unloading their stuff into cars,” Jordan told me in an interview. “I think it was like the first time that I actually got like immediately scared … I think the one that affected me most was as playground. You could see unattended kids, and that’s something I want people to know about so they can understand how dangerous this is.” In a YouTube video about his research, Jordan said he was able to use footage pulled from the exposed feed to identify specific people using open source investigation tools in order to show how trivially an exposure like this could be abused.

## This post is for paid members only

Become a paid member for unlimited ad-free access to articles, bonus podcast content, and more.

[Subscribe](/membership/)

## Sign up for free access to this post

Free members get access to posts like this one along with an email round-up of our week's stories.

[Subscribe](/signup/)

Already have an account? [Sign in](/signin/)

###### More like this

[![Flock Uses Overseas Gig Workers to Build its Surveillance AI](https://www.404media.co/assets/images/img-placeholder-md.jpg?v=7e152b2c59)](/flock-uses-overseas-gig-workers-to-build-its-surveillance-ai/ "Flock Uses Overseas Gig Workers to Build its Surveillance AI")

[Flock Uses Overseas Gig Workers to Build its Surveillance AI](/flock-uses-overseas-gig-workers-to-build-its-surveillance-ai/ "Flock Uses Overseas Gig Workers to Build its Surveillance AI")

Flock accidentally exposed training materials and a panel which tracked what its AI annotators were working on. It showed that Flock, which has cameras in thousands of U.S. communities, is using workers in the Philippines to review and classify footage.

[![Joseph Cox](/content/images/size/w30/2023/08/404-joseph-01-1.jpg)
Joseph Cox](/author/joseph-cox/ "Joseph Cox")

·

Dec 1, 2025

[![Cops Used Flock to Monitor No Kings Protests Around the Country](https://www.404media.co/assets/images/img-placeholder-md.jpg?v=7e152b2c59)](/cops-used-flock-to-monitor-no-kings-protests-around-the-country/ "Cops Used Flock to Monitor No Kings Protests Around the Country")

[Cops Used Flock to Monitor No Kings Protests Around the Country](/cops-used-flock-to-monitor-no-kings-protests-around-the-country/ "Cops Used Flock to Monitor No Kings Protests Around the Country")

A massive cache of Flock lookups collated by the Electronic Frontier Foundation (EFF) shows as many as 50 federal, state, and local agencies used Flock during protests over the last year.

[![Joseph Cox](/content/images/size/w30/2023/08/404-joseph-01-1.jpg)
Joseph Cox](/author/joseph-cox/ "Joseph Cox")

·

Nov 20, 2025

[![ACLU and EFF Sue a City Blanketed With Flock Surveillance Cameras](https://www.404media.co/assets/images/img-placeholder-md.jpg?v=7e152b2c59)](/aclu-and-eff-sue-a-city-blanketed-with-flock-surveillance-cameras/ "ACLU and EFF Sue a City Blanketed With Flock Surveillance Cameras")

[ACLU and EFF Sue a City Blanketed With Flock Surveillance Cameras](/aclu-and-eff-sue-a-city-blanketed-with-flock-surveillance-cameras/ "ACLU and EFF Sue a City Blanketed With Flock Surveillance Cameras")

“Most drivers are unaware that San Jose’s Police Department is tracking their locations and do not know all that their saved location data can reveal about their private lives and activities."

[![Jason Koebler](/content/images/size/w30/2023/08/404-jason-01-copy.jpeg)
Jason Koebler](/author/jason-koebler/ "Jason Koebler")

·

Nov 18, 2025

[![Archivists Posted the 60 Minutes CECOT Segment Bari Weiss Killed](https://www.404media.co/assets/images/img-placeholder-md.jpg?v=7e152b2c59)](/archivists-posted-the-60-minutes-cecot-segment-bari-weiss-killed/ "Archivists Posted the 60 Minutes CECOT Segment Bari Weiss Killed")

[Archivists Posted the 60 Minutes CECOT Segment Bari Weiss Killed](/archivists-posted-the-60-minutes-cecot-segment-bari-weiss-killed/ "Archivists Posted the 60 Minutes CECOT Segment Bari Weiss Killed")

iCloud, Mega, and as a torrent. Archivists have uploaded the 60 Minutes episode Bari Weiss spiked.

[![Joseph Cox](/content/images/size/w30/2023/08/404-joseph-01-1.jpg)
Joseph Cox](/author/joseph-cox/ "Joseph Cox")

·

Dec 22, 2025

[![Podcast: Marisa Kabas on Landing Big Scoops as an Independent Journalist](https://www.404media.co/assets/images/img-placeholder-md.jpg?v=7e152b2c59)](/podcast-landing-big-scoops-as-an-with-marisa-kabas/ "Podcast: Marisa Kabas on Landing Big Scoops as an Independent Journalist")

[Podcast: Marisa Kabas on Landing Big Scoops as an Independent Journalist](/podcast-landing-big-scoops-as-an-with-marisa-kabas/ "Podcast: Marisa Kabas on Landing Big Scoops as an Independent Journalist")

Marisa Kabas of The Handbasket joins the pod to talk about indie journalism, the industry, and what's going on in the federal government

[![Jason Koebler](/content/images/size/w30/2023/08/404-jason-01-copy.jpeg)
Jason Koebler](/author/jason-koebler/ "Jason Koebler")

·

Dec 22, 2025

[![Scientists Discover ‘Black Widow’ Exoplanet That Defies Explanation](https://www.404media.co/assets/images/img-placeholder-md.jpg?v=7e152b2c59)](/scientists-discover-black-widow-exoplanet-that-defies-explanation/ "Scientists Discover ‘Black Widow’ Exoplanet That Defies Explanation")

[Scientists Discover ‘Black Widow’ Exoplanet That Defies Explanation](/scientists-discover-black-widow-exoplanet-that-defies-explanation/ "Scientists Discover ‘Black Widow’ Exoplanet That Defies Explanation")

An exoplanet located 750 light years from Earth has an atmosphere unlike anything previously known.

[![Becky Ferreira](/content/images/size/w30/2024/09/7BaR_tNV_400x400-1.jpg)
Becky Ferreira](/author/becky/ "Becky Ferreira")

·

Dec 20, 2025

Advertisement

•

[Go ad free](#/portal/signup)

Advertisement

•

[Go ad free](#/portal/signup)

•

Hide

### Unparalleled access to hidden worlds both online and IRL.

404 Media is an independent media company founded by technology journalists Jason Koebler, Emanuel Maiberg, Samantha Cole, and Joseph Cox.

* [About](https://www.404media.co/about/)
* [RSS](https://www.404media.co/404-media-now-has-a-full-text-rss-feed/)
* [Support/FAQ](https://www.404media.co/faq/)
* [Podcast](https://www.404media.co/the-404-media-podcast/)
* [FOIA Forum Archive](https://www.404media.co/foia-forum-archive/)
* [Merch](https://404media.myshopify.com/)
* [Advertise](https://www.404media.co/advertise-with-404-media/)
* [Thanks](https://www.404media.co/thank-yous/)
* [Privacy](https://www.404media.co/privacy-policy/)

[Twitter](https://x.com/404mediaco "Twitter")
[Bluesky](https://bsky.app/profile/404media.co "Bluesky")
[Mastodon](https://mastodon.social/%40404mediaco "Mastodon")
[Instagram](https://instagram.com/404mediaco "Instagram")
[TikTok](https://tiktok.com/%40404.media "TikTok")
[Facebook](https://www.facebook.com/404mediaco "Facebook")
[RSS](/rss "RSS")

Join the newsletter to get the latest updates.

© 2025 404 Media. Published with [Ghost](https://ghost.org).

---
source: hackernews
title: Mapping Amazing: Bee Maps
url: https://maphappenings.com/2025/11/06/bee-maps/
date: 2025-12-07
---

* [Subscribe](https://maphappenings.com/subscribe/)
* [About](https://maphappenings.com/about/)
* [Contact](https://maphappenings.com/about-2/)
* [Archive](https://maphappenings.com/archive/)

[![Map Happenings](https://maphappenings.com/wp-content/uploads/2022/07/cropped-mh-logo-3.png)](https://maphappenings.com/)

# [Map Happenings](https://maphappenings.com)

Mapping Industry Tidbits, Activity and Musings

# Mapping Amazing: Bee Maps

November 6, 2025

As many of you may know, my career started back in 1985 at a wee company called Etak. This startup, seed funded by Nolan Bushnell, [was most famous for pioneering in-vehicle navigation systems](https://maphappenings.com/2024/04/11/story-of-etak/). It was about 20 years ahead of its time.

But it didn’t stop there. Navigation systems require digital maps. The problem was that there weren’t any available at that time, so Etak had to invent a system to manufacture them at scale. And yours truly was privileged to be part of the team that designed and built the production line.

It wasn’t straightforward. Using a VAX minicomputer with 2MB RAM the team designed a system that was used to scan photographic images of topographic maps. These were then manually digitised on cobbled together PC-clones that used very expensive state-of-the-art graphics cards.

![USGS 1:24,000 Quadrangle Map for San Francisco. Credit: USGS](https://maphappenings.com/wp-content/uploads/2025/11/sf-usgs-quad.webp?w=842)

\Topographic map used as source for digital map making. Credit: USGS

Despite the herculean efforts of everybody involved there was a little problem. The accuracy of the maps depended on the source material. In our case we relied on topographic maps published by national mapping agencies. Alas, these maps were often years out of date.

Don’t forget this was 1985. Aerial imagery and satellite imagery just wasn’t readily available back then. We could (and did) contact local agencies for more up-to-date maps of critical intersections, but this was really hard to do at scale. It was just too time consuming and too expensive to track all the material down.

The original [Etak Navigator](https://maphappenings.com/wp-content/uploads/2024/04/etak-navigator-jpg.010.jpeg) didn’t provide turn-by-turn directions. Instead it guided you to your location by a flashing star on the map. You zoomed the map as you drove and had to use your own noggin to figure out which roads to take to get there. As a result the requirements for the digital map were incredibly light: we didn’t need to collect information on one way systems or turn restrictions. We just collected the streets, the street names and the addresses and we made sure we got the road topology right.

It was only later when the enterprise customers like Bosch and GM wanted real-time guidance that we had to collect more information like one ways and turn restrictions. And it was *so* painful!

Fast forward a decade to 1995 and satellite imagery and low altitude aerial photography became more readily available, so the job of collecting the basic street network got easier. But the road attributes were still hard. Digital mapping companies had to send out crews in cars, with one person driving and another person collecting data using pen, paper and a clipboard. Ouch!

It was natural to start looking at technology to collect this information automatically. Indeed in the 1990s mapping companies started mounting cameras on vehicles. But it wasn’t until 2007 when Google launched Street View that it became common enough that the general public started to notice.

People often got excited to see a Google Maps car on their street — as well cars from rival fleets such as TomTom, HERE and later Apple Maps.

![Google Maps Car](https://maphappenings.com/wp-content/uploads/2025/11/google-maps-car.webp?w=1024)

Google Maps Car

![HERE Maps Car](https://maphappenings.com/wp-content/uploads/2025/11/here-maps-car.avif?w=1024)

HERE Maps Car

![Apple Maps Car](https://maphappenings.com/wp-content/uploads/2025/11/apple-maps-car.jpg?w=1024)

Apple Maps Car

![TomTom Maps Car](https://maphappenings.com/wp-content/uploads/2025/11/tomtom-maps-car.avif?w=1024)

TomTom Maps Car

These fleets, each in their hundreds, drive around collecting all manner of data, not just the photographic images, but also a ton of information deduced from the images: for example street names, one ways, turn-restrictions, lane information, stop signs, traffic lights, speed limits to name just a few.

But there’s a problem.

Running a fleet of 100+ dedicated vehicles solely for map data collection is expensive.

And then there’s an even bigger problem: that 100+ vehicle fleet simply can’t collect everything, everywhere, all at once.

Even Google Maps can’t do this. It’s evident from a typical Google Street View image: if you look closely you can see the image capture date. Take the example image below: in this case it’s from October 2024, so now it’s over a year old[1](#b6793baa-e76e-485c-96f9-20affeb6a86f):

![Google Street View Image. Typically months or years old.](https://maphappenings.com/wp-content/uploads/2025/11/image.png?w=1024)

Google Street View Image — Credit: Google.

This issue of trying to keep volatile map data current became abundantly clear to me in 2019 when Apple Maps became the first consumer mapping product to display stop signs and traffic lights on a map.

I remember thinking: “Do you realise what you’ve just done? Do you realise what it’s going to take to maintain all that data? Do you realise just how often this data changes?!”

And it didn’t stop there. Speed limits were added to the map too. And that data is even more volatile…

But it gets worse.

Let’s move on to a product whose mapping needs go way beyond that of a consumer navigation app: autonomous vehicles.

With very few exceptions these vehicles require a map an order of magnitude more detailed. And the mapping industry has given this very detailed map a name: they call it an “HD Map”. It has not only a ton more data, but centimetre level accuracy too.

Here’s what a typical HD map looks like:

![An HD Map](https://maphappenings.com/wp-content/uploads/2025/11/image-1.png?w=800)

As you can see it’s capturing gobs of detail that you don’t typically see in your everyday Google Map or Apple Map.

The companies developing autonomous vehicles rely on HD Maps to varying degrees. Waymo and GM/Cruise are the biggest users. If you’re interested, you can nerd out on the details in this table (click/tap to embiggen):

![Comparison of Autonomous Vehicle platforms and their reliance on HD Maps](https://maphappenings.com/wp-content/uploads/2025/11/image-2-1.png?w=1024)

Credit: ChatGPT

So what on earth do you do?

First you could try collecting all the necessary data yourself and try to keep it up to date. This is what Waymo does. It may go a long way to explain the astronomical cost for their service:

![A comparison of a trip on Tesla Robotaxi, Uber, Waymo and Lyft.](https://maphappenings.com/wp-content/uploads/2025/11/image-2-2.png?w=1024)

Image credit [Leo Y. Liu](https://www.linkedin.com/in/leoyuxuanliu/). Full post [here](https://www.linkedin.com/posts/leoyuxuanliu_reflections-on-robotaxis-experience-in-activity-7389622533395791873-nck-).

Alternatively you could take another approach. And that’s where this week’s “Mapping Amazing” company, [Bee Maps](http://www.beemaps.com), comes in:

[![Bee Maps](https://maphappenings.com/wp-content/uploads/2025/11/image-2-4.png?w=1024)](www.beemaps.com)

Credit: Bee Maps

Now I should make it clear from the start that Bee Maps isn’t trying to do everything, but they are doing a hell of a lot. And it’s not just the technology that’s interesting. It’s their unique business model for accomplishing it.

So let’s dive in…

Founded by Ariel Seidman[2](#19328db6-3b1d-44d2-9b48-222c4db80175), this 45 person company based in San Francisco got its start 10 years ago in 2015. Their first product was a platform called Hivemapper. Rather bravely it required the development of hardware as well as software.[3](#16e3a9ed-17d2-42d7-aa81-d1552d7d76d4)

Why hardware?

Because you can’t just write a map data capture app for a smartphone and expect it to work in the real world. In vehicles phones overheat — quickly and easily. Also, they don’t have accurate enough GPS. Besides which in order to capture map data continuously you need a dedicated device, so it would never be practical to use a personal phone.

Bee’s hardware is now on its third generation[4](#f3a885e6-b8bc-4be0-a716-e4bf6047c3f7). The device is about the size of a small hardcover book. It takes about five minutes to install and get running. After that you never have to do anything. No app to start up. No button to press. No firmware to update. It’s completely “fire and forget”.

The device has a main camera, stereo depth cameras and, crucially, a high precision GPS. And it has built-in LTE for uploading data anywhere around the globe:

![Bee Maps map data collection device](https://maphappenings.com/wp-content/uploads/2025/11/image-2-6.png?w=1024)

The Bee Maps device — Credit: Bee Maps

The original goal of Bee Maps was to gamify the collection of map data. The incentive? As you drive and collect data you earn currency: in this case a cryptocurrency called HONEY. And the Hivemapper network is built in such a way that it can be used to incentivise users to collect data in particular locations. They call these HONEY Bursts.

The idea was that an individual who was driving all hours for their job (e.g. an Uber driver) would opt to install a Bee Maps device in their vehicle, and use it to earn some extra dosh on top of the money they were earning for their main job.

And the customers for all this data the Bee Maps devices collect? The traditional map makers of course! And it must make them salivate when they see the ever growing list of data being sucked up:

* Signs for one way, speed limit, stop, yield, turn restriction, highway exit
* Signs for parking restriction, height restriction, rail crossing
* Traffic lights
* Road widths and lanes
* Road construction
* Fire hydrants

In the US they’ve even started to collect speed cameras, gas prices and toll prices.

Bee Maps’ business model has pivoted a little since they were founded. It’s now a subscription model: [starting at US$19 per month you get access to a Bee device](https://beemaps.com/bee). And yes, you still earn HONEY cryptocurrency by driving around.

With a Bee device you get all the benefits of a traditional dashboard camera: continuous video to be used in case of accident reports and telemetry to be used for car insurance discounts.

Commercial fleets get even more: monitoring of all trips and events, including any exciting driving. Here’s a snapshot of the Bee Maps dashboard proving yours truly is a great supporter of exciting driving himself:

![Bee Maps dashboard for fleet owners](https://maphappenings.com/wp-content/uploads/2025/11/image-2-7.png?w=1024)

In effect Bee Maps is now competing with the likes of Garmin for consumer dash cams and the likes of [Samsara](https://www.samsara.com/uk/products/cameras) and [Lytx](https://www.lytx.com) for fleet dash cams.

But they have a huge edge.

Unlike their competitors, Bee Maps is earning an ever growing revenue stream from the map data they collect. And they’re already licensing their map data to some very significant customers. For example: TomTom, HERE, Mapbox, Lyft, Trimble, MAXAR, and more recently, VW.

All this has attracted new investors: Bee Maps closed a [US$32M Series A](https://beemaps.com/blog/the-future-of-ai-powered-mapping-a-letter-from-ariel-ceo) last month.

It’s not only the wealth of data that Bee Maps is capturing that’s impressive, it’s their coverage too:

* 22M unique kilometres mapped (over 13M miles) ~= 3X road network in US.
* 36% of all roads around the whole globe.
* 665M km mapped total (meaning each road has been captured multiple times)

One statistic I find particularly cool: there are tens of thousands of active weekly Bee devices. Compare that to a traditional map maker who has a few hundred dedicated mapping vehicles at best (which are certainly not all active all of the time). In other words Bee Maps’ fleet is at least two orders of magnitude bigger than a traditional mapping company. Nice.

You can start to appreciate the coverage by looking at Bee’s Strava-like heat maps. See below (click/tap to embiggen) or go [here](https://hivemapper.com/coverage) for the full interactive coverage map.

![Bee Maps coverage in Europe ](https://maphappenings.com/wp-content/uploads/2025/11/bee-maps-coverage-europe.png?w=1024)

Bee Maps: Europe

![Bee Maps coverage in Japan and South Korea](https://maphappenings.com/wp-content/uploads/2025/11/bee-maps-coverage-japan-korea.png?w=1024)

Bee Maps: Japan, Korea

![Bee Maps coverage in southeast Asia](https://maphappenings.com/wp-content/uploads/2025/11/bee-maps-coverage-sea.png?w=1024)

Bee Maps: SE Asia

![Bee Maps coverage in US and Canada](https://maphappenings.com/wp-content/uploads/2025/11/bee-maps-coverage-usa.png?w=1024)

Bee Maps: US, Canada

So I know what some of you nerdy types might be thinking: surely companies like Tesla are capturing enough data from all the cameras on their cars that they far surpass what Bee Maps is doing?

Well no.

Firstly: if you approach Elon and ask him nicely if you can license his data, he’s simply going to tell you to fuck off. Secondly: Teslas are only sold to well-to-do people, ipso facto they only generally collect data in well-to-do places. Even Toyotas don’t go everywhere!

Yeah, I know, perhaps one day the likes of the NVIDIA Drive Platform might get embedded in enough vehicles. Or perhaps we’ll all end up wearing some data-hoovering douche bag glasses from some dubious social media company. Or maybe Jony Ive will wow us all with a must-have OpenAI [facehugger](https://maphappenings.com/wp-content/uploads/2025/11/face-hugger.jpg). But that’s all speculation.

In the meantime, get real, do your part, and get [Bee Mapping](https://beemaps.com/bee)!

---

**Footnotes**

1. And yes, it’s even worse for Apple Maps. The Apple Maps “Look Around” image of the house where I used to live in Oakland, California is five years out of date. 😱 [↩︎](#b6793baa-e76e-485c-96f9-20affeb6a86f-link)
2. Ariel is pronounced “R-e-ull”, not like Ariel in the Little Mermaid. [↩︎](#19328db6-3b1d-44d2-9b48-222c4db80175-link)
3. I say “rather bravely” as so many hardware startups have failed miserably or limp along at best. Juicero, Pebble, Rabbit or Humane AI Pin anybody? [↩︎](#16e3a9ed-17d2-42d7-aa81-d1552d7d76d4-link)
4. So, Donald Trump, since I know you’re reading, I have four words for you: built in the USA. [↩︎](#f3a885e6-b8bc-4be0-a716-e4bf6047c3f7-link)

### Share this:

* [Click to share on X (Opens in new window)
  X](https://maphappenings.com/2025/11/06/bee-maps/?share=twitter)
* [Click to share on LinkedIn (Opens in new window)
  LinkedIn](https://maphappenings.com/2025/11/06/bee-maps/?share=linkedin)
* [Click to share on Facebook (Opens in new window)
  Facebook](https://maphappenings.com/2025/11/06/bee-maps/?share=facebook)
* Click to email a link to a friend (Opens in new window)
  Email

---

![]()

![]()

##

##

Loading Comments...

Write a Comment...

Email (Required)

Name (Required)

Website

###

* [Comment](https://maphappenings.com/2025/11/06/bee-maps/#comments)
* Subscribe
  Subscribed

  + [![](https://maphappenings.com/wp-content/uploads/2022/07/cropped-mh-logo-3.png?w=50)](https://maphappenings.com)

  Join 575 other subscribers

  Sign me up

  + Already have a WordPress.com account? [Log in now.](https://wordpress.com/log-in?redirect_to=https%3A%2F%2Fr-login.wordpress.com%2Fremote-login.php%3Faction%3Dlink%26back%3Dhttps%253A%252F%252Fmaphappenings.com%252F2025%252F11%252F06%252Fbee-maps%252F)
* + [![](https://maphappenings.com/wp-content/uploads/2022/07/cropped-mh-logo-3.png?w=50)](https://maphappenings.com)
  + Subscribe
    Subscribed
  + [Sign up](https://wordpress.com/start/)
  + [Log in](https://wordpress.com/log-in?redirect_to=https%3A%2F%2Fr-login.wordpress.com%2Fremote-login.php%3Faction%3Dlink%26back%3Dhttps%253A%252F%252Fmaphappenings.com%252F2025%252F11%252F06%252Fbee-maps%252F)
  + [Copy shortlink](https://wp.me/pe9lLN-16J)
  + [Report this content](https://wordpress.com/abuse/?report_url=https://maphappenings.com/2025/11/06/bee-maps/)
  + [View post in Reader](https://wordpress.com/reader/blogs/209097343/posts/4261)
  + [Manage subscriptions](https://subscribe.wordpress.com/)
  + Collapse this bar

![](https://pixel.wp.com/b.gif?v=noscript)

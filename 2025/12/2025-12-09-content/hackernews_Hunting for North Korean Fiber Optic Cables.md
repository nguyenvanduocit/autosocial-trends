---
source: hackernews
title: Hunting for North Korean Fiber Optic Cables
url: https://nkinternet.com/2025/12/08/hunting-for-north-korean-fiber-optic-cables/
date: 2025-12-09
---

# [North Korean Internet](https://nkinternet.com/)

## Insights from Internet Scanning and Open Source Intelligence

Menu
[Skip to content](#content)

* [Home](https://nkinternet.wordpress.com/)
* [Websites](https://nkinternet.com/websites/)
* [Internet](https://nkinternet.com/internet/)
  + [AS9341 – North Korea Government](https://nkinternet.com/as9341-north-korea-government/)
  + [AS131279 – Ryugyong-dong](https://nkinternet.com/as131279-ryugyong-dong/)
    - [175.45.176.0/24](https://nkinternet.com/175-45-176-024/)
    - [175.45.177.0/24](https://nkinternet.com/175-45-177-024/)
    - [175.45.178.0/24](https://nkinternet.com/175-45-178-024/)
  + [AS134544 – Cenbong Int’l Holdings Limited](https://nkinternet.com/as134544-cenbong-intl-holdings-limited/)
  + [AS20485 – Joint Stock Company TransTeleCom](https://nkinternet.com/as20485-joint-stock-company-transtelecom/)
  + [Kwangmyong](https://nkinternet.com/kwangmyong/)
  + [DNS Requests](https://nkinternet.com/dns-requests/)
* [Software](https://nkinternet.com/software/)
* [Media](https://nkinternet.com/media/)
  + [Documents](https://nkinternet.com/documents/)
  + [Photos](https://nkinternet.com/photos/)
  + [Videos](https://nkinternet.com/videos/)
* [Sources](https://nkinternet.com/sources/)
  + [Sources](https://nkinternet.com/sources/)
* [Contact](https://nkinternet.com/about/)

Search

Search for:

# Hunting For North Korean Fiber Optic Cables

[December 8, 2025](https://nkinternet.com/2025/12/08/hunting-for-north-korean-fiber-optic-cables/) / [nick](https://nkinternet.com/author/nkinternet/)

Before we go any further, one thing that I want to make clear is that the word *assume* is going to be doing some heavy lifting throughout this post. This was a rabbit hole that I recently went down and I probably have more questions than answers, but I still wanted to document what I had found so far. If you have additional information or findings you want to share, as always feel free to reach out: contact@nkinternet.com.

It all started with a PowerPoint that I came across a few weeks ago. It was presented by the DPRK to the ICAO on the state of their aviation industry and their ADS-B deployment inside North Korea. However, one slide in particular caught my eye because it showed a fiber optic cable running across the country

[![](https://nkinternet.com/wp-content/uploads/2025/12/d3e27ea5-25f1-49ed-9392-e38cb466b732.png?w=1024)](https://nkinternet.com/wp-content/uploads/2025/12/d3e27ea5-25f1-49ed-9392-e38cb466b732.png)

*You can find a full link to the presentation [here](https://nkinternet.com/wp-content/uploads/2025/11/gacadprk-presentation2019.4.pdf)*.

This got me wondering more about the physical layout of the network inside North Korea. From the map we know that there’s a connection between Pyongyang and Odaejin, although given the mountains in the middle of the country it probably isn’t a direct link. There isn’t a lot of information on fiber in North Korea, but there are a few outside sources that help provide clues about how things might be laid out.

**Historic Fiber Information**

[38North](https://www.38north.org/2017/10/mwilliams100117/) first reported the connection from Russia’s TTK to the DPRK over the Korea–Russia Friendship Bridge back in 2017. Additionally, a picture found on Flickr looking toward Tumangang after the bridge doesn’t show any utility poles and instead seems to display some kind of infrastructure in the grass to the side of the tracks. Assuming this interpretation is correct, the fiber is likely buried underground as it enters the country and passes through the vicinity of Tumangang Station.

[![](https://nkinternet.com/wp-content/uploads/2025/12/image.png?w=1024)](https://nkinternet.com/wp-content/uploads/2025/12/image.png)

*From user Moravius on Flickr* *which appears to show possible infrastructure in the grass. The white pole on the right side of the tracks are used as distance markers.*

According to a report from [The Nautilus Institute](https://www.nautilus.org/wp-content/uploads/2011/12/DPRK_Digital_Transformation.pdf) we can gather a few additional details about the internet inside North Korea

* One of the first lines was installed in September 1995 between Pyongyang and Hamhung
* In February 1998 a link between Pyongyang and Sinuiju was completed
* As of 2000, DPRK’s operational optical fiber telecom lines included: Pyongyang – Hamhung; Pyongyang – Sinuiju including all cities and counties in North Pyongan Province; Hamhung Rajin-Sonbong; Rajin-Songbong – Hunchun (China), Pyongyang – Nampo.
* In 2003 the original domestic cell phone network was built for North Korean citizens in Pyongyang, Namp’o, reportedly in all provincial capitals, on the Pyongyang-Myohyangsan tourist highway, and the Pyongyang-Kaesong and Wonsan-Hamhung highways
* The Kwangmyong network’s data is transmitted via fiber optic cable with a backbone capacity of 2.5 GB per second between all the provinces.

Based on these notes, it starts to paint a picture that the fiber link coming from Russia likely travels down the east coast of the DPRK before connecting to Pyongyang. Several city pairs—Pyongyang–Hamhung and Rajin–Sonbong—line up with earlier deployments of east-coast fiber infrastructure.

**Kwangmyong Internal Topology**

The report also notes that all of the provinces in North Korea were connected to the Kwangmyong via fiber. The Kwangmyong for those not familiar is the intranet that most citizens in the DPRK can access as they do not have access to the outside internet. While not much information is available about the Kwangmyong, these [notes](https://www.koreaittimes.com/news/articleView.html?idxno=57500) from Choi Sung, Professor of Computer Science at Namseoul University provides some additional details on how the network is laid how, as well as information on the regional networks that are connected. A map provided in his notes shows some of the main points of the Kwangmyong with three of them located along the northeast of North Korea.

[![](https://nkinternet.com/wp-content/uploads/2025/12/57500_23201_1225.jpg?w=600)](https://nkinternet.com/wp-content/uploads/2025/12/57500_23201_1225.jpg)

**Railways, Roads, and Practical Fiber Routing**

This starts to paint a rough picture of how the network is physically deployed in North Korea but we can also look to some outside sources to get some confirmation. [38North](https://www.38north.org/wp-content/uploads/2022/11/02_38-North_North-Korean-Cell-Coverage-Map.png) once again provides some great detail on cell phone towers in North Korea. The interesting thing being an apparent line down the east coast which follows major roads and highways but would also in theory have easier access to the fiber back haul to support the cell network.

[![](https://nkinternet.com/wp-content/uploads/2025/12/02_38-north_north-korean-cell-coverage-map.png?w=1024)](https://nkinternet.com/wp-content/uploads/2025/12/02_38-north_north-korean-cell-coverage-map.png)

All of this seems to suggest that the fiber lines were run along major roads and railways up the east coast. A map from Beyond Parallel shows the major rail lines, which has the Pyongra line up the east coast.

[![](https://nkinternet.com/wp-content/uploads/2025/12/railway-map_crossings_update_map-e1543948581630.jpg?w=1000)](https://nkinternet.com/wp-content/uploads/2025/12/railway-map_crossings_update_map-e1543948581630.jpg)

**Looking For Clues Along the Railway**

Some additional digging for pictures from along the line suggest that there is infrastructure deployed along the tracks, although it’s difficult to confirm from pictures exactly what is buried. The following shows what appears to be a junction box at the base of a pole along the line.

[![](https://nkinternet.com/wp-content/uploads/2025/12/image-1.png?w=600)](https://www.flickr.com/photos/josephferris76/6993153876/)

*Picture from Flickr user josephferris*

The line does have a path along it as well with mile markers. While it is used by bikes and pedestrians, it provides a nice path for supporting fiber and other communications runs along the tracks.

[![](https://nkinternet.com/wp-content/uploads/2025/12/image-2.png?w=835)](https://www.flickr.com/photos/21005841%40N04/15644749122/in/album-72157648984981562)

*Picture from Flickr user Andrew M. showing paths along the line.*

The Pyongra line also crosses through the mountains at points but it is assumed at certain junctions the fiber was laid along the AH 6/National Highway 7 up the coast as there are parts of the line discovered that do not have a path along the tracks. In these places it is assumed they follow the road, although finding pictures of the highway to further examine is challenging.

[![](https://nkinternet.com/wp-content/uploads/2025/12/image-6.png?w=1024)](https://nkinternet.com/wp-content/uploads/2025/12/image-6.png)

*Pyongra line through the mountains. At these points it’s assumed that the fiber optic cables are laid along roads/highways instead of the right of way along the railroad.*

Lastly at certain stations we can see utility boxes along the side of the track suggesting buried conduits/cables are laid along the tracks.

[![](https://nkinternet.com/wp-content/uploads/2025/12/image-3.png?w=360)](https://www.flickr.com/photos/mytripsmypics/26020131208/in/gallery-194433501%40N07-72157720168302177/)

From a video taken in 2012 there does appear to be some signs of objects along the tracks, although difficult to confirm due to the video quality. The screenshot below is the clearest I could find of a rectangular box buried in a clearing along the line.

[![](https://nkinternet.com/wp-content/uploads/2025/12/image-4.png?w=997)](https://www.flickr.com/photos/21005841%40N04/15599502031/in/pool-dprk_rail/)

*From Flickr user Andrew M. Screenshot is from ~21 seconds in the linked video*

Based on this information of what is confirmed and looking at major cities, it appears there is a route that follows Pyongyang → Wonsan → Hamhung → Chongjin → Rajin → Tumangang which follows the Pyongra line as well as the AH 6/National Highway 7 up the coast. The following map highlights a rough path.

[![](https://nkinternet.com/wp-content/uploads/2025/12/image-5.png?w=1024)](https://nkinternet.com/wp-content/uploads/2025/12/image-5.png)

Interestingly by mapping out the possible fiber locations we can start to draw conclusions based on other sources. According to a video by [Cappy’s Army](https://www.youtube.com/watch?v=AcjXPae3Mhw) he proposes that when the US Navy Seals landed in NOrth Korea in 2019 the most likely place this would have occurred is Sinpo. As the goal was to depoy a covert listening device this could also line up with supporting the idea that a fiber backbone runs down the east coast of North Korea as Sinpo would be relatively close.

**What Does This Mean For the Network?**

In addition to the fiber link via Russia, the other fiber optic cable into North Korea comes in via China by way of Sinuiju and Dandong. Although we don’t know for sure where servers are deployed inside North Korea, based on the map of Kwangmyong the first assumption is that things are mainly centralized in Pyongyang.

Out of the 1,024 IPs assigned to North Korea we observe the following behavior based on the CIDR block:

* 175.45.176.0/24 is exclusively routed via China Unicom
* 175.45.177.0/24 is exclusively routed via Russia TransTelekom
* 175.45.178.0/24 is dual-homed and can take either path before crossing into North Korea

With this information in mind, running a traceroute with the TCP flag set gives us a slightly better look at how traffic behaves once it reaches the country. For the following tests we’re going to assume there is a fiber path on the west coming in from China toward Pyongyang, as well as a path on the east side coming from Russia.

From the US east coast to 175.45.176.71, the final hop in China before entering North Korea shows roughly 50 ms of additional latency before reaching the DPRK host. This suggests there may be extra devices, distance, or internal routing inside the country before the packet reaches its final destination.

```
10 103.35.255.254 (103.35.255.254) 234.306 ms 234.082 ms 234.329 ms
11 * * *
12 * * *
13 * * *
14 175.45.176.71 (175.45.176.71) 296.081 ms 294.795 ms 294.605 ms
15 175.45.176.71 (175.45.176.71) 282.938 ms 284.446 ms 282.227 ms
```

Interestingly, running a traceroute to 175.45.177.10 shows a similar pattern in terms of missing hops, but with much lower internal latency. In fact, the ~4 ms difference between the last Russian router and the DPRK host suggests the handoff between Russia and North Korea happens very close—network-wise—to where this device is located. This contrasts with the China path, which appears to take a longer or more complex route before reaching its final destination.

```
10	188.43.225.153	185.192 ms	183.649 ms	189.089 ms
11	 * 	 *
12	 * 	 *
13	 * 	 *
14	175.45.177.10	195.996 ms	186.801 ms	186.353 ms
15	175.45.177.10	188.886 ms	201.103 ms	193.334
```

If everything is centralized in Pyongyang this would mean the handoff from Russia is completed in Pyongyang as well. However, it could also indicate that 175.45.177.0/24 is not hosted in Pyongyang at all and is instead located closer to the Russia–North Korea border. More testing is definitely required however before any conclusions can be drawn about where these devices physically reside.

**What can we learn from all of this?**

Making some assumptions we can get a better idea of how the internet works and is laid out inside North Korea. While not much is officially confirmed using some other sources we can get a possible idea of how things work. As mentioned at the start, the word assume does a lot of heavy lifting. However if you do have other information or ideas feel free to reach out at contact@nkinternet.com

---

### Discover more from North Korean Internet

Subscribe to get the latest posts sent to your email.

Type your email…

Subscribe

### Share this:

* [Click to share on X (Opens in new window)
  X](https://nkinternet.com/2025/12/08/hunting-for-north-korean-fiber-optic-cables/?share=twitter)
* [Click to share on Facebook (Opens in new window)
  Facebook](https://nkinternet.com/2025/12/08/hunting-for-north-korean-fiber-optic-cables/?share=facebook)

Like Loading...

### *Related*

[Uncategorized](https://nkinternet.com/category/uncategorized/)

# Post navigation

[← DPRK Infrastructure Update](https://nkinternet.com/2025/11/20/dprk-infrastructure-update/)

### Leave a comment [Cancel reply](/2025/12/08/hunting-for-north-korean-fiber-optic-cables/#respond)

Δ

Search for:

# Recent Posts

* [Hunting For North Korean Fiber Optic Cables](https://nkinternet.com/2025/12/08/hunting-for-north-korean-fiber-optic-cables/)
* [DPRK Infrastructure Update](https://nkinternet.com/2025/11/20/dprk-infrastructure-update/)
* [Unboxing the Arirang 182 – A North Korean Feature Phone](https://nkinternet.com/2025/08/31/unboxing-the-arirang-182-a-north-korean-feature-phone/)
* [Hangro: Investigating North Korean VPN Infrastructure Part 2](https://nkinternet.com/2025/07/16/hangro-investigating-north-korean-vpn-infrastructure-part-2/)
* [What We Discovered On a North Korean Server Part 1](https://nkinternet.com/2025/06/16/what-we-discovered-on-a-north-korean-server-part-1/)

# Recent Comments

|  |  |
| --- | --- |
|  | [North Korea Downed b…](https://hacktech.info/north-korea-downed-by-faulty-roa-by-oavioklein/) on [North Korea Offline With Inval…](https://nkinternet.com/2025/03/18/north-korea-offline-with-invalid-routes-for-as131279/comment-page-1/#comment-156) |
|  | [North Korea Offline…](https://nkinternet.wordpress.com/2025/03/18/north-korea-offline-with-invalid-routes-for-as131279/) on [North Korea whois records…](https://nkinternet.com/2025/02/23/north-korea-whois-records-hijacked/comment-page-1/#comment-155) |
| [![Marco Pisco's avatar](https://2.gravatar.com/avatar/e00c94d14a00452317e039d1fa8e6094e358b9340704c159a4dce3b81093ca6d?s=48&d=identicon&r=G)](https://marcopisco.com) | [Marco Pisco](https://marcopisco.com) on [North Korea whois records…](https://nkinternet.com/2025/02/23/north-korea-whois-records-hijacked/comment-page-1/#comment-154) |
| [![Unknown's avatar](https://yanac.hu/wp-content/uploads/2025/02/cropped-yanac1.jpg?w=48)](https://yanac.hu/2025/01/07/eszak-koreai-vpn-infrastruktura/) | [Észak-koreai VPN-inf…](https://yanac.hu/2025/01/07/eszak-koreai-vpn-infrastruktura/) on [Hangro: Investigating North Ko…](https://nkinternet.com/2025/01/06/hangro-north-korean-vpn-infrastructure/comment-page-1/#comment-150) |
| [![nick's avatar](https://0.gravatar.com/avatar/608c6d0aaaf26d015dc1ac713d72b09cf405710c964b9abb29a999bc4dd4d9b2?s=48&d=identicon&r=G)](https://nkinternet.wordpress.com) | [nkinternet](https://nkinternet.wordpress.com) on [AS9341 – North Korea…](https://nkinternet.com/2024/08/09/as9341-north-korea-government/comment-page-1/#comment-148) |

[Blog at WordPress.com.](https://wordpress.com/?ref=footer_blog)

* [Comment](https://nkinternet.com/2025/12/08/hunting-for-north-korean-fiber-optic-cables/#respond)
* Reblog
* Subscribe
  Subscribed

  + [![](https://s2.wp.com/i/logo/wpcom-gray-white.png?m=1479929237i) North Korean Internet](https://nkinternet.com)

  Join 34 other subscribers

  Sign me up

  + Already have a WordPress.com account? [Log in now.](https://wordpress.com/log-in?redirect_to=https%3A%2F%2Fr-login.wordpress.com%2Fremote-login.php%3Faction%3Dlink%26back%3Dhttps%253A%252F%252Fnkinternet.com%252F2025%252F12%252F08%252Fhunting-for-north-korean-fiber-optic-cables%252F)
* + [![](https://s2.wp.com/i/logo/wpcom-gray-white.png?m=1479929237i) North Korean Internet](https://nkinternet.com)
  + Subscribe
    Subscribed
  + [Sign up](https://wordpress.com/start/)
  + [Log in](https://wordpress.com/log-in?redirect_to=https%3A%2F%2Fr-login.wordpress.com%2Fremote-login.php%3Faction%3Dlink%26back%3Dhttps%253A%252F%252Fnkinternet.com%252F2025%252F12%252F08%252Fhunting-for-north-korean-fiber-optic-cables%252F)
  + [Copy shortlink](https://wp.me/p6he9O-fJ)
  + [Report this content](https://wordpress.com/abuse/?report_url=https://nkinternet.com/2025/12/08/hunting-for-north-korean-fiber-optic-cables/)
  + [View post in Reader](https://wordpress.com/reader/blogs/92764016/posts/975)
  + [Manage subscriptions](https://subscribe.wordpress.com/)
  + Collapse this bar

##

##

Loading Comments...

Write a Comment...

Email (Required)

Name (Required)

Website

###

%d

![](https://pixel.wp.com/b.gif?v=noscript)

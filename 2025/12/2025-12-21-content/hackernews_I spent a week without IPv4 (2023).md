---
source: hackernews
title: I spent a week without IPv4 (2023)
url: https://www.apalrd.net/posts/2023/network_ipv6/
date: 2025-12-21
---

[apalrd](/)

menu

* [resume](/resume/)
* [projects](/tags/projects/)
* [tags](/tags/)
* [youtube](https://www.youtube.com/c/apalrdsadventures)
* [discord](https://discord.gg/xJsaEukAr4)
* [mastodon](https://hachyderm.io/%40apalrd)
* [ko-fi](https://ko-fi.com/apalrd)
* [wishlist](/wishlist/)
* [sponsors](/sponsors/)

* [resume](/resume/)
* [projects](/tags/projects/)
* [tags](/tags/)
* [youtube](https://www.youtube.com/c/apalrdsadventures)
* [discord](https://discord.gg/xJsaEukAr4)
* [mastodon](https://hachyderm.io/%40apalrd)
* [ko-fi](https://ko-fi.com/apalrd)
* [wishlist](/wishlist/)
* [sponsors](/sponsors/)

# [I spent a WEEK without IPv4 to understand IPv6 transition mechanisms](https://www.apalrd.net/posts/2023/network_ipv6/)

2023-02-09

#[networking](https://www.apalrd.net/tags/networking/)

The time has come to talk about something uncomfortable to a lot of you. You’ve been using legacy methods for far too long. It’s time to move to IPv6.

But, of course, there’s a lot more to IPv6 than ‘just’ switching everything over. A lot of systems in the world still haven’t adopted it after nearly 25 years, and although software support is virtually a requirement these days, that doesn’t mean it’s widely enabled. There are also still a lot of misconceptions from network administrators who are scared of or don’t properly understand IPv6, and I want to address all of that.

But, for me to describe to you the best setup for your networks going forward, I need to understand for myself how all of the IPv6 transition mechanisms and behaviors work. To understand where transition mechanisms fail, I’m spending a fully week with only IPv6 and reporting on what works and doesn’t.

At the start of this, I’d like to absolutely stress that the NAT you know and hate was not imagined when the Internet Protocol was created, and in many ways has introduced a huge amount of headache in routing. You should stop thinking of NAT as a security mechanism and think of it as the emergency address exhaustion prevention that it is. Likewise, CG-NAT is a dire emergency address exhaustion prevention mechnism that nobody really wants to deploy unless they absolutely must. Don’t blame your provider when they deploy CG-NAT, embrace IPv6 and global routing instead.

# Contents[⌗](#contents)

* [Video](/posts/2023/network_ipv6/#video)
* [Crash Course on IPv6](/posts/2023/network_ipv6/#crash-course-on-ipv6)
* [Advantages for the Homelab](/posts/2023/network_ipv6/#advantages-for-the-homelab)
* [Transition Mechanisms](/posts/2023/network_ipv6/#transition-mechanisms)
  + [Dual Stack](/posts/2023/network_ipv6/#dual-stack)
  + [Stateless IP/ICMP Translation](/posts/2023/network_ipv6/#stateless-ipicmp-translation)
  + [NAT64](/posts/2023/network_ipv6/#nat64) - the method I’ve setup for this test
  + [464XLAT](/posts/2023/network_ipv6/#464xlat) - the method I ended up using on macOS/iOS
* [Lessons Learned](/posts/2023/network_ipv6/#lessons-learned)

# Video[⌗](#video)

[![Thumbnail](/posts/2023/network_ipv6/thumbnail.png)](TBD)

# Crash Course on IPv6[⌗](#crash-course-on-ipv6)

The biggest hurdle to implementing IPv6 on your own isn’t usually ISP support, router support, or client support. It’s the mental thought process behind un-learning the legacy methods of network design. With IPv6, we aren’t just copying the mistakes of IPv4 and adding a wider address space, we’re going back to how the internet protocol was intended to behave before we started running out of addresses, getting rid of network address translation, and generally adding a stronger focus on proper subnet design and routing.

So, here are a few quick notes that can are crucial in understanding IPv6:

* Addresses are 128 bits long and written as 8 four-letter hex blocks separated by colons (i.e. `fd69:beef:cafe:feed:face:6969:0420:0001`)
* Leading zeros can be omitted (i.e. `0420` can become `420`, but not `42`)
* Groups of zeros can be omitted with two colons, but only once in an address (i.e. `2000:1::1`, but not `2000::1::1` as that is ambiguous)
* Network prefixes are (virtually) always 64 bits long, with a 64-bit client suffix, using CIDR notation (i.e. `2000::/3`)
* All addresses should be treated as if they are globally unique, even if they are only within our organization
* You are encouraged to have as many addresses as you want on a single interface, for different purposes or scopes
* Similarly, we can have multiple routers advertising prefixes on the same layer 2 domain, and this is also encouraged
* We no longer need to centrally assign addresses via DHCP, since nodes can now assign themselves addresses in the vast 64-bit local client space
* Since everything is globally routable and unique, we have no need to do network address translation or port forwarding

# Advantages for the Homelab[⌗](#advantages-for-the-homelab)

A question I get over and over when I bring up IPv6 is ‘what advantages does this bring to my home lab network’. So, here are some reasons you should start using IPv6 within your own network:

* If you are behind carrier-grade NAT on IPv4, your IPv6 connectivity will still be globally routable and can receive incoming connections such as VPNs or game servers. I’ve found that I can host servers on a mobile hotspot using IPv6
* Peer-to-peer communications such as gaming usually have to deal with NAT traversal, but with IPv6 this is no longer an issue, especially for multiple gamers using the same connection
* IPSec VPN is widely used but often performs poorly as it struggles to traverse NAT, so like gaming this is not an issue with IPv6
* Since we always have a link-local address, we don’t need to assign addresses at all on point-to-point links or isolated networks
* If you want to host services, you don’t need to use different ports or a reverse proxy to separate traffic out of the single port on the singlular WAN address
* This same advantage applies to single servers, where you can assign multiple addresses for different services, potentially using the same port

# Transition Mechanisms[⌗](#transition-mechanisms)

The primary methods of transition are Dual-Stack, SIIT, NAT64, and 464XLAT, each of which increases in complexity and has different advantages.

## Dual Stack[⌗](#dual-stack)

In most cases, you will end up deploying dual-stack networks. With this setup, both IPv4 and IPv6 are completely deployed across the entire network. Resources are accessible via one or the other IP version, all network segments must be assigned both an IPv4 and IPv6 subnet, and all routers must have routing tables for both IPv4 and IPv6. Clients may choose to access any resource via either IP version, likely receiving both A and AAAA records for destinations. As you can see, this is manageable for home networks of only one router but generally is difficult to scale. However, it’s the easiest way to deploy IPv6 in your home network or small organization.

## Stateless IP/ICMP Translation[⌗](#stateless-ipicmp-translation)

If you’d like to avoid duplicating routing tables and all of your routing configuration, you can transition fully to IPv6, and statelessly translate between IPv4 and IPv6 at the edge of your network. This method is known as ‘Stateless IP/ICMP Translation’ and is most suited for applications where each device which needs to access the public IPv4 internet has a public IPv4 address, such as datacenters. This way, the IPv4 public addresses can be statelessly translated 1:1 to IPv6 destinations and relayed to IPv6-only servers, allowing the datacenter to operate fully IPv6 internally while still providing access to public IPv4 clients. However, this method does not perform traditional source NAT / masquarade as would normally be used in IPv4 networks.

## NAT64[⌗](#nat64)

Another mechanism is NAT64. This operates similarly to ’legacy’ IPv4 NAT, in that we have a pool of addresses which we are masquarading behind a single public address. However, in our case, the address pool is IPv6 and the public address is IPv4. So, the NAT64 server takes requests from native IPv6 clients with the destination IPv4 address encoded into the destination IPv6 address (possibly using the well-known prefix `64:ff9b::/96` and adding the 32-bit IPv4 on the end). It then performs both protocol, address, and port translation as it would in IPv4 NAT, and outgoing connections depart the NAT64 server appearing as a normal network behind a single public IPv4. The downside to this is that clients need to know that they should be connecting to IPv4 destinations. Like traditional NAT, this is a stateful transition and imposes the NAT64 gateway as a potential choke point on the network (as the NAT service always has been).

The most common mechanism used alongside NAT64 is DNS64. In this transition mechansim, the DNS sever is aware that it’s serving an IPv6-only network and the prefix in use for NAT64 translation. If a DNS server encounters a query which has no AAAA record but does have a valid A record, it will synthesize an AAAA record by combining the A record with the NAT64 prefix. Now, any client software which utilizes DNS will automatically connect to the NAT64 gateway for access to the IPV4 internet via IPv6.

## 464XLAT[⌗](#464xlat)

In addition to DNS64, the final mechanism which can be deployed in a NAT64 network is known as ‘464XLAT’. Conceptually it’s nearly identical to a combination of NAT64 at the network level and SIIT at the individual device level (4->6 1:1 followed by 6->4 NAT), although different terminology is often used due to the different organizations which developed this. The NAT64 server at the edge of the network usually performs client NAT or CG-NAT, and is known as the ‘PLAT’ (Provider side transLATor). Each client then runs its own ‘CLAT’ (Client side transLATor), which self-assigns an IPv4 address in its networking stack as well as an IPv6 address to communicate with the PLAT. Client applications which send packets using IPv4 see the CLAT’s IPv4 as the default IPv4 route, and the CLAT then performs stateless 4->6 translation. This removes or reduces the need for DNS64, imposes similar network requirements for a NAT64 gateway, and allows compatible clients to perform IPv4->IPv6 translation if they need direct IPv4 communication. This is usually the method used within ISP networks, as their NAT64/PLAT entity would already be required as a CG-NAT gateway and they can implement the CLAT service on their modem/gateway to provide the appearance of end-to-end IPv4 to clients with no downsides.

On our own networks, using 464XLAT is mostly limited by client software support. iOS and macOS both have native CLAT functions which will automatically activate, but older versions of macOS and essentially every other OS currently in use do not automatically function. That said, it’s possible to deploy both 464XLAT and DNS64 on the same network easily, so clients without native 464XLAT support will fall back on DNS64 and only have issues with IPv4 literals in certain peer-to-peer software.

# Lessons Learned[⌗](#lessons-learned)

A summary of the lessons I learned in my week without IPv4:

* IPv6 is absolutely ready for prime-time and has been for awhile
* About half of the internet sites I rely on support IPv6 natively, so there needs to be more pressure on site admins and CDNs to support IPv6 natively
* There seems to be a lack of drive (judging by forum posts) to enable IPv6 on internet services by admins, either because they don’t care to, or it’s more work to manage a public IPv4 and public IPv6 presence
* Networks should be designed IPv6-first instead of IPv4-first, and this design approach largely solves most of the major issues
* NAT64 should replace traditional NAT in network architectures and is essentially a drop-in replacement pending better software support by routers
* DNS64 alone is a mostly usable transition mechanism and is probably ’enough’ for public Wifi or other well-managed networks where you know what services are important to you or don’t mind peer-to-peer IPv4 addresses failing
* 464XLAT is a solution with no user-visible downsides and is the way ISP networks should be deployed going forward, combined with CG-NAT
* Apple has excellent IPv6 support on their devices, fully supporting automatic configuration of 464XLAT on devices with NAT64, and overall an excellent attitude to forcing IPv6 support from developers
* Other operating systems are bit of hit or miss

read other posts

---

[←
Welcome to the Editing Den! My Editing Workflow](https://www.apalrd.net/posts/2023/studio_editing/)

[Linux VM Templates in Proxmox on EASY MODE using Prebuilt Cloud Init Images!
→](https://www.apalrd.net/posts/2023/pve_cloud/)

© 2025
::[rss feed](/index.xml)
:: Theme made by [panr](https://twitter.com/panr)

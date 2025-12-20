---
source: hackernews
title: Garage – An S3 object store so reliable you can run it outside datacenters
url: https://garagehq.deuxfleurs.fr/
date: 2025-12-20
---

[![](/images/garage-logo-horizontal.svg)](https://garagehq.deuxfleurs.fr)

[ ]

[Overview](https://garagehq.deuxfleurs.fr/)
[Docs](https://garagehq.deuxfleurs.fr/documentation/)
[Blog](https://garagehq.deuxfleurs.fr/blog/)

[Download](https://garagehq.deuxfleurs.fr/download/ "Garage releases")

# Garage

![Garage](/images/garage-logo.svg)

Made for redundancy

Each chunk of data is replicated in 3 zones

![](https://garagehq.deuxfleurs.fr/icons/servers.svg)
Zone (multiple servers)

![](https://garagehq.deuxfleurs.fr/icons/datachunks.svg)
Chunks of data

## Our Goals

We made it lightweight and kept the efficiency in mind:

* Self-contained

  We ship a single dependency-free binary that runs on all Linux distributions
* Fast to deploy, safe to operate

  We are sysadmins, we know the value of operator-friendly software
* Deploy everywhere on every machine

  We do not have a dedicated backbone, and neither do you,
  so we made software that run over the Internet across multiple datacenters
* Highly resilient

  to network failures,
  network latency,
  disk failures,
  sysadmin failures

## Keeping requirements low

We worked hard to keep requirements as low as possible:

* ![](https://garagehq.deuxfleurs.fr/icons/cpu.svg)
  CPU

  Any x86\_64 CPU from the last 10 years, ARMv7 or ARMv8
* ![](https://garagehq.deuxfleurs.fr/icons/ram.svg)
  RAM

  1 GB
* ![](https://garagehq.deuxfleurs.fr/icons/disk.svg)
  Disk space

  At least 16 GB
* ![](https://garagehq.deuxfleurs.fr/icons/network.svg)
  Network

  200 ms or less, 50 Mbps or more
* ![](https://garagehq.deuxfleurs.fr/icons/hardware.svg)
  Heterogeneous hardware

  Build a cluster with whatever second-hand machines are available

## Data resiliency for everyone

We built Garage to suit your existing infrastructure:

Garage implements the Amazon S3 API
and thus is already compatible with many applications.

* [![Nextcloud](https://garagehq.deuxfleurs.fr/images/nextcloud-logo.svg)](https://nextcloud.com/ "Nextcloud")
* [![Matrix](https://garagehq.deuxfleurs.fr/images/matrix-logo.svg)](https://element.io/ "Matrix")
* [![Cyberduck](https://garagehq.deuxfleurs.fr/images/cyberduck-logo.png)](https://cyberduck.io/ "Cyberduck")
* [![Mastodon](https://garagehq.deuxfleurs.fr/images/mastodon-logo.svg)](https://mastodon.social/ "Mastodon")
* [![Rclone](https://garagehq.deuxfleurs.fr/images/rclone-logo.svg)](https://rclone.org/ "Rclone")
* [![PeerTube](https://garagehq.deuxfleurs.fr/images/peertube-logo.svg)](https://joinpeertube.org/ "PeerTube")

## Standing on the shoulders of giants

Garage leverages insights from recent research in distributed systems:

* [Dynamo: Amazon’s Highly Available Key-value Store](https://dl.acm.org/doi/abs/10.1145/1323293.1294281)
  by DeCandia et al.
* [Conflict-Free Replicated Data Types](https://hal.inria.fr/inria-00609399v1)
  by Shapiro et al.
* [Maglev: A Fast and Reliable Software Network Load Balancer](https://www.usenix.org/conference/nsdi16/technical-sessions/presentation/eisenbud)
  by Eisenbud et al.

## Sponsors and funding

Garage has benefitted multiple times from public funding:

* 2021-2022: [NGI POINTER](https://pointer.ngi.eu/) provided funding for 3 full-time employees for one year
* 2023-2024: [NLnet / NGI0 Entrust](https://nlnet.nl/entrust/) provided funding for 1 full-time employee for one year
* 2025: [NLnet / NGI0 Commons Fund](https://nlnet.nl/commonsfund/) provided funding for 1.5 full-time employee for one year

If you want to participate in funding Garage development,
either through donation or support contract,
please get in touch with us.

![NGI Pointers](https://garagehq.deuxfleurs.fr/images/ngi-pointer-eu.png)

![NLnet logo](https://garagehq.deuxfleurs.fr/images/nlnet.svg)
![NGI0 Entrust logo](https://garagehq.deuxfleurs.fr/images/NGI0_tag.svg)

This project has received funding from the European Union's Horizon 2021 research and innovation programme
within the framework of the NGI-POINTER Project funded under grant agreement N° 871528.

This project has received funding from the NGI Zero Entrust Fund,
a fund established by NLnet with financial support from the
European Commission's Next Generation Internet programme, under the aegis of DG
Communications Networks, Content and Technology under grant agreement No
101069594.

This project has received funding from the NGI Zero Commons Fund,
a fund established by NLnet with financial support from the
European Commission's Next Generation Internet programme, under the aegis of DG
Communications Networks, Content and Technology under grant agreement No
101135429.

Search
(alt + S)

(Esc)

[![](https://garagehq.deuxfleurs.fr/icons/git.svg)](https://git.deuxfleurs.fr/Deuxfleurs/garage)
![](https://garagehq.deuxfleurs.fr/icons/contact.svg)

Built with [Zola](https://www.getzola.org),
powered by [Garage](https://garagehq.deuxfleurs.fr),
hosted by [Deuxfleurs](https://deuxfleurs.fr)

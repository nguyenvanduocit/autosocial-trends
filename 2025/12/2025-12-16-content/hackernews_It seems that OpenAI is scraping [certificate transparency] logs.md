---
source: hackernews
title: It seems that OpenAI is scraping [certificate transparency] logs
url: https://benjojo.co.uk/u/benjojo/h/Gxy2qrCkn1Y327Y6D3
date: 2025-12-16
---

[home](/)
[tags](/o)
[events](/events)
[about](/about)
[login](/login)

one honk maybe more

[![](/a?a=https%3a%2f%2fbenjojo.co.uk%2fu%2fbenjojo)](https://benjojo.co.uk/u/benjojo)

[benjojo](https://benjojo.co.uk/u/benjojo)
[posted](https://benjojo.co.uk/u/benjojo/h/Gxy2qrCkn1Y327Y6D3) 12 Dec 2025 20:46 +0000

lol.

I minted a new TLS cert and it seems that OpenAI is scraping CT logs for what I assume are things to scrape from, based on the near instant response from this:

```
Dec 12 20:43:04 xxxx xxx[719]:
l=debug
m="http request"
pkg=http
httpaccess=
handler=(nomatch)
method=get
url=/robots.txt
host=autoconfig.benjojo.uk
duration="162.176µs"
statuscode=404
proto=http/2.0
remoteaddr=74.7.175.182:38242
tlsinfo=tls1.3
useragent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36; compatible; OAI-SearchBot/1.3; robots.txt; +https://openai.com/searchbot"
referrr=
size=19
cid=19b14416d95
```

[![](/a?a=https%3a%2f%2fmstdn.io%2fusers%2fwolf480pl)](https://mstdn.io/users/wolf480pl)

[wolf480pl@mstdn.io](https://mstdn.io/users/wolf480pl)
[replied](https://mstdn.io/%40wolf480pl/115708595554461422) 12 Dec 2025 20:57 +0000

in reply to: <https://benjojo.co.uk/u/benjojo/h/Gxy2qrCkn1Y327Y6D3>

[@benjojo](https://benjojo.co.uk/u/benjojo)
wp-login.php bots have been doing that for years so I'd be surprised if OpenAI didn't

[![](/a?a=https%3a%2f%2fbenjojo.co.uk%2fu%2fbenjojo)](https://benjojo.co.uk/u/benjojo)

[benjojo](https://benjojo.co.uk/u/benjojo)
[replied](https://benjojo.co.uk/u/benjojo/h/NgH2Xwlp4KhCTwHjRL) 12 Dec 2025 21:10 +0000

in reply to: <https://mstdn.io/users/wolf480pl/statuses/115708595554461422>

[@wolf480pl](https://mstdn.io/users/wolf480pl) yeah and I guess it's a non terrible way of "seeding" a "search engine"

[![](/a?a=https%3a%2f%2fmstdn.io%2fusers%2fwolf480pl)](https://mstdn.io/users/wolf480pl)

[wolf480pl@mstdn.io](https://mstdn.io/users/wolf480pl)
[replied](https://mstdn.io/%40wolf480pl/115712376924287199) 13 Dec 2025 12:59 +0000

in reply to: <https://benjojo.co.uk/u/benjojo/h/NgH2Xwlp4KhCTwHjRL>

[@benjojo](https://benjojo.co.uk/u/benjojo)
what if CT logs contained hash(domain, nonce) instead of containing the domain in plain, and the nonce was part of the CT inclusion proof?

[![](/a?a=https%3a%2f%2fbenjojo.co.uk%2fu%2fbenjojo)](https://benjojo.co.uk/u/benjojo)

[benjojo](https://benjojo.co.uk/u/benjojo)
[replied](https://benjojo.co.uk/u/benjojo/h/lPLWBh3YCbFJBH4Dt6) 13 Dec 2025 14:53 +0000

in reply to: <https://mstdn.io/users/wolf480pl/statuses/115712376924287199>

[@wolf480pl](https://mstdn.io/users/wolf480pl) the point of certificate transparency logs is so that outside observers can do the double-checking of the CAs certificate and policy in full, if you mess with any part of this, the entire system becomes deeply exploitable and difficult to end to end verify

[![](/a?a=https%3a%2f%2fmstdn.io%2fusers%2fwolf480pl)](https://mstdn.io/users/wolf480pl)

[wolf480pl@mstdn.io](https://mstdn.io/users/wolf480pl)
[replied](https://mstdn.io/%40wolf480pl/115713071072619432) 13 Dec 2025 15:55 +0000

in reply to: <https://benjojo.co.uk/u/benjojo/h/lPLWBh3YCbFJBH4Dt6>

[@benjojo](https://benjojo.co.uk/u/benjojo)
oh, duh I need to be able to find who's issuing carts for my domain ![](/d/7Dwx2vmsBj1y71Gdk9.png ":blobfacepalm:")

and I'm guessing some people look at all certs issued by CAs and verify certain criteria that may require knowing the domains...

it's kinda sad that it provides domain enumeration, but I guess putting addng zero-knowledge proofs to the mix would've been too complex

[![](/a?a=https%3a%2f%2fbenjojo.co.uk%2fu%2fbenjojo)](https://benjojo.co.uk/u/benjojo)

[benjojo](https://benjojo.co.uk/u/benjojo)
[replied](https://benjojo.co.uk/u/benjojo/h/pyX28McwZyTh14hy55) 13 Dec 2025 18:00 +0000

in reply to: <https://mstdn.io/users/wolf480pl/statuses/115713071072619432>

[@wolf480pl](https://mstdn.io/users/wolf480pl) tbh domain's are not really that secret, and if you depended on that then something was very wrong.

You can work around a lot of this stuff by "just" using wildcard certs instead

[![](/a?a=https%3a%2f%2fmstdn.io%2fusers%2fwolf480pl)](https://mstdn.io/users/wolf480pl)

[wolf480pl@mstdn.io](https://mstdn.io/users/wolf480pl)
[replied](https://mstdn.io/%40wolf480pl/115713588719701003) 13 Dec 2025 18:07 +0000

in reply to: <https://benjojo.co.uk/u/benjojo/h/pyX28McwZyTh14hy55>

[@benjojo](https://benjojo.co.uk/u/benjojo)
but then why bother with NSEC3...

[![](/a?a=https%3a%2f%2fbenjojo.co.uk%2fu%2fbenjojo)](https://benjojo.co.uk/u/benjojo)

[benjojo](https://benjojo.co.uk/u/benjojo)
[replied](https://benjojo.co.uk/u/benjojo/h/QL99Yfd3rC9RsrW7CG) 13 Dec 2025 23:29 +0000

in reply to: <https://mstdn.io/users/wolf480pl/statuses/115713588719701003>

[@wolf480pl](https://mstdn.io/users/wolf480pl) tbh I would argue why bother with DNSSEC (outside of extremely marginal situations), but NSEC3 even more

[![](/a?a=https%3a%2f%2fmastodon.social%2fusers%2fjamesog)](https://mastodon.social/users/jamesog)

[jamesog@mastodon.soc..](https://mastodon.social/users/jamesog)
[replied](https://mastodon.social/%40jamesog/115708641821797831) 12 Dec 2025 21:09 +0000

in reply to: <https://benjojo.co.uk/u/benjojo/h/Gxy2qrCkn1Y327Y6D3>

[@benjojo](https://benjojo.co.uk/u/benjojo) It's interesting to watch web server logs to see what things pick up new CT entries the quickest

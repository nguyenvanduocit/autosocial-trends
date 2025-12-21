---
source: hackernews
title: NTP at NIST Boulder Has Lost Power
url: https://lists.nanog.org/archives/list/nanog@lists.nanog.org/message/ACADD3NKOG2QRWZ56OSNNG7UIEKKTZXL/
date: 2025-12-21
---

[lists.nanog.org](/archives/)

[Sign In](/accounts/login/?next=/archives/list/nanog%40lists.nanog.org/message/ACADD3NKOG2QRWZ56OSNNG7UIEKKTZXL/)
[Sign Up](/accounts/signup/?next=/archives/list/nanog%40lists.nanog.org/message/ACADD3NKOG2QRWZ56OSNNG7UIEKKTZXL/)

[Manage this list](/mailman3/lists/nanog.lists.nanog.org/)
[Sign In](/accounts/login/?next=/archives/list/nanog%40lists.nanog.org/message/ACADD3NKOG2QRWZ56OSNNG7UIEKKTZXL/)
[Sign Up](/accounts/signup/?next=/archives/list/nanog%40lists.nanog.org/message/ACADD3NKOG2QRWZ56OSNNG7UIEKKTZXL/)

×

#### Keyboard Shortcuts

### Thread View

* `j`: Next unread message
* `k`: Previous unread message
* `j a`: Jump to all threads* `j l`: Jump to MailingList overview

[thread](/archives/list/nanog%40lists.nanog.org/thread/ACADD3NKOG2QRWZ56OSNNG7UIEKKTZXL/#ACADD3NKOG2QRWZ56OSNNG7UIEKKTZXL)

# NTP at NIST Boulder has lost power

![](https://secure.gravatar.com/avatar/60f9bf1e24e31cddc837efda430b7fd5.jpg?s=120&d=mm&r=g)

## [Anne Johnson](/archives/users/7d15e5ca98314a6eb9a4440754550cbc/ "See the profile for Anne Johnson")

20 Dec
2025

20 Dec
'25

7:28 a.m.

reposting from the Internet time service list
jeff.s...@nist.gov <jeff.sherman@nist.gov>: Dec 19 05:18PM -0800
Dear colleagues,
In short, the atomic ensemble time scale at our Boulder campus has failed
due to a prolonged utility power outage. One impact is that the Boulder
Internet Time Services no longer have an accurate time reference. At time
of writing the Boulder servers are still available due a standby power
generator, but I will attempt to disable them to avoid disseminating
incorrect time.
The affected servers are:
time-a-b.nist.gov
time-b-b.nist.gov
time-c-b.nist.gov
time-d-b.nist.gov
time-e-b.nist.gov
ntp-b.nist.gov (authenticated NTP)
No time to repair estimate is available until we regain staff access and
power. Efforts are currently focused on obtaining an alternate source of
power so the hydrogen maser clocks survive beyond their battery backups.
More details follow.
Due to prolonged high wind gusts there have been a combination of utility
power line damage and preemptive utility shutdowns (in the interest of
wildfire prevention) in the Boulder, CO area. NIST's campus lost utility
power Wednesday (Dec. 17 2025) around 22:23 UTC. At time of writing utility
power is still off to the campus. Facility operators anticipated needing to
shutdown the heat-exchange infrastructure providing air cooling to many
parts of the building, including some internal networking closets. As a
result, many of these too were preemptively shutdown with the result that
our group lacks much of the monitoring and control capabilities we
ordinarily have. Also, the site has been closed to all but emergency
personnel Thursday and Friday, and at time of writing remains closed.
At initial power loss, there was no immediate impact to the NIST atomic
time scale or distribution services because the projects are afforded
standby power generators. However, we now have strong evidence one of the
crucial generators has failed. In the downstream path is the primary signal
distribution chain, including to the Boulder Internet Time Service. Another
campus building houses additional clocks backed up by a different power
generator; if these survive it will allow us to re-align the primary time
scale when site stability returns without making use of external clocks or
reference signals.
Best wishes,
-Jeff Sherman
project email: internet-time-service@nist.gov
AnneJ (watching from Edinburgh, UK)

[1](#like "You must be logged-in to vote.")
[0](#dislike "You must be logged-in to vote.")

Reply

[Sign in to reply online](/accounts/login/?next=/archives/list/nanog%40lists.nanog.org/message/ACADD3NKOG2QRWZ56OSNNG7UIEKKTZXL/)
Use email software

[Back to the thread](/archives/list/nanog%40lists.nanog.org/thread/ACADD3NKOG2QRWZ56OSNNG7UIEKKTZXL/#ACADD3NKOG2QRWZ56OSNNG7UIEKKTZXL)

[Back to the list](/archives/list/nanog%40lists.nanog.org/)

![HyperKitty](/static/hyperkitty/img/logo.png)
Powered by [HyperKitty](http://hyperkitty.readthedocs.org) version 1.3.12.

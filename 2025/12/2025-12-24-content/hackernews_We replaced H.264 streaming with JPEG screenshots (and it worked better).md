---
source: hackernews
title: We replaced H.264 streaming with JPEG screenshots (and it worked better)
url: https://blog.helix.ml/p/we-mass-deployed-15-year-old-screen
date: 2025-12-24
---

[![HelixML](https://substackcdn.com/image/fetch/$s_!uVK-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6ac6823-53fa-4485-b35d-65c2770f5cb8_1280x1280.png)](/)

# [HelixML](/)

SubscribeSign in

# We Mass-Deployed 15-Year-Old Screen Sharing Technology and It's Actually Better

### Or: How JPEG Screenshots Defeated Our Beautiful H.264 WebCodecs Pipeline

[![Luke Marsden's avatar](https://substackcdn.com/image/fetch/$s_!fz2p!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fe83a5474-4943-4f2d-ae84-070ba6dd2042_400x400.jpeg)](https://substack.com/%40lewq)

[Luke Marsden](https://substack.com/%40lewq)

Dec 18, 2025

4

1

1

Share

*Part 2 of our video streaming saga. [Read Part 1: How we replaced WebRTC with WebSockets →](https://blog.helix.ml/p/we-killed-webrtc-and-nobody-noticed)*

## The Year is 2025 and We’re Sending JPEGs

Let me tell you about the time we spent three months building a gorgeous, hardware-accelerated, WebCodecs-powered, 60fps H.264 streaming pipeline over WebSockets...

Thanks for reading HelixML! Subscribe for free to receive new posts and support my work.

Subscribe

...and then replaced it with `grim | curl` when the WiFi got a bit sketchy.

I wish I was joking.

---

## Act I: Hubris (Also Known As “Enterprise Networking Exists”)

We’re building [Helix](https://github.com/helixml/helix), an AI platform where autonomous coding agents work in cloud sandboxes. Users need to watch their AI assistants work. Think “screen share, but the thing being shared is a robot writing code.”

Last week, we explained how we replaced WebRTC with a custom WebSocket streaming pipeline. This week: why that wasn’t enough.

**The constraint that ruined everything:** It has to work on enterprise networks.

You know what enterprise networks love? HTTP. HTTPS. Port 443. That’s it. That’s the list.

You know what enterprise networks hate?

* **UDP** — Blocked. Deprioritized. Dropped. “Security risk.”
* **WebRTC** — Requires TURN servers, which requires UDP, which is blocked
* **Custom ports** — Firewall says no
* **STUN/ICE** — NAT traversal? In *my* corporate network? Absolutely not
* **Literally anything fun** — Denied by policy

We tried WebRTC first. Worked great in dev. Worked great in our cloud. Deployed to an enterprise customer.

“The video doesn’t connect.”

*checks network* — Outbound UDP blocked. TURN server unreachable. ICE negotiation failing.

We could fight this. Set up TURN servers. Configure enterprise proxies. Work with IT departments.

Or we could accept reality: **Everything must go through HTTPS on port 443.**

So we built a **pure WebSocket video pipeline**:

* H.264 encoding via GStreamer + VA-API (hardware acceleration, baby)
* Binary frames over WebSocket (L7 only, works through any proxy)
* WebCodecs API for hardware decoding in the browser
* 60fps at 40Mbps with sub-100ms latency

We were so proud. We wrote Rust. We wrote TypeScript. We implemented our own binary protocol. We measured things in microseconds.

**Then someone tried to use it from a coffee shop.**

---

## Act II: Denial

“The video is frozen.”

“Your WiFi is bad.”

“No, the video is definitely frozen. And now my keyboard isn’t working.”

*checks the video*

It’s showing what the AI was doing 30 seconds ago. And the delay is growing.

Turns out, 40Mbps video streams don’t appreciate 200ms+ network latency. Who knew.

When the network gets congested:

1. Frames buffer up in the TCP/WebSocket layer
2. They arrive in-order (thanks TCP!) but increasingly delayed
3. Video falls further and further behind real-time
4. You’re watching the AI type code from 45 seconds ago
5. By the time you see a bug, the AI has already committed it to main
6. Everything is terrible forever

“Just lower the bitrate,” you say. Great idea. Now it’s 10Mbps of blocky garbage that’s *still* 30 seconds behind.

---

## Act III: Bargaining

We tried everything:

**“What if we only send keyframes?”**

This was our big brain moment. H.264 keyframes (IDR frames) are self-contained. No dependencies on previous frames. Just drop all the P-frames on the server side, send only keyframes, get ~1fps of corruption-free video. Perfect for low-bandwidth fallback!

We added a `keyframes_only` flag. We modified the video decoder to check `FrameType::Idr`. We set GOP to 60 (one keyframe per second at 60fps). We tested.

We got exactly ONE frame.

One single, beautiful, 1080p IDR frame. Then silence. Forever.

```
[WebSocket] Keyframe received (frame 121), sending
[WebSocket] ...
[WebSocket] ...
[WebSocket] It's been 14 seconds why is nothing else coming
[WebSocket] Failed to send audio frame: Closed
```

*checks Wolf logs* — encoder still running

*checks GStreamer pipeline* — frames being produced

*checks Moonlight protocol layer* — **nothing coming through**

We’re using [Wolf](https://games-on-whales.github.io/wolf/stable/), an excellent open-source game streaming server (seriously, the documentation is great). But our WebSocket streaming layer sits on top of the Moonlight protocol, which is reverse-engineered from NVIDIA GameStream. Somewhere in that protocol stack, *something* decides that if you’re not consuming P-frames, you’re not ready for more frames. Period.

We poked around for an hour or two, but without diving deep into the Moonlight protocol internals, we weren’t going to fix this. The protocol wanted all its frames, or no frames at all.

**“What if we implement proper congestion control?”**

*looks at TCP congestion control literature*

*closes tab*

**“What if we just... don’t have bad WiFi?”**

*stares at enterprise firewall that’s throttling everything*

---

## Act IV: Depression

One late night, while debugging why the stream was frozen again, I opened our screenshot debugging endpoint in a browser tab:

```
GET /api/v1/external-agents/abc123/screenshot?format=jpeg&quality=70
```

The image loaded instantly.

A pristine, 150KB JPEG of the remote desktop. Crystal clear. No artifacts. No waiting for keyframes. No decoder state. Just... pixels.

I refreshed. Another instant image.

I mashed F5 like a degenerate. 5 FPS of perfect screenshots.

I looked at my beautiful WebCodecs pipeline. I looked at the JPEGs. I looked at the WebCodecs pipeline again.

No.

No, we are not doing this.

We are professionals. We implement proper video codecs. We don’t spam HTTP requests for individual frames like it’s 2009.

---

## Act V: Acceptance

typescript

```
// Poll screenshots as fast as possible (capped at 10 FPS max)
const fetchScreenshot = async () => {
  const response = await fetch(`/api/v1/external-agents/${sessionId}/screenshot`)
  const blob = await response.blob()
  screenshotImg.src = URL.createObjectURL(blob)
  setTimeout(fetchScreenshot, 100) // yolo
}
```

We did it. We’re sending JPEGs.

And you know what? **It works perfectly.**

---

## Why JPEGs Actually Slap

Here’s the thing about our fancy H.264 pipeline:

PropertyH.264 StreamJPEG SpamBandwidth40 Mbps (constant)100-500 Kbps (varies with complexity)StateStateful (corrupt = dead)Stateless (each frame independent)Latency sensitivityVery highDoesn’t careRecovery from packet lossWait for keyframe (seconds)Next frame (100ms)Implementation complexity3 months of Rust`fetch()` in a loop

A JPEG screenshot is **self-contained**. It either arrives complete, or it doesn’t. There’s no “partial decode.” There’s no “waiting for the next keyframe.” There’s no “decoder state corruption.”

When the network is bad, you get... fewer JPEGs. That’s it. The ones that arrive are perfect.

And the size! A 70% quality JPEG of a 1080p desktop is like **100-150KB**. A single H.264 keyframe is 200-500KB. We’re sending LESS data per frame AND getting better reliability.

---

## The Hybrid: Have Your Cake and Eat It Too

We didn’t throw away the H.264 pipeline. We’re not *complete* animals.

Instead, we built adaptive switching:

1. **Good connection** (RTT < 150ms): Full 60fps H.264, hardware decoded, buttery smooth
2. **Bad connection detected**: Pause video, switch to screenshot polling
3. **Connection recovers**: User clicks to retry video

The key insight: **we still need the WebSocket for input**.

Keyboard and mouse events are tiny. Like, 10 bytes each. The WebSocket handles those perfectly even on a garbage connection. We just needed to stop sending the massive video frames.

So we added one control message:

json

```
{"set_video_enabled": false}
```

Server receives this, stops sending video frames. Client polls screenshots instead. Input keeps flowing. Everyone’s happy.

15 lines of Rust. I am not joking.

rust

```
if !video_enabled.load(Ordering::Relaxed) {
    continue; // skip frame, it's screenshot time baby
}
```

---

## The Oscillation Problem (Lol)

We almost shipped a hilarious bug.

When you stop sending video frames, the WebSocket becomes basically empty. Just tiny input events and occasional pings.

**The latency drops dramatically.**

Our adaptive mode sees low latency and thinks: “Oh nice! Connection recovered! Let’s switch back to video!”

Video resumes. 40Mbps floods the connection. Latency spikes. Mode switches to screenshots.

Latency drops. Mode switches to video.

Latency spikes. Mode switches to screenshots.

**Forever. Every 2 seconds.**

The fix was embarrassingly simple: once you fall back to screenshots, **stay there until the user explicitly clicks to retry**.

typescript

```
setAdaptiveLockedToScreenshots(true) // no oscillation for you
```

We show an amber icon and a message: "Video paused to save bandwidth. Click to retry."

Problem solved. User is in control. No infinite loops.

---

## Ubuntu Doesn't Ship JPEG Support in grim Because Of Course It Doesn't

Oh, you thought we were done? Cute.

`grim` is a Wayland screenshot tool. Perfect for our needs. Supports JPEG output for smaller files.

Except Ubuntu compiles it without libjpeg.
```
$ grim -t jpeg screenshot.jpg
error: jpeg support disabled
```

*incredible*

So now our Dockerfile has a build stage that compiles grim from source:

dockerfile

```
FROM ubuntu:25.04 AS grim-build
RUN apt-get install -y meson ninja-build libjpeg-turbo8-dev ...
RUN git clone https://git.sr.ht/~emersion/grim && \
    meson setup build -Djpeg=enabled && \
    ninja -C build
```

We're building a screenshot tool from source so we can send JPEGs in 2025. This is fine.

---

## The Final Architecture
```
┌─────────────────────────────────────────────────────────────┐
│                     User's Browser                          │
├─────────────────────────────────────────────────────────────┤
│  WebSocket (always connected)                               │
│  ├── Video frames (H.264) ──────────── when RTT < 150ms    │
│  ├── Input events (keyboard/mouse) ── always               │
│  └── Control messages ─────────────── {"set_video_enabled"} │
│                                                              │
│  HTTP (screenshot polling) ──────────── when RTT > 150ms    │
│  └── GET /screenshot?quality=70                             │
└─────────────────────────────────────────────────────────────┘
```

**Good connection:** 60fps H.264, hardware accelerated, beautiful **Bad connection:** 2-10fps JPEGs, perfectly reliable, works everywhere

The screenshot quality adapts too:

* Frame took >500ms? Drop quality by 10%
* Frame took <300ms? Increase quality by 5%
* Target: minimum 2 FPS, always

---

## Lessons Learned

1. **Simple solutions often beat complex ones.** Three months of H.264 pipeline work. One 2am hacking session the night before production deployment: “what if we just... screenshots?”
2. **Graceful degradation is a feature.** Users don’t care about your codec. They care about seeing their screen and typing.
3. **WebSockets are for input, not necessarily video.** The input path staying responsive is more important than video frames.
4. **Ubuntu packages are missing random features.** Always check. Or just build from source like it’s 2005.
5. **Measure before optimizing.** We assumed video streaming was the only option. It wasn’t.

---

## Try It Yourself

Helix is open source: [github.com/helixml/helix](https://github.com/helixml/helix)

The shameful-but-effective screenshot code:

* `api/cmd/screenshot-server/main.go` — 200 lines of Go that changed everything
* `MoonlightStreamViewer.tsx` — React component with adaptive logic
* `websocket-stream.ts` — WebSocket client with `setVideoEnabled()`

The beautiful H.264 pipeline we’re still proud of:

* `moonlight-web-stream/` — Rust WebSocket server
* Still used when your WiFi doesn’t suck

---

*We’re building Helix, open-source AI infrastructure that works in the real world — even on terrible WiFi. We started by [killing WebRTC](LINK-TO-PART-1), then we killed our replacement. Sometimes the 15-year-old solution is the right one.*

*Star us on GitHub: [github.com/helixml/helix](https://github.com/helixml/helix)*

Thanks for reading HelixML! Subscribe for free to receive new posts and support my work.

Subscribe

4

1

1

Share

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Neural Foundry's avatar](https://substackcdn.com/image/fetch/$s_!Iq23!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2247318d-d8d3-455a-b1a7-8cd6168b3c25_360x360.png)](https://substack.com/profile/307819638-neural-foundry?utm_source=comment)

[Neural Foundry](https://substack.com/profile/307819638-neural-foundry?utm_source=substack-feed-item)

[5d](https://blog.helix.ml/p/we-mass-deployed-15-year-old-screen/comment/189322515 "Dec 18, 2025, 9:27 PM")

Liked by Luke Marsden

The oscillation bug you almost shipped is gold. I've hit similar feedback loops where the monitoring itself changes system behavior enough to flip the condition being moniterd. The decision to make fallback user-initiated instead of automatic is the right call. The stateless nature of JPEG frames solving TCP head-of-line blocking is understated here too since each frame is self-contained you sidestep the whole "waiting for retransmit" problem that kills H.264 under packet loss.

Expand full comment

ReplyShare

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2025 Luke Marsden · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture

This site requires JavaScript to run correctly. Please [turn on JavaScript](https://enable-javascript.com/) or unblock scripts

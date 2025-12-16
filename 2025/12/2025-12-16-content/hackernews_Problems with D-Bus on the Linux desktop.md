---
source: hackernews
title: Problems with D-Bus on the Linux desktop
url: https://blog.vaxry.net/articles/2025-dbusSucks
date: 2025-12-16
---

Vaxry's blog

a programming blog!

D-Bus is a disgrace to the Linux desktop

15 XII 2025

25.4k

There has been quite a bunch of interest in this post, I've added a FAQ section at the bottom.
 I will be adding stuff there if more FAQs pop up. Thanks!

D-Bus was introduced by GNOME folks about 20 years ago. For software made only 20 years ago, as opposed to 40 like X, it's surprisingly almost equally as bad.

As a service, D-Bus is incredibly handy and useful, and overall, I believe the *idea* should absolutely be used by more apps. However, the implementation... oh boy.

# What is D-Bus?

Everyone has heard about D-Bus, but what is it, actually?

D-Bus' idea is pretty simple: let applications, services and other *things* expose methods or properties in a way that other apps can find them in one place, on the bus.

Let's say we have a service that monitors the weather. Instead of each app knowing how to talk to each weather service, or even worse, implementing one itself, it can connect to the bus, and see if any service on the system exposes some weather API, then use it to get weather.

Great, right? And yeah, the idea is wonderful.

# What went wrong?

D-Bus is a *lenient*, *unorganized* and *forgiving* bus. Those three add to one of the biggest, fundamental, and conceptual blunders to any protocol, language or system.

The most important blunders are:

* Objects on the bus can register whatever they want.
* Objects on the bus can call whatever they want, however they want, whenever they want.
* The protocol allows and even in a sense incentivises vendor-specific unchecked garbage.

What this means in practice is the definition of **"Garbage in, garbage out"**.

## D-Bus standards, part 1

Okay, apps need to communicate, right? Well, in some way right? Where do we find the way?

Uhh... somewhere online, probably. Nobody actually knows because some of them are here, some there, many are unfinished, unreadable, or convoluted garbage docs, and no client follows them anyways.

Let's take a look at some gems. *These are actual docs*

![](../resource/articleDbus/image1.png)

Truly secure.

![](../resource/articleDbus/image2.png)

I guess service implementors should learn telepathy.

![](../resource/articleDbus/image3.png)

So is it a draft or widely used?

D-Bus standards are a mess. And that's if we assume that implementors on both sides actually follow them (they often don't, as we will learn in a moment...)

## D-Bus standards, part 2

Okay, let's say we have a standard and we understand it. Great! Now...

**nobody gives a shit**, literally. Even if you read a spec, nothing, literally nothing, guides, ensures, or helps you stick to it. NOTHING. You send anonymous calls with whatever bullshit you want to throw in.

Let me tell you a story...

Back when I was writing xdg-desktop-portal-hyprland, I had to use a few dbus protocols (xdg portals run on dbus) to implement some of the communication. If we go to the portal documentation, we can find the protocols.

Great! So I implemented it. It worked more-or-less. Then, I implemented *restore tokens*, which allow the app to restore its previously saved share configuration. And here, dbus falls apart.

None of the apps, I repeat, *fucking none* followed the spec. I wrote a spec-compliant mechanism and *nothing fucking used it*. Why? Simple, they all used a different spec, which came out of *fucking* nowhere, I legit couldn't find a single doc with it. What I ended up doing was I looked at KDE which already had an impl and mimic'd that.

What the actual fuck. "Spec" my ass.

Fun fact: **THIS IS STILL THE CASE!** The spec advertises a "restore\_token" string prop on SelectSources and Start, where no app does this and uses "restore\_data" in "options".

## D-Bus standards, part 3

Let me just say one word: *variants*. What in the actual, everloving fuck? Half of D-Bus protocols have either this BS, or some "a{sv}" (array of string + variant) passed somewhere.

Putting something like this, even *allowing* that in a core spec should be subject to a permanent ban from creating software. What this allows, and even incentivises, is for apps to send random shit over the wire and hope the other side understands it. (see the example above in part 2, prime dbus) This has been tried many times, most notably in X with atoms, and it has time and time again proven to only bring disaster.

## D-Bus standards, part 4

Ever heard of permissions? Neither have D-Bus developers. D-Bus is as insecure as it gets. Everybody sees everything and calls whatever. If the app doesn't have a specific security mechanism, cowabunga it is. Furthermore, there is no such thing as a "rejection" in a universal sense. Either the protocol invents its own "rejection" or just... something happens, god knows what, actually.

This is one of the prime reasons flatpak apps can *not* see your session bus.

## D-Bus standards, part 5

Ever seen kwallet or gnome-keyring? Yeah, these things. These are supposed to be "secret storage" for things like signing keys, passwords, etc. They can be protected by a password, which means they are secure... right?

No. No, they aren't. These secrets may be encrypted on disk, which technically prevents them from being stolen if your laptop is stolen. If you just cringed at that because disk encryption has been a thing for 20 years now or so, you're not alone.

However, the best thing is this: *any app* on the bus can read *all secrets* in the store if the store is unlocked. No, this is not a fucking joke. Once you input that password, any app can just read all of them without you noticing.

This is the *real* stance of GNOME developers on the issue:

![](../resource/articleDbus/image4.png)

Honestly, I am at a loss of words as to how to describe this without being *extremely* rude.

Security so good microsoft might steal it for their recall.

# Enough is enough

I've had enough of D-Bus in my apps. I would greatly benefit from a session (and later, system) bus for my ecosystem, but I will not stand the absolute shitfest that D-Bus is.

That is why, I've decided to take matters into my own hands. I am writing a new bus. From the ground up, with zero copying, interop, or other recognition of D-Bus. There are *so many* stupid ideas crammed into D-Bus that I do not wish to have any of them poison my own.

## XKCD 927

![](https://imgs.xkcd.com/comics/standards.png "xkcd927")

A lot of people quote this xkcd comic for each new implementation. However, this is not exactly the same.

For example, with wayland, when you switch, you abandon X. You cannot run an X11 session together with a wayland one, simply not how it works.

You *can*, however, run two session buses. Or three. Or 17. Nothing stops you. That's why *gradual* migration is absolutely possible. Sure, these buses can't talk to each other, but you can also create a proxy client that can "translate" dbus APIs into new ones.

## Wire

The first thing I focused on was hyprwire. I needed a wire protocol anyways for hypr\* stuff like hyprlauncher, hyprpaper, etc.

The wire protocol is inspired by how Wayland decided to handle things. Its most important strengths are:

* consistency: the wire itself enforces types and message arguments. No "a{sv}", no "just send something lol"
* simplicity: the wire protocol is fast and simple. Nobody needs complicated struct types, these just add annoyances.
* speed: fast handshakes and protocol exchanges, connections are estabilished very quickly.

Hyprwire is already used for IPC in hyprpaper, hyprlauncher and parts of hyprctl, and has been serving us well.

## Bus

The bus is called hyprtavern, as it is not *exactly* what D-Bus is, but it's more like a tavern.

Apps register *objects* on the bus, which have exposed protocols and key properties defined by the protocols. These objects can be discovered by other apps connecting to the bus.

In a sense, hyprtavern acts like a *tavern*, where each app is a *client*, that can advertise the *languages* they speak, but also go up to someone else and *strike up a conversation* if they have a language in common.

Some overall improvements over D-Bus, in no particular order:

* Permissions: baked in, in-spec permissions. Suitable for exposing to sandboxed apps by default.
* Strict protocols: don't know the language? Don't poison the wire. Worth noting this does not stop you from making your own extensions, it just enforces you stay in-spec.
* Simplified API: D-Bus has a lot of stupid ideas (shoutout broadcast) that we intentionally do *not* inherit.
* Way better defaults: The core spec also includes a few things that are optional (and dumb) in D-Bus like an actually secure kv store.

### Kv

With relation to the Secrets API discussed a bit above, I wanted to mention kv.

hyprtavern-kv is the default implementation of the *core* protocol for a kv store. A kv store is a "key-value" store, which means apps register values for "keys", e.g. "user\_secret\_key = password".

This is essentially what D-Bus Secrets API does, but instead of being a security joke, it's actually secure by-design.

Any app can register secrets, which *only it can read back*. Secrets *cannot be enumerated*. This means that when "/usr/bin/firefox" sets a "passwords:superwebsite.com = animebooba", an app called "~/Downloads/totally\_legit.sh" can not see the value, or the key, or that firefox even set anything.

This also (will) work with Flatpak, Snap and AppImage applications by additionally using their Flatpak ID, Snap ID or AppImage path respectively. This is not implemented, but planned.

This kv store is always encrypted, but a default password can be used which means it will be unlocked by default and the store file can be trivially decrypted. The difference is that if you set a password here, it will actually be secure, even if an app with access to the bus tries to steal all of the secrets.

Additionally, this protocol is *core*. It *must* be implemented by the bus, which means all apps can benefit from a secure secret storage.

## Is hyprtavern ready?

No, absolutely not. I started work on it just recently, and I still need to cook a bit. It's coming though, really!

I hope to get it widely used within hypr\* by 0.54 of hyprland (that is the release after the upcoming 0.53).

## Do I expect adoption?

No, definitely not at the beginning. But, it's an easier transition than X11 -> Wayland, and I didn't expect Hyprland to be widely adopted either, but here we are.

Time will tell. All I can say is that it is *just better* than D-Bus.

An important part of adoption will probably be bindings to other languages. The libraries are all in C++, but since they aren't very big (by design), making Rust / Go / Python bindings shouldn't be hard for someone experienced with those languages.

The wire format is also simple and open, so you could also write a Memory-Safe™ libhyprwire in Rust for example.

# Closer

D-Bus has been an annoyance of mine for years now, but I finally have the ecosystem and resources to write something to replace it.

Let's hope we can make the userspace a bit nicer to work with :)

# Addendum

This post is quickly gathering attention so I will answer some FAQs:

Reinventing the wheel won't help

The wheel is fundamentally broken. D-Bus is *unfixable* due to its core principles being terrible.

Where docs?

As I've said, hyprtavern is a heavy WIP. Once it is ready for app developers, which I hope to be done with within a month, I will write extensive docs about both the wire protocol (so that you can implement it yourself if you don't like libhyprwire) and the tavern itself.

Why not use wayland?

I've implemented a few improvements to hyprwire for bus usage (e.g. array types), and additionally wayland is not meant to be a generic IPC protocol. Connecting is restricted to sockets and WAYLAND\_DISPLAY, for example. One could fork it, but at this point, it's better to write your own impl.

Can we add dbus-compatibility translators?

Yes. You can write a for example hyprtavern-dbus-notification-proxy which sets up a dbus notification service and exposes the events as an appropriate tavern protocol. Worth noting of course such a protocol doesn't exist yet as I am working on the core spec atm. There will be though.

Why C++ and not memory safe rust?

Because I am a C++ dev. You are free to reimplement the bus / wire in Rust. You are also free to write bindings. BSD-3.

Hyprland (and related) historically have had less and less memory issues over time thanks to our shift to hyprutils and very common (almost religious) refcounting practices. However, nothing's stopping you from rewriting things in Rust.

The portal docs are actually correct, you just were reading the wrong ones

Yes, a person on hackernews named mahkoh pointed this out (thanks!). This doesn't change the fact that:

* the docs are poorly separated, such that I could not easily find that information.
* the names for things for app -> portal and portal -> portal impl are different (wtf, what are you guys smoking?)
* the website he links didn't exist (IIRC at all, or at least in its current state, IIRC it was a mostly blank page) back when I was impl'ing it.
* most importantly: **DBus allows you to do whatever while a real protocol would enforce the types outright and forbid invalid usage**.

Fragmentation, gnome and kde have different needs

Hasn't stopped them from both using D-Bus to this day. Apparently you can have one bus for both.

What about symlinks in paths?

Just resolve them. For chrooted apps, both Linux and BSD provide a way to get root from pid. I've been told it will break on Nix, but I will let the Nix folks figure this one out as I don't use it.

Where can we follow development or see the protocol?

Hyprwire's wire format is not yet documented, but it's quite simple. Docs will be written by me once tavern's ready.

For the core tavern protocol spec WIP, please see here. Please note it's of course a WIP so breaking changes do happen as I try to accomodate more usecases. Feedback is welcome though, feel free to leave feedback if you're an app or DE developer with a specific usecase in mind.

---

Questions, comments, mistakes? Ping me a mail at vaxry [at] vaxry.net and I'll get back to ya.

---

Made with literal caveman technology by vaxry, 2025.

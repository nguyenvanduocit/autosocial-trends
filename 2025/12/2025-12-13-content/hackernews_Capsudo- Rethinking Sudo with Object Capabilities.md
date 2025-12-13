---
source: hackernews
title: Capsudo: Rethinking Sudo with Object Capabilities
url: https://ariadne.space/2025/12/12/rethinking-sudo-with-object-capabilities.html
date: 2025-12-13
---

[![Profile Picture](https://avatars.micro.blog/avatars/2025/05/1725515.jpg)](https://ariadne.space/)

# Ariadne's Space

Follow [@ariadne on Micro.blog](https://micro.blog/ariadne).

* [Home](https://ariadne.space/)
* [About](/about/)
* [Archive](/archive/)

# Rethinking sudo with object capabilities

Dec 12, 2025 · 7 min read

I hate `sudo` with a passion. It represents everything I find offensive about the modern Unix security model:

* like `su`, it must be a SUID binary to work
* it is monolithic: everything `sudo` does runs as `root`, there is no privilege separation
* it uses a non-declarative and non-hierarchical configuration format leading to forests of complex access-control policies and user errors due to lack of concision
* it supports plugins to extend the policy engine which run directly in the privileged SUID process

I could go on, but hopefully you get the point. Alpine moved to `doas` as the default privilege escalation tool several years ago, in Alpine 3.15, because of the large attack surface that `sudo` brings due to its design.

Systems built around identity-based access control tend to rely on ambient authority: policy is centralized and errors in the policy configuration or bugs in the policy engine can allow attackers to make full use of that ambient authority. In the case of a SUID binary like `doas` or `sudo`, that means an attacker can obtain root access in the event of a bug or misconfiguration.

What if there was a better way? Instead of thinking about privilege escalation as becoming root for a moment, what if it meant being handed a narrowly scoped capability, one with just enough authority to perform a specific action and nothing more? Enter the [object-capability model](https://en.wikipedia.org/wiki/Object-capability_model).

In an object-capability system, there is no global decision point that asks who you are and what you might be allowed to do. Authority is explicit and local: a program can only perform an action if it has been given the capability to do so. This makes privilege boundaries visible, composable, and far easier to reason about, shifting privilege escalation from a question of identity to a question of possession.

Inspired by the object-capability model, I’ve been working on a project named [capsudo](https://github.com/kaniini/capsudo). Instead of treating privilege escalation as a temporary change of identity, capsudo reframes it as a mediated interaction with a service called `capsudod` that holds specific authority, which may range from full root privileges to a narrowly scoped set of capabilities depending on how it is deployed.

## Delegating root privilege with object capabilities

What does that look like in practice? First, let’s consider a system service which needs to perform a few privileged operations, such as mounting and unmounting filesystems, and how capsudo can be used to provide capabilities to that service. With capsudo, we have a few different options. We could, for example, grant generic `mount` and `umount` capabilities, or alternatively we could grant constrained `mount` and `umount` capabilities to specific device nodes instead.

First, let’s take a look at what generic `mount` and `umount` capabilities would look like, as it is a good example for showing how constrained capabilities work. To begin with, consider a volume management service running under the `mountd` user. We will grant capabilities to that `mountd` user to then invoke using `capsudo` by running a few instances of the `capsudod` daemon.

```
root# capsudod -o mountd:mountd -s /run/user/mountd/cap/mount -- mount &
root# capsudod -o mountd:mountd -s /run/user/mountd/cap/umount -- umount &
```

You might notice that the capabilities above have had commands bound to them. This is an important feature of capsudo which I will elaborate on in a moment.

Let’s say that the user has plugged in a USB stick and wants it to be mounted to `/media/usb`. To do this with capsudo, the volume manager simply makes use of the `/run/user/mountd/cap/mount` capability which has been delegated to it:

```
mountd$ capsudo -s /run/user/mountd/cap/mount -- /dev/sdb1 /media/usb
```

What is going on here? When `capsudod` was started, it bound the capability it provides to the `mount` command by setting the executable it will run. This means that the `/run/user/mountd/cap/mount` capability cannot run any other command besides `mount`. This is technically a suboptimal delegation, however. So let’s fix that delegation by stopping the `capsudod` process and fixing it:

```
root# capsudod -s /run/user/mountd/cap/mount -- /usr/sbin/mount
```

Now when the capability is invoked, `capsudo` will run `/usr/sbin/mount` directly rather than checking the `PATH` environment variable it was spawned with.

We can build on this by creating a specific capability for mounting the device node, and another for unmounting the specific mount point:

```
root# capsudod -s /run/user/mountd/cap/mount-dev-sdb1 -- /usr/sbin/mount /dev/sdb1
root# capsudod -s /run/user/mountd/cap/umount-media-usb -- /usr/sbin/umount /media/usb
```

These would then be invoked as one would expect:

```
mountd$ capsudo -s /run/user/mountd/cap/mount-dev-sdb1 -- /media/usb
mountd$ capsudo -s /run/user/mountd/cap/umount-media-usb
```

So in essence a *capability* is represented as a Unix socket, an optional argv list and optionally a set of mandatory environmental variables, creating a very composable interface for delegating authority.

## Non-root delegations, or service accounts meet service capabilities

Now let’s talk about a scenario where traditionally root privilege is not required: service accounts.

Suppose we have a web application deployment system where developers are allowed to update files in a specific directory and restart a service, but otherwise shouldn’t have administrative access to the system. Traditionally, this might still be implemented using `sudo`, despite the fact that no global privileges are actually needed.

With capsudo, we can instead run the `capsudod` daemon under a dedicated service account which only owns the resources it is meant to manage.

Assume a deployment service running under the `www-deployment` user, which owns `/srv/www/app` and is allowed to reload the uWSGI service via a capability delegated to the `www-deployment` user. We can start `capsudod` instances under that user directly:

```
root# capsudod -o www-deployment:www-deployment \
   -s /run/user/www-deployment/cap/service-uwsgi \
   -- /usr/sbin/rc-service uwsgi &
www-deployment$ capsudod -o www-deployment:www-developers \
   -s /run/user/www-deployment/cap/update-site -- /usr/bin/rsync -a &
www-deployment$ capsudod -o www-deployment:www-developers \
   -s /run/user/www-deployment/cap/reload-site -- \
   /usr/bin/capsudo -s /run/user/www-deployment/cap/service-uwsgi \
      -- reload &
```

A developer in the `www-developers` group might then invoke these capabilities:

```
dev$ capsudo -s /run/user/www-deployment/cap/update-site \
  -- ./build/ /srv/www/app/
dev$ capsudo -s /run/user/www-deployment/cap/reload-site
```

### Unpacking the delegations

There is a lot going on here, so let’s walk through it step by step.

First, the system administrator delegates a small amount of authority to the `www-deployment` service account. This is done by running a `capsudod` instance that is able to manage the uWSGI service:

```
root# capsudod -o www-deployment:www-deployment \
  -s /run/user/www-deployment/cap/service-uwsgi \
  -- /usr/sbin/rc-service uwsgi &
```

This capability is owned by the `www-deployment` user and allows exactly one operation: invoking the system’s service manager to act on the `uwsgi` service. No other services can be touched, and no other commands can be executed through this capability.

Second, the `www-deployment` account uses that authority to construct more narrowly scoped capabilities for others. Two additional `capsudod` instances are started under the `www-deployment` account, but with ownership granted to the `www-developers` group:

```
www-deployment$ capsudod -o www-deployment:www-developers \
   -s /run/user/www-deployment/cap/update-site -- /usr/bin/rsync -a &

www-deployment$ capsudod -o www-deployment:www-developers \
   -s /run/user/www-deployment/cap/reload-site -- \
   /usr/bin/capsudo -s /run/user/www-deployment/cap/service-uwsgi -- reload &
```

The first of these allows developers to update the application files using `rsync`, but only through the `www-deployment` account’s existing filesystem permissions. The second is more interesting: it does not directly reload the service. Instead, it *delegates a constrained use* of the previously granted uWSGI capability.

When a developer invokes `reload-site`, they are not calling `rc-service` themselves, and they are not interacting with the system service manager directly. They are invoking a capability that is itself *built on top of another capability*, with additional constraints applied.

The important property here is that authority only ever moves downward in scope. It is possible to further delegate a subset of the authority granted by a capability, but not more. This kind of authority layering is a natural fit for the object-capability model, but it is awkward and fragile to express with identity-based access control.

Identity-based access control asks who should be allowed to act. Object-capability systems ask where authority should live and how it should flow. `capsudo` today is an exploration of what happens when you take the second question seriously, treating privilege escalation as explicit delegation and opening the door to further refinements like passing concrete resources, such as pre-opened file descriptors, instead of whole identities.

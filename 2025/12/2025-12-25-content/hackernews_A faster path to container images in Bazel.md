---
source: hackernews
title: A faster path to container images in Bazel
url: https://www.tweag.io/blog/2025-12-18-rules_img/
date: 2025-12-25
---

[![Tweag](data:image/svg+xml;base64...)](/)

[News](/news)

Capabilities

![Dropdown arrow](data:image/svg+xml;base64...)

[Functional Engineering](/functional-engineering)[Scalable Builds](/scalable-builds)

Open source

![Dropdown arrow](data:image/svg+xml;base64...)

[Technical groups](/groups)[Contributions and projects](/opensource)

[Careers](https://moduscreate.com/careers)[Research](/research)[Blog](/blog)[Contact](/contact)[Modus Create](https://moduscreate.com)

[News](/news)

Capabilities

![Dropdown arrow](data:image/svg+xml;base64...)

[Functional Engineering](/functional-engineering)[Scalable Builds](/scalable-builds)

Open source

![Dropdown arrow](data:image/svg+xml;base64...)

[Technical groups](/groups)[Contributions and projects](/opensource)

[Careers](https://moduscreate.com/careers)[Research](/research)[Blog](/blog)[Contact](/contact)[Modus Create](https://moduscreate.com)

## Announcing rules\_img: a faster path to container images in Bazel

18 December 2025 — by Malte Poll

[tools](/tag/tools)[build-systems](/tag/build-systems)[bazel](/tag/bazel)

Say you have a Bazel project that builds a web application, and you want to deploy it as a Docker container. The app is already built by Bazel, so you just need to package it into an image with the right base layers and configuration. This should be quick. Bazel is good at this sort of thing. But when you add container image building to your setup, something surprising happens: your builds start downloading gigabytes of base image data, your CI slows down, and pushing images feels slow. This is the story of why that happens and how [`rules_img`](https://github.com/bazel-contrib/rules_img) fixes it.

> Prefer watching to reading? [The content of this post is also available in video form](https://www.youtube.com/watch?v=biYXmAv4Ppk).

## The components

Before we dive in, we need to establish where data lives and where it moves. There are three main players:

* **The registry** (like Docker Hub or gcr.io): A remote server that stores container images. You download base images from here and push your built images back to it.
* **Your local machine**: Where you run `bazel build` or `bazel run`. This is your laptop or workstation.
* **Remote execution and remote cache**: A remote caching backend (like Aspect Workflows, BuildBuddy, EngFlow, or Google’s RBE) that runs Bazel actions on remote machines and caches the results. Optional, but common in CI and larger projects.

The core tension is simple: to build a container image that extends a base image, you need information about that base. The question is how much information, and where does it need to be?

## The scenario

Here’s what building a container image looks like with `rules_oci`, the current recommended approach. I’ll show the data flow explicitly:

```
# Pull a base image
# → Downloads manifest + config + all layer blobs from registry to local machine
pull(
    name = "ubuntu",
    image = "index.docker.io/library/ubuntu:24.04",
    digest = "sha256:1e622c5...",
)

# Build your image
# → Creates a directory containing all blobs (base layers + your layer)
# → With remote execution: all blobs are inputs and outputs of this actions
oci_image(
    name = "app_image",
    base = "@ubuntu",  # References the complete local directory
    tars = [":app_layer.tar"],
    entrypoint = ["/app/bin/server"],
)

# Push to registry
# → Downloads all image blobs from remote cache to local machine
# → Uploads missing blobs from local machine to registry
oci_push(
    name = "push",
    image = ":app_image",
    repository = "gcr.io/my-project/app",
)
```

**Data flow summary:**

1. Registry → Local machine: full base image (hundreds of MB)
2. Local machine → Remote cache: full base image (anything that’s not already cached)
3. Remote cache → Remote Executor (creating an image): full image (hundreds of MB)
4. Remote cache → Local machine: full image (hundreds of MB)
5. Local machine → Registry: missing layers

Here’s the same thing with `rules_img`:

```
# Pull a base image
# → Downloads only manifest + config JSON from registry to local machine (~10 KB)
# → Layer blobs stay in the registry
pull(
    name = "ubuntu",
    registry = "index.docker.io",
    repository = "library/ubuntu",
    tag = "24.04",
    digest = "sha256:1e622c5...",
)

# Build a layer
# → Writes layer tar + metadata to Bazel's content-addressable storage
# → With remote execution: layer blob stays in remote cache
image_layer(
    name = "app_layer",
    srcs = {
        "/app/bin/server": "//cmd/server",  # Bazel-built binary
        "/app/config": "//configs:prod",
    },
)

# Assemble the image
# → Writes manifest JSON referencing base layers + your layers (by digest)
# → Only metadata is read and written
image_manifest(
    name = "app",
    base = "@ubuntu",  # References only metadata, not blobs
    layers = [":app_layer"],  # References only metadata, not blobs
    entrypoint = ["/app/bin/server"],
)

# Push (at bazel run time, not build time)
# → Checks registry: which blobs are already present?
# → Streams only missing blobs: remote cache → local machine → registry
# → If layers are already in registry: nothing to transfer
image_push(
    name = "push_app",
    image = ":app",
    registry = "ghcr.io",
    repository = "my-project/app",
    tag = "latest",
)
```

**Data flow summary:**

1. Registry → Local machine: only manifest + config (~10 KB)
2. Local machine → Remote cache: only metadata on base images
3. Remote cache → Local machine → Registry: only missing blobs (often just your new layers)
4. Base layers (almost) never move through local machine or remote executors

## A two‑minute primer on images

An OCI image is a bundle of metadata and bytes. The bytes live in *layers*, which are compressed tar archives that encode file additions and deletions. The metadata lives in three JSON objects:

* The *config*: what to run, environment variables, user, working directory, and the list of uncompressed layer digests (also called diff IDs)
* The *manifest*: pointers to one config and many layer blobs, identified by digest, size, and media type
* The *index*: for multi‑architecture images, a list of per‑platform manifests

Tags in a registry point at a manifest digest. The digests are content-addressed, so the same bytes always mean the same name everywhere.

[![A container image is just some tar files in a trenchcoat](/static/2c013ad4d476d25db7c0322b0665dd58/fcda8/layers_in_a_trenchcoat.png "A container image is just some tar files in a trenchcoat")](/static/2c013ad4d476d25db7c0322b0665dd58/2bef9/layers_in_a_trenchcoat.png)

**How builds usually work.** `docker build` executes a Dockerfile *inside* a base image. Each step like `RUN`, `COPY`, or `ADD` runs against a snapshot of the previous root file system and produces a new layer. The final image is the base’s layers plus the layers created by those steps. This is convenient, but it assumes you have the base image bytes locally while you build.

**How Bazel thinks about it.** Bazel does not need to *run inside* the base at all. It builds your program artifacts the same way it always does[1](#fn-1), then assembles an image by writing a config and a manifest that reference the base image *by digest* alongside the new layers you produced. Bazel needs the base’s identity to compose a correct manifest and, later, to upload or load the image. But it doesn’t have to materialize the base layers during the build itself[2](#fn-2).

**Why this matters for performance.** Assembling an image is easy. It’s mostly JSON with a few checksums. The hard part is *data locality*: getting the right bytes to the right place at the right time. Do the executors have to download layers just to write a small manifest? Does a pusher really need to pull all blobs to a workstation before uploading them again? Does a local daemon have to ingest layers it already owns? `rules_img` answers those questions by moving metadata first and moving bytes only at the edges.

## The status quo: rules\_oci

The first major ruleset for building container images in Bazel was `rules_docker`, which integrated with every language ecosystem: Python, Node.js, Java, Scala, Groovy, C++, Go, Rust, and D. This approach proved extremely hard to maintain. Any change in a language ruleset could ripple into `rules_docker`. Today it is mostly unmaintained and lacks official bzlmod support.

The current recommendation is [`rules_oci`](https://github.com/bazel-contrib/rules_oci), which takes the opposite approach: use only off‑the‑shelf tools, maintain a strict complexity budget, and delegate layer creation to language rulesets or end users. This design results in a maintainable project with a narrow scope that’s easy to understand.

![Data transfers performed by rules_oci when pulling a base image](/ba15c362074a6f3d56c09848d334ea3b/rules_oci_repo_rule.svg)

Data transfers performed by rules\_oci when pulling a base image

Under the hood, `rules_oci` represents images as complete [OCI layouts](https://specs.opencontainers.org/image-spec/image-layout/) on disk. When you pull a base image, the repository rule downloads the full image—all blobs, all layers—into a tree artifact. When you build an image with `oci_image` or `oci_image_index`, the result is again a directory containing every blob of that image. Layers are always tar files, with no separate metadata to describe them, and the ruleset does not use Bazel providers to pass structured information between targets. This approach is simple and works well for local builds, but as we scaled to Remote Execution, we encountered bottlenecks that this design did not address.

## From bottlenecks to breakthroughs: how rules\_img works

I started with a simple goal: build container images in Bazel and let Remote Execution carry the weight. I used `rules_oci` in my experiments, the recommended way of building container images in Bazel today[3](#fn-3). I was surprised by the inefficiencies I saw. Repository rules that pulled base images ran again and again in CI, even when nothing had changed[4](#fn-4). My laptop shoveled data uphill to the remote cache before any real work could begin. Actions that only wrote a few lines of JSON insisted on dragging entire layer blobs along for the ride. When the build finally finished on RBE, Bazel downloaded every layer into a push tool’s runfiles, only to upload them to a registry a moment later. Loading images into Docker added insult to injury by ignoring layers that were already present. None of that felt like Bazel, so I ran experiments until a pattern emerged.

**The breakthrough: treat images as metadata first.** The key was to see the whole build as a metadata pipeline and to move bytes only at the edges. Keep base images shallow until you truly need a blob. Assemble manifests from digests and sizes, not gigabytes. Push and load by streaming from content‑addressable storage straight to the destination, and skip anything that already exists there. Once that clicked, the rest of the design fell into place.

**Pulling, without the pain.** Base pulls were the first time sink. In `rules_img`, the repository rule fetches only the manifest and config JSON files at build time. Just enough metadata to know what layers exist and their digests. The actual layer blobs are never downloaded during the build[5](#fn-5). They wait until the run phase when you `bazel run` a push or load target. CI becomes predictable, and Remote Execution doesn’t spend its morning downloading CUDA for the fourth time this week. Less data moves during builds, and the cache behaves like a cache.

**Stop hauling bytes uphill.** The next fix addressed the torrent of developer-to-remote uploads. We generate metadata-only providers wherever possible. The heavy blobs live in content-addressable storage (CAS) and stream later to whoever needs them, whether that’s a registry or a local daemon. Your workstation stops being a relay, cold starts are faster, and incremental builds are a breeze.

**Let manifests stay tiny.** Manifest assembly had been oddly heavyweight. We reshaped the graph so each layer is built in a single action that computes both the blob and the metadata that describes it[6](#fn-6). The layer blob stays in Bazel’s CAS, while only a small JSON descriptor (digest, size, media type) flows through the build graph. Downstream actions consume only this metadata during the build phase, so they schedule quickly, cache well, and avoid pulling gigabytes across executors. The manifests remain correct, and the path to them is light. The actual blob bytes only move later during `bazel run` when you push or load.

**Push without the round trip.** Pushing used to mean downloading all layers to a local tool and then sending them back up again. With `rules_img`, we defer all blob transfers to the run phase (`bazel run //:push`). The build phase only produces a lightweight push specification: a JSON file listing what needs pushing. When you run the pusher, it first asks the registry what blobs it already has, then streams only the missing ones directly from CAS. In environments where your registry speaks the same CAS protocol, the push is close to zero‑copy. For very large monorepos, you can even emit pushes as a side effect of Build Event Service uploads. The principle is simple. Build time produces metadata, run time moves bytes, and nothing passes through your workstation unnecessarily. See the [push strategies documentation](https://github.com/bazel-contrib/rules_img/blob/main/docs/push-strategies.md) for other configurations including direct CAS-to-registry transfers.

**Loading should be incremental.** `docker load` treats every import like a blank slate. When containerd is available, `rules_img` talks to its content store and streams only what is missing[7](#fn-7). It can also load a single platform from a multi‑platform image, which keeps feedback loops tight[8](#fn-8). If containerd isn’t available, we fall back to `docker load` and tell you what you’re giving up.

**Extra touches that add up.** Performance rarely comes from one trick alone. We use hardlink-based deduplication inside layers so identical files don’t bloat your tars. We support [eStargz](https://github.com/containerd/stargz-snapshotter/blob/main/docs/estargz.md) to make layers seekable and quick to start with the stargz snapshotter.

**Quick start.** If you want to try it, here is a minimal setup:

```
# MODULE.bazel
bazel_dep(name = "rules_img", version = "<version>")

pull = use_repo_rule("@rules_img//img:pull.bzl", "pull")

# Pulls manifest+config only (no layer blobs yet)
pull(
    name = "ubuntu",
    registry = "index.docker.io",
    repository = "library/ubuntu",
    tag = "24.04",
    digest = "sha256:1e622c5f073b4f6bfad6632f2616c7f59ef256e96fe78bf6a595d1dc4376ac02",
)
```

```
# BUILD.bazel
load("@rules_img//img:layer.bzl", "image_layer")
load("@rules_img//img:image.bzl", "image_manifest")

image_layer(
    name = "app_layer",
    srcs = {
        "/app/bin/server": "//cmd/server",
        "/app/config": "//configs:prod",
    },
    compress = "zstd",
)

image_manifest(
    name = "app_image",
    base = "@ubuntu",
    layers = [":app_layer"],
)
```

Optional `.bazelrc` speed dials if you like the metadata‑first defaults:

```
common --@rules_img//img/settings:compress=zstd
common --@rules_img//img/settings:estargz=enabled
common --@rules_img//img/settings:push_strategy=lazy
# Or: cas_registry / bes (see docs for setup)
```

## Conclusion: container images that feel native to Bazel

The performance gains are real. Pulling large base images on fresh machines takes seconds instead of minutes. Loading into Docker takes milliseconds for incremental updates instead of reloading the full image, which could waste 1–5 minutes in the workflows we examined. Manifest assembly actions run dramatically faster, especially on RBE systems that fetch inputs eagerly[9](#fn-9). Building push targets no longer destroys the benefits of Build without the Bytes. Where other rulesets might download gigabytes to your machine, `rules_img` downloads only a few kilobytes of metadata, saving many gigabytes in transfers and minutes per push. A comprehensive benchmark would warrant its own blog post given the wide matrix of possible configurations, RBE backends, image sizes, and network conditions.

Our aim with `rules_img` is straightforward: make Bazel feel native for container images, with no unnecessary bytes and no unnecessary waits. By treating images as metadata with on-demand bytes, we get faster CI, quieter laptops, and a build graph that scales without drama. Try it, tell us what flies, and tell us what still hurts. There’s more to tune, and we intend to keep tuning.

**Get started with rules\_img: [github.com/bazel-contrib/rules\_img](https://github.com/bazel-contrib/rules_img)**

---

1. Bazel actions only access explicitly declared inputs within the execroot, whilst `docker build` mounts all base image layers into the build environment. This means Bazel builds layers independently of base layers, though specific toolchains (like C++) can use workarounds such as `--sysroot` to simulate filesystem mounting.[↩](#fnref-1)
2. Building without mounting base layers risks creating binaries incompatible with the target image, as there’s no way to execute code atop existing layers during the build. The [container-structure-test](https://github.com/GoogleContainerTools/container-structure-test) tool addresses this by enabling tests on the final image, including running commands in a Docker daemon with assertions.[↩](#fnref-2)
3. `rules_oci` is a good ruleset for building container images in Bazel, and I am a happy user most of the time. However, I quickly noticed that it is only optimized for local execution. I want to stress that most of the issues I’m describing here only matter for the remote execution case.[↩](#fnref-3)
4. Repository rules downloading image blobs are inefficient in stateless CI environments where caches aren’t preserved between runs. Whilst CI can be configured to preserve these directories, `rules_img` avoids the problem entirely by fetching only metadata.[↩](#fnref-4)
5. This is configurable. If you really need access to base image layers at build time (for instance, to run a container structure test), you can set the `layer_handling` attribute accordingly ([docs](https://github.com/bazel-contrib/rules_img/blob/main/docs/pull.md#pull)).[↩](#fnref-5)
6. In `rules_oci`, manifest assembly receives entire layer tar files as inputs, transferring multi-gigabyte blobs with remote execution. `rules_img` instead generates small metadata files (containing digests and diff IDs) when writing layers, passing only this fixed-size metadata to downstream actions. Layer contents enter the build graph once, remain in CAS, while tiny metadata flows through subsequent actions.[↩](#fnref-6)
7. `docker load` requires a tar file with all layers, even those previously loaded. Whilst hacks exist to skip lower layers by abusing chain IDs, `rules_oci` doesn’t support this. `rules_img` interfaces directly with containerd’s content-addressable store to check blob existence and load only missing layers. Docker will soon expose its own content store via the socket ([moby/moby#44369](https://github.com/moby/moby/issues/44369)), making this approach more widely accessible.[↩](#fnref-7)
8. Platform filtering is not yet implemented for the containerd backend ([rules\_img#107](https://github.com/bazel-contrib/rules_img/issues/107)). The `docker load` fallback path does support platform filtering.[↩](#fnref-8)
9. Some RBE systems can make action inputs available lazily on-demand, while others fetch all declared inputs eagerly before the action runs. For the latter, avoiding multi-gigabyte layer inputs makes a dramatic difference in action scheduling and execution time.[↩](#fnref-9)

## Behind the scenes

Malte Poll

Malte is a software engineer with a background in security.
In the process of improving the supply chain security and reproducibility
of security-critical software, he has gained experience with Bazel and Nix.
He is passionate about building secure and reliable systems and enjoys
tinkering with immutable operating systems.

If you enjoyed this article, you might be interested in  [joining the Tweag team](/careers).

![](data:image/svg+xml;base64...)

Company

[About](https://moduscreate.com/about/)[Open Source](/opensource)[Careers](https://moduscreate.com/careers)[Contact Us](/contact)

What we do

[Functional Engineering](/functional-engineering)[Scalable builds](/scalable-builds)

Insights

[Blog](/blog)[Modus Blog](https://moduscreate.com/insights/blog/)[Research](/research)

Connect with us

[![GitHub](data:image/svg+xml;base64...)](https://github.com/tweag)[![YouTube](data:image/svg+xml;base64...)](https://www.youtube.com/c/tweag)[![X](data:image/svg+xml;base64...)](https://twitter.com/tweagio)[![LinkedIn](data:image/svg+xml;base64...)](https://www.linkedin.com/company/tweag-i-o/)[![Bluesky](data:image/svg+xml;base64...)](https://bsky.app/profile/tweag.io)[![Mastodon](data:image/svg+xml;base64...)](https://social.tweag.io/%40tweag)

© 2025 Modus Create, LLC

[Privacy Policy](https://moduscreate.com/privacy-policy/)[Sitemap](https://moduscreate.com/sitemap/)

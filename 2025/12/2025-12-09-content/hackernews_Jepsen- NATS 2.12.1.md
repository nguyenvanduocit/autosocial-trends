---
source: hackernews
title: Jepsen: NATS 2.12.1
url: https://jepsen.io/analyses/nats-2.12.1
date: 2025-12-09
---

# [JEPSEN](/)

[Blog](/blog)[Analyses](/analyses)[Talks](/talks)[Consistency](/consistency)[Services](/services)[Ethics](/analyses/ethics)

[![PDF download](/images/file-pdf.svg "PDF download")](/analyses/nats-2.12.1.pdf)

# NATS 2.12.1

Kyle Kingsbury

2025-12-08

[NATS](https://nats.io/) is a distributed streaming system. Regular NATS streams offer only best-effort delivery, but a subsystem, called JetStream, guarantees messages are delivered at least once. We tested NATS JetStream, version 2.12.1, and found that it lost writes if data files were truncated or corrupted on a minority of nodes. We also found that coordinated power failures, or an OS crash on a single node combined with network delays or process pauses, can cause the loss of committed writes and persistent split-brain. This data loss was caused (at least in part) by choosing to flush writes to disk every two minutes, rather than before acknowledging them. We also include a belated note on data loss due to process crashes in version 2.10.22, which was fixed in 2.10.23. NATS has now documented the risk of its default `fsync` policy, and the remaining issues remain under investigation. This research was performed independently by Jepsen, without compensation, and conducted in accordance with the [Jepsen ethics policy](https://jepsen.io/analyses/ethics).

# 1 Background

[NATS](https://nats.io/) is a popular streaming system. Producers [publish messages to streams](https://docs.nats.io/nats-concepts/core-nats/pubsub), and consumers subscribe to those streams, fetching messages from them. Regular NATS streams are allowed to drop messages. However, NATS has a subsystem called [JetStream](https://docs.nats.io/nats-concepts/jetstream), which [uses](https://docs.nats.io/running-a-nats-service/configuration/clustering/jetstream_clustering) the [Raft consensus algorithm](https://raft.github.io/) to replicate data among nodes. JetStream promises [“at least once”](https://docs.nats.io/nats-concepts/jetstream#exactly-once-semantics) delivery: messages may be duplicated, but acknowledged messages[1](#fn1) should not be lost.[2](#fn2) Moreover, JetStream streams are [totally ordered logs](https://docs.nats.io/nats-concepts/jetstream#persistent-and-consistent-distributed-storage).

JetStream is intended to [“self-heal and always be available”](https://docs.nats.io/nats-concepts/jetstream). The documentation also states that [“the formal consistency model of NATS JetStream is Linearizable”](https://docs.nats.io/nats-concepts/jetstream#persistent-and-consistent-distributed-storage). At most one of these claims can be true: the [CAP theorem](https://www.cs.princeton.edu/courses/archive/spr22/cos418/papers/cap.pdf) tells us that [Linearizable](https://jepsen.io/consistency/models/linearizable) systems can not be totally available.[3](#fn3) In practice, they tend to be available so long as a majority of nodes are non-faulty and communicating. If, say, a single node loses network connectivity, operations must fail on that node. If three out of five nodes crash, all operations must fail.

Indeed, a [later section](https://docs.nats.io/nats-concepts/jetstream#persistent-and-consistent-distributed-storage) of the JetStream docs acknowledges this fact, saying that streams with three replicas can tolerate the loss of one server, and those with five can tolerate the simultaneous loss of two.

> Replicas=5 - Can tolerate simultaneous loss of two servers servicing the stream. Mitigates risk at the expense of performance.

When does NATS guarantee a message will be durable? The [JetStream developer docs](https://docs.nats.io/using-nats/developer/develop_jetstream?q=sync#publish-to-a-stream) say that once a JetStream client’s `publish` request is acknowledged by the server, that message has “been successfully persisted”. The [clustering configuration documentation](https://docs.nats.io/running-a-nats-service/configuration/clustering/jetstream_clustering#the-quorum) says:

> In order to ensure data consistency across complete restarts, a quorum of servers is required. A quorum is ½ cluster size + 1. This is the minimum number of nodes to ensure at least one node has the most recent data and state after a catastrophic failure. So for a cluster size of 3, you’ll need at least two JetStream enabled NATS servers available to store new messages. For a cluster size of 5, you’ll need at least 3 NATS servers, and so forth.

With these guarantees in mind, we set out to test NATS JetStream behavior under a variety of simulated faults.

# 2 Test Design

We designed a [test suite](https://github.com/jepsen-io/nats) for NATS JetStream using the [Jepsen testing library](https://github.com/jepsen-io/jepsen), using [JNATS](https://github.com/nats-io/nats.java) (the official Java client) at version 2.24.0. Most of our tests ran in Debian 12 containers under LXC; [some tests](https://github.com/jepsen-io/nats/tree/4760f97f86350c5c9983478656dbcbcdade33817/antithesis) ran in [Antithesis](https://antithesis.com/), using the official NATS Docker images. In all our tests we created a single JetStream stream with a target replication factor of five. Per NATS’ recommendations, our clusters generally contained three or five nodes. We tested a variety of versions, but the bulk of this work focused on NATS 2.12.1.

The test harness [injected a variety of faults](https://github.com/jepsen-io/nats/blob/4760f97f86350c5c9983478656dbcbcdade33817/src/jepsen/nats/nemesis.clj), including process pauses, crashes, network partitions, and packet loss, as well as single-bit errors and truncation of data files. We limited file corruption to a minority of nodes. We also simulated power failure—a crash with partial amnesia—using the [LazyFS](https://github.com/dsrhaslab/lazyfs) filesystem. LazyFS allows Jepsen to drop any writes which have not yet been flushed using a call to (e.g.) `fsync`.

Our tests did not measure Linearizability or [Serializability](https://jepsen.io/consistency/models/serializable). Instead we ran [several producer processes](https://github.com/jepsen-io/nats/blob/4760f97f86350c5c9983478656dbcbcdade33817/src/jepsen/nats/queue.clj#L238-L266), each bound to a single NATS client, which published globally unique values to a single JetStream stream. Each message included the process number and a sequence number within that process, so message `4-0` denoted the first `publish` attempted by process `4`, message `4-1` denoted the second, and so on. At the end of the test we ensured all nodes were running, resolved any network partitions or other faults, subscribed to the stream, and [attempted to read all acknowledged messages from the the stream](https://github.com/jepsen-io/nats/blob/4760f97f86350c5c9983478656dbcbcdade33817/src/jepsen/nats/queue.clj#L189-L201). Each reader called `fetch` until it had observed (at least) the last acknowledged message published by each process, or timed out.

We measured JetStream’s at-least-once semantics [based on the union of all published and read messages](https://github.com/jepsen-io/nats/blob/4760f97f86350c5c9983478656dbcbcdade33817/src/jepsen/nats/queue.clj#L359-L424). We considered a message *OK* if it was attempted and read. Messages were *lost* if they were acknowledged as published, but never read by any process. We divided lost messages into three epochs, based on the first and last OK messages written by the same process.[4](#fn4) We called those lost before the first OK message the *lost-prefix*, those lost after all the last OK message the *lost-postfix*, and all others the *lost-middle*. This helped to distinguish between lagging readers and true data loss.

In addition to verifying each acknowledged message was delivered to at least one consumer across all nodes, we also checked the set of messages read by all consumers connected to a specific node. We called it *divergence*, or *split-brain*, when an acknowledged message was missing from some nodes but not others.

# 3 Results

We begin with a belated note on total data loss in version 2.10.22, then continue with four findings related to data loss and replica divergence in version 2.12.1: two with file corruption, and two with power failures.

## 3.1 Total Data Loss on Crash in 2.10.22 (#6888)

Before discussing version 2.12.1, we present a long-overdue finding from earlier work. In versions 2.10.20 through 2.10.22 (released 2024-10-17), we found that process crashes alone could cause the total loss of a JetStream stream and all its associated data. Subscription requests would return `"No matching streams for subject"`, and `getStreamNames()` would return an empty list. These conditions would persist for hours: [in this test run](https://github.com/user-attachments/files/20133999/20250509T191519.377-0500.zip), we waited 10,000 seconds for the cluster to recover, but the stream never returned.

Jepsen reported this issue to NATS as [#6888](https://github.com/nats-io/nats-server/issues/6888), but it appears that NATS had already identified several potential causes for this problem and resolved them. In [#5946](https://github.com/nats-io/nats-server/pull/5946), a cluster-wide crash occurring shortly after a stream was created could cause the loss of the stream. A new leader would be elected with a snapshot which preceded the creation of the stream, and replicate that empty snapshot to followers, causing everyone to delete their copy of the stream. In [#5700](https://github.com/nats-io/nats-server/pull/5700), tests running in [Antithesis](https://antithesis.com/) found that out-of-order delivery of snapshot messages could cause streams to be deleted and re-created as well. In [#6061](https://github.com/nats-io/nats-server/pull/6061), process crashes could cause nodes to delete their local Raft state. All of these fixes were released as a part of 2.10.23, and we no longer observed the problem in that version.

## 3.2 Lost Writes With `.blk` File Corruption (#7549)

NATS has several checksum mechanisms meant to detect data corruption in on-disk files. However, we found that single-bit errors or truncation of JetStream’s `.blk` files could cause the cluster to lose large windows of writes. This occurred even when file corruption was limited to just one or two nodes out of five. For instance, [file corruption in this test run](https://s3.amazonaws.com/jepsen.io/analyses/nats-2.12.1/20251116T061217-blk-bitflip.zip) caused NATS to lose 679,153 acknowledged writes out of 1,367,069 total, including 201,286 which were missing even though later values written by the same process were later read.

![A timeseries plot of write loss over time. A large block of writes is lost around sixty seconds, followed by a few which survive, and then the rest of the successfully acknowledged writes are lost as well.](nats-2.12.1/blk-bitflip-loss.png)

In some cases, file corruption caused the quiet loss of [just a single message](https://s3.amazonaws.com/jepsen.io/analyses/nats-2.12.1/20251116T030143-blk-bitflip-single-loss.zip). In others, writes vanished in large blocks. Even worse, bitflips could cause split-brain, where different nodes returned different sets of messages. In [this test](https://s3.amazonaws.com/jepsen.io/analyses/nats-2.12.1/20251120T093731-blk-bitflip-split-brain.zip), NATS acknowledged a total of 1,479,661 messages. However, single-bit errors in `.blk` files on nodes `n1` and `n3` caused nodes `n1`, `n3`, and `n5` to lose up to 78% of those acknowledged messages. Node `n1` lost 852,413 messages, and nodes `n3` and `n5` lost 1,167,167 messages, despite `n5`’s data files remaining intact. Messages were lost in prefix, middle, and postfix: the stream, at least on those three nodes, resembled Swiss cheese.

NATS is investigating this issue ([#7549](https://github.com/nats-io/nats-server/issues/7549)).

## 3.3 Total Data Loss With Snapshot File Corruption (#7556)

When we truncated or introduced single-bit errors into JetStream’s snapshot files in `data/jetstream/$SYS/_js_/`, we found that nodes would sometimes decide that a stream had been orphaned, and delete all its data files. This happened even when only a minority of nodes in the cluster experienced file corruption. The cluster would never recover quorum, and the stream remained unavailable for the remainder of the test.

In [this test run](https://s3.amazonaws.com/jepsen.io/analyses/nats-2.12.1/20251115T142345-snap-bitflip-quorum-break.zip), we introduced single-bit errors into snapshots on nodes `n3` and `n5`. During the final recovery period, node `n3` became the metadata leader for the cluster and decided to clean up `jepsen-stream`, which stored all the test’s messages.

```
[1010859] 2025/11/15 20:27:02.947432 [INF]
Self is new JetStream cluster metadata leader
[1010859] 2025/11/15 20:27:14.996174 [WRN]
Detected orphaned stream 'jepsen >
jepsen-stream', will cleanup
```

Nodes `n3` and `n5` then deleted all files in the stream directory. This might seem defensible—after all, some of `n3`’s data files *were* corrupted. However, `n3` managed to become the leader of the cluster despite its corrupt state! In general, leader-based consensus systems must be careful to ensure that any node which becomes a leader is aware of majority committed state. Becoming a leader, then opting to delete a stream full of committed data, is particularly troubling.

Although nodes `n1`, `n2`, and `n4` retained their data files, `n1` struggled to apply snapshots; `n4` declared that `jepsen-stream` had no quorum and stalled. Every attempt to subscribe to the stream threw `[SUB-90007] No matching streams for subject`. Jepsen filed issue [#7556](https://github.com/nats-io/nats-server/issues/7556) for this, and the NATS team is looking into it.

## 3.4 Lazy `fsync` by Default (#7564)

NATS JetStream promises that once a `publish` call has been acknowledged, it is “successfully persisted”. This is not exactly true. By default, NATS calls `fsync` to flush data to disk only once every two minutes, but acknowledges messages immediately. Consequently, recently acknowledged writes are generally *not* persisted, and could be lost to coordinated power failure, kernel crashes, etc. For instance, simulated power failures in [this test run](https://github.com/user-attachments/files/23631053/20251119T075152.133-0600.zip) caused NATS to lose roughly thirty seconds of writes: 131,418 out of 930,005 messages.

![A timeseries plot of data loss over time. Acknowledged writes are fine for the first 125 seconds, then all acknowledged writes are lost for the remainder of the test.](nats-2.12.1/power-loss.png)

Because the default flush interval is quite large, even killing a single node at a time is sufficient to cause data loss, so long as nodes fail within a few seconds of each other. In [this run](https://github.com/user-attachments/files/23631363/20251119T085347.396-0600.zip), a series of single-node failures in the first two minutes of the test caused NATS to delete the entire stream, along with all of its messages.

There are only two mentions of this behavior in the NATS documentation. The first is in the [2.10 release notes](https://docs.nats.io/release-notes/whats_new/whats_new_210). The second, [buried in the configuration docs](https://docs.nats.io/running-a-nats-service/configuration), describes the `sync_interval` option:

> Change the default fsync/sync interval for page cache in the filestore. By default JetStream relies on stream replication in the cluster to guarantee data is available after an OS crash. If you run JetStream without replication or with a replication of just 2 you may want to shorten the fsync/sync interval. You can force an fsync after each messsage [sic] with `always`, this will slow down the throughput to a few hundred msg/s.

Consensus protocols often require that nodes sync to disk before acknowledging an operation. For example, the famous 2007 paper [Paxos Made Live](https://www.cs.utexas.edu/~lorenzo/corsi/cs380d/papers/paper2-1.pdf) remarks:

> Note that all writes have to be flushed to disk immediately before the system can proceed any further.

The [Raft thesis](https://web.stanford.edu/~ouster/cgi-bin/papers/OngaroPhD.pdf) on which NATS is based is clear that nodes must “flush [new log entries] to their disks” before acknowledging. Section 11.7.3 discusses the possibility of instead writing data to disk asynchronously, and concludes:

> The trade-off is that data loss is possible in catastrophic events. For example, if a majority of the cluster were to restart simultaneously, the cluster would have potentially lost entries and would not be able to form a new view. Raft could be extended in similar ways to support disk-less operation, but we think the risk of availability or data loss usually outweighs the benefits.

For similar reasons, replicated systems like [MongoDB](https://www.mongodb.com/docs/manual/reference/replica-configuration/#mongodb-rsconf-rsconf.writeConcernMajorityJournalDefault), [etcd](https://www.redhat.com/en/blog/a-guide-to-etcd), [TigerBeetle](https://docs.tigerbeetle.com/single-page/), [Zookeeper](https://thinkingaboutdistributedsystems.blogspot.com/2017/09/what-we-can-learn-from-zookeepers.html), [Redpanda](https://www.redpanda.com/blog/top-performance-considerations-redpanda), and [TiDB](https://docs.pingcap.com/tidb/stable/release-5.0.0/#configuration-file-parameters) sync data to disk before acknowledging an operation as committed.

However, some systems do choose to `fsync` asynchronously. YugabyteDB’s default is [to acknowledge un-fsynced writes](https://docs.yugabyte.com/stable/reference/configuration/yb-master/#durable-wal-write). Liskov and Cowling’s [Viewstamped Replication Revisited](http://pmg.csail.mit.edu/papers/vr-revisited.pdf) assumes replicas are “highly unlikely to fail at the same time”—but acknowledges that if they were to fail simultaneously, state would be lost. Apache Kafka [makes a similar choice](https://jack-vanlightly.com/blog/2023/4/24/why-apache-kafka-doesnt-need-fsync-to-be-safe), but claims that it is not vulnerable to coordinated failure because Kafka “doesn’t store unflushed data in its own memory, but in the page cache”. This offers resilience to the Kafka process itself crashing, but not power failure.[5](#fn5) Jepsen remains skeptical of this approach: as [Alagappan et al.](https://www.usenix.org/system/files/conference/osdi16/osdi16-alagappan.pdf) argue, [extensive](https://www.usenix.org/system/files/conference/atc13/atc13-cidon.pdf) [literature](https://pages.cs.wisc.edu/~akella/CS838/F15/838-CloudPapers/hdfs.pdf) [on](https://static.googleusercontent.com/media/research.google.com/en//people/jeff/SOCC2010-keynote-slides.pdf) [correlated](https://aws.amazon.com/message/2329B7/) [failures](https://www.datacenterknowledge.com/cloud/lightning-in-belgium-disrupts-google-cloud-services-updated-) [suggests](https://www.usenix.org/legacy/event/osdi10/tech/full_papers/Ford.pdf) [we](https://haeberlen.cis.upenn.edu/papers/glacier-nsdi2005.pdf) [should](http://issg.cs.duke.edu/publications/disasters-fast04.pdf) [continue](https://web.archive.org/web/20080108203642/https%3A//radar.oreilly.com/archives/2007/07/365_main_datace.html) [to](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/45499.pdf) [take](https://www.energy.gov/oe/august-2003-blackout) [this](https://www.webofscience.com/wos/woscc/full-record/WOS%3A000245079400017?&SID=USW2EC0C58xDjRHQcDMVJJIWW5emx) [risk](https://web.archive.org/web/20170519044830/https%3A//www.joyent.com/blog/postmortem-for-outage-of-us-east-1-may-27-2014) [seriously](https://pace.gatech.edu/2025/04/02/cooling-failure-in-coda-datacenter/). Heat waves, grid instability, fires, lightning, tornadoes, and floods are not necessarily constrained to a single availability zone.

Jepsen suggests that NATS change the default value for `fsync` to `always`, rather than every two minutes. Alternatively, NATS documentation should prominently disclose that JetStream may lose data when nodes experience correlated power failure, or fail in rapid succession ([#7564](https://github.com/nats-io/nats-server/issues/7564)).

## 3.5 A Single OS Crash Can Cause Split-Brain (#7567)

In response to #7564, NATS engineers [noted](https://github.com/nats-io/nats-server/issues/7564) that most production deployments run with each node in a separate availability zone, which reduces the probability of correlated failure. This raises the question: how many power failures (or hardware faults, kernel crashes, etc.) are required to cause data loss? Perhaps surprisingly, in an asynchronous network the answer is “just one”.

To understand why, consider that a system which remains partly available when a minority of nodes are unavailable must allow states in which a committed operation is present—solely in memory—on a bare majority of nodes. For example, in a leader-follower protocol the leader of a three-node cluster may consider a write committed as soon as a single follower has responded: it has two acknowledgements, counting itself. Under normal operation there will usually be some window of committed operations in this state.[6](#fn6).

Now imagine that one of those two nodes loses power and restarts. Because the write was stored only in memory, rather than on disk, the acknowledged write is no longer present on that node. There now exist two out of three nodes which do *not* have the write. Since the system is fault-tolerant, these two nodes must be able to form a quorum and continue processing requests—creating new states of the system in which the acknowledged write never happened.

Strictly speaking, this fault requires nothing more than a single power failure (or HW fault, kernel crash, etc.) and an asynchronous network—one which is allowed to deliver messages arbitrarily late. Whether it occurs in practice depends on the specific messages exchanged by the replication system, which node fails, how long it remains offline, the order of message delivery, and so on. However, one can reliably induce data loss by killing, pausing, or partitioning away a minority of nodes before and after a simulated OS crash.

For example, process pauses and a single simulated power failure in [this test run](https://github.com/user-attachments/files/23638240/20251119T155654.398-0600.zip) caused JetStream to lose acknowledged writes for windows roughly on par with `sync_interval`. Stranger still, the cluster entered a persistent split-brain which continued after all nodes were restarted and the network healed. Consider these two plots of lost writes, based on final reads performed against nodes `n1` and `n5` respectively:

![A plot of data loss on n1. A few seconds of writes are lost around 42 seconds.](nats-2.12.1/single-kill-split-brain-n1.png)

![A plot of data loss on n5. About six seconds of writes are lost at 58 seconds.](nats-2.12.1/single-kill-split-brain-n5.png)

Consumers talking to `n1` failed to observe a short window of acknowledged messages written around 42 seconds into the test. Meanwhile, consumers talking to `n5` would miss acknowledged messages written around 58 seconds. Both windows of write loss were on the order of our choice of `sync_interval = 10s` for this run. In repeated testing, we found that any node in the cluster could lose committed writes, including the node which failed, those which received writes before the failure, and those which received writes afterwards.

The fact that a single power failure can cause data loss is not new. In 2023, RedPanda wrote [a detailed blog post](https://www.redpanda.com/blog/why-fsync-is-needed-for-data-safety-in-kafka-or-non-byzantine-protocols) showing that Kafka’s default lazy `fsync` could lead to data loss under exactly this scenario. However, it is especially concerning that this scenario led to persistent replica divergence, not just data loss! We filed [#7567](https://github.com/nats-io/nats-server/issues/7567) for this issue, and the NATS team is investigating.

| № | Summary | Event Required | Fixed in |
| --- | --- | --- | --- |
| [#6888](https://github.com/nats-io/nats-server/issues/6888) | Stream deleted on crash in 2.10.22 | Crashes | 2.10.23 |
| [#7549](https://github.com/nats-io/nats-server/issues/7549) | Lost writes due to `.blk` file corruption | Minority truncation or bitflip | Unresolved |
| [#7556](https://github.com/nats-io/nats-server/issues/7556) | Stream deleted due to snapshot file corruption | Minority truncation or bitflip | Unresolved |
| [#7564](https://github.com/nats-io/nats-server/issues/7564) | Write loss due to lazy `fsync` policy | Coordinated OS crash | Documented |
| [#7567](https://github.com/nats-io/nats-server/issues/7567) | Write loss and split-brain | Single OS crash and pause | Unresolved |

# 4 Discussion

In NATS 2.10.22, process crashes could cause JetStream to forget a stream ever existed (#6888). This issue was identified independently by NATS and resolved in version 2.10.23, released on 2024-12-10. We did not observe data loss with simple network partitions, process pauses, or crashes in version 2.12.1.

However, we found that in NATS 2.12.1, file corruption and simulated OS crashes could both lead to data loss and persistent split-brain. Bitflips or truncation of either `.blk` (#7549) or snapshot (#7556) files, even on a minority of nodes, could cause the loss of single messages, large windows of messages, or even cause some nodes to delete their stream data altogether. Messages could be missing on some nodes and present on others. NATS has multiple checksum mechanisms designed to limit the impact of file corruption; more thorough testing of these mechanisms seems warranted.

By default, NATS only flushes data to disk every two minutes, but acknowledges operations immediately. This approach can lead to the loss of committed writes when several nodes experience a power failure, kernel crash, or hardware fault concurrently—or in rapid succession (#7564). In addition, a single OS crash combined with process crashes, pauses, or network partitions can cause the loss of acknowledged messages and persistent split-brain (#7567). We recommended NATS change the default value of `fsync` to `always`, or clearly document these hazards. NATS has [added new documentation](https://github.com/nats-io/nats.docs/pull/896) to the [JetStream Concepts page](https://docs.nats.io/nats-concepts/jetstream#persistent-and-consistent-distributed-storage).

This documentation [also describes](https://docs.nats.io/nats-concepts/jetstream#goals) several goals for JetStream, including that “[t]he system must self-heal and always be available.” This is impossible: the CAP theorem states that Linearizable systems cannot be totally available in an asynchronous network. In our three and five-node clusters JetStream generally behaved like a typical Raft implementation. Operations proceeded on a majority of connected nodes but isolated nodes were unavailable, and if a majority failed, the system as a whole became unavailable. Jepsen suggests clarifying this part of the documentation.

As always, Jepsen takes an experimental approach to safety verification: we can prove the presence of bugs, but not their absence. While we make extensive efforts to find problems, we cannot prove correctness.

## 4.1 LazyFS

This work demonstrates that systems which do not exhibit data loss under normal process crashes (e.g. `kill -9 <PID>`) may lose data or enter split-brain under simulated OS-level crashes. Our tests relied heavily on [LazyFS](https://github.com/dsrhaslab/lazyfs), a project of [INESC TEC](https://www.inesctec.pt/en) at the University of Porto.[7](#fn7) After killing a process, we used LazyFS to simulate the effects of a power failure by dropping writes to the filesystem which had not yet been `fsync`ed to disk.

While this work focused purely on the loss of unflushed writes, LazyFS can also simulate linear and non-linear torn writes: an anomaly where a storage device persists part, but not all, of written data thanks to (e.g.) IO cache reordering. Our 2024 paper [When Amnesia Strikes](https://www.vldb.org/pvldb/vol17/p3017-ramos.pdf) discusses these faults in more detail, highlighting bugs in PostgreSQL, Redis, ZooKeeper, etcd, LevelDB, PebblesDB, and the Lightning Network.

## 4.2 Future Work

We designed only a simple workload for NATS which checked for lost records either across all consumers, or across all consumers bound to a single node. We did not check whether single consumers could miss messages, or the order in which they were delivered. We did not check NATS’ claims of Linearizable writes or Serializable operations in general. We also did not evaluate JetStream’s “exactly-once semantics”. All of these could prove fruitful avenues for further tests.

In some tests, we [added and removed](https://github.com/jepsen-io/nats/blob/4760f97f86350c5c9983478656dbcbcdade33817/src/jepsen/nats/nemesis.clj#L67-L263) nodes from the cluster. This work [generated some preliminary results](https://github.com/nats-io/nats-server/issues/7545). However, the NATS documentation for membership changes was incorrect and incomplete: it gave [the wrong command](https://github.com/nats-io/nats.docs/pull/893) for removing peers, and there appears to be an undocumented but mandatory [health check step](https://github.com/nats-io/nats-server/issues/7545#issuecomment-3528168499) for newly-added nodes. As of this writing, Jepsen is unsure how to safely add or remove nodes to a NATS cluster. Consequently, we leave membership changes for future research.

*Our thanks to [INESC TEC](https://www.inesctec.pt/en) and everyone on the LazyFS team, including Maria Ramos, João Azevedo, José Pereira, Tânia Esteves, Ricardo Macedo, and João Paulo. Jepsen is also grateful to Silvia Botros, Kellan Elliott-McCrea, Carla Geisser, Coda Hale, and Marc Hedlund for their expertise regarding datacenter power failures, correlated kernel panics, disk faults, and other causes of OS-level crashes. Finally, our thanks to [Irene Kannyo](https://www.irenekannyo.com/) for her editorial support. This research was performed independently by Jepsen, without compensation, and conducted in accordance with the [Jepsen ethics policy](https://jepsen.io/analyses/ethics).*

---

1. Throughout this report we use “acknowledged message” to describe a message whose `publish` request was acknowledged successfully by some server. NATS also offers a separate notion of acknowledgement, which indicates when a message has been processed and need not be delivered again.[↩︎](#fnref1)
2. JetStream also promises “exactly once semantics” in some scenarios. We leave this for later research.[↩︎](#fnref2)
3. The CAP theorem’s definition of “availability” requires that all operations on non-faulty nodes must succeed.[↩︎](#fnref3)
4. This is overly conservative: in a system with Linearizable writes, we should never observe a lost message which was acknowledged prior to the invocation of the `publish` call for an OK message, regardless of process. However, early testing with NATS suggested that it might be better to test a weaker property, and come to stronger conclusions about data loss.[↩︎](#fnref4)
5. [Redpanda argues](https://www.redpanda.com/blog/why-fsync-is-needed-for-data-safety-in-kafka-or-non-byzantine-protocols) that the situation is actually worse: a single power failure, combined with network partitions or process pauses, can cause Kafka to lose committed data.[↩︎](#fnref5)
6. Some protocols, like Raft, consider an operation committed as soon as it is acknowledged by a majority of nodes. These systems offer lower latencies, but at any given time there are likely a few committed operations which are missing from a minority of nodes due to normal network latency. Other systems, like Kafka, require acknowledgement from *all* “online” nodes before considering an operation committed. These systems offer worse latency in healthy clusters (since they must wait for the slowest node) but in exchange, committed operations can only be missing from some node when the fault detector decides that node is no longer online (e.g. due to elevated latency).[↩︎](#fnref6)
7. Jepsen contributed some funds, testing, and integration assistance to LazyFS, but most credit belongs to the LazyFS team.[↩︎](#fnref7)

Copyright © Jepsen, LLC.

We do our best to provide accurate information, but if you see a
mistake, please let us know.

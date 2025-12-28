---
source: hackernews
title: Clock synchronization is a nightmare
url: https://arpitbhayani.me/blogs/clock-sync-nightmare/
date: 2025-12-28
---

[Arpit Bhayani](/)

[Blogs](/blogs)    [Papershelf](/papershelf)   [Bookshelf](/bookshelf)    [Courses](/courses)   [Talks](/talks)

* [Blogs](/blogs)
* [Engineering Explorations](/knowledge-base/engineering-explorations)

* [Blogs](/blogs)
* [Engi...](/knowledge-base/engineering-explorations)

# Clock Synchronization Is a Nightmare

![](https://edge.arpitbhayani.me/img/arpit-6.jpg)

[Arpit Bhayani](/)

curious, tinkerer, and explorer

Time seems simple. But we engineers lose sleep over something as basic as keeping clocks in sync. Here’s why…

The answer lies in this one simple statement - there is no global clock. When you have thousands of machines spread across data centers, continents, and time zones, each operating independently, the simple question of “what time is it?” becomes surprisingly complex.

Clock synchronization sits at the core of some of the most challenging problems in distributed systems, affecting everything from database consistency to debugging to financial transactions.

Let’s dig deeper…

## The Illusion of Accurate Time

Every computer has an internal clock, typically driven by a [quartz crystal oscillator](https://en.wikipedia.org/wiki/Crystal_oscillator). These oscillators work by vibrating at a specific frequency when voltage is applied. The standard frequency for most computer clocks is 32768 Hz, chosen because it is a power of two and makes counting down to one second straightforward.

The catch: quartz crystals are not perfect. Their oscillation frequency varies based on many factors; here are a few…

Temperature is the biggest culprit. Standard quartz crystals exhibit frequency drift in the tens of parts per million when temperature changes. A temperature deviation of ~10 degrees Celsius can cause drift equivalent to about 110 seconds per year. The crystal vibrates faster or slower depending on ambient temperature, and data center environments are not perfectly controlled.

Another culprit is manufacturing variation. No two crystals are identical. Even crystals from the same production batch will have slightly different characteristics. Aging compounds this problem as crystals change properties over time.

The result is that two computers started at exactly the same time, never communicating with each other, will inevitably drift apart. After just one day, they might differ by hundreds of milliseconds. After a month, they could be seconds apart.

## Why Clock Skew Breaks Things

Clock skew = the difference in time between two clocks at any given instant. Clock drift = the rate at which clocks diverge over time. Both cause serious problems in distributed systems.

Consider a simple example with a distributed make system. You edit a source file on your client machine, which has a clock slightly behind the server where the compiled object file lives. When `make` runs, it compares timestamps. If the server clock is ahead, the object file appears newer than the source file you just edited, and make does not recompile. Your changes silently disappear from the build.

```
Scenario: UNIX make with unsynchronized clocks

Client machine clock: 10:00:00 (lagging)
Server machine clock: 10:00:05 (ahead)

1. Edit util.c at client time 10:00:00
2. util.o on server has timestamp 10:00:03
3. Make compares: util.o (10:00:03) vs util.c (10:00:00)
4. Conclusion: util.o is newer, skip recompilation
5. Result: Your changes are ignored
```

Database systems face even more critical timestamp issues. When two transactions happen at nearly the same time on different nodes, the database must determine which happened first. If clocks are out of sync, the database might order them incorrectly, violating consistency guarantees.

Imagine a banking system where a customer deposits money at one branch (Node A) and immediately withdraws at another branch (Node B). If Node B clock is behind Node A, the withdrawal transaction might get a timestamp earlier than the deposit. A snapshot read at the wrong time could show the withdrawal but not the deposit, making it appear the customer withdrew money they did not have.

Logging and debugging become nearly impossible when clocks disagree. Distributed tracing relies on timestamps to reconstruct the sequence of events across services. When clocks are skewed, the resulting traces show impossible sequences where effects appear before causes.

## Physical Clock Synchronization

The simplest approach to clock synchronization is to periodically query a trusted time server and adjust local clocks accordingly. Let’s look at different algorithms and approaches based on this…

### Cristian Algorithm

Cristian algorithm, proposed in 1989, works with a centralized time server assumed to have accurate time. A client requests the time, the server responds with its current time, and the client adjusts.

The challenge is network delay. By the time the response arrives, the server time is stale. Cristian algorithm estimates the one way delay as half the round trip time.

```
# Cristian's Algorithm
def synchronize_clock():
    t0 = local_time()           # Record time before request
    server_time = request_time_from_server()
    t1 = local_time()           # Record time after response

    round_trip = t1 - t0
    one_way_delay = round_trip / 2

    # Adjust local clock
    new_time = server_time + one_way_delay
    set_local_clock(new_time)

    # Error bound: +/- (t1 - t0) / 2
```

This works reasonably well when network delays are symmetric, meaning request and response take the same time. In practice, delays are often asymmetric due to different routing paths, varying network congestion, and processing delays.

### Berkeley Algorithm

The Berkeley algorithm takes a different approach, assuming no single machine has an accurate time. Instead, it uses consensus among multiple machines.

A designated time daemon periodically polls all machines for their clock values. It computes the average, discards outliers, and tells each machine how much to adjust. Rather than sending absolute times, which would suffer from network delays, it sends relative adjustments.

```
Berkeley Algorithm Steps:
1. Time daemon polls machines: "What time do you have?"
2. Responses: Machine A: 10:00:05, Machine B: 10:00:02, Machine C: 10:00:08
3. Time daemon clock: 10:00:04
4. Average: (5 + 2 + 8 + 4) / 4 = 4.75 → 10:00:05
5. Adjustments sent:
   - Machine A: slow down by 0s (already at target)
   - Machine B: speed up by 3s
   - Machine C: slow down by 3s
   - Daemon: speed up by 1s
```

A critical detail: computers should never jump their clocks backward. Doing so violates the assumption of monotonic time that many algorithms depend on. Instead of rewinding, the Berkeley algorithm slows clocks gradually to let them catch up.

### Network Time Protocol

NTP uses a hierarchical system of time servers organized into strata.

Stratum 0 devices are high precision time sources like atomic clocks and GPS receivers. Stratum 1 servers connect directly to stratum 0 sources. Each lower stratum synchronizes with the level above, with stratum numbers increasing up to 15.

NTP can typically maintain time within tens of milliseconds over the public internet and can achieve sub-millisecond accuracy on local area networks. However, several factors limit its precision.

```
NTP Accuracy Limitations:
- Public internet: 10-100 ms typical
- LAN with good conditions: 100-500 µs
- Network asymmetry: can cause 100+ ms errors
- Variable latency: introduces jitter
- Operating system delays: software timestamps add microseconds
```

Network asymmetry is particularly problematic. If the path from client to server differs from server to client, the assumption that one way delay equals half the round-trip breaks down. Satellite links where uplink and downlink have different latencies are a classic example.

Operating system overhead adds uncertainty. When an NTP packet arrives, it passes through the network stack, gets timestamped by the kernel, and eventually reaches the NTP daemon. Each step introduces variable delays measured in microseconds.

## Milliseconds Are Not Enough

For many applications, NTP accuracy is sufficient. Web servers, file systems, and most business applications tolerate clocks being tens of milliseconds apart. But some domains demand much tighter synchronization.

Financial trading systems measure latency in microseconds. A trade timestamped incorrectly by even a few milliseconds can have significant legal and financial implications. High-frequency trading strategies depend on knowing the precise order of events.

Telecommunications systems require synchronization for TDM (Time Division Multiplexing) where different users share a channel by taking turns. If timing drifts, transmissions from different users collide.

Scientific experiments, particularly in physics, need nanosecond precision to correlate measurements across instruments.

### Precision Time Protocol

PTP, defined by IEEE 1588, achieves sub-microsecond accuracy by using hardware timestamping. Instead of the operating system recording when a packet arrived, specialized network interface cards timestamp packets as they cross the wire, eliminating software delays.

```
PTP vs NTP Precision:
- NTP: milliseconds (software timestamping)
- PTP: nanoseconds (hardware timestamping)

Key PTP improvements:
- Hardware timestamps at NIC level
- Boundary clocks at switches maintain precision
- Two-way message exchange calculates asymmetric delays
```

PTP requires support throughout the network path. Switches must be PTP aware, acting as boundary clocks that maintain synchronization hop by hop. This makes PTP expensive to deploy but essential for applications requiring nanosecond precision.

[Meta announced in 2022](https://engineering.fb.com/2022/11/21/production-engineering/precision-time-protocol-at-meta/) that they were migrating from NTP to PTP across their data centers. The investment in PTP infrastructure paid off in reduced errors and better debugging capability.

## Logical clocks and causality

Lamport introduced the concept of logical clocks based on a simple observation: if two events are causally related, we should be able to order them. If event A sends a message that event B receives, A happened before B. If both events happen on the same process, the earlier one happens before the later one.

Events that are not connected by any chain of causality are concurrent. They could have happened in either order, and from the system’s perspective, there is no meaningful way to distinguish.

### Lamport Timestamps

[Lamport timestamps](https://en.wikipedia.org/wiki/Lamport_timestamp) implement this intuition with a simple algorithm. Each process maintains a counter. Before any event, increment the counter. When sending a message, include the counter value. When receiving a message, set your counter to the maximum of your current value and the received value, then increment.

```
class LamportClock:
    def __init__(self):
        self.time = 0

    def local_event(self):
        self.time += 1
        return self.time

    def send_event(self):
        self.time += 1
        return self.time  # Include in message

    def receive_event(self, received_time):
        self.time = max(self.time, received_time) + 1
        return self.time
```

If event A has a lower Lamport timestamp than event B, we know one of two things: either A happened before B, or they are concurrent. The converse is guaranteed: if A happened before B, then A has a lower timestamp than B.

```
Process P1: [0] ---(1)---> send m ---(2)---> local event
                    |
                    v
Process P2: [0] --------> receive m ---(2)---> local event ---(3)

P1 events: (1, 2)
P2 events: (2, 3)
The receive on P2 happens after send on P1 (causality preserved)
```

The limitation is that Lamport timestamps cannot tell you if two events are concurrent. Events with timestamps 5 and 7 might be causally related or might have happened independently on different processes with no communication between them.

### Vector Clocks

Vector clocks extend Lamport timestamps to capture full causality information. Instead of a single counter, each process maintains a vector with an entry for every process in the system.

```
class VectorClock:
    def __init__(self, process_id, num_processes):
        self.id = process_id
        self.clock = [0] * num_processes

    def local_event(self):
        self.clock[self.id] += 1
        return self.clock.copy()

    def send_event(self):
        self.clock[self.id] += 1
        return self.clock.copy()

    def receive_event(self, received_clock):
        for i in range(len(self.clock)):
            self.clock[i] = max(self.clock[i], received_clock[i])
        self.clock[self.id] += 1
        return self.clock.copy()

    @staticmethod
    def compare(vc1, vc2):
        less = any(vc1[i] < vc2[i] for i in range(len(vc1)))
        greater = any(vc1[i] > vc2[i] for i in range(len(vc1)))

        if less and not greater:
            return "vc1 happened before vc2"
        elif greater and not less:
            return "vc2 happened before vc1"
        elif not less and not greater:
            return "equal"
        else:
            return "concurrent"
```

Comparing two vector clocks tells you precisely whether one event happened before another or whether they are concurrent. If every entry in VC1 is less than or equal to the corresponding entry in VC2, and at least one is strictly less, then VC1 happened before VC2.

```
Vector Clock Example (3 processes: P1, P2, P3):

P1: [1,0,0] ---> [2,0,0] ---send m---> [3,0,0]
                              |
P2: [0,1,0] ---------> [0,2,0] ---receive m---> [3,3,0]

P3: [0,0,1] ---> [0,0,2]

Compare [2,0,0] and [0,0,2]:
- 2 > 0 (first element)
- 0 < 2 (third element)
Result: CONCURRENT (no causal relationship)

Compare [2,0,0] and [3,3,0]:
- 2 < 3 (first element)
- 0 < 3 (second element)
Result: [2,0,0] happened before [3,3,0]
```

The downside of vector clocks is space overhead. With N processes, each timestamp requires O(N) space. For large distributed systems with thousands of nodes, this becomes impractical. Various optimizations exist, including compressed vector clocks and interval tree clocks, but the fundamental scaling challenge remains.

## Google Spanner and TrueTime

Google faced the clock synchronization problem at an unprecedented scale with Spanner, its globally distributed database. They needed strong consistency guarantees across data centers spanning continents, which requires knowing the order of transactions.

Here’s a [video of me explaining this](https://www.youtube.com/watch?v=--YbYCfMnxc).

Their solution was TrueTime, a globally distributed clock infrastructure that provides time with bounded uncertainty.

TrueTime uses two types of time sources in every data center. GPS receivers get time directly from satellites. Atomic clocks provide backup and cross-validation. Using both provides resilience since they have independent failure modes. GPS can fail due to antenna problems or signal interference. Atomic clocks can drift, but are not affected by the same issues.

The key innovation is that TrueTime does not return a single timestamp. It returns an interval guaranteed to contain the true time.

```
- TT.now() returns [earliest, latest]
  - True time is guaranteed to be within this interval
  - Interval width is the uncertainty bound ε

- TT.after(t) returns true if t is definitely in the past
- TT.before(t) returns true if t is definitely in the future

Typical uncertainty: 1-7 milliseconds
```

When a Spanner transaction commits, it gets a timestamp. Before reporting the commit to the client, Spanner waits until TT.after(commit\_timestamp) returns true. This “commit wait” ensures that any subsequent transaction starting after the wait will see a later timestamp.

```
Spanner Commit Wait:

1. Transaction T1 prepares to commit
2. T1 gets commit timestamp ts = TT.now().latest
3. T1 waits until TT.after(ts) is true
4. T1 reports commit to client

Why this works:
- After waiting, we know ts is in the past
- Any transaction T2 starting after this point
  will get a timestamp > ts
- Therefore: T1 commit before T2 start → ts(T1) < ts(T2)
- External consistency is preserved
```

The waiting period is typically around 7 milliseconds in the worst case. This seems like a significant latency cost, but it is actually overlapped with other commit processing, and the benefit of strong consistency is worth the cost for Spanner’s use cases.

## Hybrid Logical Clocks

Not everyone has Google’s resources to deploy atomic clocks in every data center. CockroachDB, inspired by Spanner but designed to run on commodity hardware, uses hybrid logical clocks (HLC).

Again, I have a podcast with their Chief Architect, Ben Darnell. We discussed this at length, [give it a watch](https://www.youtube.com/watch?v=yhqW82Ka-gw).

HLC combines physical time with a logical component. The physical part stays close to wall clock time. The logical part handles cases where multiple events happen within the same physical clock tick or when clock skew causes issues.

```
class HybridLogicalClock:
    def __init__(self):
        self.physical = 0  # Wall clock time
        self.logical = 0   # Logical counter

    def now(self, wall_time):
        if wall_time > self.physical:
            self.physical = wall_time
            self.logical = 0
        else:
            self.logical += 1
        return (self.physical, self.logical)

    def receive(self, wall_time, received_physical, received_logical):
        if wall_time > self.physical and wall_time > received_physical:
            self.physical = wall_time
            self.logical = 0
        elif received_physical > self.physical:
            self.physical = received_physical
            self.logical = received_logical + 1
        elif self.physical > received_physical:
            self.logical += 1
        else:  # Equal physical times
            self.logical = max(self.logical, received_logical) + 1
        return (self.physical, self.logical)
```

HLC maintains the property that timestamps are closely related to physical time, making debugging easier. You can look at an HLC timestamp and know approximately when an event occurred. The logical component ensures that causally related events are always ordered correctly, even if physical clocks skew.

```
Node A physical clock: 100
Node B physical clock: 98 (slightly behind)

Node A event: HLC = (100, 0)
Node A sends message to B

Node B receives at its physical time 99:
- received HLC = (100, 0)
- local physical = 99
- Since 100 > 99: new HLC = (100, 1)

Node B event is correctly ordered after Node A event
despite Node B's physical clock being behind.
```

CockroachDB uses HLC with a configurable maximum clock offset, typically 500 milliseconds for local deployments. If a node detects its clock is too far off from others, it removes itself from the cluster rather than risk consistency violations.

YugabyteDB similarly uses HLC and recently integrated with AWS Time Sync Service for tighter synchronization. Their benchmarks showed up to 3x reduction in transaction latency when using precision time sources because smaller uncertainty bounds mean less waiting during transactions.

## How do You Pick One Over the Other

### Choosing synchronization granularity

What level of synchronization do you actually need? Many systems do fine with NTP providing millisecond accuracy. If your transactions take tens of milliseconds anyway, submillisecond clock skew is irrelevant.

If you need stronger ordering guarantees, consider whether logical clocks might suffice. Lamport timestamps are trivial to implement and have zero space overhead beyond a single integer per message.

For databases requiring external consistency or systems where physical time matters (audit logs, financial records), invest in better synchronization infrastructure. PTP hardware, dedicated time servers, and careful network design can get you to microseconds.

### Handling clock anomalies

Clocks can jump backward. NTP might suddenly correct a clock that drifted significantly. Virtual machine migrations can cause clock discontinuities. Operating system bugs can cause time to jump.

Robust systems detect these anomalies. Monitor the rate of clock change and alert on sudden jumps. Never use raw system time for critical ordering; use a clock abstraction that can handle anomalies.

```
class SafeClock:
    def __init__(self):
        self.last_time = 0
        self.offset = 0

    def now(self):
        system_time = get_system_time()

        if system_time < self.last_time:
            # Clock jumped backward!
            # Adjust offset to maintain monotonicity
            self.offset += (self.last_time - system_time) + 1
            log_warning("Clock jumped backward")

        result = system_time + self.offset
        self.last_time = result
        return result
```

### Dealing with Leap Seconds

Leap seconds are inserted occasionally to keep UTC aligned with Earth’s rotation. A minute with a leap second has 61 seconds. Many systems handle this poorly.

```
Leap second: 2016-12-31 23:59:60 UTC

Problem scenarios:
- Systems that assume minutes have 60 seconds crash
- Time-based scheduling produces duplicate events
- Logs show impossible timestamps

Solutions:
- Leap smearing: Gradually adjust over hours (Google, AWS)
- Step adjustment: Jump clock (traditional NTP)
- Ignore: Use TAI instead of UTC (specialized systems)
```

Google and AWS use “[leap smearing](https://developers.google.com/time/smear)” where the leap second is spread over a longer period, making each second slightly longer or shorter. This avoids the discontinuity but means your clock is not precisely UTC during the smear period.

The good news is that the International Bureau of Weights and Measures has decided to stop adding leap seconds by 2035, eliminating this problem for future systems.

## The fundamental tradeoff

Clock synchronization in distributed systems always involves a tradeoff between accuracy, latency, and complexity.

Tighter synchronization requires either better hardware (atomic clocks, GPS, PTP-capable switches) or more communication overhead (frequent sync messages, consensus protocols). Both have costs.

Looser synchronization is cheaper but means your timestamps are less reliable. You might need to build larger safety margins into your algorithms or accept weaker ordering guarantees.

Logical clocks sidestep physical time entirely but require passing clock information with every message and cannot tell you when something actually happened.

There is no universal right answer. The appropriate solution depends on your requirements, budget, and tolerance for complexity.

## Footnote

Clock synchronization is an interesting and difficult problem, and the solution ranges from simple NTP to Google’s atomic clock infrastructure for global databases.

It is important to ack that perfect synchronization is impossible, and hence we should choose appropriate bounds and trade-offs while picking one over the other.

---

If you find this helpful and interesting,

* share it on [HackerNews](https://news.ycombinator.com/submitlink?u=https%3A%2F%2Farpitbhayani.me%2Fblogs%2Fclock-sync-nightmare%2F&t=Clock%20Synchronization%20Is%20a%20Nightmare)
* subscribe to my [RSS feed](/rss.xml) and get notified the moment I publish a new one.

![Arpit Bhayani](https://edge.arpitbhayani.me/img/arpit-6.jpg)

Staff Engg at GCP Memorystore, Creator of [DiceDB](https://github.com/dicedb/dice), ex-Staff Engg for Google Ads and GCP Dataproc, ex-Amazon Fast Data, ex-Director of Engg. SRE and
Data Engineering at Unacademy. I spark engineering curiosity through my
no-fluff engineering videos on
[YouTube](https://www.youtube.com/c/ArpitBhayani)
and my courses

* [System Design Masterclass](/masterclass)
* [System Design for Beginners](/system-design-for-beginners)
* [Redis Internals](/redis-internals)

Writings and Learnings

[Blogs](/blogs)

[Papershelf](/papershelf)

[Bookshelf](/bookshelf)

[RSS Feed](/rss.xml)

Courses

[System Design Masterclass](/masterclass)

[System Design for Beginners](/system-design-for-beginners)

[Redis Internals](/redis-internals)

Legal and Contact

[About me](/about-arpit-bhayani)

[Terms and Conditions](/terms-conditions)

[Privacy Policy](/privacy-policy)

[Refund Policy](/refund-policy)

[Contact Me](/contact)

Everything Else

[Collaborate with me](/collaborate)

[DiceDB](https://github.com/DiceDB/Dice)

[Revine](https://revine.arpitbhayani.me)

[The Smarter Chimp](https://twitter.com/TheSmarterChimp)

Arpit's Newsletter read by 145,000 engineers

Weekly essays on real-world system design, distributed systems, or a
deep dive into some super-clever algorithm.

[Subscribe on LinkedIn](https://www.linkedin.com/newsletters/asli-engineering-6921728898456072192/) [Subscribe on Substack](https://arpit.substack.com)

The courses listed on this website are offered by

Relog Deeptech Pvt. Ltd.
203, Sagar Apartment, Camp Road, Mangilal Plot, Amravati, Maharashtra, 444602
GSTIN: 27AALCR5165R1ZF

[YouTube (180k)](https://youtube.com/c/ArpitBhayani "Arpit Bhayani")   [Twitter (100k)](https://twitter.com/arpit_bhayani "Follow Arpit Bhayani on Twitter")   [LinkedIn (250k)](https://linkedin.com/in/arpitbhayani "Follow Arpit Bhayani on LinkedIn")   [GitHub (6k)](https://github.com/arpitbbhayani "Follow Arpit Bhayani on GitHub")

* © Arpit Bhayani, 2025

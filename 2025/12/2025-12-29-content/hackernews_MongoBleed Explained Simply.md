---
source: hackernews
title: MongoBleed Explained Simply
url: https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply
date: 2025-12-29
---

[![Stanislav’s Big Data Stream](https://substackcdn.com/image/fetch/$s_!0Zr3!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd8f34605-e9a0-4c47-95a1-af5cd14854c7_256x256.png)](/)

# [![Stanislav’s Big Data Stream](https://substackcdn.com/image/fetch/$s_!_oiL!,e_trim:10:white/e_trim:10:transparent/h_72,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc549de26-ba8f-4c6c-a6dc-5f32da7a0b46_1344x256.png)](/)

SubscribeSign in

# MongoBleed explained simply

### CVE-2025-14847 allows attackers to read any arbitrary data from the database's heap memory. It affects all MongoDB versions since 2017, here's how it works:

[![Stanislav Kozlovski's avatar](https://substackcdn.com/image/fetch/$s_!0lM5!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F03bc8810-0db9-40c9-a28b-1a4752b7a135_800x800.jpeg)](https://substack.com/%40stanislavkozlovski)

[Stanislav Kozlovski](https://substack.com/%40stanislavkozlovski)

Dec 28, 2025

1

1

Share

**MongoBleed,** officially CVE-2025-14847, is a recently-uncovered extremely sensitive vulnerability affecting basically all versions of MongoDB since **~2017**.

It is a bug in the **zlib[1](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-1-182764771)** message compression path in MongoDB.

It allows an attacker to read off any uninitialized heap memory, meaning ***anything*** that was allocated to memory from a previous database operation could be read.

The bug was introduced in 2017[2](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-2-182764771). It is dead-easy to exploit - it only requires connectivity to the database (no auth needed). It is fixed as of writing, but some EOL versions (3.6, 4.0, 4.2) will not get it.

# MongoDB Basics

Let’s get a few basics out of the way before we explain the bug:

* MongoDB uses its own TCP wire protocol instead of e.g HTTP. This is standard for databases, especially ones chasing high performance.
* Mongo uses the [BSON](https://en.wikipedia.org/wiki/BSON) format for messages[3](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-3-182764771). It’s basically binary json but with some key optimizations. We will talk about one later because it is essential to the exploit.
* Mongo doesn’t have endpoints or RPCs. It only uses a single op code called OP\_MSG.
* The OP\_MSG command contains a BSON message. The contents of the message denote what type of request it is. Concretely, it’s the first field of the message that marks the request type. [4](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-4-182764771)
* The request can be compressed. In that case, an OP\_COMPRESSED message is sent [which wraps](https://www.mongodb.com/docs/manual/reference/mongodb-wire-protocol#op_compressed) the now-compressed OP\_MSG BSON.
* The request then looks like this:

```
     OP_COMPRESSED message
┌────────────────────────────┐
│ standard header (16 bytes) │
├────────────────────────────┤
│ originalOpcode (int32)     │
│ uncompressedSize (int32)   │
│ compressorId (int8)        │
│ compressed OP payload      │
└────────────────────────────┘
```

* Critically, the `uncompressedSize` field denotes how large the payload is once it’s uncompressed.

# Exploit Part 1

The first part of the exploit is to get the server to wrongfully think that an overly-large OP\_MSG is coming.

An attacker can send a falsefully large `` `uncompressedSize` `` field, say 1MB[5](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-5-182764771), when in reality the underlying message is 1KB uncompressed.

This will make the server **allocate a 1MB buffer** in memory to decompress the message into. This is fine.

The critical bug here is that, once finished decompressing, the server does NOT check the actual resulting size of the newly-uncompressed payload.

Instead, it trusts the user’s input and uses that as the canonical size of the payload, even if it got a different number.[6](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-6-182764771)

The result is an in-memory representation of the BSON message which looks something like this:

```
[ 1KB of REAL DATA |      999KB of UNREFERENCED HEAP GARBAGE       ]
                   ↑                                               ↑
        actual length (1KB)                     user input length (1MB)
```

### Unreferenced Heap Garbage

Like in every programming language, when a variables in the code goes out of scope, the runtime marks the memory it previously took up as available.

In most modern languages, the memory gets zeroed out. In other words, the old bytes that used to take up the space get deleted.

In C/C++, this doesn’t happen. When you allocate memory via `` `malloc()` ``, you get whatever was previously there.

Since Mongo is writen in C++, that unreferenced heap garbage part can represent anything that was in memory from previous operations, including:

* Cleartext passwords and credentials
* Session tokens / API keys
* Customer data and PII
* Database configs and system info
* Docker paths and client IP addresses

```
[ REAL BSON DATA | password: 123 | apiKey: jA2sa | ip: 219.117.127.202 ]
```

---

# Exploit Part 2

Now that the server has wrongfully allocated some potentially-sensitive data to the input message, the only thing left for the attacker is to somehow get the server return the data.

### BSON

As mentioned, BSON is Mongo’s way of serializing JSON. [As mentioned on its site](https://bsonspec.org/), it was designed with efficiency in mind:

> ### **3. Efficient**
>
> En­cod­ing data to BSON and de­cod­ing from BSON can be per­formed very quickly in most lan­guages due to **the use of C data types**.

### C Strings

C famously uses **null-terminated** **strings**[7](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-7-182764771). A null-terminated string means that a null byte is used to mark the end of the string:

```
char* s = "hello"
// in memory, this is represented as an array of characters with the last element being the null terminator: h e l l o \0
```

The way such strings get parsed is very simple - the deserializer reads every character until it finds a null terminator.

### Malicious BSON Input

If you recall, I said earlier that the first field of the BSON message denotes what type of “RPC” the command is.

As such, the first thing a server does when handling an incoming message over the wire is… parse the first field!

Because fields are strings, and strings are null-terminated CStrings, the deserializing logic in the MongoDB server parses the field until the first null terminator found.

An attacker can send a compressed, invalid BSON object that does NOT contain a null terminator. This forces the server to continue scanning through foreign data in the wrongly-allocated memory buffer until it finds the first null terminator (**\0**)

```
# Conceptual
[ REAL DATA |             UNREFERENCED HEAP GARBAGE                 ]
# Practical Example
[ { "a      | password: 123\0 | apiKey: jA2sa | ip: 219.117.127.202 ]
```

As the first null terminator is right after the password, the server would now think that the first field of the BSON is:

```
"a      | password: 123"
```

Obviously that is an invalid BSON field, so the server responds with an error to the client. In order to be helpful, the response contains an error message that shows which field was invalid:

```
{
  "ok": 0,
  "errmsg": "invalid BSON field name 'a      | password: 123'",
  "code": 2,
  "codeName": "BadValue"
}
```

Boom. The attacker successfully got the server to leak data to it.

Any serious attacker would then run this over and over again, thousands of time a second, until they believe they’ve scanned the majority of the database’s heap. They can then repeat this ad infinitum.

---

# 💥 Impact

### 1. Ease of Exploitation - “Pre-Auth”

The impact of this is particularly nasty, because the request-response parsing cycle happens **before any authentication can be made**. This makes sense, since you cannot begin to authenticate a request you still haven’t deserialized.

This allows **any** attacker to gain access to **any** piece of potentially-sensitive data. The only thing they need is internet access to the database.

Exposing your database to the internet is a practice that’s heavily frowned upon[8](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-8-182764771). At the same time, Shodan shows that there are over [213,000 publicly-accessible Mongo databases](https://www.shodan.io/search?query=Product%3A%22MongoDB%22).

### 2. Eight Years of Vulnerability (handled badly)

The PR that introduced the bug was from [May 2017](https://github.com/mongodb/mongo/pull/1152). This means that, roughly from version 3.6.0, any publicly-accessible MongoDB instance has been vulnerable to this.

It is unknown whether the exploit was known and exploited by actors prior to its disclosure. Given the simplicity of it, I bet it was.

As of the exploit’s disclosure, which happened on 19th of December, it has been a race to patch the database.

Sifting through Git history, it seems like the fix was initially committed on [the 17th of December](https://api.github.com/repos/mongodb/mongo/commits/505b660a14698bd2b5233bd94da3917b585c5728). Interestingly enough, it was only merged a full 5 days after - [on the 22nd of December](https://github.com/mongodb/mongo/commit/505b660a14698bd2b5233bd94da3917b585c5728#diff-e5f6e2daef81ce1c3c4e9f7d992bd6ff9946b3b4d98a601e4d9573e5ef0cb07dR77) (1-line fix btw).

That beig said, MongoDB 8.0.17 containing the fix [was released on Dec 19](https://www.mongodb.com/docs/manual/release-notes/8.0/?utm_source=chatgpt.com#8.0.17---dec-19--2025), consistent with the CVE publish data. But JIRA activity shows that patches went out on [the 22nd of December](https://jira.mongodb.org/browse/SERVER-115508).

Because there’s no official timeline posted, members of the community like me have to guess. As of writing, 10 days later in Dec 28, 2025, Mongo have [still NOT properly addressed the issue publicly](https://www.mongodb.com/company/blog).

They only issued [a community disclosure](https://www.mongodb.com/community/forums/t/important-mongodb-patch-available/332977) of the CVE **a full five days** after the publication of it. It is then, on the 24th of December, that they announced that all of their database instances in their cloud service Atlas were fully patched.

I believe this implies that all Atlas databases exposed to the internet were vulnerable to this issue for almost a week. By default, Atlas databases use an IP allowlist for connectivity. But users could configure it to allow connections from anywhere.

Mongo says that they haven’t verified exploitation so far:

> “at this time, we have no evidence that this issue has been exploited or that any customer data has been compromised”

### 3. Ease of Mitigation

Mitigation is admittedly very easy, you have one of two choices:

1. Update to the newest patch
2. Disable [zlib network compression](https://feed.yopp.me/%40alex/115787980028314032)

I found the latter wasn’t circulated a lot in online talk, but I understand is just as good as a short-term mitigation.

---

# A bit of Drama?

The tech lead for Security at Elastic coined the name MongoBleed by posting a Python script that acts as a proof of concept to exploiting the vulnerability: <https://github.com/joe-desimone/mongobleed>

[![](https://substackcdn.com/image/fetch/$s_!USk9!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb8c4b36c-c9bb-4fa1-b454-8a489b1433ab_1170x706.png)](https://substackcdn.com/image/fetch/%24s_%21USk9%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A//substack-post-media.s3.amazonaws.com/public/images/b8c4b36c-c9bb-4fa1-b454-8a489b1433ab_1170x706.png)

https://cyberplace.social/@GossiTheDog/115786817774728155

[![X avatar for @dez_](https://substackcdn.com/image/fetch/$s_!PzDh!,w_40,h_40,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fpbs.substack.com%2Fprofile_images%2F1350629165633036293%2FWMZ3Dd9C.jpg)

Joe Desimone@dez\_

🎅mongobleed - poc for CVE-2025-14847. Leaks data from mongodb instances due to flaw in zlib message decompression. Reminiscent of heartbleed ❤️

![](https://pbs.substack.com/media/G9DitlgXEAI-h2z.png)

12:39 AM · Dec 26, 2025 · 68.6K Views

---

11 Replies · 109 Reposts · 554 Likes](https://x.com/dez_/status/2004351287715156023?s=20)

This is particularly interesting, because despite being different systems, Mongo competes with Elastic on [Vector Search](https://www.mongodb.com/products/platform/atlas-vector-search), [Text Search](https://www.mongodb.com/products/platform/atlas-search) and [Analytical](https://www.mongodb.com/solutions/use-cases/analytics) use cases.

---

# Summary

* The exploit allows attackers to read arbitrary heap data, including user data, plaintext passwords, api keys/secrets, and more.
* It is performed by leveraging a simple, malformed zlib-compressed request.
* MongoDB versions from 2017-2025 are vulnerable to this exploit.
* Rough timeline:

  + June 1, 2017: [Commit introducing the bug](https://github.com/mongodb/mongo/commit/85d4a3a085c67f2258b60b07259db73e2f29ea50) gets merged.
  + Dec 17, 2025: Code for the fix is written ([original commit date](https://api.github.com/repos/mongodb/mongo/commits/505b660a14698bd2b5233bd94da3917b585c5728)).
  + Dec 19, 2025: CVE [officially published](https://www.cve.org/CVERecord?id=CVE-2025-14847).
  + Dec 22, 2025: Code with the fix *[is merged](https://github.com/mongodb/mongo/commit/505b660a14698bd2b5233bd94da3917b585c5728)*.
  + Dec 24, 2025: MongoDB [announce the patch](https://www.mongodb.com/community/forums/t/important-mongodb-patch-available/332977), say all Atlas databases are patched.
* On Dec 24th, MongoDB reported they have no evidence of anybody exploiting the CVE. Given the fact this exploit lived on for ~8 years, and their honey-pot cloud service Atlas took a full 5 days to patch since the official CVE publish date… I find that hard to believe.
* MongoDB have not apologized yet.
* There are [over 213k+](https://www.shodan.io/search?query=Product%3A%22MongoDB%22) potentially vulnerable internet-exposed MongoDB instances, ensuring that this exploit is **web scale**:

# Interesting Links

* Official CVE: <https://nvd.nist.gov/vuln/detail/CVE-2025-14847> (Dec 19, 2025)
* PR introducing the bug: <https://github.com/mongodb/mongo/pull/1152> (May 2017)
* Commit fixing the issue: <https://github.com/mongodb/mongo/commit/505b660a14698bd2b5233bd94da3917b585c5728#diff-e5f6e2daef81ce1c3c4e9f7d992bd6ff9946b3b4d98a601e4d9573e5ef0cb07dR77>
* Security Report on the incident, including fix versions: <https://www.ox.security/blog/attackers-could-exploit-zlib-to-exfiltrate-data-cve-2025-14847/>
* Write-up on how to detect exploitation attempts via log analysis: <https://blog.ecapuano.com/p/hunting-mongobleed-cve-2025-14847>
* Somebody also vibe-coded a detector: <https://github.com/Neo23x0/mongobleed-detector>

---

## Other Reads You May Like:

[![how AWS S3 serves 1 petabyte per second on top of slow HDDs](https://substackcdn.com/image/fetch/$s_!CqEG!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2c663f78-f119-45d7-83e8-fb4268dae83f_1456x1048.png)

#### how AWS S3 serves 1 petabyte per second on top of slow HDDs

[Stanislav Kozlovski](https://substack.com/profile/1057029-stanislav-kozlovski)

·

Sep 24

[Read full story](https://bigdata.2minutestreaming.com/p/how-aws-s3-scales-with-tens-of-millions-of-hard-drives)](https://bigdata.2minutestreaming.com/p/how-aws-s3-scales-with-tens-of-millions-of-hard-drives)

[![Event Streaming is Topping Out](https://substackcdn.com/image/fetch/$s_!2QUZ!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc9bd8bb2-f073-4e43-8ad5-e2446470645d_1456x816.png)

#### Event Streaming is Topping Out

[Stanislav Kozlovski](https://substack.com/profile/1057029-stanislav-kozlovski)

·

Nov 3

[Read full story](https://bigdata.2minutestreaming.com/p/event-streaming-is-topping-out)](https://bigdata.2minutestreaming.com/p/event-streaming-is-topping-out)

[![Why Was Apache Kafka Created?](https://substackcdn.com/image/fetch/$s_!EmZj!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F84290a14-c8e4-4bd7-b10c-03dfe7793d37_1200x630.png)

#### Why Was Apache Kafka Created?

[Stanislav Kozlovski](https://substack.com/profile/1057029-stanislav-kozlovski)

·

Aug 22

[Read full story](https://bigdata.2minutestreaming.com/p/why-was-apache-kafka-created)](https://bigdata.2minutestreaming.com/p/why-was-apache-kafka-created)

---

For more free high-signal technical content — subscribe! I only post when I have something interesting to say:

Subscribe

[1](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-anchor-1-182764771)

zlib is a library for compression. It uses the DEFLATE algorithm under the hood, but produces results in a specific wire format to ease sending such data over the wire. (e.g includes metadata like flags, checksums, etc)

[2](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-anchor-2-182764771)

Here is [the PR](https://github.com/mongodb/mongo/pull/1152) that introduced it. I’m not aware of Mongo’s public review practices, but it appears as if nobody explicitly reviewed the change.

[3](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-anchor-3-182764771)

They actually created it. There’s a very good site for it - <https://bsonspec.org/>

[4](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-anchor-4-182764771)

Weird, I know. Here are examples of different commands, just so you get a sense:

* Insert a document into the users table

```
{
  "insert": "users",
  "documents": [{ "name": "alice", "age": 30 }]
}
```

* Delete users with `inactive=true`

```
{
  "delete": "users",
  "deletes": [ { "q": { "inactive": true }, "limit": 0 }]
}
```

* Check the server’s status

```
{ "serverStatus": 1 }
```

[5](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-anchor-5-182764771)

I’m making this number up. There is probably some limit on the server side as to how large a request can be - perhaps 1MB is too large.

[6](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-anchor-6-182764771)

Here is the line (pre-fix): <https://github.com/mongodb/mongo/blame/b2f3ca9c996ba409e7d48601fca16c28fd58b774/src/mongo/transport/message_compressor_zlib.cpp#L83>

[![](https://substackcdn.com/image/fetch/$s_!pgmc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F899eecde-a646-4535-96e3-bc4d1c760fe4_2108x912.png)](https://substackcdn.com/image/fetch/%24s_%21pgmc%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A//substack-post-media.s3.amazonaws.com/public/images/899eecde-a646-4535-96e3-bc4d1c760fe4_2108x912.png)

`` `output` `` is the large memory buffer that was allocated earlier

The code, instead, ought to return the referenced `` `length` `` field, as that gets updated with the actual length that was seen post-compression.

[7](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-anchor-7-182764771)

This has been the cause of [many security issues](https://en.wikipedia.org/wiki/Null-terminated_string#:~:text=Null%2Dtermination%20has%20historically%20created%20security%20problems.%5B5) in the past.

[8](https://bigdata.2minutestreaming.com/p/mongobleed-explained-simply#footnote-anchor-8-182764771)

The most common comment I saw online is that you “deserved it” if you exposed your DB to the wild. 😁

1

1

Share

Previous

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2025 Stanislav Kozlovski · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture

This site requires JavaScript to run correctly. Please [turn on JavaScript](https://enable-javascript.com/) or unblock scripts

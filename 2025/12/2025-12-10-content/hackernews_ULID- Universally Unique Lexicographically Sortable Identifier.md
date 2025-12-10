---
source: hackernews
title: ULID: Universally Unique Lexicographically Sortable Identifier
url: https://packagemain.tech/p/ulid-identifier-golang-postgres
date: 2025-12-10
---

[![packagemain.tech](https://substackcdn.com/image/fetch/$s_!ya8w!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F42332f2e-7531-44b1-920c-bba7831fcdbe_777x777.png)](/)

# [![packagemain.tech](https://substackcdn.com/image/fetch/$s_!SmSF!,e_trim:10:white/e_trim:10:transparent/h_72,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcc9c835b-8ed4-4104-bf6a-992aec86a321_950x206.png)](/)

SubscribeSign in

# ULID: Universally Unique Lexicographically Sortable Identifier

### Using ULID identifiers in Go programs running on Postgres database.

[![Alex Pliutau's avatar](https://substackcdn.com/image/fetch/$s_!bpaK!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa0c6d6c3-6518-472f-8cce-d1c6a4ad5177_2000x2000.jpeg)](https://substack.com/%40pliutau)

[Alex Pliutau](https://substack.com/%40pliutau)

Dec 01, 2025

9

3

Share

The **UUID** format is a highly popular and amazing standard for unique identifiers. However, despite its ubiquity, it can be suboptimal for many common use-cases because of several inherent limitations:

* It isn’t the most character efficient or human-readable.
* **UUID v1/v2** is impractical in many environments, as it requires access to a unique, stable MAC address.
* **UUID v3/v5** requires a unique seed.
* **UUID v4** provides no other information than true randomness, which can lead to database fragmentation in data structures like B-trees, ultimately hurting write performance.

## Introducing the ULID Identifier

Few projects I worked on used the **ULID (Universally Unique Lexicographically Sortable Identifier)**, and I really enjoyed working with it, and would love to share this experience with you. Specifically for Go programs using Postgres database. But the same applies to other languages or databases too.

You can find the full spec here - [github.com/ulid/spec](https://github.com/ulid/spec)

```
ulid() // 01ARZ3NDEKTSV4RRFFQ69G5FAV
```

## What makes ULID great?

ULID addresses the drawbacks of traditional UUID versions by focusing on four key characteristics:

* Lexicographically sortable. Yes, you can sort the IDs. This is the single biggest advantage for database indexing.
* Case insensitive.
* No special characters (URL safe).
* It’s compatible with UUID, so you can still use native UUID columns in your database, for example.

ULID’s structure is key to its sortability. It is composed of 128 bits, just like a UUID, but those bits are structured for function: 48 bits of timestamp followed by 80 bits of cryptographically secure randomness.

```
 01AN4Z07BY      79KA1307SR9X4MV3

|----------|    |----------------|
 Timestamp          Randomness
   48bits             80bits
```

## Go+Postgres example

The power of ULID is its seamless integration into existing systems, even those relying on the UUID data type. Here is a demonstration using Go with the popular `pgx` [driver](https://github.com/jackc/pgx) for PostgreSQL and the [oklog/ulid](https://github.com/oklog/ulid) package.

The code below first connects to a running PostgreSQL instance and creates a table where the primary key is of type `UUID`. We then insert records using both standard UUID v4 and ULID.

```
package main

import (
  "context"
  "fmt"
  "os"

  "github.com/google/uuid"
  "github.com/jackc/pgx/v5"
  "github.com/oklog/ulid/v2"
)

func main() {
  ctx := context.Background()

  conn, err := pgx.Connect(ctx, "postgres://...")
  if err != nil {
    panic(err)
  }
  defer conn.Close(ctx)

  _, err = conn.Exec(ctx, `
CREATE TABLE IF NOT EXISTS ulid_test (
  id UUID PRIMARY KEY,
  kind TEXT NOT NULL,
  value TEXT NOT NULL
);`)
  if err != nil {
    panic(err)
  }

  insertUUID(ctx, conn, “1”)
  insertUUID(ctx, conn, “2”)
  insertUUID(ctx, conn, “3”)
  insertUUID(ctx, conn, “4”)
  insertUUID(ctx, conn, “5”)

  insertULID(ctx, conn, “1”)
  insertULID(ctx, conn, “2”)
  insertULID(ctx, conn, “3”)
  insertULID(ctx, conn, “4”)
  insertULID(ctx, conn, “5”)
}

func insertUUID(ctx context.Context, conn *pgx.Conn, value string) {
  id := uuid.New()
  conn.Exec(ctx, "INSERT INTO ulid_test (id, value, kind) VALUES ($1, $2, 'uuid')", id, value)

  fmt.Printf("Inserted UUID: %s\n", id.String())
}

func insertULID(ctx context.Context, conn *pgx.Conn, value string) {
  id := ulid.Make()

  // as you can see, we don’t need to format the ULID as a string, it can be used directly
  conn.Exec(ctx, "INSERT INTO ulid_test (id, value, kind) VALUES ($1, $2, 'ulid')", id, value)

  fmt.Printf("Inserted ULID: %s\n", id.String())
}
```

The oklog/ulid package implements the necessary interfaces (specifically, database/sql/driver.Valuer and encoding.TextMarshaler) that allow it to be automatically converted into a compatible format (like a string or []byte representation of the UUID) that the pgx driver can successfully map to the PostgreSQL UUID column type. This allows developers to leverage the sortable advantages of ULID without having to change the underlying database schema type in many popular environments.

This allows developers to leverage the sortable advantages of ULID without having to change the underlying database schema type in many popular environments.

## Sortability and Performance

The time-based prefix means that new ULIDs will always be greater than older ULIDs, ensuring that records inserted later will be physically placed at the end of the index. This contrasts sharply with UUID v4, where the sheer randomness means records are scattered throughout the index structure.

With traditional UUID v4, sorting records by their insert time is not possible without an extra column. When using ULID, the sort order is inherent in the ID itself, as demonstrated by the following database query output:

```
select * from ulid_test where kind = 'ulid' order by id;

019aaae4-be9c-d307-238f-be1692b3e8d7 | ulid | 1
019aaae4-be9d-011f-b82e-b870ca2abe9d | ulid | 2
019aaae4-be9f-e9d7-6efc-5b298ecc572b | ulid | 3
019aaae4-bea0-deae-6408-d89e7e3ce030 | ulid | 4
019aaae4-bea1-8ed2-c2f5-144bb1ffedde | ulid | 5
```

As we can see, the records are returned in the same order they were inserted. Furthermore, ULID is much shorter and cleaner when used in contexts like a URL:

```
/users/01KANDQMV608PBSMF7TM9T1WR4
```

ULID can generate 1.21e+24 unique ULIDs per millisecond, which should be more than enough for most applications.

## Limitations and the future

There’s really no major drawback to using ULID, but you should understand its limitations. For very (and I mean **very**) high-volume write systems, ULIDs can become problematic. Since all writes are clustered around the current timestamp, you will have **hot spots** around the current index key, which can potentially lead to slower writes and increased latency on that specific index block.

While other alternative identifiers exist, such as **CUID** or **NanoID**, the benefits of ULID have become a major factor in the evolution of unique identifier standards.

It is worth noting that the newest proposed standard for unique identifiers, **UUID v7**, aims to address the sortability and database performance issues of older UUID versions by adopting a similar time-ordered structure to ULID.

## Resources

* <https://github.com/ulid/spec>
* <https://github.com/oklog/ulid>
* <https://github.com/jackc/pgx>
* [Watch on our Youtube channel](https://www.youtube.com/watch?v=otW7nLd8P04)

9

3

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Will G.'s avatar](https://substackcdn.com/image/fetch/$s_!fl1y!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcdb47c36-14b9-485b-9aa1-81523052a35f_542x544.jpeg)](https://substack.com/profile/88631610-will-g?utm_source=comment)

[Will G.](https://substack.com/profile/88631610-will-g?utm_source=substack-feed-item)

[7d](https://packagemain.tech/p/ulid-identifier-golang-postgres/comment/183514765 "Dec 2, 2025, 9:19 PM")

Liked by Alex Pliutau

Love this!

Expand full comment

ReplyShare

[![Matthew's avatar](https://substackcdn.com/image/fetch/$s_!lqFd!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F31a87a03-1059-4c61-8e8f-61010ca3d6bf_144x144.png)](https://substack.com/profile/333490321-matthew?utm_source=comment)

[Matthew](https://substack.com/profile/333490321-matthew?utm_source=substack-feed-item)

[7d](https://packagemain.tech/p/ulid-identifier-golang-postgres/comment/183604695 "Dec 3, 2025, 2:27 AM")Edited

Uhhh what about UUID v7? Doesn't that solve the key painpoint?

Plus natively supported in postgres 18

<https://www.thenile.dev/blog/uuidv7>

Bit of a weird omission 😅

(I'm not implying ULID shouldn't be used, or is bad)

Expand full comment

ReplyShare

[1 reply by Alex Pliutau](https://packagemain.tech/p/ulid-identifier-golang-postgres/comment/183604695)

[1 more comment...](https://packagemain.tech/p/ulid-identifier-golang-postgres/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2025 Aliaksandr Pliutau · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture

This site requires JavaScript to run correctly. Please [turn on JavaScript](https://enable-javascript.com/) or unblock scripts

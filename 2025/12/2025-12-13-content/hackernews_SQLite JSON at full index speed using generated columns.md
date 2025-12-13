---
source: hackernews
title: SQLite JSON at full index speed using generated columns
url: https://www.dbpro.app/blog/sqlite-json-virtual-columns-indexing
date: 2025-12-13
---

[![Logo](/_next/image?url=%2Ficon.webp&w=96&q=75)![Logo](/_next/image?url=%2Ficon-dark.webp&w=96&q=75)

DB Pro](/)

* [Product](/#product)
* [Pricing](/#pricing)
* [About](/about)
* [Blog](/blog)

Toggle theme[Download](/download)

[Back to Blog](/blog)

# SQLite JSON Superpower: Virtual Columns + Indexing

![Jay Freeman](/_next/image?url=%2Fheadshot-jay.webp&w=64&q=75)Jay Freeman

December 12, 2025

We absolutely love [SQLite](https://sqlite.org) here at DB Pro. You'd be hard-pressed to find anyone who actively dislikes it. Sure, it has limitations, and I do mean limitations, not weaknesses. SQLite can absolutely be used in production when it's deployed properly and tuned with care.

SQLite has also seen something of a resurgence over the past few years. From being forked into projects like libSQL and Turso, to powering popular backend frameworks such as PocketBase, it’s clearly having a moment again.

As I said though, we love it. It even powers the local database inside DB Pro itself. For our use case, there really isn’t a better alternative.

Because we’ve been using SQLite in anger over the past three months, we’ve learnt a huge amount about it, including plenty of things we didn’t know before.

So I’m planning to write a short series of blog posts covering some of the cooler, more interesting features and nuances of SQLite that we’ve discovered along the way. This is the first of those posts.

First of all. Did you know SQLite has JSON functions and operators? I didn't until recently! I came across [this Hacker News comment when researching SQLite's JSON operators](https://news.ycombinator.com/item?id=37083561).

HACKERNEWS

```
The cool thing for working with json is to store each json document as is in one column, then make virtual columns that store some specific information you want to query, using some combination of json_extract, then index those columns.
This makes for super-fast search, and the best part is you don't have to choose what to index at insert time; you can always make more virtual columns when you need them.

(You can still also search non-indexed, raw json, although it may take a long time for large collections).

I love SQLite so much - bambax
```

![SQLite GIF](https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExdWhzOXd6cjViMHcwaXpiYnAzYjU0aWlpNjQ0bWtzYTl3MXQ2azVyOSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/huh7lUqEG4irK/giphy.gif)

I read that, then read it again, then again until I understood what bambax was saying. I was sort of in disbelief.

So I had to give it a try to see if it works. We've got an embedded SQLite-in-the-browser component on our blog and so I wanted to throw together some working examples for you guys (but mostly for me).

Let's break down what bambax is saying:

1. Store the JSON document raw
2. Create virtual generated columns using `json_extract`
3. Add indexes to those generated columns
4. Query JSON at full B-tree index speed

This means you never have to choose your indexing strategy up front. If you later realise you need to query on a new JSON field, you simply add another generated column, add an index, and you're done.

No data migration. No schema rewrite. No ETL. Just pure flexibility.

## 1. Store Raw JSON

First, create a simple table with a JSON column:

⚡

Loading SQL environment...

Your JSON documents are stored naturally, exactly as they arrive. No schema gymnastics required either!

## 2. Add Virtual Generated Columns

Here's where bambax is saying the magic happens. Let's add virtual generated columns. Generated columns compute values on demand. They don't actually store data:

⚡

Loading SQL environment...

I believe that no writes occur. There's no backfilling. It's instant. The virtual columns are computed on-the-fly from your JSON data whenever you query them.

## 3. Add Indexes for Full Performance

Now is the icing on the cake. We add indexes to make these virtual columns blazing fast:

⚡

Loading SQL environment...

Suddenly your JSON behaves like normal relational columns with full index support.

## 4. Query at Full Speed

Now your queries are blazing fast. So let's give it a go with some examples:

⚡

Loading SQL environment...

## 5. Need a New Query Pattern Later?

I believe this is one of the strongest points to this pattern. If at a later date your JSON shape changes (expected), you can just add another column and create another index.

For example, you realise you need to query by `user_id`:

SQL

```
ALTER TABLE events
ADD COLUMN user_id INTEGER
GENERATED ALWAYS AS (json_extract(data, '$.user.id')) VIRTUAL;

CREATE INDEX idx_events_user_id ON events(user_id);
```

![gif](https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExcjFwamNpcWx6dnVlajN4cWQxejZ3NHpxa2oxczZ5MGwydzZiMTd0ciZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/dbd6jN0Atb9i8/giphy.gif)

Boom. optimised without touching existing rows.

## Why This Pattern is So Powerful

This pattern completely changed how I think about working with JSON in SQLite. You get the flexibility of schemaless data, combined with the performance and ergonomics of a relational database, without committing yourself too early or painting yourself into a corner.

So, thanks bambax!

There are plenty more of these little SQLite superpowers hiding in plain sight. This is just the first one I wanted to share.

Thanks for reading!

Jay

---

[```
      *
     /|\
    /_|_\
   /__|__\
  /___|___\
     |||
```

Want to take part in a fun 24 day SQL challenge?

Advent of SQL 2025](/advent-of-sql)

[![Logo](/_next/image?url=%2Ficon.webp&w=96&q=75)![Logo](/_next/image?url=%2Ficon-dark.webp&w=96&q=75)](/)

## Your Complete Data Workbench

We're on a mission to build the best database desktop app around. Join us on this journey and help shape the future of database management.

[Download for macOS](/download)[Learn More](/blog/introducing-db-pro)

### Stay in the loop

Get product updates, launch news, and insights delivered to your inbox.

Subscribe

---

### Product

* [Product](/#product)
* [Pricing](/#pricing)
* [Download](/download)

### Databases

* [PostgreSQL](/postgres-desktop-client)
* [MySQL](/mysql-desktop-client)
* [SQLite](/sqlite-desktop-client)
* [Supabase](/supabase-desktop-client)
* [Turso](/turso-desktop-client)

### Account

* [Sign up](/sign-up)
* [Sign in](/sign-in)

### Company

* [About](/about)
* [Blog](/blog)
* [Contact](/contact)

### Community

* [Discord](https://discord.gg/FKKF65msZY)
* [YouTube](https://www.youtube.com/%40dbproapp)

### Legal

* [Privacy](/privacy)
* [Terms](/terms)
* [Cookies](/cookies)

Copyright © 2025 DB Pro. All rights reserved.

Made with  in United Kingdom

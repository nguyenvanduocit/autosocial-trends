---
source: hackernews
title: Instant database clones with PostgreSQL 18
url: https://boringsql.com/posts/instant-database-clones/
date: 2025-12-24
---

* [SQL Labs](https://labs.boringsql.com "SQL Labs")
* [Guides](https://boringsql.com/guides/ "Guides")
* [Talks](https://boringsql.com/talks/ "Talks")
* [Archive](https://boringsql.com/archive/ "Archive")
* [About](https://boringsql.com/about/ "About")

[Home](https://boringsql.com) »
[Posts](https://boringsql.com/posts/) »
[Instant database clones with PostgreSQL 18](https://boringsql.com/posts/instant-database-clones/)

# Instant database clones with PostgreSQL 18

2025-12-22 · 6 min · Radim Marek

Table of Contents

* [CREATE DATABASE ... STRATEGY](#create-database-strategy)
* [FILE\_COPY](#file-copy)
* [The benchmark](#the-benchmark)
* [Working with cloned data](#working-with-cloned-data)
* [XFS proof](#xfs-proof)
* [Things to be aware of](#things-to-be-aware-of)

Have you ever watched long running migration script, wondering if it's about
to wreck your data? Or wish you can "just" spin a fresh copy of database for
each test run? Or wanted to have reproducible snapshots to reset between
runs of your test suite, (and yes, because you are reading boringSQL) needed
to reset the learning environment?

When your database is a few megabytes, `pg_dump` and restore works fine. But
what happens when you're dealing with hundreds of megabytes/gigabytes - or more?
Suddenly "just make a copy" becomes a burden.

You've probably noticed that PostgreSQL connects to `template1` by default. What
you might have missed is that there's a whole templating system hiding in plain
sight. Every time you run

```
CREATE DATABASE dbname;
```

PostgreSQL quietly clones standard system database `template1` behind the
scenes. Making it same as if you would use

```
CREATE DATABASE dbname TEMPLATE template1;
```

The real power comes from the fact that you can replace `template1` with any
database. You can find more at [Template Database
documentation](https://www.postgresql.org/docs/current/manage-ag-templatedbs.html).

In this article, we will cover a few tweaks that turn this templating system
into an instant, zero-copy database cloning machine.

## CREATE DATABASE ... STRATEGY

Before PostgreSQL 15, when you created a new database from a template, it
operated strictly on the file level. This was effective, but to make it
reliable, Postgres had to flush all pending operations to disk (using
`CHECKPOINT`) before taking a consistent snapshot. This created a massive I/O
spike - a "Checkpoint Storm" - that could stall your production traffic.

Version 15 of PostgreSQL introduced new parameter `CREATE DATABASE ... STRATEGY = [strategy]` and at the same time changed the default behaviour how the new
databases are created from templates. The new default become `WAL_LOG` which
copies block-by-block via the Write-Ahead Log (WAL), making I/O sequential (and
much smoother) and support for concurrency without facing latency spike. This
prevented the need to CHECKPOINT but made the database cloning operation
potentially significantly slower. For an empty `template1`, you won't notice the
difference. But if you try to clone a 500GB database using WAL\_LOG, you are
going to be waiting a long time.

The `STRATEGY` parameter allows us to switch back to the original method
`FILE_COPY` to keep the behaviour, and speed. And since PostgreSQL 18, this
opens the whole new set of options.

## FILE\_COPY

Because the `FILE_COPY` strategy is a proxy to operating system file operations,
we can change how the OS handles those files.

When using standard file system (like `ext4`), PostgreSQL reads every byte of
the source file and writes it to a new location. It's a physical copy. However
starting with PostgreSQL 18 - `file_copy_method` gives you options to switch
that logic; while default option remains `copy`.

With modern filesystems (like ZFS, XFS with reflinks, APFS, etc.) you can switch
it to `clone` and leverage `CLONE` (`FICLONE` on Linux) operation for almost
instant operation. And it won't take any additional space.

All you have to do is:

* Linux with XFS or ZFS support (we will use XFS for the demostration) or similar
  operating system. MacOS APFS is also fully supported. FreeBSD with ZFS also
  supported (which normally would be my choice, but haven't got time to test so
  far)
* PostgreSQL cluster on that file system
* update the configuration `file_copy_method = clone`
* and reload the configuration

## The benchmark

We need some dummy data to copy. This is the only part of the tutorial where you
have to wait. Let's generate a ~6GB database.

```
CREATE DATABASE source_db;
\c source_db

CREATE TABLE boring_data (
    id serial PRIMARY KEY,
    payload text
);

-- generate 50m rows
INSERT INTO boring_data (payload)
SELECT md5(random()::text) || md5(random()::text)
FROM generate_series(1, 50000000);

-- force a checkpoint
CHECKPOINT;
```

You can verify the database now has roughly 6GB of data.

```
Name              | source_db
Owner             | postgres
Encoding          | UTF8
Locale Provider   | libc
Collate           | en_US.UTF-8
Ctype             | en_US.UTF-8
Locale            |
ICU Rules         |
Access privileges |
Size              | 6289 MB
Tablespace        | pg_default
Description       |
```

While enabling `\timing` you can test the default (WAL\_LOG) strategy. And on my
test volume (relatively slow storage) I get

```
CREATE DATABASE slow_copy TEMPLATE source_db;
CREATE DATABASE
Time: 67000.615 ms (01:07.001)
```

Now, let's verify our configuration is set for speed:

```
show file_copy_method;
 file_copy_method
------------------
 clone
(1 row)
```

Let's request the semi-instant clone of the same database, without taking
extra disk space at the same time.

```
CREATE DATABASE fast_clone TEMPLATE source_db STRATEGY=FILE_COPY;
CREATE DATABASE
Time: 212.053 ms
```

That's a quite an improvement, isn't it?

## Working with cloned data

That was the simple part. But what is happening behind the scenes?

When you clone a database with `file_copy_method = clone`, PostgreSQL doesn't
duplicate any data. The filesystem creates new metadata entries that point to
the same physical blocks. Both databases share identical storage.

This can create some initial confusion. If you ask PostgreSQL for the size:

```
SELECT pg_database_size('source_db') as source,
       pg_database_size('fast_clone') as clone;
```

PostgreSQL reports both as ~6GB because that's the logical size - how much data
each database "contains" - i.e. logical size.

```
-[ RECORD 1 ]------
source | 6594041535
clone  | 6594041535
```

The interesting part happens when you start writing. PostgreSQL doesn't update
tuples in place. When you UPDATE a row, it writes a new tuple version somewhere
(often a different page entirely) and marks the old one as dead. The filesystem
doesn't care about PostgreSQL internals - it just sees writes to 8KB pages. Any
write to a shared page triggers a copy of that entire page.

A single UPDATE will therefore trigger copy-on-write on multiple pages:

* the page holding the old tuple
* the page receiving the new tuple
* index pages if any indexed columns changed
* FSM and visibility map pages as PostgreSQL tracks free space

And later, VACUUM touches even more pages while cleaning up dead tuples. In this
case diverging quickly from the linked storage.

## XFS proof

Using the database OID and relfilenode we can verify the both databases are now
sharing physical blocks.

```
root@clone-demo:/var/lib/postgresql# sudo filefrag -v /var/lib/postgresql/18/main/base/16402/16404
Filesystem type is: 58465342
File size of /var/lib/postgresql/18/main/base/16402/16404 is 1073741824 (262144 blocks of 4096 bytes)
 ext:     logical_offset:        physical_offset: length:   expected: flags:
   0:        0..    2031:   10471550..  10473581:   2032:             shared
   1:     2032..   16367:   10474098..  10488433:  14336:   10473582: shared
   2:    16368..   32751:   10497006..  10513389:  16384:   10488434: shared
   3:    32752..   65519:   10522066..  10554833:  32768:   10513390: shared
   4:    65520..  129695:   10571218..  10635393:  64176:   10554834: shared
   5:   129696..  195231:   10635426..  10700961:  65536:   10635394: shared
   6:   195232..  262143:   10733730..  10800641:  66912:   10700962: last,shared,eof
/var/lib/postgresql/18/main/base/16402/16404: 7 extents found
root@clone-demo:/var/lib/postgresql#
root@clone-demo:/var/lib/postgresql#
root@clone-demo:/var/lib/postgresql# sudo filefrag -v /var/lib/postgresql/18/main/base/16418/16404
Filesystem type is: 58465342
File size of /var/lib/postgresql/18/main/base/16418/16404 is 1073741824 (262144 blocks of 4096 bytes)
 ext:     logical_offset:        physical_offset: length:   expected: flags:
   0:        0..    2031:   10471550..  10473581:   2032:             shared
   1:     2032..   16367:   10474098..  10488433:  14336:   10473582: shared
   2:    16368..   32751:   10497006..  10513389:  16384:   10488434: shared
   3:    32752..   65519:   10522066..  10554833:  32768:   10513390: shared
   4:    65520..  129695:   10571218..  10635393:  64176:   10554834: shared
   5:   129696..  195231:   10635426..  10700961:  65536:   10635394: shared
   6:   195232..  262143:   10733730..  10800641:  66912:   10700962: last,shared,eof
/var/lib/postgresql/18/main/base/16418/16404: 7 extents found
```

All it takes is to update some rows using

```
update boring_data set payload = 'new value' || id where id IN (select id from boring_data limit 20);
```

and the situation will start to change.

```
root@clone-demo:/var/lib/postgresql# sudo filefrag -v /var/lib/postgresql/18/main/base/16402/16404
Filesystem type is: 58465342
File size of /var/lib/postgresql/18/main/base/16402/16404 is 1073741824 (262144 blocks of 4096 bytes)
 ext:     logical_offset:        physical_offset: length:   expected: flags:
   0:        0..      39:   10471550..  10471589:     40:
   1:       40..    2031:   10471590..  10473581:   1992:             shared
   2:     2032..   16367:   10474098..  10488433:  14336:   10473582: shared
   3:    16368..   32751:   10497006..  10513389:  16384:   10488434: shared
   4:    32752..   65519:   10522066..  10554833:  32768:   10513390: shared
   5:    65520..  129695:   10571218..  10635393:  64176:   10554834: shared
   6:   129696..  195231:   10635426..  10700961:  65536:   10635394: shared
   7:   195232..  262143:   10733730..  10800641:  66912:   10700962: last,shared,eof
/var/lib/postgresql/18/main/base/16402/16404: 7 extents found
root@clone-demo:/var/lib/postgresql# sudo filefrag -v /var/lib/postgresql/18/main/base/16418/16404
Filesystem type is: 58465342
File size of /var/lib/postgresql/18/main/base/16418/16404 is 1073741824 (262144 blocks of 4096 bytes)
 ext:     logical_offset:        physical_offset: length:   expected: flags:
   0:        0..      39:   10297326..  10297365:     40:
   1:       40..    2031:   10471590..  10473581:   1992:   10297366: shared
   2:     2032..   16367:   10474098..  10488433:  14336:   10473582: shared
   3:    16368..   32751:   10497006..  10513389:  16384:   10488434: shared
   4:    32752..   65519:   10522066..  10554833:  32768:   10513390: shared
   5:    65520..  129695:   10571218..  10635393:  64176:   10554834: shared
   6:   129696..  195231:   10635426..  10700961:  65536:   10635394: shared
   7:   195232..  262143:   10733730..  10800641:  66912:   10700962: last,shared,eof
/var/lib/postgresql/18/main/base/16418/16404: 8 extents found
root@clone-demo:/var/lib/postgresql#
```

In this case extent 0 no longer has shared flag, first 40 blocks size (with
default size 4KB) now diverge, making it total of 160KB. Each database now has
its own copy at different physical address. The remaining extents are still
shared.

## Things to be aware of

Cloning is tempting but there's one serious limitation you need to be aware if
you ever attempt to do it in production. The source database can't have any
active connections during cloning. This is a PostgreSQL limitation, not a
filesystem one. For production use, this usually means you create a dedicated
template database rather than cloning your live database directly. Or given the
relatively short time the operation takes you have to schedule the cloning in
times where you can temporary block/terminate all connections.

Other limitation is that the cloning only works within a single filesystem. If
your databases spans multiple table spaces on different mount points, cloning
will fall back to regular physical copy.

Finally, in most managed cloud environments (AWS RDS, Google Cloud SQL), you
will not have access to the underlying filesystem to configure this. You are
stuck with their proprietary (and often billed) functionality. But for your own
VMs or bare metal? Go ahead and try it.

### 📅 Upcoming Events

Join me at these PostgreSQL meetups to chat about databases, SQL, and all things data

* 28 Jan '26
  [Prague PostgreSQL Dev Day 2026
  speaking](https://p2d2.cz/en/)

[View past talks and slides →](/talks/)

* [postgresql](https://boringsql.com/tags/postgresql/)
* [expert](https://boringsql.com/tags/expert/)

[← Previous
VACUUM Is a Lie (About Your Indexes)](https://boringsql.com/posts/vacuum-is-lie/)

© 2024 [boringSQL](https://boringsql.com), site by Clusterity s.r.o.; hosted in EU at [Hetzner Cloud](https://hetzner.cloud/?ref=FBqY7NPcckJ3).

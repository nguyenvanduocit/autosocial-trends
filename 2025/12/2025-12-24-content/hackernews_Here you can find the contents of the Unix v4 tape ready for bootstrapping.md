---
source: hackernews
title: Here you can find the contents of the Unix v4 tape ready for bootstrapping
url: http://squoze.net/UNIX/v4/README
date: 2025-12-24
---

[github](https://github.com/aap/) |
[mastodon](https://mastodon.sdf.org/%40aap) |
[TX-0](http://tx-0.net) |
[PDP-1](http://pdp-1.net) |
[PDP-6/10](http://pdp-6.net) |

Related sites:
| [site map](/sitemap)

# [squoze.net](/)

* [› B/](/B/)
* [› C/](/C/)
* [› NB/](/NB/)
* [» *UNIX/*](/UNIX/)
* + [› 32v/](/UNIX/32v/)
  + [› V6 on rl01](/UNIX/V6_on_rl01)
  + [› bltj/](/UNIX/bltj/)
  + [› fs/](/UNIX/fs/)
  + [› restored/](/UNIX/restored/)
  + [› sysIII pdp11/](/UNIX/sysIII_pdp11/)
  + [› sysIII vax/](/UNIX/sysIII_vax/)
  + [› sysV pdp11/](/UNIX/sysV_pdp11/)
  + [› v1man/](/UNIX/v1man/)
  + [› v2man/](/UNIX/v2man/)
  + [› v3man/](/UNIX/v3man/)
  + [» *v4/*](/UNIX/v4/)
  + - [» *README*](/UNIX/v4/README)
  + [› v4man/](/UNIX/v4man/)
  + [› v5/](/UNIX/v5/)
  + [› v5man/](/UNIX/v5man/)
  + [› v6/](/UNIX/v6/)
  + [› v6man/](/UNIX/v6man/)
  + [› v7/](/UNIX/v7/)
  + [› v8/](/UNIX/v8/)
* [› ling/](/ling/)
* [› math/](/math/)
* [› misc/](/misc/)
* [› plan 9/](/plan_9/)

# UNIX Fourth Edition

Here you can find the contents of
the [UNIX v4 tape](https://archive.org/details/utah_unix_v4_raw)
ready for bootstrapping, including a tar file of the filesystem.

* <unix_v4.tap> is the original tape file in simh format
* <bootstrap> are the first 38400 bytes of the tape
* <disk.rk> is the rest of the tape, an RK05 image
* <unix_v4.tar> is the filesystem extracted
* <install.ini> is an ini file for simh to install the system
* <boot.ini> is an ini file for simh to boot the system

At first I extracted the disk image manually from the tape,
which resulted in `bootstrap` and `disk.rk`.
These are really nothing more than
the first 38400 bytes of the raw tape content and the rest.
Because `unix_v4.tap` is block based, one first has to strip it of its block sizes
to get the raw content.

Actually it's easier to just use the tape as it is and install a new system from it.

# Installing the system

To install the system we just dump an RK05 disk image from tape to disk:

```
% pdp11 install.ini

[...]
        ; boot from TM0, now in mboot
=list
dldr
dtf
list
mboot
mcopy
rkf
tboot
uboot
=mcopy
'p' for rp; 'k' for rk
k
disk offset
0
tape offset
75
count
4000
=
```

Afterwards, we can just load `uboot` from tape to start UNIX:

```
=uboot
k
unix
mem = 64530

login: root
# ls
bin
dev
etc
lib
mnt
tmp
unix
usr
# sync
# sync
# sync
# ^E        ; end emulation
```

# Running the system

To boot the system we don't need the tape.
Instead, we load `uboot` directly from the boot sector.
We specify `k` for RK05, then the filename `unix`.

```
% pdp11 boot.ini

[...]
        ; boot from RK0, now in uboot
k
unix
mem = 64530

login: root
#
```

# Rebuilding the kernel

Put the following into `/usr/sys/run`:

```
rm -f low.o mch.o conf.o lib1 lib2

chdir ken
cc -c *.c
sh mklib
rm *.o

chdir ../dmr
cc -c *.c
sh mklib
rm *.o

chdir ..
cc -c conf/conf.c
mv conf/conf.o conf.o
as conf/low.s
mv a.out low.o
as conf/mch.s
mv a.out mch.o
ld -x low.o mch.o conf.o lib1 lib2
```

`/usr/sys/ken/mklib`:

```
ar r ../lib1 main.o
ar r ../lib1 alloc.o
ar r ../lib1 iget.o
ar r ../lib1 prf.o
ar r ../lib1 rdwri.o
ar r ../lib1 slp.o
ar r ../lib1 subr.o
ar r ../lib1 text.o
ar r ../lib1 trap.o
ar r ../lib1 sig.o
ar r ../lib1 sysent.o
ar r ../lib1 sys1.o
ar r ../lib1 sys2.o
ar r ../lib1 sys3.o
ar r ../lib1 sys4.o
ar r ../lib1 nami.o
ar r ../lib1 fio.o
ar r ../lib1 clock.o
```

`/usr/sys/dmr/mklib`:

```
ar r ../lib2 bio.o
ar r ../lib2 tty.o
ar r ../lib2 malloc.o
ar r ../lib2 pipe.o
ar r ../lib2 cat.o
ar r ../lib2 dc.o
ar r ../lib2 dn.o
ar r ../lib2 dc.o
ar r ../lib2 dn.o
ar r ../lib2 dp.o
ar r ../lib2 kl.o
ar r ../lib2 mem.o
ar r ../lib2 pc.o
ar r ../lib2 rf.o
ar r ../lib2 rk.o
ar r ../lib2 tc.o
ar r ../lib2 tm.o
ar r ../lib2 vs.o
ar r ../lib2 vt.o
ar r ../lib2 partab.o
ar r ../lib2 rp.o
ar r ../lib2 lp.o
ar r ../lib2 dhdm.o
ar r ../lib2 dh.o
ar r ../lib2 dhfdm.o
```

Then in `/usr/sys`:

```
# sh run
alloc.c:
clock.c:
fio.c:
iget.c:
main.c:
nami.c:
prf.c:
rdwri.c:
sig.c:
60: Warning: assignment understood
61: Warning: assignment understood
slp.c:
subr.c:
sys1.c:
sys2.c:
sys3.c:
sys4.c:
sysent.c:
text.c:
trap.c:
bio.c:
cat.c:
dc.c:
dh.c:
dhdm.c:
dhfdm.c:
dn.c:
dp.c:
dv.c:
kl.c:
lp.c:
malloc.c:
mem.c:
partab.c:
pc.c:
pipe.c:
rf.c:
rk.c:
rp.c:
tc.c:
tm.c:
tty.c:
vs.c:
vt.c:
#
```

Now install and boot the new kernel:

```
# mv a.out /nunix
# sync
# sync
# sync
# ^E
Simulation stopped, PC: 002040 (MOV (SP)+,177776)
sim> b rk
k
nunix
mem = 64529

login: root
#
```

# TODO

There are a bunch of other things I would like to document
(or do in the first place):

* automatic installation (expect)
* configure kernel (/dev/mem missing)
* add device files
* add man pages
* try rp disk
* try rf swap (ps assumes rf0)

Reconstruction:

* get pre-v4 nsys kernel to work (buffer handling seems broken, i suspect synchronization bugs)
* get B restoration to work

[Powered by werc](http://werc.cat-v.org/)

[User Login](/_users/login)

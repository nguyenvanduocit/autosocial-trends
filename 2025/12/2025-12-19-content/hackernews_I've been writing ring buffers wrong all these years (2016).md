---
source: hackernews
title: I've been writing ring buffers wrong all these years (2016)
url: https://www.snellman.net/blog/archive/2016-12-13-ring-buffers/
date: 2025-12-19
---

[Juho Snellman's Weblog](https://www.snellman.net/blog/)

[Home](https://www.snellman.net/blog/) – [About](/blog/archive/about/) – [Archives](/blog/list/index/) – [Subscribe](/blog/subscribe/) – [Links](/lastweek/)

## [I've been writing ring buffers wrong all these years](https://www.snellman.net/blog/archive/2016-12-13-ring-buffers/)

Posted on 2016-12-13 in [General](/blog/list/general/)

So there I was, implementing a one element ring buffer. Which,
I'm sure you'll agree, is a perfectly reasonable data structure.

It was just surprisingly annoying to write, due to reasons we'll
get to in a bit. After giving it a bit of thought, I realized I'd always
been writing ring buffers "wrong", and there was a better way.

### Array + two indices

There are two common ways of implementing a queue with a ring buffer.

One is to use an array as the backing storage plus two indices
to the array; read and write. To shift a
value from the head of the queue, index into the array by the read
index, and then increment the read index. To push a value to the back,
index into the array by the write index, store the value in that
offset, and then increment the write index.

Both indices will always be in the range 0..(capacity - 1). This
is done by masking the value after an index gets incremented.

That implementation looks basically like:

```
uint32 read;
uint32 write;
mask(val)  { return val & (array.capacity - 1); }
inc(index) { return mask(index + 1); }
push(val)  { assert(!full()); array[write] = val; write = inc(write); }
shift()    { assert(!empty()); ret = array[read]; read = inc(read); return ret; }
empty()    { return read == write; }
full()     { return inc(write) == read; }
size()     { return mask(write - read); }
```

The downside of this representation is that you always waste one element
in the array. If the array is 4 elements, the queue can hold at most 3. Why?
Well, an empty buffer will have a read pointer that's equal
to the write pointer; a buffer with capacity N and size N would also
have have a read pointer equal to the write pointer. Like this:

![](/blog/stc/images/rb-normal.png)

The 0 and 4 element cases are indistinguishable, so we need to prevent one
from ever happening. Since empty queues are kind of necessary, it follows
that the latter case needs to go. The queue has to be defined
as full when one element in the array is still unused. And that's the
way I've always done it.

Losing one element isn't a huge deal when the ring buffer has thousands
of elements. But when the array is supposed to have just one element...
That's 100% overhead, 0% payload!

### Array + index + length

The alternative is to use one index field and one length field. Shifting
an element indexes to the array by the read index, increments the read
index, and then decrements the length. Pushing an element writes to
the slot that "length" elements after the read index, and then
increments the length. That looks something like this:

```
uint32 read;
uint32 length;
mask(val)  { return val & (array.capacity - 1); }
inc(index) { return mask(index + 1); }
push(val)  { assert(!full()); array[mask(read + length++)] = val; }
shift()    { assert(!empty()); --length; ret = array[read]; read = inc(read); return ret; }
empty()    { return length == 0; }
full()     { return length == array.capacity; }
size()     { return length; }
```

This uses the full capacity of the array, with the code not getting much more
complex.

But at least I've never liked this representation. The most common use
for ring buffers is for it to be the intermediary between a concurrent
reader and writer (be it two threads, to processes sharing memory, or
a software process communicating with hardware). And for that, the
index + size representation is kind of miserable. Both the reader and
the writer will be writing to the length field, which is bad for
caching. The read index and the length will also need to always be
read and updated atomically, which would be awkward.

(Obviously my one element ring buffer wasn't going to be used in a
concurrent setting. But it's a matter of principle.)

### Array + two unmasked indices

So is there an option that gets the benefits of both
representations, without introducing a third state variable?
(Whether it's two indices + a size, or two indices + some kind
of a full vs. empty flag). Turns out there is, and it's really
simple. It uses two indices, but with one tweak compared to the
first solution: don't squash the indices into the correct
range when they are incremented, but only when they are used to index into
the array. Instead you let them grow unbounded, and eventually
wrap around to zero once the unsigned integer overflows. So:

![](/blog/stc/images/rb-nowrap.png)

This reclaims the wasted slot.
The code modifying the indices also becomes simpler, since the
clumsy ordering of increments vs. array accesses was only needed for
maintaining the invariant that the index is always in range.

```
uint32 read;
uint32 write;
mask(val)  { return val & (array.capacity - 1); }
push(val)  { assert(!full()); array[mask(write++)] = val; }
shift()    { assert(!empty()); return array[mask(read++)]; }
```

Checking the status of the ring also gets simpler:

```
empty()    { return read == write; }
full()     { return size() == array.capacity }
size()     { return write - read; }
```

This all works, assuming the following restrictions:

* The implementation language supports wraparound on unsigned
  integer overflow.
  If it doesn't, this approach doesn't really buy anything. (What will
  happen in these languages is that the indices get promoted to bignums
  which will be bad, or they get promoted to doubles which will be worse.
  So you'll need to manually restrict their range anyway).* The capacity must always be a power of two. (**Edit**: This
    limitation does not come just from the definition of `mask`
    using a bitwise `and`.
    It applies even if mask were defined using modular arithmetic or a
    conditional. It's required for the code to be correct on unsigned
    integer overflow.)* The maximum capacity can only be half the range of the index
      data types. (So 2^31-1 when using 32 bit unsigned integers).
      In a way that could be interpreted as stealing the top bit of the
      index to function as a flag. But the case against flags isn't so much
      the extra memory as having to maintain the extra state.

All of those seem like non-issues. What kind of a monster would make
a non-power of two ring anyway?

This is of course not a new invention. The earliest instance I
could find with a bit of searching was from 2004, with Andrew Morton
[mentioning
in it a code review](http://lkml.iu.edu/hypermail/linux/kernel/0409.1/2709.html) so casually that it seems to have been a
well established trick. But the vast majority of implementations
I looked at do not do this.

So here's the question: Why do people use the version that's inferior
and more complicated? I've must have written a dozen ring buffers over
the years, and before being forced to really think about it, I'd always
just used the first definition. I can understand why a textbook wouldn't
take advantage of unsigned integer wraparound. But it seems like it
should be exactly the kind of cleverness that hackers would relish
using and passing on.

* Could it just be tradition? It seems likely that
  this is the kind of thing one learns by osmosis, and then never
  revisits. But even so, you'd expect the "good"
  implementations to push out the "bad" ones at some
  point, which doesn't seem to be happening in this case.* Is it resistance to having code actually take advantage of integer
    overflow, rather than it be a sign of a bug?* Are non-power of two capacities for ring buffers actually
      common?

Join me next week for the exciting sequel to this post, "I've
been tying my shoelaces wrong all these years".

If you liked this and want to be notified of new posts, [follow me on Twitter](https://twitter.com/intent/follow?screen_name=juhosnellman)

Like word games or puzzles? Try out my new word puzzle [Huewords](https://huewords.snellman.net)

Next » [Computing multiple hash values in parallel with AVX2](https://www.snellman.net/blog/archive/2017-03-19-parallel-hashing-with-avx2/)

Previous « [The hidden cost of QUIC and TOU](https://www.snellman.net/blog/archive/2016-12-01-quic-tou/)

### Comments

By Mike Spooner on 2016-12-13

If you replace masking with integer-modulo-the-size, you can use sizes that are not powers of two. More expensive but handy if you really \*need\* a 17-byte ring.

By Juho on 2016-12-13

Unfortunately I don't think that works. It needs to be a power of two or the integer overflow causes a discontinuity. Let's say we're using mod 17. The last slot before wraparound would map to slot 0 in the array:

0xffffffff % 17 == 0

Increment it by one, it wraps around to 0, which obviously again maps to slot 0.

0 % 17 == 0

By Peter Bhat Harkins on 2016-12-13

> Join me next week for the exciting sequel to this post, "I've been tying my shoelaces wrong all these years".

Yep, that's how I felt after poking around Ian's Shoelace Site a bit: <http://www.fieggen.com/shoelace/knots.htm>

By Bob on 2016-12-13

0xffffffff % 17 == 0

No, it doesn't.

% works fine for a ring buffer of any size.

By NoOverflow on 2016-12-13

Use uint64 and pretend that you will never ever need to wrap around?

By Juho on 2016-12-13

NoOverflow,

That's certainly an option. But that bumps up the memory use of the non-array parts from 3\*4 bytes to 3\*8 bytes for the indices vs. reclaiming one slot in the array. (You need three integers of this size: Read index, write index, and capacity). It might still be a win over the naive version if the elements are large enough.

By Jack on 2016-12-13

0xffffffff % 17 DOES equal 0. His point stands.

By Mike Spooner on 2016-12-14

Standing corrected, there is a discontinuity at overflow, although the 17 case is less than obvious at first glance because it divides Oxffffffff \*exactly\*. 49 is a more illuminating case: 0xffffffff % 49 == 38, and with 16-bit unsigned int, the "next" value would be 0 rather than the desired 39, followed by 1 rather than 40, a clearer oops. To use modulo, you would need to \*reduce\* the value modulo-N at each step, which is no more expensive.

By Mike Spooner on 2016-12-14

Standing corrected, there is a discontinuity at overflow, although the 17 case is less than obvious at first glance because it divides Oxffffffff \*exactly\*. 49 is a more illuminating case: 0xffffffff % 49 == 38, and with 16-bit unsigned int, the "next" value would be 0 rather than the desired 39, followed by 1 rather than 40, a clearer oops. To use modulo, you would need to \*reduce\* the value modulo-N at each step, which is no more expensive.

By vy on 2016-12-14

You may be interested in this implementation.

<http://yodaiken.com/wp-content/docs/owq.tar>

By dizzy57 on 2016-12-14

Why not store indices modulo 2\*capacity? Same as last solution, but prevents overflow. An additional bit in the index can be viewed as a 'fold number'. An array is full when indices refer to the same cell on different folds, and empty if they are on the same fold. And we don't need more then two folds, as indices can't be more than 'capacity' elements apart.

By jj on 2016-12-14

dizzy57 has the answer. All the talk of when to mask is misguided. The original implementation had a simple problem, the need for one extra boolean to distinguish empty from full. The proposed fix uses all of the bits that were being masked off to implement that bit. Instead, just use one (which is what dizzy57's proposal does).

By Freddy Bob on 2016-12-14

<http://www.fieggen.com/shoelace/ianknot.htm>

By Howard Lewis Ship on 2016-12-14

I wish I was at my office where I have my copy of The Art of Computer Programming; I strongly suspect if Knuth covers Ring Buffers this would be there.

By Ali on 2016-12-14

This can be trivially solved by using a INT\_MAX for the read pointer when the array is empty.

uint32 read; uint32 write; mask(val) { return val & (array.capacity - 1); } inc(index) { return mask(index + 1); } push(val) { assert(!full()); if (empty()) read = write; array[write] = val; write = inc(write); } shift() { assert(!empty()); ret = array[read]; read = inc(read); if (read == write) read = INT\_MAX; return ret; } empty() { return read == INT\_MAX; } full() { return read == write; } size() { return mask(write - read); }

By Toma on 2016-12-14

What kind of a monster would make a non-power of two ring? The kind that ran out of microcontroller SRAM for a large enough power of two ring, but could spare CPU cycles for an expensive modulo operation.

By Darrell Wright on 2016-12-14

uin64\_t is so large that it would take over 100 years to overflow. Don't worry about it. :)

By SmallStepForMan on 2016-12-14

Instead if using 2 indices for read/write, use 1 index for write, and an int for count pending. Read\_index=write\_index - pending. When pending=0, there is nothing more to read, and when pending=capacity, your buffer is full.

By Jim Fisher on 2016-12-14

I spent a few minutes working out how `val & (array.capacity - 1)` could possibly work. I now realize that this only works if the array has a power-of-two length. This seems to be an implicit assumption of the article.

By sepia on 2016-12-14

I think a two indices version is the better option to go with when you intend to do it thread safe. If there was a way to atomically put a new indice in place, the code would be thread safe by default. x86 provides an atomic version in case you prefer to play with assembler rather than mutexes.

By Alex on 2016-12-14

This looks similar to willemt's mmap ring buffers: https://github.com/willemt/cbuffer

The main difference is that you use mask() to wrap the index before indexing into the array, and willemt asks the operating system to map the memory out there to the same place so it's automatic.

The justification and the details are a bit different, but his implementation, like yours, has the nice property that offer() and poll() (his names for these functions) each only need to write one pointer.

By Tensility on 2016-12-15

@Toma: 2^0 == 1

By Anonymous on 2016-12-15

Hardware engineers have been doing this for decades -- we generally describe the head/tail (put/get) as free-running indexes here.

By Dave on 2016-12-15

See Disruptor pattern for something better

By Jesper on 2016-12-15

I reached the same conclusion when writing https://github.com/jbro/ring\_buffer.

It also uses a neat trick with mmap, so the rings pages are wired into memory twice. That way, when writing multiple elements, you can just write them over the end, and they will magically appear in the front of the ring.

Only downside, is that instead of having your power of two constraint, I have a must be a multiple of the page\_size.

By Bram on 2016-12-15

Does it still work if write wraps around at 4G writes? (Note: I don't mean wrap around in buffer, but overflow in uint32.

By Chris on 2016-12-15

All in all, unless the subtly weird code resulting from this lengthy analysis yields \*significantly\* better performance and big-O scaling, I think it's better just to use the original, straightforward, intuitive algorithm, with the dreaded Third Variable (I prefer the empty-vs-non-empty flag, myself). That leaves behind more-readable, more-easily-understood code for the next guy. If everybody were an ivory-tower programming guru who could look at the code that resulted from all this navel-gazing and understand it without having to have this article handy to reverse-engineer it, that would be fine. But, frankly, in thirty years as a software engineer, I've met \*maybe one\* guy who could have done it - - but he was back in the days when Men Were Men and I myself could decompile a puece of executable code back to Fortran, by hand. In the last twenty-five years, certainly, there's been nobody else. So, if you use this in production code, and your product is anything other than a write-once, never-look-at-it-again utility library, OS, etc., the next guy in your job is going to curse you, especially if it's been a while and this article has fallen off the Internet. That poor bastard. (At the very least, \*print\* this article, make six copies, and leave them behind when you go.)

I'm also going slowly insane trying to figure out why on Earth anybody would need a \*one-element\* ring buffer. That's barely a ring buffer at all, so I deduce you must be less interested in the buffering aspect than in the lockstep serialization of writes and reads - - and from, again, the clarity-and-readability standpoint, there are undoubtedly better ways to achieve that. But who am I? What do I know? Maybe there's some well-known purpose for a one-element ring buffer, that wasn't yet well-known the lady time I implemented one (circa 1991). Just tell us!

By Chris on 2016-12-15

Typo correction in the second-to-last sentence of my first post: "lady" should be "last". (This is what I get for writing from my phone, in bed, first thing upon waking up.)

By Juho on 2016-12-15

Chris,

I wrote a bit on that in this HN comment: <https://news.ycombinator.com/item?id=13178348>

Bram,

Yes. Any time this post talks about overflow, it is talking about exactly about that: an unsigned integer wrapping around to 0.

By Adam Sawicki on 2016-12-15

That's a great post, thank you so much! I think that people (including me) tend to do it the "two masked indices" way before they learn the better way is that this is the most intuitive solution that comes first when thinking about the problem.

By wheels on 2016-12-15

I've always used the first method, personally. Once, though, I had to fix a bug in someone else's ring buffer implementation, which involved several \*pages\* of code implementing a state machine with 64 states.

As much as I'd have liked to rip it out and replace it, the entire system it was part of had so many intertwingled "moving parts" that I couldn't guarantee that it would be safe.

By Edward Kmett on 2017-04-18

The Chase-Lev workstealing deque is based on the "good" approach mentioned here:

<https://pdfs.semanticscholar.org/3771/77bb82105c35e6e26ebad1698a20688473bd.pdf>

The main additions are growing the array, and ensuring reads/writes are done in the correct manner to ensure 'stealing' an be done lock-free, but all indices are left unmasked.

By Aristotle Pagaltzis on 2017-04-23

I had the same thought as dizzy57 but also actually went and implemented it to get a feel for it. It turns out to be more complicated than the original approach, but just slightly, and less complicated than the length+index approach.

uint32 read; uint32 write;

mask(val) { return val & (array.capacity - 1); }

mask2(val) { return val & (array.capacity \* 2 - 1); }

push(val) { assert(!full()); array[mask(write++)] = val; write = mask2(write); }

shift() { assert(!empty()); ret = array[mask(read++)]; read = mask2(read); return ret; }

empty() { return size() == 0; }

full() { return size() == array.capacity; }

size() { return mask(write - read); }

This avoids the wasted buffer slot, only updates one index for any given operation, and generalises to arrays of arbitrary sizes (by switching to arithmetic mask()/mask2() functions).

By Aristotle Pagaltzis on 2017-04-23

Actually, the empty() I gave is unnecessarily complex (and dizzy57 was first on that too)  just `return read==write` will do.

By By Aristotle Pagaltzis on 2017-04-23

Also, I botched size()  once slots are no longer wasted, clamping the value to `array.capacity - 1` is wrong:

size() { return mask2(write - read); }

I should have written some tests (Disclaimer: and I still havent.)

By TheBonobo on 2017-08-10

BUMP!

Surely, there is a text book version which is easier to understand that uses mod n+1 (\*) arithmetic?

Let me write one...

(\*In addition to the existing mod n code)

By mjm on 2017-09-06

I seem to be missing something obvious. Since write should increment ahead of read, there should be a time that write has rolled and read hasn't. It seems to me that (write - read) for determining full() would not work correctly then. What am I missing?

By mjm on 2017-09-06

I seem to be missing something obvious. Since write should increment ahead of read, there should be a time that write has rolled and read hasn't. It seems to me that (write - read) for determining full() would not work correctly then. What am I missing?

By Juho on 2017-09-07

mjm,

The wraparound will work correctly since these are unsigned integers and the ring size is a power of two.

By Jens on 2017-11-08

> And for that, the index + size representation is kind of miserable. Both the reader and the writer will be writing to the length field, which is bad for caching.

I don't follow this argument. Assuming that writer and reader update distinct fields you still have bad caching. Because assuming both values are on the same cache line (which is often the case I'll guess) you have to keep the cache line coherent anyway. But I do understand that from a threading point of view updating more than one value atomically is worse than updating only one.

By andrewl on 2018-01-08

I have the same concern as mjm. If write and read approach MAX\_UINT and then write wraps but read does not, size() is miscalculated.

By Juho on 2018-01-10

Andrew; no, it will be calculated correctly. That's the whole point here. If you don't believe me, just try it with some example numbers! :)

By Ben on 2018-09-29

I created two threads using pthread, one for write() and one for read(), with a sleep of 6 seconds. However, at some point in time either write assert with full, and read assert with empty.

How to simulate this in pthread ?

By sizer on 2018-11-28

How does size work?

Example with a capacity of 2: 1. Push a value. Write is 1, read is 0. Size == 1. 2. Shift value off. Write is 1, read is 1, Size == 0. 3. Push 4 values. Write is 5, read is 1. Size == 4?? Even mask(write-read) == 0. Size should be 2.

By Juho on 2018-11-29

sizer,

The buffer would be full after you'd pushed two of the four values, and the remaining two pushes would fail.

By random on 2019-03-16

Isn't the whole purpose of having a ring buffer to dump the entire buffer for debugging purpose? I still don't get why we need a read idx and write idx? The idx could be fetched atomically like \_\_sync\_fetch\_and\_add (&cachedidx, 1)

Latest states of the important data structures could be stored in the ring buffer by pushing it into the next idx. If the buffer is full, the very first entry (oldest) will get overwritten.

t is the record getting stored and MAX is power of 2 size. idx could be fetched atomically.

rb.idx = (rb.idx+1)%MAX; rb.f[rb.idx] = t;

Am I am missing something? Of course it works with the buffer size of power of 2.

By Juho on 2019-03-16

Random,

By far the most common use case for ring buffers is to have two concurrent actors that communicate through the buffer. One reads data, the other writes. You need separate read / write pointers for that.

By Random on 2019-03-17

Thanks Juho. I figured it out immediately after posting the comment. In my project, all the ring buffer is doing is dumping/holding the important data at the time of crash. Different driver codes are just saving the states of important data structure and just writing into the ring buffer. I don't see any driver reading the data but I could easily foresee the use case you mentioned. It could very well be used as a producer consumer scenario. But then again, if this is the whole purpose of sharing data, why not just use a simple queue? May be, the use case a) doesn't want to wait for read/write, b) use the space more efficiently by adjusting the read/write indices instead of a single front and rear index. The indices access needs to be atomic though.

By spectras on 2019-03-27

This approach has a major drawback however, which I didn't see mentioned:

It uses and index.

As long as you're storing integers it's fine, but generalizing the approach will incur a geavy performance hit because array indexing has to multiply the index by sizeof(item).

As soon as that size is not a power of 2, this becomes very costly.

Perhaps it is why you don't see that method used too much: it's amazing for power-of-2-sized-items, it's awful for all other items.

By Andrew on 2019-06-25

Use of the post-increment operator, ++, in the example is neat but it creates a subtle concurrency bug.

array[mask(write++)] = val

this is not atomic and expands to the equivalent:

index = mask(write); ++write; array[index] = val; // error this address is not protected from read.

Needs to be { array[mask(write)] = val; ++write; } and { val = array[mask(read)]; ++read; return val } instead.

By foli on 2019-10-16

Hi Juho,

I'm not clear on "The maximum capacity can only be half the range of the index data types. (So 2^31-1 when using 32 bit unsigned integers).". Shouldn't that be "The maximum index ..."? As stated, the capacity has to be a) power of two and b) <= 2^31 - 1, which means the maximum capacity is only 2^30 and two bits are stolen instead of one.

What is the problem with indexes between 0 and 2^31 inclusive and the maximum capacity of 2^31?

By foli on 2019-10-16

Meant to say "... with indexes between 0 and 2^31-1 inclusive ...", of course.

By Ola Liljedahl on 2020-02-17

With 32-bit indexes a ring buffer can handle up to 2^32-1 elements. assert(capacity > 0 && capacity < (1ULL << 32)); u32 ringsize = roundup\_pow2(capacity); rb->head = rb->tail = 0; rb->mask = ringsize - 1; rb->capacity = capacity; available to enqueue = rb->capacity - (rb->tail - rb->head); available to dequeue = rb->tail - rb->head; enqueue: rb->ring[rb->tail++ & rb->mask] = elem; dequeue: elem = rb->ring[rb->head++ & rb->mask];

Blocking and non-blocking ring buffers here: <https://github.com/ARM-software/progress64/blob/master/src/p64_ringbuf.c>

By Wizard Ike on 2020-03-26

The ring buffer can have a capacity of 2^31 as this will use indices from 0 to 2^31-1 leaving one bit free to distinguish between empty and full.

By Tarun Elankath on 2020-05-09

Lovely, useful and succinct article. Thanks for writing this. I am also really glad for the comment showing the example of why mod won't work with unsigned integer overflow.

By John Colman on 2021-11-27

Agree with the other guys on using modulus for translation, although the compiler will likely translate this into a bitmask for any power of two anyway as this is always a quicker operation.

By TheDiveO on 2023-01-24

Just for the record: this design is prominently used in the Linux kernel in the sockets for the so-called eXpress Data Path XDP. See also here: <https://www.kernel.org/doc/html/v6.0/networking/af_xdp.html#rings>

As for others raising the issue of cache contention, the libxdp implementation of the rings features some probably helpful comments for more insight into how to tune this algorithm.

By Quantix on 2023-05-19

See also Bip Buffer (2003): <https://www.codeproject.com/Articles/3479/The-Bip-Buffer-The-Circular-Buffer-with-a-Twist>

By mcejp on 2023-12-06

Since I keep coming back to this every so often, I made a Gist out of Aristotle Pagaltzis' code including the suggested corrections:

<https://gist.github.com/mcejp/719d3485b04cfcf82e8a8734957da06a>

I also included operations for block read/write.

By Etienne Lorrain on 2024-07-11

If you need an array of size a power of 2, one way which makes it impossible to be wrong is to store the variable "log2size" instead of "arraysize". Even after 10 years, nobody will try to define an array of 17... Declare the array as "array[2 << log2size];" Then you can replace the "mod" by "shifts" too.

By Etienne Lorrain on 2024-07-11

Correction, sorry: Declare the array as "array[1 << log2size];" Please change original comment...

Post comment

|  |  |
| --- | --- |
| Name |  |
| Message |  |
|  | As an antispam measure, you need to write a super-secret password below. Today's password is "xyzzy" (without the quotes). |
| Password |  |
|  |  |

**Formatting guidelines for comments:** No tags, two newlines can be used for separating paragraphs,
http://... is automatically turned into a link.

Links

[2020-07-13](/lastweek/page/36)

[2020-06-22](/lastweek/page/35)

[2020-06-15](/lastweek/page/34)

Recent posts

[LLMs are cheap](https://www.snellman.net/blog/archive/2025-06-02-llms-are-cheap/)

2025-06-02

[Web Environment Integrity vs. Private Access Tokens - They're the same thing!](https://www.snellman.net/blog/archive/2023-07-25-web-integrity-api-vs-private-access-tokens/)

2023-07-25

[A monorepo misconception - atomic cross-project commits](https://www.snellman.net/blog/archive/2021-07-21-monorepo-atomic/)

2021-07-21

[Writing a procedural puzzle generator](https://www.snellman.net/blog/archive/2019-05-14-procedural-puzzle-generator/)

2019-05-14

[Optimizing a breadth-first search](https://www.snellman.net/blog/archive/2018-07-23-optimizing-breadth-first-search/)

2018-07-23

[Numbers and tagged pointers in early Lisp implementations](https://www.snellman.net/blog/archive/2017-09-04-lisp-numbers/)

2017-09-04

[Why PS4 downloads are so slow](https://www.snellman.net/blog/archive/2017-08-19-slow-ps4-downloads/)

2017-08-19

[The mystery of the hanging S3 downloads](https://www.snellman.net/blog/archive/2017-07-20-s3-mystery/)

2017-07-20

[I don't want no 'wantarray'](https://www.snellman.net/blog/archive/2017-07-18-wantarray/)

2017-07-18

[The origins of XXX as FIXME](https://www.snellman.net/blog/archive/2017-04-17-xxx-fixme/)

2017-04-17

Recent comments

[LLMs are cheap](/blog/archive/2025-06-02-llms-are-cheap/#comments)

2025-09-17

[The hidden cost of QUIC and TOU](/blog/archive/2016-12-01-quic-tou/#comments)

2025-07-13

[The most obsolete infrastructure money could buy - my worst job ever](/blog/archive/2015-09-01-the-most-obsolete-infrastructure-money-could-buy/#comments)

2025-05-15

---
source: hackernews
title: Zmij: Faster floating point double-to-string conversion
url: https://vitaut.net/posts/2025/faster-dtoa/
date: 2025-12-18
---

[Posts](/posts/)[Talks](/talks/)[Papers](/papers/)[Projects](/projects/)

- CATALOG -

+ [Performance](#performance)
+ [What are the improvements?](#what-are-the-improvements)
+ [Is this a new algorithm?](#is-this-a-new-algorithm)
+ [Where this can be used](#where-this-can-be-used)
+ [Current limitations](#current-limitations)
+ [“Fun” fact](#fun-fact)

*menu*
[vitaut.net](https://vitaut.net/)
*dark\_mode*

[vitaut.net](https://vitaut.net/)

Faster double-to-string conversion

2025-12-13

![](/img/dragons.jpg#floatright "Guess which dragon represents which algorithm")

There comes a time in every software engineer’s life when they come up with a
new binary-to-decimal floating-point conversion method. I guess my time has
come. I just wrote one, mostly over a weekend: <https://github.com/vitaut/zmij>.

It incorporates lessons learned from implementing
[Dragon4](https://fmt.dev/papers/p372-steele.pdf),
[Grisu](https://dl.acm.org/doi/10.1145/1809028.1806623) and
[Schubfach](https://fmt.dev/papers/Schubfach4.pdf) along with a few new ideas
from myself and others. The main guiding principle is Alexandrescu’s
“no work is less work than some work” so a number of improvements come from
removing things from Schubfach (conditional branches, computations and even
candidate numbers).

## Performance

Here is how it performs on [dtoa-benchmark](https://github.com/fmtlib/dtoa-benchmark):

* ~68% faster than [Dragonbox](https://github.com/jk-jeon/dragonbox), the
  previous leader (at least among algorithms with correctness proofs)
* ~2 times faster than [my Schubfach implementation](https://github.com/vitaut/schubfach) that closely follows the original paper
* ~3.5 times faster than [`std::to_chars`](https://en.cppreference.com/w/cpp/utility/to_chars.html) from libc++ (Ryu?)
* ~6.8 times faster than Google’s [double-conversion](https://github.com/google/double-conversion) (Grisu3)
* ~59 times (not percent!) faster than `sprintf` on macOS (Dragon4?)

Converting a single double takes about 10 to 20 ns on Apple M1.

## What are the improvements?

Here is a list of improvements compared to Schubfach:

* Selection from 1-3 candidates instead of 2-4
* Fewer integer multiplications in the shorter case
* Faster logarithm approximations
* Faster division and modulo
* Fewer conditional branches
* More efficient significand and exponent output

Let’s take a look at some of them.

The first small improvement is having a single branch to quickly check for
special cases: NaN, infinity, zero or subnormals. There are still additional
checks within that path but the common case is more streamlined.

Another improvement is using faster fixed-point logarithm approximations.
Schubfach does the following:

```
// log10_2_sig = round(log10(2) * 2**log10_2_exp)
constexpr int64_t log10_2_sig = 661'971'961'083;
constexpr int log10_2_exp = 41;

// Computes floor(log10(pow(2, e))) for e <= 5456721.
auto floor_log10_pow2(int e) noexcept -> int {
  return e * log10_2_sig >> log10_2_exp;
}
```

which uses 64-bit multiplication:

```
floor_log10_pow2(int):
        movsxd  rcx, edi
        movabs  rax, 661971961083
        imul    rax, rcx
        sar     rax, 41
        ret
```

However, for the range of inputs (exponents) we could use 32-bit approximations:

```
// log10_2_sig = round(log10(2) * 2**log10_2_exp)
constexpr int log10_2_sig = 315'653;
constexpr int log10_2_exp = 20;

auto floor_log10_pow2(int e) noexcept -> int {
  return e * log10_2_sig >> log10_2_exp;
}
```

resulting in

```
floor_log10_pow2(int):
        imul    eax, edi, 315653
        sar     eax, 20
        ret
```

Dragonbox also uses 32-bit approximations with slightly different constants.

Similarly, we can replace some integer divisions with integer multiplications.
Compilers already know how to do this, but we can do better when we know that
the range of inputs is small:

```
// Returns {value / 100, value % 100} correct for values of up to 4 digits.
inline auto divmod100(uint32_t value) noexcept -> divmod_result {
  assert(value < 10'000);
  constexpr int exp = 19;  // 19 is faster or equal to 12 even for 3 digits.
  constexpr int sig = (1 << exp) / 100 + 1;
  uint32_t div = (value * sig) >> exp;  // value / 100
  return {div, value - div * 100};
}
```

Another optimization and simplification is branchless handling of irregular
rounding intervals. I wrote about rounding intervals in my [earlier blog
post](/posts/2025/smallest-dtoa/), but for the purposes of this post
it is sufficient to know that a rounding interval for a floating-point number
is an interval that contains all real numbers that round back to that number.
Normally the intervals are symmetric, except when there is a jump in the
exponent (the irregular case):

Most algorithms handle irregular intervals via a completely separate path or at
least some branching. This is not terrible, because irregular cases are rare for
random floating-point numbers. However, it is possible to handle it cheaply and
branchlessly, avoiding extra complexity, which is what I did.

A more interesting improvement comes from a talk by Cassio Neri
[Fast Conversion From Floating Point Numbers](https://www.youtube.com/watch?v=w0WrRdW7eqg). In Schubfach, we look at four
candidate numbers. The first two, of which at most one is in the rounding
interval, correspond to a larger decimal exponent. The other two, of which at
least one is in the rounding interval, correspond to the smaller exponent.
Cassio’s insight is that we can directly construct a single candidate
from the upper bound in the first case.

This improvement has a nice effect: it allows us to avoid scaling the value
itself by a power of 10, because we only need the lower and upper bounds.
This saves two 64-bit integer multiplications in the shorter case.

Unfortunately, this does not help in the longer case, but there are improvements
to be made there as well. Classic Schubfach first checks whether there is only
one candidate from the second set in the rounding interval and returns early in
that case. We can combine this check with the closedness check. This seems
counterintuitive, because we do more work (sorry, Andrei), but it eliminates a
poorly predicted conditional branch and also simplifies the code.

So we go from this:

```
uint64_t dec_sig_over = dec_sig_under + 1;
bool under_in = lower + bin_sig_lsb <= (dec_sig_under << 2);
bool over_in = (dec_sig_over << 2) + bin_sig_lsb <= upper;
if (under_in != over_in) {
  // Only one of dec_sig_under or dec_sig_over are in the rounding interval.
  return write(buffer, under_in ? dec_sig_under : dec_sig_over, dec_exp);
}

// Both dec_sig_under and dec_sig_over are in the interval - pick the closest.
int cmp = scaled_sig - ((dec_sig_under + dec_sig_over) << 1);
bool under_closer = cmp < 0 || cmp == 0 && (dec_sig_under & 1) == 0;
return write(buffer, under_closer ? dec_sig_under : dec_sig_over, dec_exp);
```

to this:

```
// Pick the closest of dec_sig_under and dec_sig_over and check if it's in
// the rounding interval.
int64_t cmp = int64_t(scaled_sig - ((dec_sig_under + dec_sig_over) << 1));
bool under_closer = cmp < 0 || (cmp == 0 && (dec_sig_under & 1) == 0);
bool under_in = (dec_sig_under << 2) >= lower;
write(buffer, (under_closer & under_in) ? dec_sig_under : dec_sig_over,
      dec_exp);
```

There are also many improvements in significand and exponent output. The
simplest one, which has been used for many years in [{fmt}](https://github.com/fmtlib/fmt) and which I learned from Alexandrescu’s talk
“Three Optimization Tips for C++”, is using a lookup table to output pairs of
decimal digits. This alone halves the number of integer multiplications and is
particularly important here, because the significand is often 16–17 digits long.

Another trick is branchless removal of trailing zeros using another small
lookup table, which I believe comes from the excellent [Drachennest](https://github.com/abolz/Drachennest) project by Alexander Bolz. There are
ideas for improving this further and potentially getting rid of the lookup
table entirely.

## Is this a new algorithm?

Does it deserve to be called a new algorithm, or is it just an optimization of
Schubfach?

I am not sure, but at the very least it deserves a [separate GitHub project](https://github.com/vitaut/zmij) =).

## Where this can be used

This method, or at least some elements of it, will be used in {fmt},
and it is also a good candidate for JSON serialization in [Thrift](https://github.com/facebook/fbthrift) and elsewhere. If you have other
applications that could benefit from faster floating-point formatting,
feel free to check it out now, or wait until it is integrated into {fmt}.

Thanks to my ISO C++ paper [P2587](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p2587r3.html)
“to\_string or not to\_string”, `std::to_string` will also be able to use this
or a similar method. This will make this standard API both performant and
actually useful.

## Current limitations

Despite the name, the implementation is not fully [polished](https://pl.wikipedia.org/wiki/%C5%BBmij) yet. In particular, it currently
supports only exponential, also known as scientific, format, although adding
fixed format should be straightforward.

## “Fun” fact

My former colleague David Gay wrote an early `dtoa` implementation back at
Bell Labs, and it was widely used for many years.

Type,Function,Digit,Time(ns)
randomdigit,doubleconv,1,58.900000
randomdigit,doubleconv,2,72.250000
randomdigit,doubleconv,3,75.770000
randomdigit,doubleconv,4,76.840000
randomdigit,doubleconv,5,79.360000
randomdigit,doubleconv,6,84.940000
randomdigit,doubleconv,7,88.640000
randomdigit,doubleconv,8,88.620000
randomdigit,doubleconv,9,89.120000
randomdigit,doubleconv,10,84.300000
randomdigit,doubleconv,11,83.890000
randomdigit,doubleconv,12,84.170000
randomdigit,doubleconv,13,86.290000
randomdigit,doubleconv,14,88.660000
randomdigit,doubleconv,15,91.590000
randomdigit,doubleconv,16,95.600000
randomdigit,doubleconv,17,100.830000
randomdigit,dragonbox,1,18.520000
randomdigit,dragonbox,2,19.840000
randomdigit,dragonbox,3,20.190000
randomdigit,dragonbox,4,21.400000
randomdigit,dragonbox,5,20.450000
randomdigit,dragonbox,6,21.060000
randomdigit,dragonbox,7,20.090000
randomdigit,dragonbox,8,20.830000
randomdigit,dragonbox,9,20.120000
randomdigit,dragonbox,10,20.730000
randomdigit,dragonbox,11,20.300000
randomdigit,dragonbox,12,21.130000
randomdigit,dragonbox,13,20.470000
randomdigit,dragonbox,14,21.470000
randomdigit,dragonbox,15,20.770000
randomdigit,dragonbox,16,21.640000
randomdigit,dragonbox,17,24.210000
randomdigit,ryu,1,46.870000
randomdigit,ryu,2,47.230000
randomdigit,ryu,3,45.530000
randomdigit,ryu,4,44.860000
randomdigit,ryu,5,44.160000
randomdigit,ryu,6,43.330000
randomdigit,ryu,7,38.400000
randomdigit,ryu,8,37.480000
randomdigit,ryu,9,35.840000
randomdigit,ryu,10,36.810000
randomdigit,ryu,11,33.700000
randomdigit,ryu,12,32.760000
randomdigit,ryu,13,31.290000
randomdigit,ryu,14,30.270000
randomdigit,ryu,15,29.370000
randomdigit,ryu,16,29.360000
randomdigit,ryu,17,28.340000
randomdigit,fmt,1,18.850000
randomdigit,fmt,2,22.250000
randomdigit,fmt,3,20.660000
randomdigit,fmt,4,21.340000
randomdigit,fmt,5,19.700000
randomdigit,fmt,6,21.130000
randomdigit,fmt,7,19.650000
randomdigit,fmt,8,21.800000
randomdigit,fmt,9,23.030000
randomdigit,fmt,10,24.950000
randomdigit,fmt,11,22.880000
randomdigit,fmt,12,24.480000
randomdigit,fmt,13,22.150000
randomdigit,fmt,14,23.700000
randomdigit,fmt,15,21.730000
randomdigit,fmt,16,23.290000
randomdigit,fmt,17,27.590000
randomdigit,null,1,0.940000
randomdigit,null,2,0.930000
randomdigit,null,3,0.930000
randomdigit,null,4,0.930000
randomdigit,null,5,0.920000
randomdigit,null,6,0.930000
randomdigit,null,7,0.920000
randomdigit,null,8,0.930000
randomdigit,null,9,0.930000
randomdigit,null,10,0.930000
randomdigit,null,11,0.930000
randomdigit,null,12,0.930000
randomdigit,null,13,0.920000
randomdigit,null,14,0.930000
randomdigit,null,15,0.930000
randomdigit,null,16,0.930000
randomdigit,null,17,0.930000
randomdigit,ostringstream,1,787.710000
randomdigit,ostringstream,2,802.180000
randomdigit,ostringstream,3,817.250000
randomdigit,ostringstream,4,827.280000
randomdigit,ostringstream,5,837.930000
randomdigit,ostringstream,6,852.870000
randomdigit,ostringstream,7,868.720000
randomdigit,ostringstream,8,878.130000
randomdigit,ostringstream,9,878.560000
randomdigit,ostringstream,10,881.930000
randomdigit,ostringstream,11,890.890000
randomdigit,ostringstream,12,899.630000
randomdigit,ostringstream,13,911.180000
randomdigit,ostringstream,14,919.890000
randomdigit,ostringstream,15,924.540000
randomdigit,ostringstream,16,933.480000
randomdigit,ostringstream,17,928.500000
randomdigit,schubfach,1,20.490000
randomdigit,schubfach,2,24.060000
randomdigit,schubfach,3,23.550000
randomdigit,schubfach,4,23.580000
randomdigit,schubfach,5,23.170000
randomdigit,schubfach,6,22.990000
randomdigit,schubfach,7,22.640000
randomdigit,schubfach,8,22.530000
randomdigit,schubfach,9,22.430000
randomdigit,schubfach,10,24.610000
randomdigit,schubfach,11,26.290000
randomdigit,schubfach,12,26.090000
randomdigit,schubfach,13,25.860000
randomdigit,schubfach,14,25.570000
randomdigit,schubfach,15,25.500000
randomdigit,schubfach,16,27.290000
randomdigit,schubfach,17,33.430000
randomdigit,sprintf,1,646.780000
randomdigit,sprintf,2,666.000000
randomdigit,sprintf,3,681.240000
randomdigit,sprintf,4,690.980000
randomdigit,sprintf,5,701.070000
randomdigit,sprintf,6,713.790000
randomdigit,sprintf,7,722.250000
randomdigit,sprintf,8,732.100000
randomdigit,sprintf,9,736.390000
randomdigit,sprintf,10,753.240000
randomdigit,sprintf,11,752.060000
randomdigit,sprintf,12,761.710000
randomdigit,sprintf,13,774.910000
randomdigit,sprintf,14,784.310000
randomdigit,sprintf,15,786.900000
randomdigit,sprintf,16,790.640000
randomdigit,sprintf,17,780.570000
randomdigit,to\_chars,1,52.270000
randomdigit,to\_chars,2,53.260000
randomdigit,to\_chars,3,50.200000
randomdigit,to\_chars,4,48.790000
randomdigit,to\_chars,5,47.100000
randomdigit,to\_chars,6,45.920000
randomdigit,to\_chars,7,44.720000
randomdigit,to\_chars,8,43.230000
randomdigit,to\_chars,9,42.230000
randomdigit,to\_chars,10,42.990000
randomdigit,to\_chars,11,39.940000
randomdigit,to\_chars,12,38.360000
randomdigit,to\_chars,13,37.260000
randomdigit,to\_chars,14,36.100000
randomdigit,to\_chars,15,34.940000
randomdigit,to\_chars,16,34.090000
randomdigit,to\_chars,17,34.880000
randomdigit,zmij,1,10.110000
randomdigit,zmij,2,10.230000
randomdigit,zmij,3,10.380000
randomdigit,zmij,4,10.080000
randomdigit,zmij,5,12.680000
randomdigit,zmij,6,11.870000
randomdigit,zmij,7,10.730000
randomdigit,zmij,8,10.620000
randomdigit,zmij,9,13.040000
randomdigit,zmij,10,12.490000
randomdigit,zmij,11,11.550000
randomdigit,zmij,12,11.420000
randomdigit,zmij,13,14.070000
randomdigit,zmij,14,14.250000
randomdigit,zmij,15,13.080000
randomdigit,zmij,16,14.940000
randomdigit,zmij,17,18.920000

---

Last modified on 2025-12-13

Next
No newer posts.[Previous
to\_chars or not to\_chars?](/posts/2025/to-chars/)

Please enable JavaScript to view the [comments powered by Disqus.](https://disqus.com/?ref_noscript)

[vitaut.net](https://vitaut.net/)

[Posts](/posts/)[Talks](/talks/)[Papers](/papers/)[Projects](/projects/)

Hugo Theme [Diary](https://github.com/amazingrise/hugo-theme-diary) by [Rise](https://risehere.net/)
Ported from [Makito](https://mak1t0.cc/)'s [Journal.](https://github.com/SumiMakito/hexo-theme-journal/)

©
2025 vitaut.net

- CATALOG -

+ [Performance](#performance)
+ [What are the improvements?](#what-are-the-improvements)
+ [Is this a new algorithm?](#is-this-a-new-algorithm)
+ [Where this can be used](#where-this-can-be-used)
+ [Current limitations](#current-limitations)
+ [“Fun” fact](#fun-fact)

[*keyboard\_arrow\_up*](#top)dark\_mode

Hugo Theme [Diary](https://github.com/amazingrise/hugo-theme-diary) by [Rise](https://risehere.net/)
Ported from [Makito](https://mak1t0.cc/)'s [Journal.](https://github.com/SumiMakito/hexo-theme-journal/)

©
2025 vitaut.net

[Support Ukraine](https://www.stopputin.net/)

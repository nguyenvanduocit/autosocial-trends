---
source: hackernews
title: Illuminating the processor core with LLVM-mca
url: https://abseil.io/fast/99
date: 2025-12-15
---

[![Abseil](/img/absl_80px.png)](/)

Toggle navigation

* [About](/about)
* [C++ Guide](/docs/cpp)
* [C++ Tips](/tips/)
* [Fast Tips](/fast/)
* [Python Guide](/docs/python)
* [Blog](/blog/)
* [Community](/community/)
* [SWE Book](/resources/swe-book)

- [Home](/)

---

- [About](/about)

---

- [Docs](/docs/)
  [* C++ Devguide](/docs/cpp)
  [* C++ Quick Start](/docs/cpp/quickstart)

---

- [Tips](/tips/)

---

- [Blog](/blog/)

---

- [Community](/community/)

[![Abseil](/img/absl_80px.png)](/)

Toggle navigation

* [About](/about/)
* [C++ Guide](/docs/cpp)
* [C++ Tips](/tips/)
* [Fast Tips](/fast/)
* [Python Guide](/docs/python)
* [Blog](/blog/)
* [Community](/community/)
* [SWE Book](/resources/swe-book)

- [Home](/)

---

- [About](/about/)

---

- [C++ Guide](/docs/cpp)
  [* Quickstart](/docs/cpp/quickstart)

---

- [Blog](/blog/)

---

- [Tips](/tips/)

---

- [Community](/community/)

#

* ##### [Performance Guide](/fast/)
* ###### Fast Tips
* ###### [Fast TotW #7: Optimizing for application productivity](/fast/7)
* ###### [Fast TotW #9: Optimizations past their prime](/fast/9)
* ###### [Fast TotW #21: Improving the efficiency of your regular expressions](/fast/21)
* ###### [Fast TotW #26: Fixing things with hashtable profiling](/fast/26)
* ###### [Fast TotW #39: Beware microbenchmarks bearing gifts](/fast/39)
* ###### [Fast TotW #52: Configuration knobs considered harmful](/fast/52)
* ###### [Fast TotW #53: Precise C++ benchmark measurements with Hardware Performance Counters](/fast/53)
* ###### [Fast TotW #60: In-process profiling: lessons learned](/fast/60)
* ###### [Fast TotW #62: Identifying and reducing memory bandwidth needs](/fast/62)
* ###### [Fast TotW #64: More Moore with better API design](/fast/64)
* ###### [Fast TotW #70: Defining and measuring optimization success](/fast/70)
* ###### [Fast TotW #72: Optimizing optimization](/fast/72)
* ###### [Fast TotW #74: Avoid sweeping street lights under rugs](/fast/74)
* ###### [Fast TotW #75: How to microbenchmark](/fast/75)
* ###### [Fast TotW #79: Make at most one tradeoff at a time](/fast/79)
* ###### [Fast TotW #83: Reducing memory indirections](/fast/83)
* ###### [Fast TotW #87: Two-way doors](/fast/87)
* ###### [Fast TotW #88: Measurement methodology: Avoid the jelly beans trap](/fast/88)
* ###### [Fast TotW #90: How to estimate](/fast/90)
* ###### [Fast TotW #93: Robots never sleep](/fast/93)
* ###### [Fast TotW #94: Decision making in a data-imperfect world](/fast/94)
* ###### [Fast TotW #95: Spooky action at a distance](/fast/95)
* ###### [Fast TotW #97: Virtuous ecosystem cycles](/fast/97)
* ###### [Fast TotW #98: Measurement has an ROI](/fast/98)
* ###### [Fast TotW #99: Illuminating the processor core with llvm-mca](/fast/99)

# Performance Tip of the Week #99: Illuminating the processor core with llvm-mca

Originally posted as Fast TotW #99 on September 29, 2025

*By [Chris Kennelly](/cdn-cgi/l/email-protection#e6858d838888838a8a9fa6818989818a83c885898b)*

Updated 2025-10-07

Quicklink: [abseil.io/fast/99](https://abseil.io/fast/99)

The [RISC](https://en.wikipedia.org/wiki/Reduced_instruction_set_computer)
versus [CISC](https://en.wikipedia.org/wiki/Complex_instruction_set_computer)
debate ended in a draw: Modern processors decompose instructions into
[micro-ops](https://en.wikipedia.org/wiki/Micro-operation) handled by backend
execution units. Understanding how instructions are executed by these units can
give us insights on optimizing key functions that are backend bound. In this
episode, we walk through using
[`llvm-mca`](https://llvm.org/docs/CommandGuide/llvm-mca.html) to analyze
functions and identify performance insights from its simulation.

## Preliminaries: Varint optimization

`llvm-mca`, short for Machine Code Analyzer, is a tool within LLVM. It uses the
same datasets that the compiler uses for making instruction scheduling
decisions. This ensures that improvements made to compiler optimizations
automatically flow towards keeping `llvm-mca` representative. The flip side is
that the tool is only as good as LLVM’s internal modeling of processor designs,
so certain quirks of individual microarchitecture generations might be omitted.
It also models the processor [behavior statically](#limitations), so cache
misses, branch mispredictions, and other dynamic properties aren’t considered.

Consider Protobuf’s `VarintSize64` method:

```
size_t CodedOutputStream::VarintSize64(uint64_t value) {
#if PROTOBUF_CODED_STREAM_H_PREFER_BSR
  // Explicit OR 0x1 to avoid calling absl::countl_zero(0), which
  // requires a branch to check for on platforms without a clz instruction.
  uint32_t log2value = (std::numeric_limits<uint64_t>::digits - 1) -
                       absl::countl_zero(value | 0x1);
  return static_cast<size_t>((log2value * 9 + (64 + 9)) / 64);
#else
  uint32_t clz = absl::countl_zero(value);
  return static_cast<size_t>(
      ((std::numeric_limits<uint64_t>::digits * 9 + 64) - (clz * 9)) / 64);
#endif
}
```

This function calculates how many bytes an encoded integer will consume in
[Protobuf’s wire
format](https://protobuf.dev/programming-guides/encoding/#varints). It first
computes the number of bits needed to represent the value by finding the log2
size of the input, then approximates division by 7. The size of the input can be
calculated using the `absl::countl_zero` function. However this has two possible
implementations depending on whether the processor has a `lzcnt` (Leading Zero
Count) instruction available or if this operation needs to instead leverage the
`bsr` (Bit Scan Reverse) instruction.

Under the hood of `absl::countl_zero`, we need to check whether the argument is
zero, since `__builtin_clz` (Count Leading Zeros) models the behavior of x86’s
`bsr` (Bit Scan Reverse) instruction and has unspecified behavior if the input
is 0. The `| 0x1` avoids needing a branch by ensuring the argument is non-zero
in a way the compiler can follow.

When we have `lzcnt` available, the compiler optimizes `x == 0 ? 32 :
__builtin_clz(x)` in `absl::countl_zero` to `lzcnt` without branches. This makes
the `| 0x1` unnecessary.

Compiling this gives us two different assembly sequences depending on whether
the `lzcnt` instruction is available or not:

`bsr` (`-march=ivybridge`):

```
orq     $1, %rdi
bsrq    %rdi, %rax
leal    (%rax,%rax,8), %eax
addl    $73, %eax
shrl    $6, %eax
```

`lzcnt` (`-march=haswell`):

```
lzcntq  %rdi, %rax
leal    (%rax,%rax,8), %ecx
movl    $640, %eax
subl    %ecx, %eax
shrl    $6, %eax
```

### Analyzing the code

We can now use [Compiler Explorer](https://godbolt.org/z/39EMsWq7z) to run these
sequences through `llvm-mca` and get an analysis of how they would execute on a
simulated Skylake processor (`-mcpu=skylake`) for a single invocation
(`-iterations=1`) and include `-timeline`:

`bsr` (`-march=ivybridge`):

```
Iterations:        1
Instructions:      5
Total Cycles:      10
Total uOps:        5

Dispatch Width:    6
uOps Per Cycle:    0.50
IPC:               0.50
Block RThroughput: 1.0

Timeline view:
Index     0123456789

[0,0]     DeER .   .   orq      $1, %rdi
[0,1]     D=eeeER  .   bsrq     %rdi, %rax
[0,2]     D====eER .   leal     (%rax,%rax,8), %eax
[0,3]     D=====eER.   addl     $73, %eax
[0,4]     D======eER   shrl     $6, %eax
```

`lzcnt` (`-march=haswell`):

```
Iterations:        1
Instructions:      5
Total Cycles:      9
Total uOps:        5

Dispatch Width:    6
uOps Per Cycle:    0.56
IPC:               0.56
Block RThroughput: 1.0

Timeline view:
Index     012345678

[0,0]     DeeeER  .   lzcntq    %rdi, %rax
[0,1]     D===eER .   leal      (%rax,%rax,8), %ecx
[0,2]     DeE---R .   movl      $640, %eax
[0,3]     D====eER.   subl      %ecx, %eax
[0,4]     D=====eER   shrl      $6, %eax
```

This can also be obtained via the command line

```
$ clang file.cpp -O3 --target=x86_64 -S -o - | llvm-mca -mcpu=skylake -iterations=1 -timeline
```

There’s two sections to this output, the first section provides some summary
statistics for the code, the second section covers the execution “timeline.” The
timeline provides interesting detail about how instructions flow through the
execution pipelines in the processor. There are three columns, and each
instruction is shown on a separate row. The three columns are as follows:

* The first column is a pair of numbers, the first number is the iteration,
  the second number is the index of the instruction. In the above example
  there’s a single iteration, number 0, so just the index of the instruction
  changes on each row.
* The third column is the instruction.
* The second column is the timeline. Each character in that column represents
  a cycle, and the character indicates what’s happening to the instruction in
  that cycle.

The timeline is counted in cycles. Each instruction goes through several steps:

* `D` the instruction is dispatched by the processor; modern desktop or server
  processors can dispatch many instructions per cycle. Little Arm cores like
  the Cortex-A55 used in smartphones are more limited.
* `=` the instruction is waiting to execute. In this case, the instructions
  are waiting for the results of prior instructions to be available. In other
  cases, there might be a bottleneck in the processor’s backend.
* `e` the instruction is executing.
* `E` the instruction’s output is available.
* `-` the instruction has completed execution and is waiting to be retired.
  Instructions generally retire in program order, the order instructions
  appear in the program. An instruction will wait to retire until prior ones
  have also retired. On some architectures like the Cortex-A55, there is no
  `R` phase in the timeline as some instructions [retire
  out-of-order](https://chipsandcheese.com/i/149874023/out-of-order-retire).
* `R` the instruction has been retired, and is no longer occupying execution
  resources.

The output is lengthy, but we can extract a few high-level insights from it:

* The `lzcnt` implementation is quicker to execute (9 cycles) than the “bsr”
  implementation (10 cycles). This is seen under the `Total Cycles` summary as
  well as the timeline.
* The routine is simple: with the exception of `movl`, the instructions depend
  on each other sequentially (`E`-finishing to `e`-starting vertically
  aligning, pairwise, in the timeline view).
* Bitwise-`or` of `0x1` delays `bsrq`’s input being available by 1 cycle,
  contributing to the longer execution cost.
* Note that although `movl` starts immediately in the `lzcnt` implementation,
  it can’t retire until prior instructions are retired, since we retire in
  program order.
* Both sequences are 5 instructions long, but the `lzcnt` implementation has
  higher [instruction-level parallelism
  (ILP)](https://en.wikipedia.org/wiki/Instruction-level_parallelism) because
  the `mov` has no dependencies. This demonstrates that counting instructions
  need not tell us the [cycle cost](/fast/7).

`llvm-mca` is flexible and can model other processors:

* AMD Zen3 ([Compiler Explorer](https://godbolt.org/z/xbq9PqG8z)), where the
  cost difference is more stark (8 cycles versus 12 cycles).
* Arm Neoverse-V2 ([Compiler Explorer](https://godbolt.org/z/sE3T65n8M)), a
  server CPU where the difference is 7 vs 9 cycles.
* Arm Cortex-A55 ([Compiler Explorer](https://godbolt.org/z/vPP5EPrcW)), a
  popular little core used in smartphones, where the difference is 8 vs 10
  cycles. Note how the much smaller dispatch width results in the `D` phase of
  instructions starting later.

### Throughput versus latency

When designing [microbenchmarks](/fast/75), we sometimes want to distinguish
between throughput and latency microbenchmarks. If the input of one benchmark
iteration does not depend on the prior iteration, the processor can execute
multiple iterations in parallel. Generally for code that is expected to execute
in a loop we care more about throughput, and for code that is inlined in many
places interspersed with other logic we care more about latency.

We can use `llvm-mca` to model execution of the block of code in a tight loop.
By specifying `-iterations=100` on the `lzcnt` version, we get a very different
set of results because one iteration’s execution can overlap with the next:

```
Iterations:        100
Instructions:      500
Total Cycles:      134
Total uOps:        500

Dispatch Width:    6
uOps Per Cycle:    3.73
IPC:               3.73
Block RThroughput: 1.0
```

We were able to execute 100 iterations in only 134 cycles (1.34 cycles/element)
by achieving high ILP.

Achieving the best performance may sometimes entail trading off the latency of a
basic block in favor of higher throughput. Inside of the protobuf implementation
of `VarintSize`
([protobuf/wire\_format\_lite.cc](https://github.com/protocolbuffers/protobuf/tree/main/src/google/protobuf/wire_format_lite.cc)),
we use a vectorized version for realizing higher throughput albeit with worse
latency. A single iteration of the loop takes 29 cycles to process 32 elements
([Compiler Explorer](https://godbolt.org/z/TczKTaGd8)) for 0.91 cycles/element,
but 100 iterations (3200 elements) only requires 1217 cycles (0.38
cycles/element - about 3x faster) showcasing the high throughput once setup
costs are amortized.

## Understanding dependency chains

When we are looking at CPU profiles, we are often tracking when instructions
*retire*. Costs are attributed to instructions that took longer to retire.
Suppose we profile a small function that accesses memory pseudo-randomly:

```
unsigned Chains(unsigned* x) {
   unsigned a0 = x[0];
   unsigned b0 = x[1];

   unsigned a1 = x[a0];
   unsigned b1 = x[b0];

   unsigned b2 = x[b1];

   return a1 | b2;
}
```

`llvm-mca` models memory loads being an L1 hit ([Compiler
Explorer](https://godbolt.org/z/6PzTPYv8T)): It takes 5 cycles for the value of
a load to be available after the load starts execution. The output has been
annotated with the source code to make it easier to read.

```
Iterations:        1
Instructions:      6
Total Cycles:      19
Total uOps:        9

Dispatch Width:    6
uOps Per Cycle:    0.47
IPC:               0.32
Block RThroughput: 3.0

Timeline view:
                    012345678
Index     0123456789

[0,0]     DeeeeeER  .    .  .   movl    (%rdi), %ecx         // ecx = a0 = x[0]
[0,1]     DeeeeeER  .    .  .   movl    4(%rdi), %eax        // eax = b0 = x[1]
[0,2]     D=====eeeeeER  .  .   movl    (%rdi,%rax,4), %eax  // eax = b1 = x[b0]
[0,3]     D==========eeeeeER.   movl    (%rdi,%rax,4), %eax  // eax = b2 = x[b1]
[0,4]     D==========eeeeeeER   orl     (%rdi,%rcx,4), %eax  // eax |= a1 = x[a0]
[0,5]     .DeeeeeeeE--------R   retq
```

In this timeline the first two instructions load `a0` and `b0`. Both of these
operations can happen immediately. However, the load of `x[b0]` can only happen
once the value for `b0` is available in a register - after a 5 cycle delay. The
load of `x[b1]` can only happen once the value for `b1` is available after
another 5 cycle delay.

This program has two places where we can execute loads in parallel: the pair
`a0` and `b0` and the pair `a1 and b1` (note: `llvm-mca` does not correctly
model the memory load uop from `orl` for `a1` starting). Since the processor
retires instructions in program order we expect the profile weight to appear on
the loads for `a0`, `b1`, and `b2`, even though we had parallel loads in-flight
simultaneously.

If we examine this profile, we might try to optimize one of the memory
indirections because it appears in our profile. We might do this by miraculously
replacing `a0` with a constant ([Compiler
Explorer](https://godbolt.org/z/b68KzsKMP)).

```
unsigned Chains(unsigned* x) {
   unsigned a0 = 0;
   unsigned b0 = x[1];

   unsigned a1 = x[a0];
   unsigned b1 = x[b0];

   unsigned b2 = x[b1];

   return a1 | b2;
}
```

```
Iterations:        1
Instructions:      5
Total Cycles:      19
Total uOps:        8

Dispatch Width:    6
uOps Per Cycle:    0.42
IPC:               0.26
Block RThroughput: 2.5

Timeline view:
                    012345678
Index     0123456789

[0,0]     DeeeeeER  .    .  .   movl    4(%rdi), %eax
[0,1]     D=====eeeeeER  .  .   movl    (%rdi,%rax,4), %eax
[0,2]     D==========eeeeeER.   movl    (%rdi,%rax,4), %eax
[0,3]     D==========eeeeeeER   orl     (%rdi), %eax
[0,4]     .DeeeeeeeE--------R   retq
```

Even though we got rid of the “expensive” load we saw in the CPU profile, we
didn’t actually change the overall length of the critical path that was
dominated by the 3 load long “b” chain. The timeline view shows the critical
path for the function, and performance can only be improved if the duration of
the critical path is reduced.

## Optimizing CRC32C

CRC32C is a common hashing function and modern architectures include dedicated
instructions for calculating it. On short sizes, we’re largely dealing with
handling odd numbers of bytes. For large sizes, we are constrained by repeatedly
invoking `crc32q` (x86) or similar every few bytes of the input. By examining
the repeated invocation, we can look at how the processor will execute it
([Compiler Explorer](https://godbolt.org/z/nEYsWWzWs)):

```
uint32_t BlockHash() {
 asm volatile("# LLVM-MCA-BEGIN");
 uint32_t crc = 0;
 for (int i = 0; i < 16; ++i) {
   crc = _mm_crc32_u64(crc, i);
 }
 asm volatile("# LLVM-MCA-END" : "+r"(crc));
 return crc;
}
```

This function doesn’t hash anything useful, but it allows us to see the
back-to-back usage of one `crc32q`’s output with the next `crc32q`’s inputs.

```
Iterations:        1
Instructions:      32
Total Cycles:      51
Total uOps:        32

Dispatch Width:    6
uOps Per Cycle:    0.63
IPC:               0.63
Block RThroughput: 16.0

Instruction Info:

[1]: #uOps
[2]: Latency
[3]: RThroughput
[4]: MayLoad
[5]: MayStore
[6]: HasSideEffects (U)

[1]    [2]    [3]    [4]    [5]    [6]    Instructions:
1      0     0.17                        xorl   %eax, %eax
1      3     1.00                        crc32q %rax, %rax
1      1     0.25                        movl   $1, %ecx
1      3     1.00                        crc32q %rcx, %rax
...

Resources:
[0]   - SKLDivider
[1]   - SKLFPDivider
[2]   - SKLPort0
[3]   - SKLPort1
[4]   - SKLPort2
[5]   - SKLPort3
[6]   - SKLPort4
[7]   - SKLPort5
[8]   - SKLPort6
[9]   - SKLPort7

Resource pressure per iteration:
[0]    [1]    [2]    [3]    [4]    [5]    [6]    [7]    [8]    [9]
-      -     4.00   18.00   -     1.00    -     5.00   6.00    -

Resource pressure by instruction:
[0]    [1]    [2]    [3]    [4]    [5]    [6]    [7]    [8]    [9]    Instructions:
-      -      -      -      -      -      -      -      -      -     xorl       %eax, %eax
-      -      -     1.00    -      -      -      -      -      -     crc32q     %rax, %rax
-      -      -      -      -      -      -      -     1.00    -     movl       $1, %ecx
-      -      -     1.00    -      -      -      -      -      -     crc32q     %rcx, %rax
-      -      -      -      -      -      -     1.00    -      -     movl       $2, %ecx
-      -      -     1.00    -      -      -      -      -      -     crc32q     %rcx, %rax
...
-      -      -      -      -      -      -      -     1.00    -     movl       $15, %ecx
-      -      -     1.00    -      -      -      -      -      -     crc32q     %rcx, %rax
-      -      -      -      -     1.00    -     1.00   1.00    -     retq

Timeline view:
                    0123456789          0123456789          0
Index     0123456789          0123456789          0123456789

[0,0]     DR   .    .    .    .    .    .    .    .    .    .   xorl    %eax, %eax
[0,1]     DeeeER    .    .    .    .    .    .    .    .    .   crc32q  %rax, %rax
[0,2]     DeE--R    .    .    .    .    .    .    .    .    .   movl    $1, %ecx
[0,3]     D===eeeER .    .    .    .    .    .    .    .    .   crc32q  %rcx, %rax
[0,4]     DeE-----R .    .    .    .    .    .    .    .    .   movl    $2, %ecx
[0,5]     D======eeeER   .    .    .    .    .    .    .    .   crc32q  %rcx, %rax
...
[0,30]    .    DeE---------------------------------------R  .   movl    $15, %ecx
[0,31]    .    D========================================eeeER   crc32q  %rcx, %rax
```

Based on the “`Instruction Info`” table, `crc32q` has latency 3 and throughput
1: Every clock cycle, we can start processing a new invocation on port 1 (`[3]`
in the table), but it takes 3 cycles for the result to be available.

Instructions decompose into individual micro operations (or “uops”). The
resources section lists the processor execution pipelines (often referred to as
ports). Every cycle uops can be issued to these ports. There are constraints -
no port can take every kind of uop and there is a maximum number of uops that
can be dispatched to the processor pipelines every cycle.

For the instructions in our function, there is a one-to-one correspondence so
the number of instructions and the number of uops executed are equivalent (32).
The processor has several backends for processing uops. From the resource
pressure tables, we see that while `crc32` must execute on port 1, the `movl`
executes on any of ports 0, 1, 5, and 6.

In the timeline view, we see that for our back-to-back sequence, we can’t
actually begin processing the 2nd `crc32q` for several clock cycles until the
1st `crc32q` hasn’t completed. This tells us that we’re underutilizing port 1’s
capabilities, since its throughput indicates that an instruction can be
dispatched to it once per cycle.

If we restructure `BlockHash` to compute 3 parallel streams with a simulated
combine function (the code uses a bitwise or as a placeholder for the correct
logic that this approach requires), we can accomplish the same amount of work in
fewer clock cycles ([Compiler Explorer](https://godbolt.org/z/ha9KdYovh)):

```
uint32_t ParallelBlockHash(const char* p) {
 uint32_t crc0 = 0, crc1 = 0, crc2 = 0;
 for (int i = 0; i < 5; ++i) {
   crc0 = _mm_crc32_u64(crc0, 3 * i + 0);
   crc1 = _mm_crc32_u64(crc1, 3 * i + 1);
   crc2 = _mm_crc32_u64(crc2, 3 * i + 2);
 }
 crc0 = _mm_crc32_u64(crc0, 15);
 return crc0 | crc1 | crc2;
}
```

```
Iterations:        1
Instructions:      36
Total Cycles:      22
Total uOps:        36

Dispatch Width:    6
uOps Per Cycle:    1.64
IPC:               1.64
Block RThroughput: 16.0

Timeline view:
                    0123456789
Index     0123456789          01

[0,0]     DR   .    .    .    ..   xorl %eax, %eax
[0,1]     DR   .    .    .    ..   xorl %ecx, %ecx
[0,2]     DeeeER    .    .    ..   crc32q       %rcx, %rcx
[0,3]     DeE--R    .    .    ..   movl $1, %esi
[0,4]     D----R    .    .    ..   xorl %edx, %edx
[0,5]     D=eeeER   .    .    ..   crc32q       %rsi, %rdx
[0,6]     .DeE--R   .    .    ..   movl $2, %esi
[0,7]     .D=eeeER  .    .    ..   crc32q       %rsi, %rax
[0,8]     .DeE---R  .    .    ..   movl $3, %esi
[0,9]     .D==eeeER .    .    ..   crc32q       %rsi, %rcx
[0,10]    .DeE----R .    .    ..   movl $4, %esi
[0,11]    .D===eeeER.    .    ..   crc32q       %rsi, %rdx
...
[0,32]    .    DeE-----------R..   movl $15, %esi
[0,33]    .    D==========eeeER.   crc32q       %rsi, %rcx
[0,34]    .    D============eER.   orl  %edx, %eax
[0,35]    .    D=============eER   orl  %ecx, %eax
```

The implementation invokes `crc32q` the same number of times, but the end-to-end
latency of the block is 22 cycles instead of 51 cycles The timeline view shows
that the processor can issue a `crc32` instruction every cycle.

This modeling can be evidenced by [microbenchmark](/fast/75) results for
`absl::ComputeCrc32c`
([absl/crc/crc32c\_benchmark.cc](https://github.com/abseil/abseil-cpp/blob/master/absl/crc/crc32c_benchmark.cc)).
The real implementation uses multiple streams (and correctly combines them).
Ablating these shows a regression, validating the value of the technique.

```
name                          CYCLES/op    CYCLES/op    vs base
BM_Calculate/0                   5.007 ± 0%     5.008 ± 0%         ~ (p=0.149 n=6)
BM_Calculate/1                   6.669 ± 1%     8.012 ± 0%   +20.14% (p=0.002 n=6)
BM_Calculate/100                 30.82 ± 0%     30.05 ± 0%    -2.49% (p=0.002 n=6)
BM_Calculate/2048                285.6 ± 0%     644.8 ± 0%  +125.78% (p=0.002 n=6)
BM_Calculate/10000               906.7 ± 0%    3633.8 ± 0%  +300.78% (p=0.002 n=6)
BM_Calculate/500000             37.77k ± 0%   187.69k ± 0%  +396.97% (p=0.002 n=6)
```

If we create a 4th stream for `ParallelBlockHash` ([Compiler
Explorer](https://godbolt.org/z/eo36ejca7)), `llvm-mca` shows that the overall
latency is unchanged since we are bottlenecked on port 1’s throughput. Unrolling
further adds additional overhead to combine the streams and makes prefetching
harder without actually improving performance.

To improve performance, many fast CRC32C implementations use other processor
features. Instructions like the carryless multiply instruction (`pclmulqdq` on
x86) can be used to implement another parallel stream. This allows additional
ILP to be extracted by using the other ports of the processor without worsening
the bottleneck on the port used by `crc32`.

## Limitations

While `llvm-mca` can be a useful tool in many situations, its modeling has
limits:

* Memory accesses are modeled as L1 hits. In the real world, we can have much
  longer stalls when we need to access the L2, L3, or even [main
  memory](/fast/62).
* It cannot model branch predictor behavior.
* It does not model instruction fetch and decode steps.
* Its analysis is only as good as LLVM’s processor models. If these do not
  accurately model the processor, the simulation might differ from the real
  processor.

  For example, many ARM processor models are incomplete, and `llvm-mca` picks
  a processor model that it estimates to be a good substitute; this is
  generally fine for compiler heuristics, where differences only matter if it
  would result in different generated code, but it can derail manual
  optimization efforts.

## Closing words

Understanding how the processor executes and retires instructions can give us
powerful insights for optimizing functions. `llvm-mca` lets us peer into the
processor to let us understand bottlenecks and underutilized resources.

## Further reading

* [uops.info](https://uops.info)
* Chandler Carruth’s “[Going Nowhere
  Faster](https://www.youtube.com/watch?v=2EWejmkKlxs)” talk
* Agner Fog’s “[Software Optimization
  Resources](https://www.agner.org/optimize/)”

[![Subscribe to the Abseil Blog](//gstatic.com/ac/dashboard/feedburner-32.png)](http://feeds.feedburner.com/abseilio "Subscribe to Abseil Blog & Tips")

![](/img/typography_white.png)

[About Abseil](/about/)* [Introduction](/about/intro)
* [Philosophy](/about/philosophy)
* [Compatibility](/about/compatibility)
* [Design Notes](/about/design/)

---

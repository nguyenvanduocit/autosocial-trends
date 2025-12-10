---
source: hackernews
title: My favourite small hash table
url: https://www.corsix.org/content/my-favourite-small-hash-table
date: 2025-12-10
---

[<](et-soc-riscv-instructions "Older: Which RISC-V instructions does the ET-SoC-1 give us?, posted November 12, 2025")

# My favourite small hash table

Posted at [corsix.org](/) on December 9, 2025

I'm the kind of person who thinks about the design and implementation of hash tables. One design which I find particularly cute, and I think deserves a bit more publicity, is Robin Hood open-addressing with linear probing and power-of-two table size. If you're not familiar with hash table terminology, that might look like a smorgasbord of random words, but it should become clearer as we look at some actual code.

To keep the code simple to start with, I'm going to assume:

1. Keys are randomly-distributed 32-bit integers.
2. Values are also 32-bit integers.
3. If the key `0` is present, its value is not `0`.
4. The table occupies at most 32 GiB of memory.

Each slot in the table is either empty, or holds a key and a value. The combination of properties (1) and (2) allows a key/value pair to be stored as a 64-bit integer, and property (3) means that the 64-bit value `0` can be used to represent an empty slot (some hash table designs also need a special value for representing tombstones, but this design doesn't need tombstones). Combining a key and a value into 64 bits couldn't be easier: the low 32 bits hold the key, and the high 32 bits hold the value.

The structure for the table itself needs a pointer to the array of slots, the length of said array, and the number of non-empty slots. As the length is always a power of two, it's more useful to store `length - 1` instead of `length`, which leads to `mask` rather than `length`, and property (4) means that `mask` can be stored as 32 bits. As the load factor should be less than 100%, we can assume `count < length`, and hence `count` can also be 32 bits. This leads to a mundane-looking:

```
struct hash_table_t {
  uint64_t* slots;
  uint32_t mask;
  uint32_t count;
};
```

Property (1) means that we don't need to hash keys, as they're already randomly distributed. Every possible key `K` has a "natural position" in the slots array, which is just `K & mask`. If there are collisions, the slot in which a key *actually* ends up might be different to its natural position. The "linear probing" part of the design means that if `K` cannot be in its natural position, the next slot to be considered is `(K + 1) & mask`, and if not that slot then `(K + 2) & mask`, then `(K + 3) & mask`, and so on. This leads to the definition of a "chain": if `K` is some key present in the table, `CK` denotes the sequence of slots starting with `K`'s natural position and ending with `K`'s actual position. We have the usual property of open-addressing: none of the slots in `CK` are empty slots. The "Robin Hood" part of the design then imposes an additional rather interesting property: for each slot `S` in `CK`, `Score(S.Index, S.Key) ≥ Score(S.Index, K)`, where:

* `S.Index` is the index of `S` in the `slots` array (not the index of it in `CK`).
* `S.Key` is the key present in slot `S` (i.e. the low 32 bits of `slots[S.Index]`).
* `Score(Index, Key)` is `(Index - Key) & mask`.

These properties give us the termination conditions for the lookup algorithm: for a possible key `K`, we look at each slot starting from `K`'s natural position, and either we find `K`, or we find an empty slot, or we find a slot with `Score(S.Index, S.Key) < Score(S.Index, K)`. In either of the latter two cases, `K` cannot have been present in the table. In the function below, `Score(S.Index, K)` is tracked as `d`. In a language with a modern type system, the result of a lookup would be `Optional<Value>`, but if sticking to plain C, property (3) can be used to make something similar: the 64-bit result is zero if the key is absent, and otherwise the value is in the low 32 bits of the result (which may themselves be zero, but the full 64-bit result will be non-zero). The logic is thus:

```
uint64_t table_lookup(hash_table_t* table, uint32_t key) {
  uint32_t mask = table->mask;
  uint64_t* slots = table->slots;
  for (uint32_t d = 0;; ++d) {
    uint32_t idx = (key + d) & mask;
    uint64_t slot = slots[idx];
    if (slot == 0) {
      return 0;
    } else if (key == (uint32_t)slot) {
      return (slot >> 32) | (slot << 32);
    } else if (((idx - (uint32_t)slot) & mask) < d) {
      return 0;
    }
  }
}
```

If using a rich 64-bit CPU architecture, many of the expressions in the above function are cheaper than they might initially seem:

* `slots[idx]` involves zero-extending `idx` from 32 bits to 64, multiplying it by `sizeof(uint64_t)`, adding it to `slots`, and then loading from that address. All this is a single instruction on x86-64 or arm64.
* `key == (uint32_t)slot` involves a comparison using the low 32 bits of a 64-bit register, which is a completely standard operation on x86-64 or arm64.
* `(slot >> 32) | (slot << 32)` is a rotation by 32 bits, which again is a single instruction on x86-64 or arm64.

On the other hand, if using riscv64, things are less good:

* If the `Zba` extension is present, `sh3add.uw` is a single instruction for zero-extending `idx` from 32 bits to 64, multiplying it by `sizeof(uint64_t)`, and adding it to `slots`. If not, each step is a separate instruction, though the zero-extension can be eliminated [with a slight reformulation](https://gist.github.com/corsix/65ffbd66bbd199b3d77e4ec24c3d3724) to encourage the compiler to fold the zero-extension onto the load of `table->mask` (as riscv64 usually defaults to making sign-extension free, in contrast to x86-64/arm64 which usually make zero-extension free). Regardless, the load is always its own instruction.
* `key == (uint32_t)slot` hits a gap in the riscv64 ISA: it doesn't have any 32-bit comparison instructions, so this either becomes a 32-bit subtraction followed by a 64-bit comparison against zero, or promotion of both operands from 32 bits to 64 bits followed by a 64-bit comparison.
* If the `Zbb` extension is present, rotations are a single instruction. If not, they're three instructions, and so it becomes almost worth reworking the slot layout to put the key in the high 32 bits and the value in the low 32 bits.

Moving on from lookup to insertion, there are various different options for what to do when the key being inserted is already present. I'm choosing to show a variant which returns the old value (in the same form as `table_lookup` returns) and then overwrites with the new value, though other variants are obviously possible. The logic follows the same overall structure as seen in `table_lookup`:

```
uint64_t table_set(hash_table_t* table, uint32_t key, uint32_t val) {
  uint32_t mask = table->mask;
  uint64_t* slots = table->slots;
  uint64_t kv = key + ((uint64_t)val << 32);
  for (uint32_t d = 0;; ++d) {
    uint32_t idx = ((uint32_t)kv + d) & mask;
    uint64_t slot = slots[idx];
    if (slot == 0) {
      // Inserting new value (and slot was previously empty)
      slots[idx] = kv;
      break;
    } else if ((uint32_t)kv == (uint32_t)slot) {
      // Overwriting existing value
      slots[idx] = kv;
      return (slot >> 32) | (slot << 32);
    } else {
      uint32_t d2 = (idx - (uint32_t)slot) & mask;
      if (d2 < d) {
        // Inserting new value, and moving existing slot
        slots[idx] = kv;
        table_reinsert(slots, mask, slot, d2);
        break;
      }
    }
  }
  if (++table->count * 4ull >= mask * 3ull) {
    // Expand table once we hit 75% load factor
    table_rehash(table);
  }
  return 0;
}
```

To avoid the load factor becoming too high, the above function will sometimes grow the table by calling this helper function:

```
void table_rehash(hash_table_t* table) {
  uint32_t old_mask = table->mask;
  uint32_t new_mask = old_mask * 2u + 1u;
  uint64_t* new_slots = calloc(new_mask + 1ull, sizeof(uint64_t));
  uint64_t* old_slots = table->slots;
  uint32_t idx = 0;
  do {
    uint64_t slot = old_slots[idx];
    if (slot != 0) {
      table_reinsert(new_slots, new_mask, slot, 0);
    }
  } while (idx++ != old_mask);
  table->slots = new_slots;
  table->mask = new_mask;
  free(old_slots);
}
```

Both of `table_set` and `table_rehash` make use of a helper function which is very similar to `table_set`, but doesn't need to check for overwriting an existing key and also doesn't need to update `count`:

```
void table_reinsert(uint64_t* slots, uint32_t mask, uint64_t kv, uint32_t d) {
  for (;; ++d) {
    uint32_t idx = ((uint32_t)kv + d) & mask;
    uint64_t slot = slots[idx];
    if (slot == 0) {
      slots[idx] = kv;
      break;
    } else {
      uint32_t d2 = (idx - (uint32_t)slot) & mask;
      if (d2 < d) {
        slots[idx] = kv;
        kv = slot;
        d = d2;
      }
    }
  }
}
```

That covers lookup and insertion, so next up is key removal. As already hinted at, this hash table design doesn't need tombstones. Instead, removing a key involves finding the slot containing that key and then shifting slots left until finding an empty slot or a slot with `Score(S.Index, S.Key) == 0`. This removal strategy works due to a neat pair of emergent properties:

* If slot `S` has `Score(S.Index, S.Key) != 0`, it is viable for `S.Key` to instead be at `(S.Index - 1) & mask` (possibly subject to additional re-arranging to fill the gap formed by moving `S.Key`).
* If slot `S` has `Score(S.Index, S.Key) == 0`, and `S` is part of some chain `CK`, then `S` is at the very start of `CK`. Hence it is viable to turn `(S.Index - 1) & mask` into an empty slot without breaking any chains.

This leads to the tombstone-free removal function, which follows the established pattern of returning either the old value or zero:

```
uint64_t table_remove(hash_table_t* table, uint32_t key) {
  uint32_t mask = table->mask;
  uint64_t* slots = table->slots;
  for (uint32_t d = 0;; ++d) {
    uint32_t idx = (key + d) & mask;
    uint64_t slot = slots[idx];
    if (slot == 0) {
      return 0;
    } else if (key == (uint32_t)slot) {
      uint32_t nxt = (idx + 1) & mask;
      --table->count;
      while (slots[nxt] && ((slots[nxt] ^ nxt) & mask)) {
        slots[idx] = slots[nxt];
        idx = nxt;
        nxt = (idx + 1) & mask;
      }
      slots[idx] = 0;
      return (slot >> 32) | (slot << 32);
    } else if (((idx - (uint32_t)slot) & mask) < d) {
      return 0;
    }
  }
}
```

The final interesting hash table operation is iterating over all keys and values, which is just an array iteration combined with filtering out zeroes:

```
void table_iterate(hash_table_t* table, void(*visit)(uint32_t key, uint32_t val)) {
  uint64_t* slots = table->slots;
  uint32_t mask = table->mask;
  uint32_t idx = 0;
  do {
    uint64_t slot = slots[idx];
    if (slot != 0) {
      visit((uint32_t)slot, (uint32_t)(slot >> 32));
    }
  } while (idx++ != mask);
}
```

---

That wraps up the core concepts of this hash table, so now it is time to revisit some of the initial simplifications.

If keys are 32-bit integers but are *not* randomly-distributed, then we just need an invertible hash function from 32 bits to 32 bits, the purpose of which is to take keys following ~any real-world pattern and emit a ~random pattern. The `table_lookup`, `table_set`, and `table_remove` functions gain `key = hash(key)` at the very start but are otherwise unmodified (noting that if the hash function is invertible, hash equality implies key equality, hence no need to explicitly check key equality), and `table_iterate` is modified to apply the inverse function before calling `visit`. If hardware CRC32 / CRC32C instructions are present (as is the case on sufficiently modern x86-64 and arm64 chips), these can be used for the task, although their inverses are annoying to compute, so perhaps not ideal if iteration is an important operation. If CRC32 isn't viable, [one option](https://github.com/skeeto/hash-prospector/issues/19) out of many is:

|  |  |
| --- | --- |
| ``` uint32_t u32_hash(uint32_t h) {   h ^= h >> 16;   h *= 0x21f0aaad;   h ^= h >> 15;   h *= 0x735a2d97;   h ^= h >> 15;   return h; } ``` | ``` uint32_t u32_unhash(uint32_t h) {   h ^= h >> 15; h ^= h >> 30;   h *= 0x97132227;   h ^= h >> 15; h ^= h >> 30;   h *= 0x333c4925;   h ^= h >> 16;   return h; } ``` |

If keys and values are larger than 32 bits, then the design can be augmented with a separate array of key/value pairs, with the design as shown containing a 32-bit hash of the key and the array index of the key/value pair. To meet property (3) in this case, either the hash function can be chosen to never be zero, or "array index plus one" can be stored rather than "array index". It is not possible to make the hash function invertible in this case, so `table_lookup`, `table_set`, and `table_remove` *do* need extending to check for key equality after confirming hash equality. Iteration involves walking the separate array of key/value pairs rather than the hash structure, which has the added benefit of iteration order being related to insertion order rather than hash order. As another twist on this, if keys and values are variably-sized, then the design can instead be augmented with a separate array of *bytes*, with key/value pairs serialised somewhere in that array, and the hash structure containing a 32-bit hash of the key and the *byte* offset (within the array) of the key/value pair.

Of course, a design can only stretch so far. If you're after a concurrent lock-free hash table, look elsewhere. If you can rely on 128-bit SIMD instructions being present, you might instead want to group together every 16 key/value pairs, keep an 8-bit hash of each key, and rely on SIMD to perform 16 hash comparisons in parallel. If you're building hardware rather than software, it can be appealing to have multiple hash functions, each one addressing its own SRAM bank. There is no one-size-fits-all hash table, but I've found the one shown here to be good for a lot of what I do.

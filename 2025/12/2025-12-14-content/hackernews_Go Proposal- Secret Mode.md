---
source: hackernews
title: Go Proposal: Secret Mode
url: https://antonz.org/accepted/runtime-secret/
date: 2025-12-14
---

[![](/assets/logo.svg)](/)
[**Anton Zhiyanov**](/)

[projects](/#projects)[books](/#books)[blog](/all/)[about](/#about)[◐](#theme "Switch theme")

# Go proposal: Secret mode

*Part of the [Accepted!](/tags/accepted/) series, explaining the upcoming Go changes in simple terms.*

Automatically erase used memory to prevent secret leaks.

Ver. 1.26 • Stdlib • Low impact

## Summary

The new `runtime/secret` package lets you run a function in *secret mode*. After the function finishes, it immediately erases (zeroes out) the registers and stack it used. Heap allocations made by the function are erased as soon as the garbage collector decides they are no longer reachable.

```
secret.Do(func() {
    // Generate a session key and
    // use it to encrypt the data.
})
```

This helps make sure sensitive information doesn't stay in memory longer than needed, lowering the risk of attackers getting to it.

The package is experimental and is mainly for developers of cryptographic libraries, not for application developers.

## Motivation

Cryptographic protocols like WireGuard or TLS have a property called "forward secrecy". This means that even if an attacker gains access to long-term secrets (like a private key in TLS), they shouldn't be able to decrypt past communication sessions. To make this work, session keys (used to encrypt and decrypt data during a specific communication session) need to be erased from memory after they're used. If there's no reliable way to clear this memory, the keys could stay there indefinitely, which would break forward secrecy.

In Go, the runtime manages memory, and it doesn't guarantee when or how memory is cleared. Sensitive data might remain in heap allocations or stack frames, potentially exposed in core dumps or through memory attacks. Developers often have to use unreliable "hacks" with reflection to try to zero out internal buffers in cryptographic libraries. Even so, some data might still stay in memory where the developer can't reach or control it.

The solution is to provide a runtime mechanism that automatically erases all temporary storage used during sensitive operations. This will make it easier for library developers to write secure code without using workarounds.

## Description

Add the `runtime/secret` package with `Do` and `Enabled` functions:

```
// Do invokes f.
//
// Do ensures that any temporary storage used by f is erased in a
// timely manner. (In this context, "f" is shorthand for the
// entire call tree initiated by f.)
//   - Any registers used by f are erased before Do returns.
//   - Any stack used by f is erased before Do returns.
//   - Any heap allocation done by f is erased as soon as the garbage
//     collector realizes that it is no longer reachable.
//   - Do works even if f panics or calls runtime.Goexit. As part of
//     that, any panic raised by f will appear as if it originates from
//     Do itself.
func Do(f func())
```

```
// Enabled reports whether Do appears anywhere on the call stack.
func Enabled() bool
```

The current implementation has several limitations:

* Only supported on linux/amd64 and linux/arm64. On unsupported platforms, `Do` invokes `f` directly.
* Protection does not cover any global variables that `f` writes to.
* Trying to start a goroutine within `f` causes a panic.
* If `f` calls `runtime.Goexit`, erasure is delayed until all deferred functions are executed.
* Heap allocations are only erased if ➊ the program drops all references to them, and ➋ then the garbage collector notices that those references are gone. The program controls the first part, but the second part depends on when the runtime decides to act.
* If `f` panics, the panicked value might reference memory allocated inside `f`. That memory won't be erased until (at least) the panicked value is no longer reachable.
* Pointer addresses might leak into data buffers that the runtime uses for garbage collection. Do not put confidential information into pointers.

The last point might not be immediately obvious, so here's an example. If an offset in an array is itself secret (you have a `data` array and the secret key always starts at `data[100]`), don't create a pointer to that location (don't create a pointer `p` to `&data[100]`). Otherwise, the garbage collector might store this pointer, since it needs to know about all active pointers to do its job. If someone launches an attack to access the GC's memory, your secret offset could be exposed.

The package is mainly for developers who work on cryptographic libraries. Most apps should use higher-level libraries that use `secret.Do` behind the scenes.

As of Go 1.26, the `runtime/secret` package is experimental and can be enabled by setting `GOEXPERIMENT=runtimesecret` at build time.

## Example

Use `secret.Do` to generate a session key and encrypt a message using AES-GCM:

```
// Encrypt generates an ephemeral key and encrypts the message.
// It wraps the entire sensitive operation in secret.Do to ensure
// the key and internal AES state are erased from memory.
func Encrypt(message []byte) ([]byte, error) {
    var ciphertext []byte
    var encErr error

    secret.Do(func() {
        // 1. Generate an ephemeral 32-byte key.
        // This allocation is protected by secret.Do.
        key := make([]byte, 32)
        if _, err := io.ReadFull(rand.Reader, key); err != nil {
            encErr = err
            return
        }

        // 2. Create the cipher (expands key into round keys).
        // This structure is also protected.
        block, err := aes.NewCipher(key)
        if err != nil {
            encErr = err
            return
        }

        gcm, err := cipher.NewGCM(block)
        if err != nil {
            encErr = err
            return
        }

        nonce := make([]byte, gcm.NonceSize())
        if _, err := io.ReadFull(rand.Reader, nonce); err != nil {
            encErr = err
            return
        }

        // 3. Seal the data.
        // Only the ciphertext leaves this closure.
        ciphertext = gcm.Seal(nonce, nonce, message, nil)
    })

    return ciphertext, encErr
}
```

Note that `secret.Do` protects not just the raw key, but also the `cipher.Block` structure (which contains the expanded key schedule) created inside the function.

This is a simplified example, of course — it only shows how memory erasure works, not a full cryptographic exchange. In real situations, the key needs to be shared securely with the receiver (for example, through key exchange) so decryption can work.

## Links & Credits

𝗣 [21865](https://go.dev/issue/21865) 👥 [Dave Anderson](https://github.com/danderson), [Filippo Valsorda](https://github.com/FiloSottile), [Jason A. Donenfeld](https://github.com/zx2c4), [Russ Cox](https://github.com/rsc)

𝗖𝗟 [704615](https://go.dev/cl/704615) 👥 [Daniel Morsing](https://github.com/DanielMorsing), [Keith Randall](https://github.com/randall77)

[★ Subscribe](/subscribe/) to keep up with new posts.

09 Dec, 2025

[thank-go](/tags/thank-go/)
[accepted](/tags/accepted/)

← [Gist of Go: Concurrency internals](/go-concurrency/internals/)

[Gist of Go: Concurrency is out!](/go-concurrency-released/) →

### See also

[Go proposal: Type-safe error checking](/accepted/errors-astype/)

[Go proposal: Goroutine metrics](/accepted/goroutine-metrics/)

[Go proposal: Context-aware Dialer methods](/accepted/net-dialer-context/)

* Subscribe:
  [Newsletter](https://buttondown.email/antonz)[Twitter](https://x.com/ohmypy)[Bluesky](https://bsky.app/profile/antonz.org)[Mastodon](https://c.im/%40antonz)[LinkedIn](https://www.linkedin.com/in/nalgeon/)[GitHub](https://github.com/nalgeon)[RSS](/index.xml)
* Email: m@antonz.org

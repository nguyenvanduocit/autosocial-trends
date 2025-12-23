---
source: hackernews
title: Things I learnt about passkeys when building passkeybot
url: https://enzom.dev/b/passkeys/
date: 2025-12-23
---

[enzom.dev](/)
web & app development

[🔥 apps](/)
[blog](/b)
[about enzo](/b/about-enzo)
[subscribe](https://forms.gle/mMXeqRuCJYBeFoDn9)
[twitter](https://x.com/_enzo_dev)
[youtube](https://www.youtube.com/%40enzdev)

2025-w51
 Folder: [passkeybot](/b/f/passkeybot)

# Things I learnt about passkeys when building passkeybot

I recently released
[passkeybot.com](http://passkeybot.com), a hosted sign in
page that allows you to add passkey auth to your site with just a few
server side HTTP handlers.

Here are the things I learnt in the process.

## What Secure Enclave Processors (SEP) are

Apple devices have secure enclaves which are like a separate tiny
computer living inside the main CPU that has its own isolated
encrypted memory and OS. It can create secrets that never leave the
secure enclave. The main OS can only prove it has possession of that
secret by asking the secure enclave to sign some data and getting the
signature as a response (it can only use this message protocol with
the SEP).

When the user signs in with their passkey with User Verification =
true, the SEP requires a biometric/passcode auth first before signing
the data with the private key.

Other devices have something similar to the SEP, but are branded with
different names.

Phone SIM cards are actually a form of secure element. SIM cards are
CPUs that run a stripped down version of Java, and use the same
principle of “secrets can never leave the SIM” and “prove possession
with message signing”.

## User Presence (UP) vs User Verification (UV)

**Presence** means “the user tapped a button and was
there”, **verification** means “the user entered their
biometric or passcode”.

You can request which one you require with the JS passkey API.

The difference is **presence can** be faked by anyone
with the unlocked device by pressing a button, but
**verification** always requires the re-auth of the user
with biometrics or a passcode.

## What an authenticator is

An authenticator is the hardware and software that holds the
private/public key pairs and signs the passkey challenge to prove it
has the private key. On Apple devices that is the SEP.

The browser asks the user which authenticator they want to use, then
uses OS level APIs to interact with the chosen authenticator.

For example:

* User chooses on-device **Apple SEP** → site calls JS
  API → browser uses Swift API for passkey operations.
* User chooses **Yubikey** → site calls JS API → browser
  uses Yubikey API over USB for passkey operations

The interesting thing here is that the JS API normalises all these
different possible authenticator APIs. Under the hood the browser
implements all the possible API protocols for different
authenticators.

The Chrome Dev Tools also has a virtual authenticator to bypass
reptetive OS password entry for testing.

## What attestation is

**Signing proves possession**: Being able to sign with
the private key proves you have possession of it.

**Attestation proves device hardware used**: Attestation
proves what hardware and software combination created the passkey
pair. It allows enforcing policies for what set of hardware devices
are trusted, and which are blocked.

The issue is that attestation data also allows fingerprinting as it
reveals exactly what hardware the user is using.

Hardware attestation only occurs for the creation of the passkey pair
(not on every auth). This creates an issue: if keys are synced to
another device, the attestation is no longer valid. So if you require
strict attestation that specific hardware is used, you create a new
keypair for every device instead of allowing use of a passkey pair
that has moved to a non-attested device.

Without attestation the secure enclave is still used, it is just not
proven to the webauthn client API.

Apple hardware has attestation disabled by default unless you have
[enterprise device management](https://support.apple.com/en-gb/guide/deployment/depc0aadd3fe/web)
enabled. This lets enterprises define an allow-list of trusted
authenticator hardware.

## Passkeys are just for authentication, not for general signing of intent

When the user authenticates with a passkey, they sign a challenge that
is a hash unique to that particular sign in flow. The challenge hash
needs to have 16 bytes or more of random data to avoid replays, but it
can also include a hash over other metadata.

The authenticator GUI only shows “sign in to your\_domain.com”. It
never allows a more general “sign this content for your\_domain.com”.
E.g. “sign this transaction request to move £50 to Bob's account”.

## The JS code must be secure, but it cannot be verified

If the JS code of a site is compromised, the attacker can read all
personal data. They could also trick the user into signing something
with their authenticator (as the authenticator does not show the user
what they are actually signing) - this leads to a real private key
signing over faked challenge data.

The browser has Subresource Integrity (SRI) which only allows
executing JS scripts with a given hash. But the root document HTML is
not checked in that case, which means the attacker can change the SRI
hashes to match their own JS. Chrome Extensions also allow injecting
JS.

It would be interesting if authenticators could also attest as to what
HTML/JS was loaded on the page to rule out that they have been
compromised.

## Immediate mediation - an upcoming “fast sign in” API

[This Chrome origin trial](https://developer.chrome.com/blog/webauthn-immediate-mediation-ot)
will add an option the passkey API:

```
navigator.credentials.get({mediation: "immediate"})
```

This allows you to sign in a user who already has a passkey quickly.

If they do not have a passkey, you can decide what to do from JS
(instead of having the browser show them a UI to find passkeys on
other devices).

There is no way to get a list of the user's passkeys from JS -
lists are always shown from the browser UI.

The immediate mediation option allows your JS to get an immediate “the
user has 0 local keys” message without any user interaction:

* 0 keys: Immediate JS response with NotAllowedError,
  **JS decides next step**.
* 1 key: Immediately ask the user to sign in with that one.
* >1 key: Ask the user to choose.

## Related Origin Requests

[Related Origin Requests](https://web.dev/articles/webauthn-related-origin-requests)
allow you as a domain owner to define a list of other domains that can
create passkeys for your domain. They work by having you serve the
list from `/.well-known/webauthn`.

This is what [passkeybot.com](http://passkeybot.com) uses
to allow domain owners to grant permissions to passkeybot.

But RORs do not work over HTTP, only HTTPS. The reason is the
authenticator requests the well-known file over HTTPS only, so
localhost will not work.

They are also not supported in iOS 18 or Firefox.

If an authenticator creates a passkey for the root domain, that
passkey will work for all subdomains. But if it creates it for a
subdomain, it only ever works on that specific subdomain.

## The counter is just a “heuristic”

Authenticators store a counter which increments for each passkey
usage. In theory this can detect a cloned authenticator. But much of
the recommendations say to use the counter as a heuristic rather than
evidence of a cloned authenticator because there are many legitimate
reasons the counter can be wrong. I think in practice this counter is
often ignored.

## Use passkeys stored on nearby devices using Bluetooth

You can sign into a public computer that does not have your passkeys
by having your own device's authenticator communicate with it over
Bluetooth Low Energy (BLE). Bluetooth is used to assert your close
proximity with the device you are signing into. Your keys never leave
your device, the signing protocol travels between the two devices.

## Deleting passkeys with the Signal API

The JS API cannot list or modify the list of passkeys, only the
browser GUI or Apple Passwords can do that.

But you can asynchronously signal that you want to delete a passkey.
It is only a hint, and you will not receive back any confirmation as
that may leak user data.

The
[Signal API methods](https://developer.mozilla.org/en-US/docs/Web/API/PublicKeyCredential/signalUnknownCredential_static)
currently are:

```
PublicKeyCredential.signalUnknownCredential({ rpId, credentialId })
PublicKeyCredential.signalAllAcceptedCredentials({ rpId, userId, allAcceptedCredentialIds })
PublicKeyCredential.signalCurrentUserDetails({ rpId, userId, name, displayName })
```

## user.id and userHandle represent “one account”

The `user.id` and userHandle are the same value, but with
different names in different JS API calls.

They are used to map many passkeys to a single logical account. It
should be set and stored as passkey APIs require the
`user.id` (like the signal APIs above).

You can generate one unique `user.id` per new passkey and
just store the user => passkey relation in your database, but this
may prevent “per account” passkey grouping and management in the
browser UI.

## crypto.subtle.generateKey can create non-extractable keys

[generateKey](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto/generateKey)
is a JS API that allows you to create new key pairs, where the private
key cannot be extracted similar to passkeys.

```
crypto.subtle.generateKey(algorithm, extractable, keyUsages)
```

You can perform general operations like signing with this key pair,
but in the case of JS being compromised, the private key cannot be
read and moved to a different device. But compromised JS can still
sign using that unextractable private key.

## PKCE = "Proof Key for Code Exchange” was retrofitted into OAuth

[PKCE](https://datatracker.ietf.org/doc/html/rfc7636) is a
protocol that works like a one time password: at the start of every
sign in flow an actor creates a code\_verifier and code\_challenge.

The code\_verifier is a secret random 32 bytes held on the flow
initiating actor. The code\_challenge is the sha256 hash of those
bytes, and is shared with the user going through the sign in flow.

The code\_challenge is also sent to the auth service that verifies the
user and creates a (token, code\_challenge).

PKCE protects this token by only allowing the holder of the
code\_verifier secret to redeem it by sending (token, code\_verifier).

This means even if the token is stolen, only the actor that started
the flow can redeem it with the auth API.

PKCE was originally designed for environments that cannot hold static
secrets because the source code can be read - like JS or desktop apps.
Instead of embedding static secrets, these actors dynamically create
secrets at runtime at the start of each sign in flow (the
code\_verifier and code\_challenge pair).

Passkeybot uses PKCE to avoid having to manage API bearer token
secrets for each API client. Note: Passkeybot uses the general
principle behind PKCE, but naming and interaction differs from the
OAuth standard.

It is interesting how they managed to retrofit PKCE into the OAuth
standard to solve the “token interception” problem. It only requires a
sha256 hash function so is very easy to implement.

## Digital Credentials API is a browser bridge to the native OS wallet

This is a passkey adjacent JS API. The
[Digital Credentials API](https://developer.chrome.com/blog/digital-credentials-api-shipped)
will allow you to request things from the user's native OS wallet
like IDs, tickets, badges, membership cards. It allows you to do
things like prove your age or ability to drive without having to share
your actual identity cards.

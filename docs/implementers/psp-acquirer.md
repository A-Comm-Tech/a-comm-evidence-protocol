# PSP / Acquirer Quickstart — the chain writer

**Goal:** one evidence pipeline that serves every merchant you process for — and pre-dispute deflection that keeps qualifying disputes out of monitoring-program ratios.

You sit at the highest-leverage point in the standard: you already hold the authorization data, the webhooks, the dispute-evidence APIs, and the merchant relationships. A PSP that writes chains gives every merchant on its platform "checkbox" adoption.

## Integration surfaces

1. **Chain writer at webhook time.** Emit `authorization` artifacts from your charge events (you already have every §3.6 field: auth code, `network_transaction_id`, AVS/CVV/3DS results, ECI, DS transaction ID, SCA status/exemption). Accept merchant-side `intent`/`cart` artifacts via SDK; accept platform countersignatures when present. Seal at authorization; append `fulfillment`/`refund` post-seal (§3.10).
2. **Delegation wrapper capture.** For the agentic protocols you already integrate (checkout sessions, mandates, agent-identity tokens), populate the §3.9 sub-wrappers at capture time — including **key-witness snapshots** of the third-party key material you verified against, so chains stay verifiable after upstream key rotation (§3.9.11, §4.3). Pragmatic starting subset: your checkout-session protocol + one mandate format + one agent-identity format.
3. **Verification service.** Verify at seal time; serve **cached verification results** for inquiry-speed channels (sub-second SLAs), full re-verification only for representment-bound exports (§6.2). Index at seal time — settlement and fulfillment arrive late by design.
4. **Dispute renderers.** DisputeBundle → your evidence API using Appendix C: text fields vs **file-upload** fields are distinguished per current APIs, with the normative format-budget table (2 MB PDF floor, 19-page scheme package cap, 16-char filenames). The **compelling-evidence renderer** (§6.4) emits structured elements from the custodian raw plane + the cross-chain `customerHistory` object (§6.1) — that's the pre-dispute deflection path, and deflected items are the ones that never hit your merchants' (and your) monitoring ratios.

## Operational notes (learned the hard way, so you don't)

- **Canonicalization is the bug farm.** RFC 8785 + NFC + integer-only. Run the vectors (§6.3) in every language of your stack before anything ships.
- **Fail closed on wrapped credentials:** decode envelope-shaped fields; refuse full PANs/security codes; store digest + extracted claims (§3.9.20, §8.3).
- **Idempotency is event-native:** key artifact writes on your event IDs / protocol `jti`s, not wall-clock buckets (§8.4).
- **Salt management is a KMS program:** per-merchant salts, `salt_kid` per chain, old salts retained for the verification window (§2.7). Deterministic hashes are only Deterministic for verifiers with salt access — plan the disclosure procedure (§2.8, §8.2.1).
- **Export gate:** a bundle that fails verification MUST NOT be submitted (§6.2). Wire that as a hard check, not a warning.
- **Legal hold:** deletion at `retention_until` suspends for chains in open disputes/litigation (§8.5).

## The economics

Representment wins recover money after the fact; **pre-dispute deflection changes the ratio math**. Order-detail responses at inquiry time, compelling-evidence qualification, and prior-transaction matching all run on data AEP chains already hold — the renderers translate it into each program's intake format. For agent-initiated transactions specifically, chains are the only place the *delegation ceremony* (who authorized the agent, under what constraints) exists in exportable form — and network compelling-evidence programs are beginning to accept agentic-provider identity elements directly (late-2026 rule changes).

# Merchant Quickstart — Minimal Merchant Profile (§8.6)

**Goal:** dispute-grade evidence chains for agent-initiated orders, implementable in days, without waiting for anyone else in the ecosystem.

## What you build

The Minimal Merchant Profile is four artifacts you can emit from data you already have:

| Artifact | Source you already have | When |
|---|---|---|
| `intent` (§3.3) | Product page / add-to-cart event (product id, price displayed, timestamps) | Shopper/agent lands on a product |
| `cart` (§3.5) | Checkout session (line items, integer minor-unit totals, `subtotal + shipping + tax − discounts === total`) | Cart confirmed |
| `authorization` (§3.6) | **Your PSP's webhook** — charge id, auth code, amount, `network_transaction_id`, AVS/CVV/3DS results | Payment authorized → **chain seals** |
| `fulfillment` / `refund` (§3.7, §3.10) | Carrier webhook / your refund event | Post-seal appends |

Discovery, referral, and delegation artifacts are **not your job** under this profile — they belong to agent platforms and PSPs, and their absence does not penalize your chain's completeness (§8.6).

## Steps

1. **Keys and salt.** Generate an Ed25519 keypair (KMS-held); publish the public key at `/.well-known/aep-public-key` with a `key_id`. Generate a per-merchant HMAC salt with a `salt_kid`; plan the ≤365-day rotation (§2.7, §4.3).
2. **Canonicalization.** Use an RFC 8785 (JCS) library + NFC normalization + integer-only amounts. **Run the test vectors first** (§6.3) — this is where implementations break.
3. **Emit the four artifacts** on your existing event hooks. Hash-link each to its predecessor; seal at authorization.
4. **Index by `network_transaction_id`** (your PSP returns it). That's how a chain is found at dispute time (§3.6).
5. **Export.** On a dispute, produce the DisputeBundle (§6) and hand it to your PSP's dispute-evidence flow using the field mappings in Appendix C. If a bundle fails verification, it must not be submitted (§6.2).

## What you get

- A sealed, independently re-verifiable record for every agent-initiated order — the narrative and structured elements your representments are built from.
- Compelling-evidence readiness: capture `customer_device_id` / `customer_device_fingerprint` and keep raw identity values in a custodian plane (§8.2.1) and your bundles can feed network compelling-evidence and pre-dispute data-sharing programs — including, from late October 2026, agentic-payment-provider login IDs as a Visa CE 3.0 matching element.
- A chain that upgrades automatically: when your PSP or an agent platform starts emitting delegation/intent countersignatures, your evidence tier rises with zero rework on your side.

## What NOT to do

- Don't store raw IPs, addresses, or emails in artifact payloads — hashes in the chain, raw values with the custodian (§8.2).
- Don't persist any wrapped payment credential verbatim (§3.9.20).
- Don't treat tiers as liability protection — they describe verifiability, and no network rule assigns outcomes to them (§3.9.12). The win is better evidence, not a shield.

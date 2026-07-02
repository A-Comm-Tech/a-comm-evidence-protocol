# Agent Platform Quickstart — Minimal Platform Profile (§8.7)

**Goal:** make the record of what your user authorized be *your signed artifact* — not a merchant's heuristic about you — at the cost of one countersignature with keys you already operate.

## The deal, plainly

Merchants will attribute traffic to your platform whether you participate or not (referrer forensics, behavioral heuristics — recorded as low-confidence *Inferred/Asserted* claims, labeled "not verified by the named platform" on export). Participation replaces guesses about you with a precise, minimal artifact **you control**. The spec binds bundle recipients to purpose limitations (no cross-chain aggregation to reconstruct your ranking behavior, §3.1) and gives you named-actor rights: export notification, a bundle copy, and a post-seal rebuttal artifact on any chain that names you (§6.5) — plus a safe-harbor conformance term: your artifacts cannot be used against you in platform-merchant or platform-consumer disputes (§6.5).

## What you emit (and what you never do)

**You emit:**
1. A **countersigned Intent artifact** (§3.3): product, price, quantity, and your agent's identity — signed with signing infrastructure you already run (JWKS you publish; Web Bot Auth-style keys work).
2. Optionally, **`request_binding` material** (§3.9.19): the request-level linkage you already produce if you sign requests per RFC 9421.

**You never emit (the spec says SHOULD NOT for platforms):**
- `query_hash` or any derivative of user query text (§3.1/§3.3 — a coarse `query_category` enum exists if you want to volunteer vertical-level context)
- `citation_position` or any ranking-derived field (optional for platform emitters, §3.1)

**Persistence of your wire signatures requires your opt-in:** absent an explicit signal from you, implementations must store only a digest of your Web Bot Auth / identity signatures, not the signature values (§3.9.18).

## Steps

1. Publish (or reuse) a JWKS for your commerce agent identity.
2. At checkout initiation, countersign the Intent payload (JWS over the §3.3 canonical fields) and hand it to the merchant/PSP flow alongside your existing checkout protocol call — one extra field in a flow you already run.
3. Optionally register for named-actor notifications so you receive a copy whenever a chain naming you is exported (§6.5).

## Why bother

- **Checkout completion.** Declines and step-ups on agent traffic are your users' worst checkout experience. A verifiable delegation record is the input issuers and networks say they need to treat agent transactions as recognizable rather than suspicious.
- **The rogue-agent case.** The legal question "was the agent's purchase authorized?" is coming. When it arrives, you want the authoritative record of user authorization to be an artifact you signed with scope you chose — not discovery built from your header behavior.
- **Bounded exposure.** Chains are eligible for elevated evidence tiers with *only* your minimal participation (§8.7) — the standard is designed so you never have to choose between participating and protecting your users' privacy or your ranking IP.

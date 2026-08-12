# AEP v1.0.3 — Ecosystem Comment Round

**Draft under review:** [v1.0.3-rc.2](spec/aep-v1.0.3-rc.2.html) ([rendered — living draft](https://aep.a-comm.ai/) · [frozen rc.2 snapshot](https://aep.a-comm.ai/v1.0.3-rc.2) for stable section references) (2026-07-02)
**Status: Comment window July 13 – August 14, 2026.** Every comment receives a public disposition in the log below.

## Scope of this round

v1.0.3 is an additive (MINOR) release over v1.0.2. Headline changes under review:

- **Delegation wrapper coverage:** agent-identity token protocols (KYA family), Web Bot Auth wire identity, and a `request_binding` witness record.
- **Two-plane evidence model (§8.2.1):** salted hashes in the chain; dispute-usable raw values held by an Evidence Custodian role with sealed disclosure logging.
- **Anchoring & verification classes (§4.3, §5):** external timestamp/transparency-log anchoring, key-witness snapshots, key revocation, and recipient-relative verification classes.
- **Dispute-channel integration (§6):** refund artifact, cross-chain `customerHistory`, network evidence renderers, cached-verification mode, hard export gate, corrected PSP mappings (Appendix C).
- **Conformance profiles:** Minimal Merchant Profile (§8.6) and Minimal Platform Profile (§8.7); named-actor notification/rebuttal rights and platform safe-harbor terms (§6.5).
- **First-class non-card settlement (§3.6)** and EU/US regulatory posture (§9.4).

Open questions the editors specifically invite comment on:

1. Bank-rails build-out: `account` / `real_time_rails` variant objects, an open-banking consent wrapper, and per-rail export profiles (proof-of-authorization, error-resolution, RTP fraud report). Should these land in v1.0.4?
2. Hash-only chain profile with a detachable payload vault (storage economics at scale).
3. Splitting low-adoption sub-wrappers into an independently versioned extension registry at v1.1.0.
4. A compact, network-consumable verification attestation (signed summary a rail can verify without ingesting the chain).
5. The annotation/rebuttal artifact payload schema (§6.5; scheduled for v1.1.0).

## How to comment

| Channel | Use for |
|---|---|
| Issue template: **Section feedback** | Comments tied to a specific section number |
| Issue template: **Technical issue** | Bugs, contradictions, unimplementable requirements, security concerns |
| GitHub Discussions | Normative proposals and cross-cutting design questions |

One topic per issue. Cite section numbers. Claims about external protocols or network rules should cite primary sources.

## Process

- **Review window:** July 13 – August 14, 2026 (30 days).
- **Disposition:** every comment receives a public disposition — `accept` / `accept-with-modification` / `defer (versioned)` / `decline (rationale)` — recorded in the log below.
- **Output:** dispositions are batched into v1.0.3-final (additive edits) and the v1.0.4/v1.1.0 backlog (deferred items). The changelog (spec Appendix D) records what changed and why.
- **Conduct:** technical arguments only; no vendor endorsements or marketing. The editors apply the same neutrality rules to themselves — see CONTRIBUTING.md.

## Release gates for v1.0.3-final

- [ ] Conformance test vectors published at `vectors/v1.0.3` (spec §6.3)
- [ ] Reference-implementation alignment items closed (spec §3.2/§3.5/§3.6 migration notes)
- [ ] All comment dispositions recorded — see [Received, not yet dispositioned](#received-not-yet-dispositioned)
- [ ] Every `accept` / `accept-with-modification` disposition carried into spec text (Edit column reads `landed`)

---

# Disposition log

## How to read this log

**Ref.** Each disposition has a stable ref built from the GitHub number of the submission — `I26` for Issue #26, `D4` for Discussion #4. GitHub draws issues, pull requests, and discussions from a single per-repository numbering sequence, so the number alone is already unambiguous; the `I`/`D` prefix only tells you where to look. Where one submission received more than one disposition, refs are suffixed `-a`, `-b`.

**Cite refs, never row positions.** Refs are stable and order-independent. Row numbers are not: they shift whenever a disposition is inserted, and nothing checks them.

**Disposition** is one of four values, per Process above: `accept` · `accept-with-modification` · `defer (version)` · `decline (rationale)`.

**Target** names the release that carries the resulting change: `v1.0.3-final` · `v1.0.4` · `v1.1.0`, or `—` where a disposition produces no spec change.

**Edit** tracks whether the resulting spec text exists yet — `not started` · `drafted` · `landed`, or `n/a` where no edit is owed. A disposition is a decision; it is not the change. This column is the difference between the two.

**Response** records the commenter's reaction to the disposition — `accepted` · `contested` · `not recorded`. A decline that the commenter accepted and a decline the commenter contests are different public facts, and the round's legitimacy depends on showing which one it is.

**COI.** `✱` marks a submission from an editor-affiliated implementer. Per CONTRIBUTING.md the editors apply the same neutrality rules to themselves: these are self-filed, and their dispositions were recorded by a second editor.

## Index

| Ref | Source | Author | Section | Disposition | Target | Edit | Response | COI |
|---|---|---|---|---|---|---|---|---|
| [D4-a](#d4-a) | Discussion #4 | @Avouro | §3.9 (proposed §8.2.1) | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [D4-b](#d4-b) | Discussion #4 | @Avouro | §3.6 / §9.3 | decline (rationale) | — | n/a | not recorded | |
| [I26](#i26) | Issue #26 | @johnhenrypower | §3.9.2 / §2.6 / §3.6 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I30](#i30) | Issue #30 | @ankitshah009 | Cross-cutting | accept | v1.0.3-final | n/a | not recorded | |
| [I31](#i31) | Issue #31 | @ankitshah009 | §4 | defer (v1.0.4 / v1.1.0) | v1.0.4 / v1.1.0 | n/a | not recorded | |
| [I32](#i32) | Issue #32 | @ankitshah009 | §2.7 / §8.2.1 / §8 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I33](#i33) | Issue #33 | @ankitshah009 | §3.1 / §2.8 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I34](#i34) | Issue #34 | @ankitshah009 | §3.6 / §6 / §8.3 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I35](#i35) | Issue #35 | @ankitshah009 | §3.7 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I36](#i36) | Issue #36 | @ankitshah009 | §4.3 / §3.9.11 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I37](#i37) | Issue #37 | @ankitshah009 | §2.8 / §3.9.12 | defer (v1.1.0) | v1.1.0 | n/a | not recorded | |
| [I38](#i38) | Issue #38 | @ankitshah009 | §3.3 / §3.5 | defer (v1.0.4) | v1.0.4 | n/a | not recorded | |
| [I39](#i39) | Issue #39 | @ankitshah009 | §3.4 / §3.9 / §3.9.11 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I40](#i40) | Issue #40 | @ankitshah009 | §5 / §6 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I46](#i46) | Issue #46 | @jyothi-acomm-ai | §3.9.6 / §3.9.11 | accept | v1.0.3-final | not started | not recorded | ✱ |
| [I47](#i47) | Issue #47 | @jyothi-acomm-ai | §3.9.13 / §3.9.11 / §3.9.20 | accept-with-modification | v1.0.3-final | not started | not recorded | ✱ |
| [I56](#i56) | Issue #56 | @jyothi-acomm-ai | §3.9.13 / §2.8 / §8.3 | accept-with-modification | v1.0.3-final | not started | not recorded | ✱ |
| [D14-a](#d14-a) | Discussion #14 | @shunhe-wang | §6 / §3.6 / §3.9.10 | defer (v1.0.4) | v1.0.4 | n/a | accepted | |
| [D14-b](#d14-b) | Discussion #14 | @shunhe-wang | §3.9 / §9.3 / §9.4 | decline (rationale) | — | n/a | accepted | |
| [I3](#i3) | Issue #3 | @rrrodzilla | §2.7 / §4.1 / §4.3 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I5](#i5) | Issue #5 | @squishy-ctrl | §3.2 / §2.7 / §2.8 | accept-with-modification | v1.0.3-final / v1.0.4 | not started | not recorded | |
| [D6](#d6) | Discussion #6 | @squishy-ctrl | §8.8 (new) / §3.9.19 / §2.1 | accept-with-modification | v1.0.3-final | not started | accepted | |
| [D11](#d11) | Discussion #11 | @juanferrub | §2.5 / §4.3 / §8.4 | accept-with-modification | v1.0.3-final / v1.0.4 | not started | accepted | |
| [D12-a](#d12-a) | Discussion #12 | @timaxorum | §0.2 / §4.1 / Appendix B | defer (v1.1.0) | v1.1.0 | n/a | accepted | |
| [D12-b](#d12-b) | Discussion #12 | @timaxorum | §2.4 / §3.9 | accept-with-modification | v1.0.3-final / v1.0.4 | not started | accepted | |
| [D12-c](#d12-c) | Discussion #12 | @timaxorum | §3.9 | accept-with-modification | v1.0.3-final / v1.0.4 | not started | accepted | |
| [D12-d](#d12-d) | Discussion #12 | @timaxorum | §3.4 / §2.5 | defer (v1.1.0) | v1.1.0 | n/a | accepted | |
| [D12-e](#d12-e) | Discussion #12 | @timaxorum | §8.5 | accept-with-modification | v1.1.0 | n/a | accepted | |
| [D19](#d19) | Discussion #19 | @Avouro | §3.9.4 / §3.9.5 | decline (rationale) / defer (v1.1.0) | v1.1.0 | n/a | accepted | |
| [D22](#d22) | Discussion #22 | @HemmaBo-se | §5 / §4.3 / §8.2.1 | accept-with-modification | v1.0.3-final / v1.0.4 | not started | not recorded | |
| [D45](#d45) | Discussion #45 | @scottsgeorge | §3.6 / §3.10 / §6 | defer (v1.0.4) | v1.0.4 | n/a | not recorded | |
| [D48](#d48) | Discussion #48 | @Trusteedxyz | §4.3 / §9.4 / §2.8 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [D55](#d55) | Discussion #55 | @faulknerwayne73-droid | §4.3 / §3.9.12 / §6.3 | defer (v1.0.4) | v1.0.4 | n/a | not recorded | |
| [I7](#i7) | Issue #7 | @squishy-ctrl | §0.1 / §6.4 / §8.2.1 / §8.6 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I8](#i8) | Issue #8 | @squishy-ctrl | COMMENT-ROUND.md | accept | — | landed | not recorded | |
| [I15](#i15) | Issue #15 | @jyothi-acomm-ai | §5 / §6.1 / §6.2 | accept | v1.0.3-final | not started | not recorded | ✱ |
| [I16](#i16) | Issue #16 | @jyothi-acomm-ai | §6.1 | accept-with-modification | v1.0.3-final | not started | not recorded | ✱ |
| [I17](#i17) | Issue #17 | @jyothi-acomm-ai | §4.1 / §6.3 | accept | v1.0.3-final | not started | not recorded | ✱ |
| [I18](#i18) | Issue #18 | @jyothi-acomm-ai | §6.5 | accept-with-modification | v1.0.3-final | not started | not recorded | ✱ |
| [I20](#i20) | Issue #20 | @HemmaBo-se | §3.7 / §3.5 / §3.10 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I21](#i21) | Issue #21 | @HemmaBo-se | §3.9.11 / §2.8 | accept-with-modification | v1.0.3-final | not started | accepted | |
| [I23](#i23) | Issue #23 | @johnhenrypower | §3.9.18 / §3.9.11 | accept | v1.0.3-final | not started | not recorded | |
| [I24](#i24) | Issue #24 | @johnhenrypower | §3.9.15 | accept | v1.0.3-final | not started | not recorded | |
| [I25](#i25) | Issue #25 | @johnhenrypower | §3.9.18 / §3.9.11 | accept | v1.0.3-final | not started | not recorded | |
| [I42](#i42) | Issue #42 | @rabet | §3.9.N / §2.8 / §3.9.11 | accept-with-modification | v1.0.3-final / v1.0.4 | not started | not recorded | |
| [I43](#i43) | Issue #43 | @rabet | §2.8 / §3.7 / §6.1 | accept-with-modification | v1.0.3-final / v1.0.4 | not started | not recorded | |
| [I44](#i44) | Issue #44 | @Marlonm0987 | §9.4 | decline in part / defer (v1.0.4) | v1.0.3-final | not started | not recorded | |
| [I49](#i49) | Issue #49 | @Trusteedxyz | §8.6 / §3.4 | accept | v1.0.3-final | not started | not recorded | |
| [I50](#i50) | Issue #50 | @Trusteedxyz | §3.4 / §2.8 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I51](#i51) | Issue #51 | @Trusteedxyz | §3.4 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I52](#i52) | Issue #52 | @Trusteedxyz | §3.4 | defer (v1.0.4) | v1.0.4 | n/a | not recorded | |
| [I53](#i53) | Issue #53 | @Trusteedxyz | §3.4 / §3.6 | accept | v1.0.3-final | not started | not recorded | |
| [I54](#i54) | Issue #54 | @Trusteedxyz | §3.4 | accept | v1.0.3-final | not started | not recorded | |
| [I58](#i58) | Issue #58 | @shunhe-wang | §2.5 / §3.6 / §3.9.10 / §3.9.11 | defer (v1.0.4) | v1.0.4 / v1.0.3-final | not started | not recorded | |
| [I59](#i59) | Issue #59 | @shunhe-wang | §3.9.12 / §5 / §6.1 | accept-with-modification | v1.0.3-final / v1.1.0 | not started | not recorded | |
| [I60](#i60) | Issue #60 | @saishav7 | §3.9.11 / §3.9.10 | accept-with-modification | v1.0.3-final | not started | not recorded | |

## Detail

<a id="d4-a"></a>
### D4-a · Discussion #4 — pre-execution verification attestation primitive

**§3.9 (proposed §8.2.1)** · @Avouro · **accept-with-modification** · target v1.0.3-final · tracking issue #27

Pre-execution verification attestation accepted as new optional §3.9.N `pre_execution_attestation` sub-wrapper for v1.0.3-final, with required `issuer`, `issuer_relationship`, external `anchor`, transaction `binding`, and `expires_at` per thread review. Relocated from proposed §8.2.1 (PII custody, wrong hook) to §3.9.

<a id="d4-b"></a>
### D4-b · Discussion #4 — normative settlement-gating MUST on payment rails

**§3.6 / §9.3** · @Avouro · **decline (rationale)** · backlog issue #28

Normative settlement-gating MUST on payment rails declined: contradicts §9.2/§9.3 (AEP parallel to the transaction, never gating) and §3.9.12 no-network-consequence disclaimers; would jeopardize §9.4 regulatory classification. Replaced by witnessed-enforcement verification-result fields; settlement-side need routed to open question #4 (network-consumable verification attestation).

<a id="i26"></a>
### I26 · Issue #26 — identity verification in GENERIC chains

**§3.9.2 / §2.6 / §3.6** · @johnhenrypower · **accept-with-modification** · target v1.0.3-final

`verified_identity` sub-wrapper under §3.9 accepted for v1.0.3-final (additive MINOR per §0.2): presence makes Delegation present in a GENERIC chain; absence preserves current §3.9.2 behavior. `OIDC_IDV` appended to the §3.9 protocol enum; `third_party_biometric` added to the §3.6 `authentication_method` enum, joined via `session_id_hash` per §2.7.

Editors additionally fix the §2.6/§3.9.2 self-contradiction surfaced by this review: §2.6 fallback text ("MUST omit the delegation artifact entirely" under GENERIC) is aligned to the §3.9.2 rule ("omitted when `protocol = GENERIC` AND no sub-wrapper is populated"), which already anticipates sub-wrapper-bearing GENERIC chains. Resolves the §2.4 `identity_verifier` actor having no artifact to write to in GENERIC chains.

<a id="i30"></a>
### I30 · Issue #30 — cryptographic architecture review, strengths baseline

**Cross-cutting** · @ankitshah009 · **accept** · target v1.0.3-final

Positive disposition baseline recorded. The strengths enumerated (sequential previous-hash model §4, RFC 8785 JCS §4.1, Ed25519 sealing §4.3, key-compromise revocation, signal classes §2.8, evidence tiers §3.9.12, two-plane model §8.2.1, witness-not-competitor posture, minimal profiles §8.6/§8.7, dispute mapping §6/Appendix C, Discovery scoping §3.1, PCI baseline §8.3) are treated as non-negotiable for v1.0.3-final; the backlog items dispositioned under #31–#40 land without diluting them.

<a id="i31"></a>
### I31 · Issue #31 — multi-parent / DAG topology

**§4** · @ankitshah009 · **defer (v1.0.4 / v1.1.0)**

Optional multi-parent `parent_hashes[]` (DAG-capable) linkage profile deferred. The sequential previous-hash model remains the only chain topology for v1.0.3-final and the Minimal profiles. Fork-join cart, dual-discovery convergence, and post-seal parallel fulfillment/refund flows added to the open-question list for v1.0.4 scoping, with conformance vectors as a precondition for any future profile.

<a id="i32"></a>
### I32 · Issue #32 — low-entropy HMAC privacy, RTBF, selective disclosure

**§2.7 / §8.2.1 / §8** · @ankitshah009 · **accept-with-modification** · target v1.0.3-final

Language fix lands in v1.0.3-final: HMAC digests of low-entropy inputs (IPv4, coarse geo, short address canonicalizations) are normatively described as pseudonymous/correlatable, never anonymous; §8 GDPR/CCPA text aligned so implementers do not ship false "anonymized" claims; raw-IP export to intermediaries requires a documented dispute trigger.

Dual-commitment erasure appendix (chain binds to a commitment of a deletable blob; erasure = key/blob destruction with sealed `erasure` artifact) deferred to v1.0.4. ZK/predicate selective-disclosure profile deferred to v1.1.0.

<a id="i33"></a>
### I33 · Issue #33 — platform co-signature for Attested-tier Discovery attribution

**§3.1 / §2.8** · @ankitshah009 · **accept-with-modification** · target v1.0.3-final

Classification rule made explicit in v1.0.3-final: Discovery fields without a platform co-signature (publisher key, domain key, or `did:web`/well-known platform key over `JCS(discovery_payload)`) are Asserted (C) only and MUST NOT be treated as platform attestation or contribute to Attested-tier attribution. Anti-pattern guidance (extension injection, post-landing discovery rewrite) added to implementer docs.

Cryptographic Discovery→Referral binding (salted hash of the platform signature in the Referral payload) deferred to v1.0.4.

<a id="i34"></a>
### I34 · Issue #34 — restrict network_transaction_id and charge_id export

**§3.6 / §6 / §8.3** · @ankitshah009 · **accept-with-modification** · target v1.0.3-final

Network/PSP transaction identifiers (`network_transaction_id`, `charge_id`, and kin) classified restricted raw-plane by default in v1.0.3-final; the shareable chain plane carries digests (`network_transaction_id_hash` etc.) for integrity. §8.3 gains an explicit MUST: no resolvable network/PSP identifiers to discovery agents or AI platforms; raw values reach networks only via authorized representment channels.

Export disclosure manifest folded into the existing §6.1 exporter-identity open work; optional blinded-token (merchant+PSP HMAC) profile deferred to v1.0.4.

<a id="i35"></a>
### I35 · Issue #35 — carrier oracle gap, proof-of-delivery profile

**§3.7** · @ankitshah009 · **accept-with-modification** · target v1.0.3-final

Signal-class rule lands in v1.0.3-final: merchant-relayed carrier JSON is Asserted (merchant-attested at best), never carrier-Attested; only a carrier/recipient signature earns Attested-by-carrier/recipient. Spec text documents that absence of proof-of-delivery does not imply non-delivery (§3.9.12 spirit).

The Proof-of-Delivery sub-profile (carrier-signed tracking events, recipient-held key confirmation, 3PL attester with key discovery) deferred to v1.0.4; predicate/ZK region-delivery proofs tracked with the deferred profile under #32.

<a id="i36"></a>
### I36 · Issue #36 — key discovery beyond well-known URLs (did:web)

**§4.3 / §3.9.11** · @ankitshah009 · **accept-with-modification** · target v1.0.3-final

`did:web` guidance (alongside the existing well-known URL method) lands in v1.0.3-final docs, including `issuer`/`controller` URI on signed artifacts so archives remain resolvable after domain moves.

The full normative key-discovery methods registry (well-known MUST; `did:web` SHOULD; optional DNSSEC TXT), status-list/OCSP-class revocation-consumption profile, and long-term archive verification story deferred to v1.0.4, tied into the §4.3/§5 external-anchoring direction. Matches the disposition split proposed in the issue.

<a id="i37"></a>
### I37 · Issue #37 — chain-level weighted confidence model

**§2.8 / §3.9.12** · @ankitshah009 · **defer (v1.1.0)**

Chain-level weighted confidence export deferred, and any future model will be informative-only: a normative scoring formula with published weights risks being consumed as outcome assignment, contradicting the §3.9.12 principle that AEP describes verifiability while networks assign outcomes. The specific weight table is not adopted. Revisit at v1.1.0 as a non-normative annex with recomputation test vectors, an Asserted-mass cap, and failed-signature zeroing as proposed.

<a id="i38"></a>
### I38 · Issue #38 — line-item Merkle commitments

**§3.3 / §3.5** · @ankitshah009 · **defer (v1.0.4)**

Optional line-item Merkle commitment profile (`line_items_root` over canonicalized `H(JCS({sku, qty, unit_price, currency, …}))` leaves, partial-disclosure paths for disputed items, merchant-signed quote bound into the root input) deferred to v1.0.4. Compatible with the two-plane model (full line-item bodies custodian-side; root on chain); not required for Minimal profiles. Added to the backlog alongside the topology work under #31.

<a id="i39"></a>
### I39 · Issue #39 — delegation single-use session binding and replay controls

**§3.4 / §3.9 / §3.9.11** · @ankitshah009 · **accept-with-modification** · target v1.0.3-final

Verifier-side replay checks land in §3.9.11 for v1.0.3-final, extending the `request_binding` (§3.9.19) precedent: `jti`/nonce not previously accepted, TTL unexpired, and spend within remaining limit (atomic decrement semantics at merchant/PSP) become normative verification rules; single-use session-bound nonces entangled into delegation signature input RECOMMENDED. Hardware-bound user confirmation (WebAuthn/FIDO2, recorded as Attested) stays RECOMMENDED where the underlying mandate protocol supports it. Rich policy-expression reference (Cedar-class) deferred as a non-normative profile.

Terminal-seal single-use is handled under #40.

<a id="i40"></a>
### I40 · Issue #40 — single-use processing of sealed chain terminals

**§5 / §6** · @ankitshah009 · **accept-with-modification** · target v1.0.3-final

Accepted as a verifier/consumer processing rule, reframed to preserve the §9.2/§9.3 never-gating posture (cf. #4-b): a system consuming AEP evidence MUST NOT accept a given `chain_id` + `sealed_bundle_hash` (or authorization `current_hash`) more than once for the same business effect (capture, payout, representment package id) — a rule on evidence consumption, never a settlement precondition imposed by this spec.

Maximum verifier clock skew defined for nonce/timestamp checks; state-store guidance documented for Minimal Merchant vs PSP profiles; negative conformance vectors (replayed seal rejected) added to the v1.0.3-final release gates.

**Amendment required before this text lands (raised under #22).** As dispositioned, the rule forbids by its letter a refreshed superseding attestation over the same seal — a legitimate pattern for the compact verification summary deferred to v1.0.4. A supersession carve-out MUST be drafted here, keyed to a monotonic attestation sequence, so that v1.0.3-final text does not foreclose v1.0.4 work. The single-use guarantee is preserved per business effect; supersession replaces rather than repeats one.

<a id="i46"></a>
### I46 · Issue #46 — keyid as a first-class TAP signature field ✱

**§3.9.6 / §3.9.11** · @jyothi-acomm-ai · **accept** · target v1.0.3-final

`tap_key_id` promoted from legacy alias to first-class, required when the wrapper is present, for v1.0.3-final. Parse-from-`tap_signature_input` retained as an accepted fallback for one MINOR cycle, mirroring the existing `tap_domain` → `tap_authority` alias mechanics. §3.9.11 rule 3 gains a check that `tap_key_id` equals the `keyid` parameter inside `tap_signature_input` when both are present.

Confirmed against the Visa TAP Merchant Specifications ("Required Message Signature Fields"), which list `keyid` in the minimum set required for an agent to be trusted. Additive MINOR per §0.2 — no re-versioning of existing chains.

Self-filed by an editor-affiliated implementer; disposition recorded by a second editor.

<a id="i47"></a>
### I47 · Issue #47 — primary-source confirmation against Visa TAP Merchant Specifications ✱

**§3.9.13 / §3.9.11 / §3.9.20** · @jyothi-acomm-ai · **accept-with-modification** · target v1.0.3-final

Primary-source confirmation pass against the Visa TAP Merchant Specifications accepted. Items A–E land in v1.0.3-final as additive nullable fields: `consumer_recognition` `kid`/`alg`/`signature`; `payment_container` envelope `nonce`/`kid`/`alg`/`signature` plus `credential_hash` added to the `container_type` enum; `browsing_iou.uri`, `payment_service` and IOU signature fields; `card_metadata.last_four` and `short_description`; optional extracted `id_token_claims`, with `sub` restricted raw-plane by default and a digest on the chain plane per #34.

Item F verification rules land in §3.9.11 (object `nonce` equality with the message-signature nonce, signature base as all fields in the order received excluding `signature`, key selection by `kid` against the well-known JWKS or `witnessed_keys`), extending the verifier-side-rule precedent set under #39. Item G (`browsing_iou.amount_atomic` unit convention) recorded as open pending a live HTTP 402 sandbox vector.

**PROVISIONAL retained** through v1.0.3-final: this pass confirms the published consumer-recognition and payment-container artifacts only, and the VIC Payment Instructions / Signals / tokenization field sets remain access-gated; the flag may drop in v1.0.4 once those are confirmed. §3.9.15 (Mastercard VI) stays PROVISIONAL — no equivalent primary-source pass has been done against Mastercard documentation.

Self-filed by an editor-affiliated implementer; disposition recorded by a second editor.

<a id="i56"></a>
### I56 · Issue #56 — consented supplementary identifiers on consumer_recognition ✱

**§3.9.13 / §2.8 / §8.3** · @jyothi-acomm-ai · **accept-with-modification** · target v1.0.3-final

Optional nullable `additional_identifiers[]` accepted on `consumer_recognition` for v1.0.3-final, carrying `identifier_type`, `issuer`, `value_hash`, `value_ref` and `delivery`. Three constraints are normative rather than advisory:

1. Entries are Asserted (C) and MUST NOT contribute to Attested-tier attribution, consistent with #33 and #35 — the `delivery` field makes the basis for that classification inspectable rather than inferred.
2. Raw values are restricted raw-plane by default with digests on the chain plane, and MUST NOT reach discovery agents or AI platforms via §8.3 exports, per #34.
3. Presence of these fields does not imply network attestation of their contents.

Visa wire names for the identifier set remain to be confirmed against the access-gated TAP Merchant Specifications and will be reported on the issue; the accepted AEP shape does not depend on that outcome.

Self-filed by an editor-affiliated implementer; disposition recorded by a second editor.

<a id="d14-a"></a>
### D14-a · Discussion #14 — independent dispute-handoff profile

**§6 (proposed profile) / §3.6 / §3.9.10** · @shunhe-wang · **defer (v1.0.4)**

Independent dispute-handoff profile — a §6 export profile for a recipient who is neither the producer of the export nor a card network — deferred to v1.0.4 as a candidate, conditional on a named editor, and sequenced behind the export-record work (#3, #16) so that implementations are not built against a specification still under revision.

Motivation accepted: §6.1 defines the `DisputeBundle` as the export "for representment submission", and §6.4's renderers and Appendix C are network-specific, so the §3.6 and §3.9.10 non-card rails have no equivalent channel.

Scoping question carried into v1.0.4, and answered by the proposer on 10 August in favour of a single profile covering both postures, with AEP directing or gating execution in neither (issues #58 and #59 carry the resulting escrowed-posture work): the profile differs by settlement posture — against a finalized, non-escrowed transfer a disposition has no settlement-layer counterpart and relief is declaratory or prospective only, whereas an escrowed or auth-capture posture does (§3.9.10 already projects `scheme: 'auth-capture'` and `settlement_finalized`). Any such profile records settlement facts only; per §9.3 and #4-b and #40, this specification never gates settlement.

Component requirements raised in the same proposal are tracked against their own issues and receive their dispositions there: export signature #3, verification-result fields #15, export record #16, key ordering #17, safe harbour #18.

<a id="d14-b"></a>
### D14-b · Discussion #14 — forum consent as an AEP artifact type

**§3.9 / §9.3 / §9.4** · @shunhe-wang · **decline (rationale)**

Forum consent — agreement to arbitrate, or to a particular decision-maker — declined as an AEP artifact type. The proposer's reading of §3.9 is correct that it records commerce and purchase authority rather than agreement to a dispute forum, but determining what constitutes valid consent to a forum, given by a party with capacity to give it, is an adjudicative question outside this specification's scope per §9.3 ("Not a dispute manager") and the §9.4 posture that AEP evidence informs outcomes and issuer investigations without altering or determining rights.

Implementers requiring the record MAY carry it as a generic signed consent-event record carrying no representation as to enforceability; AEP takes no position on its validity.

Not contested in the proposer's 5 August reply.

<a id="i3"></a>
### I3 · Issue #3 — chain-level fields have no defined integrity mechanism

**§2.7 / §4.1 / §4.3** · @rrrodzilla · **accept-with-modification** · target v1.0.3-final

Accepted in the form the issue prefers: the reference behaviour becomes normative, and the four chain-level fields (`chain_id`, `merchant_id`, `session_id_hash`, `spec_version`) are required inside the hashed `metadata` of every artifact. §2.7 and §4.1 are corrected accordingly.

The claim is a three-way contradiction, confirmed on all legs. §2.7 and §4.1 each assert the fields are "integrity-protected by the chain seal (§2.5) and the bundle signature (§4.3)" while pointing at each other circularly, and **§4.3 defines no bundle signature at all** — it defines a per-artifact `server_signature` only. Meanwhile the published genesis vector carries `merchant_id` and `spec_version` inside `hash_input`, and its `expected_sha256` recomputes correctly over that input: the reference implementation already hashes these fields, so the prose is wrong about its own implementation. `spec_version` selecting the verification rules while sitting outside the hashed envelope is the sharpest form of the defect.

The phrase "bundle signature (§4.3)" is struck wherever it appears, since no such construct exists.

This disposition does **not** resolve the second, cumulative need raised in the thread: a canonical export hash and exporter signature over the complete §6.1 export (verification result, `customerHistory`, `exportedAt`, manifest, `supersedes`). The metadata fix authenticates chain-level fields per artifact; it does not authenticate the export snapshot. That work is scoped under #16 and is a precondition for #16 items 1 and 3. Conformance vector (c), recorded as blocked on the §2.7↔§4.3 definition, is unblocked by this disposition and added to the vector release gate.

<a id="i5"></a>
### I5 · Issue #5 — required Referral IP/UA HMACs conflict with privacy-minimal observers

**§3.2 / §2.7 / §2.8** · @squishy-ctrl · **accept-with-modification** · target v1.0.3-final (scoping) / v1.0.4 (profile)

Corroborated. §3.2 makes `consumer_ip_hash` and `user_agent_hash` required and derives the consumer actor ID as `HMAC-SHA256(consumer_ip, per-merchant-salt)`, while §2.7 caps salt rotation at once per 365 days and requires old salts be retained for verification. Emitting a conformant Referral therefore compels persistence of a long-lived, merchant-scoped pseudonym, which a truncate-and-forget observability layer cannot do.

**For v1.0.3-final:** the specification states plainly that full-profile Referral requires long-lived merchant-scoped pseudonyms, so implementers cannot read the PII-minimal framing as broader than it is. This is drafted together with #32, which lands the normative language that low-entropy HMAC digests are pseudonymous and correlatable rather than anonymous — the same defect approached from the opposite direction. §2.7 gives the explicit HMAC construction only for `session_id_hash` while §3.2 says "HMAC per §2.7" for IP and user agent; that under-specification is corrected in the same edit, since it currently leaves open whether a truncated prefix or an ephemeral salt conforms.

**Deferred to v1.0.4:** the additive privacy-minimal Referral profile answering all six points the issue raises — truncated-prefix input, ephemeral salt, granularity representation, actor-ID derivation, the effect of salt destruction on §2.8 class, and chain completeness. It lands with the Minimal Observer Profile accepted under #6, from the same author, which needs the same answers.

Editors note a hook the issue does not cite: §2.8 already classifies HMAC-derived fields including `consumer_ip_hash` as Deterministic only for verifiers holding the §2.7 salt and Asserted for everyone else, which is where the salt-destruction case attaches.

<a id="i7"></a>
### I7 · Issue #7 — chain-only conformance without an Evidence Custodian

**§0.1 / §6.4 / §8.2.1 / §8.6** · @squishy-ctrl · **accept-with-modification** · target v1.0.3-final

The ambiguity is real: §0.1 defines a conformant chain by §2, §3, §4 and §3.9 and does not mention §8.2.1, while §8.2.1 states evidence "is held in two planes" and imposes normative custodian obligations, and §8.6 says nothing about whether a Minimal Merchant must operate or delegate the raw plane. Two readings survive the text.

Reading 2 is the intended one and is made explicit: the proposed four-claim conformance structure (core-chain producer / exporter / custodian / network renderer) is adopted in §0.1, and absence of the raw plane does not affect chain validity but MUST be disclosed. This is a clarification, not a new mechanism, and it forecloses the perverse outcome of retaining PII purely to avoid conformance doubt.

Editors additionally add a SHOULD that the Evidence Custodian be operationally independent of the party whose disputes it serves, with self-custody permitted but disclosed. That text is new and is not covered by any other disposition.

Compatible with #30, which locks §8.2.1 and §8.6/§8.7 as non-negotiable: this scopes those sections rather than weakening them.

<a id="i8"></a>
### I8 · Issue #8 — stale "round not yet open" placeholder

**COMMENT-ROUND.md** · @squishy-ctrl · **accept**

Editorial. The disposition log retained a "(round not yet open)" placeholder after the window had opened. Actioned by PR #10 and the issue is closed; recorded here because the log carried no disposition for it, which left an actioned in-window submission looking unanswered against the release gate.

No spec change.

<a id="i15"></a>
### I15 · Issue #15 — §5 verification result defines no fields for values §5 and §6.2 require ✱

**§5 / §6.1 / §6.2** · @jyothi-acomm-ai · **accept** · target v1.0.3-final

Accepted as filed. `verified_at`, a `full` / `cached` mode discriminator, and a witnessed-keys indicator are added to the §5 Verification Result Schema and echoed through the §6.1 `verification` member. Three additive nullable fields; no semantics change and no re-versioning of existing chains under §0.2.

The submitter labelled this Minor; the editors record it as more than that. The requirement is stated as a MUST in three separate places — §5, the §3.9.11 preamble, and §4.3 all require recording that witnessed rather than live keys were used, and §6.2 requires a cached result to carry `verified_at` — against a schema that defines none of them. A MUST with no field to satisfy it guarantees divergent private extensions.

This is also a dependency of #60, whose security remedy requires the verification result to distinguish "no rule existed" from "checked and passed"; the third verification state lands with this schema work. The witnessed-keys indicator is drafted consistently with #36 and #47, both of which reference `witnessed_keys` in key selection.

The issue's own text cross-references "#5"; that appears to be a slip, as #5 is the Referral HMAC issue. The schema-edit partner is #16.

Self-filed by an editor-affiliated implementer; disposition recorded by a second editor.

<a id="i16"></a>
### I16 · Issue #16 — export record: exporter identity, disclosure manifest, supersession ✱

**§6.1** · @jyothi-acomm-ai · **accept-with-modification** · target v1.0.3-final

Split three ways.

**Items 1 and 3 accepted for v1.0.3-final.** Exporter identity and a `supersedes` reference land with #3's export-hash work, on which they depend. The gap is real: §6.1 records `exportedAt` and nothing else about export provenance, while §6.2 defines the verification class recipient-relatively ("a self-verified, unanchored bundle is Asserted as a whole"), so a recipient cannot compute that class without knowing who exported and whether exporter and verifier are the same party. `merchantContext` carries merchant identity, which under §8.2.1 is a different party from the exporter or custodian.

**Item 2, the per-artifact disclosure manifest, defers to v1.0.4.** The manifest as scoped in the issue is keyed to §2.2 expected counts, and that anchor is already shown insufficient in the thread: counts are aggregate minimums, exclude refund (§3.10) and annotation (§6.5) artifacts, interact with late fulfillment, and apply only to the Full profile under §8.6 — so raw count-matching misleads in both directions. The richer item-level, profile-driven registry proposed in reply (content digests, controlled reason codes, asserting actor, versioned expected-item registry identified by digest) is materially larger than the issue proposes and cannot be designed inside the final window without risking the gate.

The export disclosure manifest referred here from #34 lands in that v1.0.4 work. #14-a is sequenced behind items 1 and 3.

Self-filed by an editor-affiliated implementer; disposition recorded by a second editor.

<a id="i17"></a>
### I17 · Issue #17 — §4.1 code-point key ordering contradicts RFC 8785 and vector (h) ✱

**§4.1 / §6.3** · @jyothi-acomm-ai · **accept** · target v1.0.3-final

Erratum, accepted as filed and treated as time-critical. §4.1's normative canonicalization rule says "lexicographic key sort by Unicode code point" while the same section normatively incorporates RFC 8785, which sorts by UTF-16 code unit — as does `vectors/README.md` ("member names sort by UTF-16 code unit at every nesting level"). §4.1 is corrected to UTF-16 code-unit ordering.

The orders diverge only for non-BMP member names, where a leading surrogate (D800–DBFF) sorts below the BMP range E000–FFFF. No case in `h-canonicalization.json` uses a non-BMP member name — the vector named `key-order-utf16-code-units` tests `{b, A, a, B}`, which orders identically under both rules. An implementation faithful to the prose therefore passes every published conformance test and still computes different hashes than one faithful to the RFC. A non-BMP member-name case is added to the vector set as a release-gate item, since §6.3 makes vectors normative for conformance.

The deadline argument is accepted: correcting prose to match an already-normative incorporated RFC is an erratum now, but once v1.0.3-final ships, §0.2 classifies changing the canonical-JSON specification as MAJOR. The window for a cheap fix closes with this release.

Consistent with #30, which records RFC 8785 canonicalization as a non-negotiable strength — the fix moves the prose toward the RFC, not the reverse.

Self-filed by an editor-affiliated implementer; disposition recorded by a second editor.

<a id="i18"></a>
### I18 · Issue #18 — §6.5 safe harbour: non-attribution rule or evidence exclusion? ✱

**§6.5** · @jyothi-acomm-ai · **accept-with-modification** · target v1.0.3-final

> **Editors' note — this disposition requires maintainer confirmation before v1.0.3-final.** It changes a conformance term on governance rather than technical grounds, and no agent platform — the affected actor class from whom comment was explicitly solicited — responded during the window.

The issue raises two separable questions and proposes no resolution. They are dispositioned separately.

**Construct (accepted).** The evidentiary-use restriction is relocated out of the conformance clause. §6.5 currently reads "Conformant use of this specification is conditioned on this term," making a restriction on what a forum may consider into a technical conformance criterion — something no implementation can verify and no verifier can check. That sits poorly with the GOVERNANCE.md trajectory toward a neutral standards organization, and it cuts against the same principle already applied in #4-b and #40: AEP describes verifiability and does not determine outcomes. The non-attribution rule in the preceding sentence — that Inferred or Asserted platform attribution MUST be labelled "not verified by the named platform" and MUST NOT be presented as deterministic fact absent a countersignature — is retained as normative and is unaffected.

**Scope (deferred to v1.0.4).** Whether the restriction reaches too far — to platform-authored, platform-signed artifacts in the proceeding where they are most probative — is left open pending platform input, which the window did not produce. §8.7 extends the term to all platform-emitted artifacts, so any narrowing must be drafted against both sections.

Self-filed by an editor-affiliated implementer; disposition recorded by a second editor.

<a id="i20"></a>
### I20 · Issue #20 — §3.7 cannot be satisfied by reserved time-and-place commerce

**§3.7 / §3.5 / §3.10** · @HemmaBo-se · **accept-with-modification** · target v1.0.3-final

Corroborated, and the problem is wider than the issue states. §3.7 hard-requires `carrier`, `tracking_number`, `delivery_address_hash` and `delivery_address_match` alongside a shipping-shaped `status` enum. Lodging, vehicle rental and appointment commerce have none of these, so a merchant must either omit fulfillment entirely or write placeholder values into fields §2.8 classifies as Deterministic — which is the worse outcome, because §6 then exports fabricated values under a class that asserts they are independently recomputable.

**For v1.0.3-final:** §3.7's shipping-shaped required fields become nullable for non-shipped fulfillment, a `consumed` status member is added, and normative text states that placeholder values MUST NOT be written into fields carrying the Deterministic class. `delivery_address_match` is defined as null rather than false where no delivery address exists, so the representment signal is absent rather than negative.

**Deferred to v1.0.4:** the full variant keyed off a cart field, with `location_hash` compared against a cart-committed location and a consumption-window status. Editors note the shipping assumption begins one artifact earlier than the issue identifies — §3.5 Cart also requires `shipping_address_hash` — so the v1.0.4 variant must cover Cart and Fulfillment together. §3.10's `reason` enum likewise has no cancellation or no-show member and is scoped into the same work. This lands with the Proof-of-Delivery sub-profile deferred under #35.

Minor citation slips noted for the record: the issue cites §3.5 as Policy (it is Cart; Policy is §3.4).

<a id="i21"></a>
### I21 · Issue #21 — merchant-domain authentication is wrapper-dependent, not chain-level

**§3.9.11 / §2.8** · @HemmaBo-se · **accept-with-modification** · target v1.0.3-final

The editors adopt the split the submitter themselves proposed in the thread on 10 August.

**Option 2 accepted for v1.0.3-final.** §2.8 gains a merchant-authenticity classification rule: merchant authenticity is **Asserted** in any wrapper composition carrying no merchant-controlled key, and **Attested** only where a merchant-key-bearing wrapper is present. The class MUST survive §6 export, consistent with §2.8's existing preservation requirement. The gap is confirmed — of the four key-discovery paths in §3.9.11 rule 1, only UCP resolves to a merchant-controlled key (`merchant_public_keys_url`, JWKS at `/.well-known/ucp`); ACP uses a pre-shared HMAC, AP2 an inline `cnf.jwk`, and x402 a facilitator signer recovered from the EIP-712 signature. The S-tier-eligible composition named in §2.6 (TAP + AP2 + x402) therefore contains no assertion from a merchant key, and the meaning of a passing verification result changes with wrapper set, invisibly to the recipient.

**Option 1 deferred to v1.0.4.** A chain-level merchant-domain assertion over `merchant_id` + `chain_id` from a merchant-domain-discoverable key is a new normative primitive and belongs with the key-discovery methods registry deferred under #36.

Editors record the nuance raised in review: §4.3 requires every implementer to publish an Ed25519 verification key and every artifact to carry a `server_signature`, but nothing binds that key to `merchant_id` or to the merchant's domain, and §2.7's rejection of an unrecognized `merchant_id` is a recipient-side allowlist rather than cryptographic binding. The claim survives that objection.

The settlement-role distinction raised in the thread — that merchant identity and settlement role are separate facts, and an on-chain sender may not be the payee in relay flows — is recorded for the v1.0.4 scope rather than dispositioned here.

<a id="i23"></a>
### I23 · Issue #23 — §3.9.18 `wba_tag` MUST value rejects conformant signatures

**§3.9.18 / §3.9.11** · @johnhenrypower · **accept** · target v1.0.3-final

`wba_tag` is specified as "MUST include `web-bot-auth`", but the RFC 9421 `tag` parameter identifies interaction type and each acceptance framework registers its own value. The requirement is loosened: `wba_tag` MUST be present and MUST match the value registered by the framework identified in `wba_signature_agent`; the literal `web-bot-auth` becomes the expected value only where `wba_signature_agent` is absent.

The strongest support is internal rather than external. §3.9.6 already replaced a free-form `tap_operation` with the framework-specific enum `agent-browser-auth | agent-payer-auth`, so the spec already recognises framework-specific tag values in a sibling wrapper — the hardcoded literal in §3.9.18 is an inconsistency inside the specification, independent of any vendor documentation. The cited Mastercard tag registry values are external and not vendored in this repository; the loosening does not depend on them.

**Correction carried into the spec edit:** the issue cites §3.9.11 rule 10 in both places. Web Bot Auth is **rule 11**; rule 10 is Skyfire KYA Pay, and §3.9.18's own body already says rule 11.

Lands as a single §3.9.18 edit together with #25.

<a id="i24"></a>
### I24 · Issue #24 — §3.9.15 `credential_chain.typ` cannot express Verifiable Intent L1

**§3.9.15** · @johnhenrypower · **accept** · target v1.0.3-final

The `typ` enum is extended to `sd+jwt | kb-sd-jwt | kb-sd-jwt+kb`.

Accepted on the internal contradiction alone, without reliance on the external source: `layer` admits `L1`, and §3.9.15's own prose together with §3.9.11 rule 8 state that L1 verifies against the credential provider's published keys while L2 verifies against the user key bound via L1's `cnf` — that is, the spec already says L1 is issuer-signed and not key-bound. Admitting only key-bound `typ` values therefore makes an L1 entry declarable but unrepresentable, and an Immediate-mode chain (L1 + L2) cannot be expressed at all.

§3.9.15 remains **PROVISIONAL** through v1.0.3-final per #47, which absorbs residual risk while no primary-source pass has been done against Mastercard documentation. This issue is recorded as the first concrete finding from that side and as evidence for scheduling the pass in v1.0.4.

Editors additionally note a defect the issue does not raise: `typ` is overloaded, serving as both the JOSE header type and the Immediate-versus-Autonomous mode discriminator, alongside a separate `mode` field that already carries that meaning. Adding `sd+jwt` fixes the contradiction but leaves the overload. Decomposing `typ` to the header type alone is deferred to the v1.0.4 primary-source pass.

<a id="i25"></a>
### I25 · Issue #25 — §3.9.18 has no field for the Signature-Input nonce

**§3.9.18 / §3.9.11** · @johnhenrypower · **accept** · target v1.0.3-final · *security (replay)*

`wba_nonce` is added as a nullable field. The wrapper breaks out every other Signature-Input parameter — `wba_keyid`, `wba_alg`, `wba_tag`, `wba_created`, `wba_expires`, `wba_covered_components` — and omits only the nonce. The value survives inside `wba_signature_input` and is signature-covered, but cannot be asserted, queried or uniqueness-checked, so the chain cannot demonstrate that the replay control was applied.

The companion verification rule is **not** written freestanding. Nonce-not-previously-accepted and TTL checks are already normative §3.9.11 rules under #39, and maximum clock skew and state-store guidance per profile are defined under #40. §3.9.18 references those rather than imposing new verifier state, which keeps the single-use processing model in one place.

Internal support: §3.9.6 records that TAP dropped `tap_session_id` because "TAP is per-request stateless; nonce IS the session identifier" — the specification already treats the nonce as identity-bearing elsewhere.

Lands as a single §3.9.18 edit together with #23.

<a id="i42"></a>
### I42 · Issue #42 — `pre_execution_attestation` binding omits verified surface-state

**§3.9.N / §2.8 / §3.9.11** · @rabet · **accept-with-modification** · target v1.0.3-final (field) / v1.0.4 (rule)

**The field is accepted into v1.0.3-final**, because §3.9.N is being drafted now under #27 and this is the cheapest moment to absorb it. Optional `surface_binding {origin, content_digest, method}` is added inside the accepted `binding`, populated when `issuer_relationship ∈ {independent, psp}`. It is purely additive and alters neither `mode`, the anchor requirement, nor the expiry check.

The gap is genuine and specific to the accepted design rather than a competing proposal. #4-a grants an Attested upgrade on the strength of an independent issuer plus a verified anchor, and what an independent issuer uniquely observes is a live merchant surface — yet the accepted binding pins cart contents and time, carrying no evidence of that observation. A validly signed, correctly anchored attestation can therefore be presented at authorization against a surface materially different from the one verified at issuance, and the recipient cannot see the difference.

**The §3.9.11 re-derivation rule and the `method` registry defer to v1.0.4.** A `content_digest` over a mutable commercial surface is only recipient-re-derivable if `method` is normatively specified, and a live surface carrying prices, availability and session-scoped markup will not re-derive stably at dispute time months later. The registry has the same shape and the same versioning problem as the key-discovery registry deferred under #36.

**Drafting instruction for #27:** the accepted §2.8 text is written as a claim-level upgrade ("upgrades the temporal-precedence claim"), and this introduces a second independently-classified claim inside one sub-wrapper. §2.8 already states that classification is per-field, so the two are compatible — but whoever drafts that paragraph must decide the claim decomposition once, covering both, rather than patching it afterwards. Recorded as in-scope input to #27 open questions 4 and 5.

<a id="i43"></a>
### I43 · Issue #43 — chain artifacts carry no observation-source classification

**§2.8 / §3.7 / §6.1** · @rabet · **accept-with-modification** · target v1.0.3-final (narrow) / v1.0.4 (general)

Largely pre-empted by #35, which already lands the fulfillment rule for v1.0.3-final: merchant-relayed carrier JSON is Asserted, and only a carrier or recipient signature earns Attested-by-carrier/recipient.

**The residual delta is accepted for v1.0.3-final:** an explicit field making the observation basis inspectable rather than inferred from signature composition. This follows #56 directly, which accepted a `delivery` field for precisely that reason, and #33 before it. §2.7's `signing_actor` records who signed, never who observed, and §3.7 fixes the signing actors without distinguishing merchant-own-logistics from independent observation.

**Generalization deferred to v1.0.4/v1.1.0.** `observed_by` across all chain artifacts touches every artifact schema, Appendix B and the vector set, and is scoped with the 3PL-attester profile already deferred under #35.

**Correction recorded:** the §2.8 `issuer_relationship` rule the issue cites as existing is the accepted-but-unlanded #27 design, not current rc.2 text. The argument survives, but the spec edit must not describe it as existing.

Editors note a precedent the issue does not cite and which strengthens it: §4.3 already classifies by observer independence at bundle granularity — an unanchored bundle verified only by the exporting party is Asserted as a whole. This pushes an existing principle down to artifact granularity rather than introducing a new one.

<a id="i44"></a>
### I44 · Issue #44 — §9.4 EU/US posture leaves LATAM unrepresented

**§9.4** · @Marlonm0987 · **decline in part / defer in part** · target v1.0.3-final (note only)

**The premise is corrected and that part is declined.** The specification does not anchor principal identity on eIDAS, EUDI or W3C Verifiable Credentials — none of those terms appears in §9.4 or anywhere else in the document. §9.4's EU subsection is GDPR-only (EDPB Guidelines 01/2025, CJEU C-413/23 P, Art. 6(1)(f), the EU-US DPF, SCCs, Art. 49(1)(e)), and the sole VC reference in the specification is a glossary note recording that AP2 uses SD-JWT-VC and that VC integration is roadmap rather than the current envelope class. No §9.4 correction is owed on that ground.

**The proposed target field does not exist.** The only `authority` in the specification is `tap_authority`, the RFC 9421 `@authority` covered component — an HTTP host checked against the merchant domain on file. Attaching an issuing legal jurisdiction to it would be a category error.

**The underlying need is real and is accepted.** Nothing in the specification lets a chain distinguish "identity attested under a framework this verifier does not recognise" from "identity unverified"; both collapse to Asserted under §2.8. That is routed to the `verified_identity` sub-wrapper accepted under #26 as a jurisdiction or framework qualifier — that wrapper is being drafted for v1.0.3-final and is the correct hook. A full framework registry defers to v1.0.4 alongside #36.

**For v1.0.3-final,** §9.4 gains a one-line note that its regulatory posture describes a US/EU deployment of the reference implementation and is not a portable exemption. Per §9.3 and the reasoning in #14-b, AEP takes no position on the legal sufficiency of any national registry; implementers MAY carry such a record as a signed claim carrying no representation as to enforceability.

<a id="i49"></a>
### I49 · Issue #49 — Minimal Merchant Profile omits the policy artifact §3.4 makes mandatory

**§8.6 / §3.4** · @Trusteedxyz · **accept** · target v1.0.3-final

A genuine normative contradiction between two sections of the same release candidate. §3.4 states "Implementors MUST never authorize a transaction without a preceding policy artifact." §8.6 enumerates only `intent`, `cart` and `authorization` (plus post-seal `fulfillment` and `refund`) and declares such a chain `chain_complete: true`. `policy` appears neither in the enumeration nor in the exemption sentence, whose stated rationale — that the exempted artifacts "are emitted by agent platforms and PSPs" — cannot apply to a merchant-emitted artifact.

`policy` is added to the Minimal Merchant Profile artifact set. Appendix B's `expectedArtifactCount` already counts it in the Full profile (5 for `direct_web`: intent, policy, cart, authorization, fulfillment), confirming the omission is specific to §8.6. §8.6 is new in rc.2, so no deployed base is disturbed.

Editors additionally adopt the submitter's underlying point that auto-generated, absent and evaluated policy artifacts are three different facts. The §3.4 pilot auto-generation path is required to be distinguishable rather than relying on the de-facto and unenforced `policy_engine_vendor` / `policy_version` convention. That single edit is the primary disposition for this cluster and materially reduces #50, #51 and #52 — see the cluster note below.

<a id="i50"></a>
### I50 · Issue #50 — required `risk_score` forces deterministic engines to attest a fabricated value

**§3.4 / §2.8** · @Trusteedxyz · **accept-with-modification** · target v1.0.3-final

`risk_score` becomes optional and nullable. A deterministic rule engine produces no score, and its only conformant options today are to emit `0` — indistinguishable from a computed zero — or to synthesize a number, in a field §2.8 classifies as Attested ("model output attested by its producer"). The rc.2 reclassification from Deterministic to Attested conceded the provenance point but left unaddressed the case where the producer has no such value at all.

Making the field nullable is an internal-consistency fix rather than a new design: §3.6's `fraud_score` carries the same signal class and is **already optional**, so the same value is required in one artifact and optional in another.

The companion `risk_score_basis` enum proposed as the alternative form defers to v1.0.4 with the §3.4 growth policy below. Precedent for the accepted direction is #33, #35 and #56, all of which make the basis for a classification inspectable rather than inferred. Distinct from #37, which declined a scoring *model* rather than field provenance.

<a id="i51"></a>
### I51 · Issue #51 — policy artifact cannot distinguish an evaluated approval from a degraded one

**§3.4** · @Trusteedxyz · **accept-with-modification** · target v1.0.3-final

Optional `enforcement_result` accepted, descriptive only and carrying no gating semantics. `outcome: "approved"` currently spans at least six operationally distinct paths — all rules evaluated with none blocking, observation-only mode, allowlist or kill-switch override, fail-open on dependency failure, no applicable rules, and the §3.4 pilot auto-generation path — and a control that silently degrades to fail-open keeps emitting approvals byte-identical to evaluated ones. The error runs one way, toward over-crediting the merchant.

The submitter's framing is written against #4-b and holds: the field records what happened and imposes nothing on any settlement path, consistent with §9.2/§9.3.

The permitted value set is pinned during drafting rather than adopted verbatim here; in particular the meaning of `enforced` when `outcome` is `approved` needs to be stated precisely. What decides this for v1.0.3-final rather than v1.0.4 is the pilot path: the evidence corpus fills with indistinguishable approvals from day one, and the ambiguity is not retroactively repairable.

<a id="i52"></a>
### I52 · Issue #52 — `rules_triggered` cannot express which controls ran and did not fire

**§3.4** · @Trusteedxyz · **defer (v1.0.4)**

Corroborated — `rules_triggered` is defined as "rule identifiers that fired", and an empty array spans full evaluation with no match, no rules configured, and no engine at all. "Nine controls evaluated, none matched" is the statement with evidentiary weight for a defending merchant and cannot be expressed.

Deferred nonetheless, on a cost the issue does not address: a full evaluated-rule set can be large, and it discloses merchant rule-set composition inside an artifact that is exported to networks and platforms. That runs against the export-restriction posture adopted in #34 and §8.3 and needs scoping rather than a fast additive landing.

Two constraints on the deferred work. The proposed "in precedence order" wording is superseded by #54, which establishes that no ordering contract exists on this field. And an `enforcement_result` value of `not_applicable` under #51 resolves part of the empty-array ambiguity without a new array, which should be settled before a second field is added.

Dispositioned together with #54 as a single coherent treatment of `rules_triggered`.

<a id="i53"></a>
### I53 · Issue #53 — `escalated` collapses merchant approval and buyer confirmation

**§3.4 / §3.6** · @Trusteedxyz · **accept** · target v1.0.3-final

Optional `escalation_target: "merchant" | "consumer"` accepted, meaningful when `outcome == "escalated"`. Merchant-side human approval and out-of-band buyer confirmation are different facts, and only the second is evidence about the buyer — which is precisely the contested fact in the agentic dispute class, where a cardholder denies a purchase their own agent made. §3.4 carries both `outcome: "escalated"` and a redundant `escalated` boolean, neither with a target.

The smallest edit in this cluster and the one most directly aimed at the dispute class the specification exists to serve. Precedent for additive enum extension at v1.0.3-final is #26, which extended the §3.6 `authentication_method` enum.

**One correction for the submitter:** the claim that the fact is unrecoverable from any other artifact is too strong. §3.6's `authentication_method` enum (`3ds2_challenge`, `passkey_delegated_auth`, `ap2_user_mandate` and others) would frequently surface a buyer-confirmation step-up. What is genuinely unrecoverable is the *linkage* between the policy escalation and the authentication that answered it, and the field is justified on that basis rather than on absence.

<a id="i54"></a>
### I54 · Issue #54 — `rules_triggered` has no ordering contract

**§3.4** · @Trusteedxyz · **accept** · target v1.0.3-final

Accepted in the minimum viable form, which is smaller than the issue proposes: §3.4 gains a normative sentence stating that `rules_triggered` carries **no** ordering semantics and that consumers MUST NOT infer the deciding rule from array position. Optional `deciding_rule` is accepted alongside it.

This is an interoperability defect rather than a missing capability. Nothing requires producers to order the array and nothing stops consumers reading position 0 as decisive, so two conformant implementations can attribute the same block to different rules and both are correct. RFC 8785 governs member-name ordering, not array element order, so canonicalization does not settle it. The reported production case — alphabetical ordering misattributing a store-level emergency stop, invisible in the emitted evidence — is exactly the failure the sentence forecloses.

Testable in the §6.3 vector set, which is already a release gate; the issue correctly observes that the defect is untestable under the current text because there is no declared ordering to test against.

Dispositioned together with #52.

**Cluster note (#49–#54).** These six are one author, one artifact, one filing date, and are not six independent items. #49 and #50 correct existing normative text and stand alone. #51, #52, #53 and #54 propose four new optional fields on a payload that currently has nine — a roughly 44% expansion of §3.4 in a MINOR release — and are dispositioned as one batch under a stated policy: three land in v1.0.3-final (`enforcement_result`, `escalation_target`, `deciding_rule`), one defers. The §3.4 pilot auto-generation path is the common root of #49, #50, #51 and #52, and making that artifact distinguishable is the single edit that most reduces the cluster.

<a id="i58"></a>
### I58 · Issue #58 — auth-capture is named but lacks identity, verification rules and outcomes

**§2.5 / §3.6 / §3.9.10 / §3.9.11** · @shunhe-wang · **defer (v1.0.4)**, one element pulled forward · *security*

All four gaps corroborated. `auth-capture` occurs exactly once in the specification — as a value in the §3.9.10 scheme enum — with no field set, no held-payment identifier (x402 offers only `transaction_hash`, defined as the settlement hash "if finalized", therefore absent while held), no verification rule (rule 7 fires only for `scheme === "exact"`), and no post-seal artifact for a later capture, void, reclaim or failure (§2.5's post-seal list is closed, and §3.10's `reason` enum and required `charge_ref` do not fit a held x402 payment).

**Deferred to v1.0.4, consistent with #14-a**, which already named this issue as carrying the escrowed-posture work and whose deferral the proposer accepted. A normative profile plus a new artifact type plus §2.5, §2.7 and Appendix B amendments plus six conformance vectors is not an additive MINOR edit and cannot be drafted safely inside the final window. The post-seal settlement-outcome artifact is scoped against #31 rather than separately, since post-seal lifecycle events are the fork-join case that disposition defers; #40 constrains how a capture event may be consumed; and per §9.3, #4-b and #40 any such artifact records settlement facts only.

**Gap 3 is pulled forward into v1.0.3-final** and lands with #60: an unsupported `(scheme, network)` combination MUST return `unverifiable` rather than silently passing. That is a security-relevant silent pass identical in shape to #60's, and the two share one fix.

**Editors additionally record an internal inconsistency the issue does not raise:** §2.5 states in one place that "post-seal, only fulfillment artifacts may be appended" and in another lists three permitted post-seal types (`fulfillment`, `refund`, and annotation/rebuttal records per §6.5). That contradiction is folded into whatever §2.5 edit results and is corrected in v1.0.3-final regardless of the deferral above.

The cited x402 Foundation scheme documents are external and not vendored; the AEP-side gaps stand independently of them.

<a id="i59"></a>
### I59 · Issue #59 — auth-capture settlement timing permanently depresses the evidence tier

**§3.9.12 / §5 / §6.1** · @shunhe-wang · **accept-with-modification** · target v1.0.3-final (narrow) / v1.1.0 (general)

**The narrow fix lands in v1.0.3-final.** §3.9.12's S-tier route requires "on-chain `x402.transaction_hash` (finalized)" alongside AP2-Direct and session binding — conflating an authorization-layer requirement with a settlement-layer fact inside a tier the specification itself describes as informative for adjudication. That bullet is made scheme-neutral at the authorization layer: verified scheme-specific authorization under §3.9.11, plus AP2-Direct, plus session binding. Two transactions with identical delegation evidence should not receive permanently different tiers by settlement scheme alone, particularly when §6.1 exports `evidenceTier` and a settlement-mechanics artifact therefore reaches the reviewer inside the number.

The permanence is confirmed: §3.9.12 states the ranking "may be recomputed from any archived delegation artifact", and §2.8 confirms the tier derives from the delegation artifact's signal composition, which is pre-seal and immutable.

**Items 2 and 3 defer to v1.1.0.** Reporting settlement posture as an export-time result, and recomputing an overall tier from the current signed export including verified post-seal events, is exactly the extension §2.8 already flags for a future revision ("future AEP revisions MAY extend the tier calculation across the full chain"). It also lands inside #37's deferral of chain-level confidence scoring to v1.1.0 as informative-only, and depends on the outcome artifact deferred under #58 and the export record under #16 — neither of which exists.

<a id="i60"></a>
### I60 · Issue #60 — x402 signature verification: general MUST and rule 7 diverge

**§3.9.11 / §3.9.10** · @saishav7 · **accept-with-modification** · target v1.0.3-final · *security*

The highest-priority item of the late-window submissions. §3.9.11 rule 1 states that every non-null sub-wrapper's signatures MUST verify against keys discovered via that sub-wrapper's canonical key-discovery path, and gives x402's path as EIP-712 recovery; rule 7 then fires only when `scheme === "exact"` AND `network` starts with `"eip155:"`. Both readings are textually defensible and nothing in the specification adjudicates. Under the narrow reading a record with any other network identifier is not signature-verified at all, with no error and no warning; under the broad reading the requirement is unsatisfiable on non-EVM chains where EIP-712 recovery does not exist.

**Two normative changes land in v1.0.3-final.**

1. **The reading is adjudicated.** Rule 7 is the complete specification of x402 signature verification in v1.0.x; sub-wrappers outside its scope — non-EVM chains, and the `upto` and `batch-settlement` schemes — have no verification rule defined, and the specification says so rather than leaving it implied.
2. **The verification result distinguishes "no rule existed" from "checked and passed."** `protocol_envelopes_verified[]` carries a two-state `verified` boolean today, so a recipient genuinely cannot tell an unverified envelope from a verified one. A third state lands with the §5 schema work under #15, which is a hard dependency: the security remedy here is unimplementable without it. The same fix serves #58's gap 3.

**`network` normalization is split.** An alias table for known non-CAIP-2 identifiers is added following the precedent already set in §3.9.10, where legacy v1 header names MUST alias to their v2 equivalents, with CAIP-2 RECOMMENDED for the field. Full normalization to canonical chain identity, and the treatment of non-conformant `asset` and `payTo` values, defer to v1.0.4 with the key-discovery methods registry under #36.

CAIP-2 is confirmed to appear only in schema comments and the glossary, never as a normative requirement, and the field is typed as an unconstrained string. The external index counts cited in the issue are not vendored and were not verified; they bear on urgency, not on the divergence, which is provable from the specification text alone.

<a id="d6"></a>
### D6 · Discussion #6 — Minimal Observer Profile for merchant-observed Referral

**§8.8 (new) / §3.9.19 / §2.1** · @squishy-ctrl · **accept-with-modification** · target v1.0.3-final

Recorded as the editors stated in-thread on 24 July. A new **§8.8 Minimal Observer Profile** is added for a merchant-authorized observability layer that witnesses inbound Referral and request-binding but operates no cart, authorization or platform systems: Referral as the primary artifact, optional `request_binding`, no Discovery, and one new `binding_source` value `merchant_authorized_observer` with a companion authorizing-merchant field. The observer signs with its own key as `signing_actor`. Standalone observer chains are valid but MUST be profile-labeled on export.

The gap is confirmed — §8.6 reduces to `intent`, `cart` and `authorization` and states that referral and delegation "are emitted by agent platforms and PSPs when available", while §3.9.19's `binding_source` enum admits only `aep_gateway | merchant | psp`, leaving an authorized third party with no value to assert. §3.1 confirms Discovery can be populated only by the AI platform itself.

Three constraints raised in the second editor review are normative in the drafting:

1. §2.7 requires `captured_at` to be monotonically non-decreasing across the chain, which breaks under a skewed observer clock. The merchant stamps `captured_at` at ingestion.
2. §2.1 fixes Referral at position 2 and §2.3 forbids retro-insertion, so a late observer Referral can only append out of layer order — the profile states what is permitted rather than leaving it to implementers.
3. §2.5's post-seal appends exclude Referral entirely, and `request_binding` is sealed inside the delegation artifact, so its deadline is delegation-construction time rather than seal time.

The authorizing-merchant field is Asserted absent merchant countersignature and MUST NOT reach Attested tier, per #33, #35 and #56.

**Editorial fix carried in the same release:** §2.1 layer 7 still reads "post-seal, only fulfillment artifacts may be appended", contradicting §2.5's three permitted post-seal types. This is the same contradiction independently surfaced under #58 and is corrected once.

<a id="d11"></a>
### D11 · Discussion #11 — cross-node evidence handoff for multi-actor chains

**§2.5 / §4.3 / §8.4** · @juanferrub · **accept-with-modification** · target v1.0.3-final (informative) / v1.0.4 (normative)

Recorded as the editors stated in-thread on 29 July, and the proposer confirmed the direction.

**For v1.0.3-final:** an informative "Chain custody and contribution model" subsection under §2.5, plus a non-normative sequence diagram, stating the intended model — **one chain writer, many signed contributors**, with contributions riding existing protocol calls and webhooks. Custodian-coordinated chains are explicitly not an intended pattern; the §8.2.1 custodian is a privacy-plane role, not a coordination role. The subsection also reconciles §3.3/§3.5/§3.7, which name the merchant server as wrapper signer, against §4.3's "the implementing party", which is the wording that makes the PSP-as-writer case ambiguous. Zero normative surface.

**Deferred to v1.0.4/v1.1.0:** a normative handoff or transport-binding envelope, explicitly because it interacts with open questions 4 and 5 and therefore with #28.

§8.4 already permits out-of-order pre-seal ingestion and requires reordering into `seq` order before sealing, so the mechanical basis for multi-actor contribution exists; what was missing is the statement of who writes. Complements #14-a, which addresses handoff after export rather than custody within the chain.

<a id="d12-a"></a>
### D12-a · Discussion #12 — split rc.2 into a transaction-neutral core plus profiles

**§0.2 / §4.1 / Appendix B** · @timaxorum · **defer (v1.1.0)**

Recorded as stated in-thread on 28 July; the proposer accepted all five dispositions in this discussion.

Deferred because extraction sits at or near MAJOR under §0.2 rather than being additive: the artifact-type enum is part of the §4.1 hash input, and Appendix B's expected counts are keyed to the consumer journey. The current specification would become the consumer-checkout profile. This is the parent problem behind #31 and #38, both already deferred, and it feeds open question 3.

<a id="d12-b"></a>
### D12-b · Discussion #12 — organization slot for B2B principals

**§2.4 / §3.9** · @timaxorum · **accept-with-modification** · target v1.0.3-final / v1.0.4

`org_ref { scheme, id, attestation? }` accepted, Asserted by default. §2.4's actor list carries no organization actor, so a chain cannot record that an agent acted for a named company — confirmed, and `secure_corporate` is an authentication-method value rather than an organization model. Placement is sequenced behind #3, and slips to v1.0.4 if that work does not land in time.

<a id="d12-c"></a>
### D12-c · Discussion #12 — generalize `mit_mandate_ref` to a governing-document pointer

**§3.9** · @timaxorum · **accept-with-modification** · target v1.0.3-final / v1.0.4

`governing_agreement_ref { doc_sha256, uri?, kind?, effective_at? }` accepted. `mit_mandate_ref` occurs exactly once in the specification and is its sole contract-like reference, which is too narrow for B2B chains governed by a master agreement. Placement is sequenced behind #3 on the same terms as #12-b; the editors record this as the item in this discussion at greatest risk of slipping to v1.0.4.

<a id="d12-d"></a>
### D12-d · Discussion #12 — closure state for a policy-blocked chain

**§3.4 / §2.5** · @timaxorum · **defer (v1.1.0)**, design invited now

Deferred with a correction the proposer accepted: the *evidence* of refusal already exists — §3.4 carries `outcome: 'blocked'` with `reason` required — so what is missing is a closure state letting such a chain terminate and be exported, not a record of the refusal. Related to #14-a's finding that non-representment export has no channel, and to #18.

<a id="d12-e"></a>
### D12-e · Discussion #12 — retention as a profile-level parameter

**§8.5** · @timaxorum · **accept-with-modification in principle** · target v1.1.0

Accepted in principle and sequenced with the profile architecture in #12-a, since a profile-level parameter presupposes profiles. §8.5's 24-month ceiling (`retention_until = completed_at + 730 days`) and its rule that the raw-plane clock MUST NOT exceed the chain clock are confirmed. Interacts with #32, which deferred the dual-commitment erasure appendix to v1.0.4 — that mechanism is the more likely near-term relief.

<a id="d19"></a>
### D19 · Discussion #19 — A2SPA mapping to a pre-seal execution-authorization token

**§3.9.4 / §3.9.5** · @Avouro · **decline (rationale)** for v1.0.3 · **defer (v1.1.0)** for the general case

Recorded as the editors stated in-thread on 29 July; the proposer accepted the correction in full.

**The premise is corrected first.** There is no accepted `execution_authorization_token` in v1.0.3. Discussion #4 received engagement, not a disposition, at the time this proposal was written, and the disposition log is authoritative — see #4-a for what was actually accepted and in what form.

**Declined for v1.0.3** on substance: a pre-execution, constraint-bounded authorization record is already the delegation artifact's job, and a new top-level pre-seal artifact type would touch the artifact-type enum, chain positions, `expectedArtifactCount` and the §4.1 hash input — not additive. The proposed mapping also diverges from A2SPA's own public documentation on four axes (Ed25519 against RSA-SHA256, RFC 8785 JCS against "sorted keys, no spaces", DIDs against registry-scoped identifiers, and a version that does not exist), which does not meet the primary-source bar applied in #47 and #56.

**Deferred to v1.1.0** for the general case, via the independently versioned extension registry in open question 3. Today an A2SPA payload can ride `raw_protocol_payload` (§3.9.4) subject to §3.9.20 — with the caveat that under `GENERIC` the delegation artifact is omitted entirely, so an A2SPA-only transaction has no slot. That gap is the same one #26 addresses from the identity side.

<a id="d22"></a>
### D22 · Discussion #22 — compact merchant-signed verification attestation

**§5 / §4.3 / §8.2.1** · @HemmaBo-se · **accept-with-modification** · target v1.0.3-final (status vocabulary) / v1.0.4 (attestation)

The most advanced design work in the round on open question 4, approached from the producer side: a small signed envelope under a merchant-controlled key discoverable from the merchant's own domain, restating chain identifier, merchant identifier, a commitment to the authorized amount, and an authorization reference — one key resolution, one signature check, no artifact walk.

**One element is pulled forward into v1.0.3-final.** The string `unverifiable` appears nowhere in rc.2: there is no per-layer status vocabulary distinguishing "checked and failed" from "no rule existed". That is a live gap independent of where the attestation lands, it blocks this design regardless, and it is the same gap #60 requires for its security remedy and #15 must carry in the §5 schema. The three land as one edit.

**The attestation construction defers to v1.0.4** under #28, together with #55. Both load-bearing properties map onto machinery rc.2 already has — commitments rather than values is the §8.2.1 split applied to the summary, and layered attestations with independent statuses is the §4.3 recipient-relative class applied per source.

**Conflict recorded against an accepted disposition.** #40's rule — that a consumer MUST NOT accept a given `chain_id` + `sealed_bundle_hash` more than once for the same business effect — forbids by its letter a refreshed superseding summary over the same seal. A supersession carve-out MUST be drafted into #40's text before it lands, keyed to `attestation_seq` or the equivalent. This is the one place where v1.0.3-final text as currently dispositioned would block v1.0.4 work.

Merchant-domain key discovery for this envelope depends on #21, whose chain-level assertion is deferred to v1.0.4 for the same reason.

<a id="d45"></a>
### D45 · Discussion #45 — non-card and stablecoin settlement evidence

**§3.6 / §3.10 / §6** · @scottsgeorge · **defer (v1.0.4)**

Deferred to v1.0.4 and bundled with open question 1 (bank-rails build-out), which is the same shape of work in the same release. Nothing here is safe for v1.0.3-final: a new artifact type touches the enum, Appendix B and `expectedArtifactCount`.

The gaps are confirmed. §3.6's `result` is authorization-time only (`approved | declined | review`) and artifacts are immutable, so a settlement lifecycle cannot be a mutable status field. The stablecoin object is exactly `{ chain, asset, transaction_hash, amount_atomic }` — no rate, quote or fee fields — and nothing connects `amount` to `amount_atomic`. There is no `settlement` artifact type.

The shape carried into v1.0.4: a post-seal `settlement` artifact with `settlement_id`, `charge_ref`, `status` and a rail echo, one artifact per lifecycle transition; an optional conversion block `{ rate, quote_id, quote_expires_at, quoted_at }` on the §3.6 stablecoin object, **excluding** fees and gross/net as reconciliation accounting rather than dispute evidence; and the same object mirrored on §3.10. Payment and settlement references are restricted raw-plane by default per #34.

**Scoped together with #58 and #59**, which describe the same structural gap for auth-capture: one post-seal settlement artifact serves both, and designing them separately would produce two overlapping artifact types. The proposal is written from one provider's API surface and is neutralized to a rail-generic shape before drafting.

<a id="d48"></a>
### D48 · Discussion #48 — qualified electronic timestamps are not distinguishable in the chain

**§4.3 / §9.4 / §2.8** · @Trusteedxyz · **accept-with-modification** · target v1.0.3-final

Corroborated: §4.3 presents its three anchoring options as equivalent, and nothing in a chain records which was used. Under eIDAS a qualified RFC 3161 timestamp from a provider on a Member State trusted list carries an Art. 41(2) presumption that a byte-identical token from any other authority does not, so two implementations that are indistinguishable to a verifier have materially different evidentiary standing.

**Accepted for v1.0.3-final:** a §4.3 SHOULD that an implementation record qualified status **together with the trusted-list entry relied on and the time it was consulted** — a boolean is wrong because qualification is time-varying — and a one-sentence §9.4 statement that AEP takes no position on evidentiary weight. Both are additive prose with no schema change. The proposal explicitly forecloses the gating objection that decided #4-b, and the editors record that it does so correctly.

**Ask 2 declined.** Letting the verification class in §2.8/§5 reflect qualified status would make classification into outcome assignment, which is the reasoning that deferred #37. The recorded fields carry the fact; the class does not move. This matches the proposer's own stated preference and their lowest-confidence item.

**The concrete anchor-field shape defers to v1.0.4** with the §4.3 anchor object, noting Implementing Decision (EU) 2025/2164 makes trusted-list format v6 mandatory from 29 April 2026 with no coexistence period — so any field references the list entry rather than embedding a format. The trusted-list-entry-plus-consultation-time pattern is the same shape as §4.3's key-witness snapshots and is scoped with #36's long-term archive verification work.

**Dispositioned together with #44** as one §9.4 posture decision: both say the specification's jurisdictional framing is narrower than its claims, from the EU and LATAM sides respectively.

<a id="d55"></a>
### D55 · Discussion #55 — implementer requirements for a network-consumable attestation

**§4.3 / §3.9.12 / §6.3** · @faulknerwayne73-droid · **defer (v1.0.4)**

Design input to open question 4 from the consumer side — an on-chain atomic-execution contract that cannot ingest a DisputeBundle, since JCS plus Ed25519 plus a variable-length hash walk exceeds a block gas budget by roughly two orders of magnitude. Nothing is requested for v1.0.3-final. Deferred to v1.0.4 under #28, as a single scoping unit with #22.

The offer of an MIT-licensed EVM reference verifier and conformance vectors is **accepted on the record** and is independent of where the attestation lands; §6.3 vector publication is already a release gate.

**Three conflicts recorded for #28 scoping**, all cheap to resolve now and expensive later:

1. **Flat preimage against layered status.** This proposal reduces the attestation to a single 32-byte preimage; #22's second load-bearing property is per-layer attestations with independent statuses, which a flat preimage destroys. The two-serialization reconciliation proposed in #22's thread — one canonical signing input with a JWS envelope and a compact fixed frame — mitigates but does not fully dissolve it.
2. **`issuer_relationship` enum divergence.** This proposal uses `first_party | psp_operated | network_operated | custodian_operated | third_party_witness`; the accepted `pre_execution_attestation` under #4-a uses a different and incompatible set. Two attestation types with near-identical fields and incompatible enums is exactly the interoperability hazard this round exists to catch. Because #4-a's Edit status is still `not started`, reconciling the two costs nothing today — and this MUST be settled while #27 is being drafted.
3. **Signature algorithm.** §4.3 mandates Ed25519; an EVM consumer needs EVM-verifiable primitives. Algorithm agility is unresolved and belongs in the v1.0.4 design.

The `outcome_binding: false` marker the proposal requests is consistent with §3.9.12 and with #4-b, and is carried forward.

## Coverage

Every submission received in the window is listed here until it carries a ref in the Index above. This section is the check behind the "all comment dispositions recorded" release gate.

**Status: 50 submissions received · 50 dispositioned · 0 outstanding.**

Dispositions are recorded across 56 refs, because Discussion #4 and Discussion #14 each received two and Discussion #12 received five.

### Received, not yet dispositioned

None. Every submission received during the window carries a disposition ref.

Recording a disposition is not the same as making the change. Eight accepted items still show `Edit: not started` and are tracked by the fourth release gate, not by this section.

### Out of scope for this round

- **#2** @jyothi-acomm-ai ✱ — §3.1/§7.2 `url_params` and `referral_param`. Filed 8 July, before the comment window opened on 13 July. Closed.
- **#29** @ankitshah009 — repository write-permission probe, not a specification comment. Closed.

### Numbering note

Issue, pull-request, and discussion numbers come from one shared GitHub sequence, so the numbers above never collide and gaps in the issue list are normal — they are numbers taken by pull requests, discussions, or deleted items. Do not read the highest number as a count of submissions.

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

## Received, not yet dispositioned

Every submission received in the window appears here until it carries a ref in the Index above. This list is the check behind the "all comment dispositions recorded" release gate.

**Status: 50 submissions received · 17 dispositioned · 33 outstanding.**

### Issues

- [ ] **#3** @rrrodzilla — §2 `spec_version` and chain-level fields have no defined integrity mechanism
- [ ] **#5** @squishy-ctrl — §3.2 required Referral IP/UA HMACs conflict with privacy-minimal observer profiles
- [ ] **#7** @squishy-ctrl — §§0.1/6.4/8.2.1/8.6 chain-only conformance without an Evidence Custodian
- [ ] **#8** @squishy-ctrl — COMMENT-ROUND.md stale placeholder *(actioned by PR #10; disposition still owed)*
- [ ] **#15** @jyothi-acomm-ai ✱ — §6.2/§5 verification result schema defines no fields for required values
- [ ] **#16** @jyothi-acomm-ai ✱ — §6.1 export record: exporter identity, disclosure manifest, supersession
- [ ] **#17** @jyothi-acomm-ai ✱ — §4.1 code-point vs UTF-16 code-unit key ordering (RFC 8785, vector (h))
- [ ] **#18** @jyothi-acomm-ai ✱ — §6.5 safe harbour: non-attribution rule or evidence exclusion?
- [ ] **#20** @HemmaBo-se — §3.7 Fulfillment cannot be satisfied by reserved time-and-place commerce
- [ ] **#21** @HemmaBo-se — §3.9.11 merchant-domain authentication is wrapper-dependent, not chain-level
- [ ] **#23** @johnhenrypower — §3.9.18 `wba_tag` MUST value rejects conformant Mastercard Agent Pay signatures
- [ ] **#24** @johnhenrypower — §3.9.15 `credential_chain.typ` enum cannot express Verifiable Intent L1
- [ ] **#25** @johnhenrypower — §3.9.18 `web_bot_auth` has no field for the Signature-Input nonce
- [ ] **#42** @rabet — §3.9.N `pre_execution_attestation` binding omits verified surface-state
- [ ] **#43** @rabet — §2.8 no observation-source classification; self-reported and independently-observed fulfillment indistinguishable
- [ ] **#44** @Marlonm0987 — §9.4 EU/US posture leaves LATAM unrepresented; `authority` should carry issuing jurisdiction
- [ ] **#49** @Trusteedxyz — §3.4/§8.6 Minimal Merchant Profile omits the mandatory policy artifact
- [ ] **#50** @Trusteedxyz — §3.4 required `risk_score` forces deterministic engines to attest a fabricated value
- [ ] **#51** @Trusteedxyz — §3.4 policy artifact cannot distinguish an evaluated approval from a degraded one
- [ ] **#52** @Trusteedxyz — §3.4 `rules_triggered` cannot express which controls ran and did not fire
- [ ] **#53** @Trusteedxyz — §3.4 `escalated` collapses merchant approval and buyer confirmation
- [ ] **#54** @Trusteedxyz — §3.4 `rules_triggered` has no ordering contract; cannot name the deciding rule
- [ ] **#58** @shunhe-wang — §§2.5/3.6/3.9.10–11 auth-capture lacks held-payment identity, verification rules, post-seal outcomes
- [ ] **#59** @shunhe-wang — §3.9.12 auth-capture settlement timing permanently depresses the evidence tier
- [ ] **#60** @saishav7 — §3.9.11 x402 signature verification diverges for non-CAIP-2 networks and non-EVM chains

### Discussions

- [ ] **#6** @squishy-ctrl — Minimal Observer Profile for merchant-observed Referral and `request_binding`
- [ ] **#11** @juanferrub — cross-node evidence handoff model for multi-actor transaction chains
- [ ] **#12** @timaxorum — B2B transactions: a small core with profiles
- [ ] **#19** @Avouro — optional mapping of A2SPA signed execution payloads to the pre-seal token schema
- [ ] **#22** @HemmaBo-se — compact verification attestation: a merchant-signed summary a rail can verify without the chain
- [ ] **#45** @scottsgeorge — additions for non-card / stablecoin settlement evidence
- [ ] **#48** @Trusteedxyz — qualified electronic timestamps: the anchor with a statutory presumption in the EU
- [ ] **#55** @faulknerwayne73-droid — open question 4: compact network-consumable verification attestation, implementer requirements

### Out of scope for this round

- **#2** @jyothi-acomm-ai ✱ — §3.1/§7.2 `url_params` and `referral_param`. Filed 8 July, before the comment window opened on 13 July. Closed.
- **#29** @ankitshah009 — repository write-permission probe, not a specification comment. Closed.

### Numbering note

Issue, pull-request, and discussion numbers come from one shared GitHub sequence, so the numbers above never collide and gaps in the issue list are normal — they are numbers taken by pull requests, discussions, or deleted items. Do not read the highest number as a count of submissions.

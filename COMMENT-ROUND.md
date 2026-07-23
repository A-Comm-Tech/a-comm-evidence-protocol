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
- [ ] All comment dispositions recorded below

## Disposition log

| # | Issue | Section | Disposition | Rationale / resulting change |
|---|---|---|---|---|
| — | *(no comments dispositioned yet)* | | | |

### Editor errata (self-logged during the round)

Items the editors identified during the review window. They follow the same disposition process and are batched into v1.0.3-final with the community dispositions above.

| # | Source | Section | Disposition | Rationale / resulting change |
|---|---|---|---|---|
| E-1 | Editor review of published AI-referral attribution measurements (Jul 2026) | §7.1 | accept — v1.0.3-final | The "Referrer behavior" column conflates web and native-app surfaces. App → browser transitions pass no `Referer` header on *any* platform (public measurements put 35–70% of AI referral sessions at no-referrer), so `referrer_header` is a minority signal for every platform, not a ChatGPT-specific caveat — e.g. "Perplexity consistently passes referrer" holds only for the web surface, and UTM-style parameters are appended inconsistently and cannot be relied on. Resulting change: annotate the §7.1 table per surface (web vs. native app) and reweight the recommended primary method toward `referral_param` / `merchant_tracked_link` for all four platforms. |
| E-2 | Editor review (same measurements) | §3.2 | accept — v1.0.3-final | `referrer_domain` is optional, but the spec should state that its absence is the *expected majority case* (native-app traffic carries no `Referer`), so implementers do not treat a missing referrer as an anomaly or a capture failure. Resulting change: add an informative note to §3.2 that absence of `referrer_domain` — or of a Referral/Discovery artifact — MUST NOT be read as evidence that a transaction was not AI-originated, consistent with the §3.9.12 adverse-evidence disclaimer. |

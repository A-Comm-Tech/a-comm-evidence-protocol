# AEP v1.0.3 — Ecosystem Comment Round

**Draft under review:** [v1.0.3-rc.2](spec/aep-v1.0.3-rc.2.html) ([rendered — living draft](https://aep.a-comm.ai/) · [frozen rc.2 snapshot](https://aep.a-comm.ai/v1.0.3-rc.2) for stable section references) (2026-07-02)
**Status: Comment window July 6 – August 5, 2026.** Every comment receives a public disposition in the log below.

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

- **Review window:** July 6 – August 5, 2026 (30 days).
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
| — | *(round not yet open)* | | | |

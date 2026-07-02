# A-Comm Evidence Protocol (AEP)

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Spec: v1.0.3-rc.2 draft](https://img.shields.io/badge/spec-v1.0.3--rc.2_draft-orange.svg)](spec/aep-v1.0.3-rc.2.html)
[![Comment round](https://img.shields.io/badge/comment_round-opening-brightgreen.svg)](COMMENT-ROUND.md)

**The open evidence standard for agentic commerce.**

AI agents now discover products, carry consumer intent, and complete purchases. When one of those transactions is disputed — *"I didn't authorize this," "my agent went rogue," "the goods never arrived"* — every party in the payment chain needs a trustworthy answer to a single question:

> **What actually happened in this transaction?**

AEP answers it with a **sequential, tamper-evident hash chain** of typed evidence artifacts covering the full transaction journey:

```
discovery → referral → intent → delegation → policy → cart → authorization ─┐
                                                              (seal point)  │
                                          fulfillment · refund ← post-seal ─┘
```

Each artifact is canonicalized (RFC 8785 JCS), SHA-256-linked to its predecessor, and the chain is sealed with an Ed25519 signature at authorization. The result exports as a **DisputeBundle**: a verifiable, dispute-grade evidence record that any party can independently re-verify.

## AEP is a witness, not a competitor

Agentic commerce already has protocols for identity, mandates, checkout, and settlement. AEP **wraps** them rather than replacing them. The `delegation` artifact carries sub-wrappers that preserve — verbatim or by digest — the signatures, mandates, tokens, and receipts of the protocols a transaction actually used, together with the key-discovery path that verified each one:

| Layer | Wrapped protocols |
|---|---|
| Identity-transport | Visa TAP · KYA-family agent identity tokens · Web Bot Auth (RFC 9421) · A2A signed Agent Cards |
| Commerce-delegation | AP2 mandates (SD-JWT-VC) · Mastercard Verifiable Intent · ACP checkout sessions · UCP signed exchanges |
| Settlement | x402 (on-chain proof) · KYA-Pay payment tokens · card / stablecoin / account / real-time rails |
| Tool-invocation | MCP tool calls (with a per-call result receipt AEP itself defines) |

What no wrapped protocol provides — and AEP does — is the **durable outcome trail**: identity signatures are ephemeral per-request artifacts, mandates prove authorization-time consent, but none of them produce a sealed, independently verifiable record of what was charged, delivered, refunded, and settled. That record is AEP's job.

## Who this is for

| You are a… | AEP gives you… |
|---|---|
| **Payment network** | A persistence and anchoring layer that feeds your compelling-evidence and data-sharing programs — chains index by network transaction ID, and export renderers target your evidence formats. AEP describes evidence; it never assigns weight, outcome, or liability — those are yours. |
| **Issuing bank** | Structured, re-verifiable transaction context for dispute investigations and cardholder inquiries — including the delegation ceremony that shows whether a human authorized the agent (the question your Reg E/Z investigation actually asks). |
| **Acquirer / processor** | Pre-dispute deflection inputs (order-detail responses, compelling-evidence elements, prior-transaction history) that keep qualifying disputes out of your monitoring-program ratios. |
| **PSP** | One capture surface at webhook time, one bundle format, per-channel renderers with real field-length budgets — instead of a bespoke evidence pipeline per merchant per scheme. |
| **Merchant** | The **Minimal Merchant Profile (§8.6)**: intent, cart, authorization, and post-seal fulfillment/refund — implementable in days, mostly from data your PSP already emits. Chargeback defense that upgrades as your ecosystem partners emit richer artifacts. |
| **Agent platform** | The **Minimal Platform Profile (§8.7)**: countersign the Intent artifact with keys you already operate — no query text, no ranking data, ever — plus named-actor notification and rebuttal rights over any chain that names you. When an "agent went rogue" case lands, the record of what the user authorized is *your signed artifact*, not someone's heuristic. |
| **Cardholder** | Transactions your agent makes on your behalf leave an auditable trail: what you asked for, what you authorized, what you were charged, what arrived. |

## Reading the specification

- **[Draft under review — v1.0.3-rc.2](spec/aep-v1.0.3-rc.2.html)** (this repo; single self-contained HTML page — open it in a browser, or read it rendered at [aep.a-comm.ai/v103-draft](https://aep.a-comm.ai/v103-draft))
- Prior published versions (v1.0.x) will be added as tagged releases.

Suggested reading order for engineers: §2 data model → §4 hash chain & sealing → §3.9 delegation wrappers → §5–6 verification & export → §8.6/§8.7 minimal profiles. Implementer quickstarts live in [`docs/implementers/`](docs/implementers/).

## Design principles

1. **Protocol-agnostic witness.** Record which protocols carried the transaction and how each envelope verified. Never rank, replace, or adjudicate them.
2. **Evidence, not liability.** Verification classes and evidence tiers describe cryptographic verifiability and composition — nothing more. No network or scheme rule assigns any outcome to an AEP tier, and the absence of a chain implies nothing about a transaction's legitimacy (spec §3.9.12).
3. **Two-plane privacy.** The chain carries salted hashes; dispute-usable raw values live encrypted with an **Evidence Custodian** role, disclosed only on documented dispute triggers, with every disclosure logged as a sealed artifact (§8.2.1).
4. **Implementable by anyone.** Apache-2.0 with an explicit patent grant (spec License §2). Conformance is defined by published test vectors (§6.3), not by any vendor's implementation. No fees, no registration, no permission.

## Versioning & stability

Strict SemVer for the wire format (spec §0.2). Chains are never re-versioned after creation; verifiers advertise the `spec_version` set they support; every published version remains verifiable for at least 24 months after the next MAJOR release. Current draft: **v1.0.3-rc.2** (additive/MINOR over v1.0.2), open for ecosystem comment.

## Participate

- **[Comment round](COMMENT-ROUND.md)** — the v1.0.3 review process, the open questions we're explicitly seeking input on, and the public disposition log where every comment gets an answer.
- **[Issues](../../issues)** — section feedback and technical issues (templates provided).
- **[Discussions](../../discussions)** — normative proposals and design questions.
- **[CONTRIBUTING.md](CONTRIBUTING.md)** — ground rules (Apache 2.0 §5, DCO sign-off, neutrality, primary-source citations).
- **[SECURITY.md](SECURITY.md)** — how to report security issues in the specification privately.

## License

Apache License 2.0 — see [LICENSE](LICENSE) and [NOTICE](NOTICE). The specification includes an explicit Apache §3 patent grant covering the AEP wire format, hash-chain construction, signal classification, and evidence-tier ranking. The names "A-Comm Evidence Protocol" and "AEP" are generic identifiers for this specification and are not trademarked.

## Repository layout

```
spec/                     The specification (self-contained HTML, one file per version)
vectors/                  Conformance test vectors (§6.3) — publication gates v1.0.3-final
docs/implementers/        Role-based quickstarts (merchant, agent platform, PSP/acquirer)
COMMENT-ROUND.md          Review process + public disposition log
CONTRIBUTING.md           DCO, neutrality rules, proposal process
GOVERNANCE.md             Maintainer model and standards-body trajectory
SECURITY.md               Private security reporting
```

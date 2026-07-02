# Governance

## Current model (bootstrap phase)

The specification is edited by a maintainer group stewarded by **A-Comm Inc.**, which publishes the spec under Apache 2.0 with an explicit patent grant (spec License §2) and accepts contributions under DCO (spec License §3). During the bootstrap phase:

- **Editors** merge changes. All review happens in public (issues, discussions, PRs).
- **Comment rounds** are the primary decision mechanism for normative releases: every comment receives a public disposition, and the disposition log is the authoritative record.
- **Versioning discipline** is a governance commitment, not just engineering hygiene: strict SemVer per spec §0.2, chains never re-versioned, a 24-month verifiability window per published version, and no hash/signature-semantics changes below MAJOR.
- **Neutrality rules bind the editors too:** no vendor endorsements, no partnership announcements, no competitive rankings, no liability assertions — in the spec, in this repository, or in project communications.

## Separation of roles

A-Comm Inc. operates a commercial reference implementation and an Evidence Custodian service. These are deliberately separated from the standard: the spec defines the **Custodian role** generically, conformance is defined by **published test vectors** rather than the reference implementation, and nothing in the specification requires any A-Comm product or service.

## Trajectory: neutral stewardship

The stated goal is multi-stakeholder governance. Planned steps, in order:

1. **External maintainers** — inviting reviewers from multiple ecosystem actor classes (network, issuer/acquirer, PSP, merchant, agent platform, identity provider) to join the editor group after the first comment round.
2. **A published charter** — decision rules, editor terms, and conflict-of-interest disclosures.
3. **Standards-body engagement** — AEP is positioned as the evidence/observability layer complementary to the agentic-payment mandate work now consolidating in industry bodies. Contribution or liaison of the specification to a neutral standards organization is on the roadmap, and the Apache-2.0 + DCO + patent-grant posture was chosen to make that transfer frictionless.

Progress on each step is tracked in public issues labeled `governance`.

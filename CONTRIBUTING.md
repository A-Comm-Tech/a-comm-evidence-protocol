# Contributing to the A-Comm Evidence Protocol

Thank you for helping make agentic commerce transparent and auditable. AEP is an open specification and depends on review from every actor class in the payments ecosystem — networks, issuers, acquirers, PSPs, merchants, agent platforms, identity and trust providers, bank-rail operators, and independent implementers.

## Ways to contribute

1. **Comment-round feedback** — during an open comment round (see [COMMENT-ROUND.md](COMMENT-ROUND.md)), file section feedback or technical issues using the issue templates. Every comment receives a public disposition (accept / accept-with-modification / defer / decline, with rationale).
2. **Errata** — factual or internal-consistency errors in the published spec are welcome any time as issues labeled `errata`.
3. **Normative proposals** — new artifact types, sub-wrappers, verification rules, or export profiles. Open a Discussion **before** drafting text; substantive changes require visible discussion and follow the versioning policy in spec §0.2 (additive → MINOR; hash/signature-semantics changes → MAJOR).
4. **Test vectors and implementations** — independent implementations and additional conformance vectors are the highest-value contributions an open standard can receive. If you build an implementation, tell us — we list independent implementations without endorsement ranking.

## Ground rules

- **License:** all contributions are accepted under the Apache License 2.0 (§5, Submission of Contributions).
- **DCO:** every commit MUST carry a Developer Certificate of Origin sign-off:
  `Signed-off-by: Your Name <you@example.com>` (use `git commit -s`). A CI check enforces this; pull requests without sign-offs will not be merged. The DCO text is at <https://developercertificate.org>.
- **Neutrality:** the specification names protocols and standards, not partnerships. Contributions must not add vendor endorsements, marketing claims, competitive rankings, or liability assertions (see the disclaimers in spec §3.9.12).
- **Accuracy:** claims about external protocols or network rules must cite the primary source (published spec, official rules document, or standards-body publication).

## Review process

Maintainer review is public (see [GOVERNANCE.md](GOVERNANCE.md)). Normative changes are batched into versioned releases per spec §0.2; each release carries a changelog entry (spec Appendix D), a git tag, a GitHub Release, and updated test vectors. During comment rounds, the disposition log in COMMENT-ROUND.md is the authoritative record of what was accepted and why.

# Security Policy

AEP is an evidence standard: its security properties — tamper-evidence, non-forgeability, replay resistance, PII/PAN containment — are the product. We treat weaknesses in the *specification* with the same severity most projects reserve for code.

## Reporting a vulnerability

**Please do not open a public issue for security-sensitive findings.**

Report privately via **GitHub Security Advisories** (Security → "Report a vulnerability" on this repository) or by email to **security@a-comm.ai** with subject `[AEP-SPEC]`.

In scope (examples):
- Evidence forgery or backdating: any way to produce a chain/bundle that verifies but misrepresents what happened
- Hash-chain or canonicalization weaknesses (JCS/NFC/integer-encoding ambiguities that let two byte streams verify as one)
- Signature/seal weaknesses, key-discovery spoofing, key-witness bypasses
- Replay or cross-transaction splicing of artifacts or sub-wrapper envelopes
- PII or PAN/SAD leakage paths through payloads, exports, or wrapped envelopes (spec §3.9.20, §8.2, §8.3)
- Verification-rule gaps that let an invalid envelope elevate a verification class or tier

Out of scope: vulnerabilities in third-party implementations (report to their maintainers), and issues in the wrapped protocols themselves (report to their stewards — we will happily coordinate).

## What to expect

- Acknowledgment within **3 business days**.
- A severity assessment and remediation plan within **14 days**; spec fixes ship as errata or a PATCH release per §0.2, with credit to the reporter (unless you prefer anonymity).
- Coordinated disclosure: we will not publish details before a fix is available, and we ask the same of reporters.

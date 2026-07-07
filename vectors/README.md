<!-- SPDX-License-Identifier: Apache-2.0 -->
# AEP Conformance Test Vectors

All test vectors in this directory are licensed under the **Apache License 2.0**
(see the [LICENSE](../LICENSE) and [NOTICE](../NOTICE) files at the repository root).

## Status

| Version | Directory | Status |
|---|---|---|
| v1.0.3-rc.2 | `v1.0.3-rc.2/` | **PARTIAL — (a), (b), (h) + test keys published 2026-07-06**; (c)–(g), (i)–(m) pending (see the directory README). Full publication remains the release gate for v1.0.3-final (spec §6.3) |

## What will be published (per spec §6.3)

- (a) genesis artifact: canonical JSON byte stream + expected SHA-256
- (b) full artifact chain showing each `previous_hash`
- (c) signed DisputeBundle with public key and verification trace
- (d) multi-protocol delegation showing tier evaluation
- (e) RFC 9421 signature verification example
- (f) SD-JWT-VC mandate pair example
- (g) x402 v2 settlement example with EIP-712 recovery
- (h) JCS canonicalization examples (NFC normalization, integer representation)
- (i)–(m) rc.2 additions: identity/payment-facet wrappers, Web Bot Auth, `request_binding`, `settlement_rail` variants, refund artifact, `customerHistory`

Vectors are normative for conformance: an implementation that does not reproduce
vector (a)'s hash byte-for-byte is non-conformant, regardless of source-code review.

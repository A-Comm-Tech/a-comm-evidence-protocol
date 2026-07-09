<!-- SPDX-License-Identifier: Apache-2.0 -->
# AEP v1.0.3-rc.2 conformance vectors — initial set

Machine-verifiable vectors per spec §6.3. **Vector (a) is the conformance
gate:** an implementation that does not reproduce its `expected_sha256`
byte-for-byte is non-conformant, regardless of source-code review.

| Class | File | Status |
|---|---|---|
| (a) genesis canonical bytes + SHA-256 | `a-genesis.json` | **Published** |
| (b) full 8-artifact chain, every `previous_hash` + Ed25519 signatures | `b-chain.json` | **Published** |
| (h) JCS canonicalization (NFC, UTF-16 key order, integers, verbatim strings) | `h-canonicalization.json` | **Published** |
| — deterministic Ed25519 test keypair | `keys.json` | **Published** |
| (c) signed DisputeBundle + verification trace | — | Pending — blocked on the §2.7↔§4.3 "bundle signature" definition (comment-round issue) |
| (d)–(g), (i)–(m) protocol-wrapper vectors (TAP/AP2/x402/KYA/WBA, settlement rails, request_binding, refund, customerHistory) | — | Pending — require reference wrapper envelopes per protocol steward |

## Conventions these vectors pin (byte-level)

- RFC 8785 (JCS) serialization with **NFC normalization of member names and
  string values** (an AEP §4.1 addition beyond stock JCS).
- Member names sort by **UTF-16 code unit** at every nesting level.
- Hash-input `timestamp` is RFC 3339 UTC `Z`, **whole-second precision**.
- Money is **integer minor units** (JSON numbers, never strings).
- `previous_hash` is **inside** the hash input; genesis carries `null`.
- Ed25519 `server_signature` signs the **raw 32 digest bytes**, never the
  `sha256:`+hex string. The test seed in `keys.json` is published so every
  signature is reproducible; it must never be used outside test vectors.

## Regeneration

Generated deterministically by the reference implementation:
`a-comm-vault/acls-core/scripts/generate-aep-vectors.ts`
(`npx tsx scripts/generate-aep-vectors.ts <out-dir>`). The generator runs on
the same canonicalization/signing stack locked by that repo's known-answer
test (`test/artifact-hash-kat.test.ts`).

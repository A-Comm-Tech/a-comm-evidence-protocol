# Implementer Quickstarts

Pick your role. Each quickstart tells you the **minimum conformant thing you can build**, what data you already have, and what you get for it. Section references (§) point into the [specification](../../spec/aep-v1.0.3-rc.2.html).

| Role | Start here | Effort profile |
|---|---|---|
| Merchant | [merchant.md](merchant.md) | Days — mostly data your PSP already emits |
| Agent platform (AI assistant) | [agent-platform.md](agent-platform.md) | Weeks — one countersignature with keys you already run |
| PSP / acquirer / processor | [psp-acquirer.md](psp-acquirer.md) | The deep integration — you're the natural chain writer |

**Common ground for every role**

1. **The chain:** typed artifacts, each SHA-256-hashed over an RFC 8785 (JCS) canonical form with NFC normalization and integer-only numbers (§4.1). `previous_hash` is inside the hash input — that's the tamper-evidence.
2. **The seal:** Ed25519 over the raw 32-byte digest at the authorization artifact (§4.3). Publish your verification key at a stable well-known URL; anchor the sealed hash externally (RFC 3161 / transparency log) if you want your bundles to carry more than self-asserted weight (§4.3, §5).
3. **Conformance is byte-exact:** validate against the [test vectors](../../vectors/) before trusting your canonicalization stack. If vector (a)'s hash doesn't reproduce byte-for-byte, stop and fix that first (§6.3).
4. **Privacy planes:** salted HMAC hashes in the chain; raw dispute-usable values (IP, delivery address, login ID, device ID) belong in the Evidence Custodian plane, not in artifacts (§8.2, §8.2.1).
5. **Never persist wrapped credentials blindly:** decode envelope-shaped fields and refuse anything carrying a full PAN or security code — digest + extracted non-sensitive claims instead (§3.9.20, §8.3).

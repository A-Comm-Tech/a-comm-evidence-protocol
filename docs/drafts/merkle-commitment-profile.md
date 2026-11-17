# Merkle commitment profile: paid tool calls and line items

**Status:** draft for v1.0.4. Proposal for Discussion #126 (D126), next to I38.
**Depends on:** §0.2 (versioning), §2.5 (sealing), §2.7 (`chain_id`), §3.5 (cart),
§3.6 (authorization), §3.9.4 (`protocol_metadata`), §3.9.9 (MCP), §3.9.10 (x402),
§4.1 (canonicalization), §5 (verification result), §6.1 (DisputeBundle), §8.2.1 (two planes).

In rc.2 a chain has room for one paid tool call. `protocol_metadata` has one
`tool_invocation` slot and one `http_payment` slot (§3.9.4), and the chain seals at one
§3.6 authorization (§2.5). An agent task that makes many paid calls therefore needs one
chain per call. This draft lets one chain commit to all the calls of a task under a single
Merkle root. The call records are kept outside the chain, in the payload vault of open
question 2, as I38 keeps line-item bodies custodian-side (§8.2.1). A dispute about one call
then needs that call's record and a short inclusion proof, not the whole task.

The same construction serves the line-item commitments deferred under I38. Both uses share
one leaf format and one inclusion-proof format (RFC 9162), and each has a fixed ordering
rule. The call commitment is specified in full here. The line-item placement stays with I38.

## Leaf

```
leaf_input = JCS({ "chain_id": ..., "kind": ..., "index": ..., "record": ... })
leaf_hash  = SHA-256(0x00 || leaf_input)
```

- `chain_id`: the §2.7 `chain_id` of the chain that carries the commitment. A leaf hash
  belongs to one chain, so an inclusion proof from one chain's tree does not verify against
  another chain's root. §3.9.21 binds `chain_id` for the same reason. This does not stop a
  producer from recording the same call in two tasks. Catching that takes the call's own
  identifiers, such as its x402 payment payload, compared across chains.
- `kind`: `"call"` or `"line_item"`. A record of one kind cannot be presented as the other.
- `index`: the leaf's position, from 0. The position is committed, and two identical
  records still give different leaves.
- `record`: the committed record, defined below for calls.

JCS is RFC 8785 with the §4.1 rules, as for every hash input. `0x00` is the RFC 9162 §2.1.1
leaf prefix; interior nodes use `0x01`, which keeps leaf and node hashes in separate
domains. Hashes are written `sha256:<hex>` (§4.1); the tree is computed over the raw 32-byte
digests.

## Tree

`root` is the RFC 9162 §2.1.1 Merkle Tree Hash with SHA-256 over the leaf inputs in index
order: `MTH(D_n)` with `d[i] = leaf_input_i`. Interior nodes are
`SHA-256(0x01 || left || right)`. The shape of the tree depends only on the number of
leaves, so no padding rule is needed. A commitment has at least one leaf.

## Ordering

Calls are indexed in completion order: ascending `completed_at`, then, for calls with the
same `completed_at`, ascending lowercase hex of `SHA-256(JCS(record))`. Indexes run from 0
to `leaf_count - 1` without gaps. The root is then a function of the chain and the set of
call records: two producers holding the same records for the same chain build the same root.

`completed_at` is RFC 3339 UTC with whole seconds, as the rc.2 vectors write hash-input
timestamps, so calls that run in parallel often complete in the same second. The record
digest breaks the tie.

Line items keep the order in which the cart lists them (§3.5 `items`).

## Call record

```jsonc
{
  "completed_at":    "string: RFC 3339 UTC, whole seconds. When the producer recorded the
                      call as complete, with the tool result and the payment response (required)",
  "amount_atomic":   "string: base units of the commitment's asset charged for this call (required)",
  "tool_invocation": { ... } | null,   // §3.9.9; null when the paid call is not an MCP tool call (required)
  "http_payment":    { ... }           // §3.9.10 (required)
}
```

`tool_invocation` and `http_payment` are the objects a per-call delegation artifact carries
today, so a disclosed record holds the same evidence a per-call chain would.

`amount_atomic` is the charge, not the authorized maximum. Under `exact` it MUST equal
`http_payment.amount_atomic`. Under `upto` and `batch-settlement`, x402 sets the charge after
the request, at or below the maximum the client authorized
([upto](https://github.com/x402-foundation/x402/blob/main/specs/schemes/upto/scheme_upto.md),
[batch-settlement](https://github.com/x402-foundation/x402/blob/main/specs/schemes/batch-settlement/scheme_batch_settlement.md)).
rc.2 does not say which of the two `http_payment.amount_atomic` holds, so the record carries
the charge itself.

## Call commitment

A new nullable slot in §3.9.4 `protocol_metadata`:

```jsonc
// Tool-invocation layer
"call_commitment": {                       // NEW (this draft)
  "tree_alg":            "'rfc9162-sha256' (required)",
  "root":                "string: 'sha256:...' (required)",
  "leaf_count":          "integer >= 1 (required)",
  "network":             "string: CAIP-2, as in §3.9.10 (required)",
  "asset":               "string: token contract or mint, as in §3.9.10 (required)",
  "amount_total_atomic": "string: sum of amount_atomic over all leaves (required)"
} | null
```

Rules:

1. When `call_commitment` is present, `tool_invocation` and `http_payment` MUST be null.
   Each call's sub-wrappers are in its record.
2. Every record's `http_payment.network`, normalized under the §3.9.10 alias table, MUST equal
   `network`, and its `http_payment.asset` MUST equal `asset`.
3. `leaf_count` is the number of records. `amount_total_atomic` is the sum of their
   `amount_atomic` values.
4. The commitment is written before the seal. The delegation artifact that carries it is
   captured when the task closes, so its `captured_at` is not earlier than the
   `completed_at` of any committed call. It still precedes policy, cart and authorization
   (§3.9.2), so `sealed_bundle_hash` covers the root through the `previous_hash` links
   (§2.5). A root appended after the seal would not be covered, and §2.5 has no post-seal
   type for it.
5. §3.9.5 selection of `protocol` counts the sub-wrappers in the records as present. With no
   other layer populated, calls paid over x402 give `x402`.
6. `call_commitment` is not a protocol envelope. Verifiers report it in `commitment_checks`
   (below), not in `protocol_envelopes_verified`.

A task that stops before it closes has no commitment, and its chain does not seal. That is
the closure state deferred under D12-d. This draft does not change it.

## Partial disclosure

A DisputeBundle (§6.1) carries disclosed records in a new optional member,
`commitmentDisclosures`. Like `customerHistory`, it is not a chain artifact and is not
hash-chained. Each entry is checked against a root in the sealed chain.

```jsonc
"commitmentDisclosures": [
  {
    "kind":           "'call' | 'line_item' (required)",
    "index":          "integer >= 0 (required)",
    "record":         { ... },                   // the committed record (required)
    "inclusion_path": ["string: 'sha256:...'"]   // RFC 9162 §2.1.3, leaf to root (required)
  }
] | null
```

For each disclosure, a verifier:

1. Takes the chain's `chain_id` (§2.7), and `root` and `leaf_count` from the sealed
   commitment of that `kind`.
2. Recomputes `leaf_input` and `leaf_hash` from `chain_id`, `kind`, `index` and `record`.
3. Runs RFC 9162 §2.1.3.2 with `leaf_index = index` and `tree_size = leaf_count`, and compares
   the result with `root`.
4. For a call, checks call commitment rule 2, the `exact` amount rule, and that
   `completed_at` is not later than the delegation artifact's `captured_at`. Where several
   calls are disclosed, it checks that their indexes follow the ordering rule.

`tree_size` MUST be taken from the sealed `leaf_count`, never from the disclosure. An RFC 9162
inclusion path does not fix the tree size on its own: in the worked example below, the path
for index 1 of the 3-leaf tree also verifies with `tree_size` 4.

## Amounts

The total is checkable only with full disclosure. A verifier holding every record checks
that their `amount_atomic` values sum to `amount_total_atomic`. With fewer records it cannot,
and its result says so.

This draft does not use a Merkle sum tree, for two reasons:

- A sum tree changes the interior node hash, so its proofs are no longer RFC 9162 inclusion
  proofs. If #74 settles on RFC 9162 receipts for transparency-log anchoring, verifiers would
  need a second proof code path.
- With one leaf disclosed, a sum tree proves that the disclosed amount is counted in the
  committed total. It does not prove that the amounts of the undisclosed calls are right, so
  checking the total would still take full disclosure.

How `amount_total_atomic` relates to the §3.6 authorization is open (Open items, item 1).

## Verification result (§5)

A new member, echoed in the bundle's `verification` object like the other §5 members:

```jsonc
"commitment_checks": [
  {
    "kind":               "'call' | 'line_item'",
    "leaf_count":         "integer: from the sealed commitment",
    "leaves_disclosed":   "integer: distinct indexes disclosed for this kind",
    "leaves_verified":    "integer: distinct indexes whose disclosure passed every check above",
    "amount_sum_checked": "boolean | null: true only when leaves_verified = leaf_count; null for line_item",
    "amount_sum_matches": "boolean | null: the sum check when amount_sum_checked is true, otherwise null"
  }
] | null    // null when the chain carries no commitment
```

1. An inclusion proof shows that one record is in the committed set. It says nothing about
   the other records. A verifier, renderer or export MUST NOT present a commitment as
   verified as a whole unless `leaves_verified` equals `leaf_count`.
2. A disclosure that fails counts in `leaves_disclosed` and not in `leaves_verified`. It does
   not change `valid`, `hash_chain_intact`, `signatures_valid` or `chain_complete`, which
   still describe the chain itself.

## Line items (I38)

A line-item leaf has `kind: "line_item"`, and its record is one entry of the §3.5 `items`
array. In the cart, a line-item commitment needs three of the values a call commitment
carries: `tree_alg`, `root` and `leaf_count`. Their field names, the merchant-signed quote
and any check against the cart totals are left to I38. Disclosure and §5 reporting work as
for calls, with `amount_sum_checked` null.

## Versioning

The change is additive: one nullable `protocol_metadata` slot (the v1.0.3 slots were added
the same way), one optional §5 member and one optional DisputeBundle member. That is MINOR
under §0.2. The §4.1 hash input definition is unchanged. The leaf input is a new hash input
and follows §4.1. Turning `tool_invocation` or `http_payment` into arrays instead would
change the §3.9.4 top-level schema, which §0.2 lists as MAJOR.

## Working group

Core Specification & Schemas.

## Worked example

An agent makes three paid calls to an MCP search tool during one task. Each call is paid
under x402 `batch-settlement` on Base Sepolia (`eip155:84532`), 1000 base units of USDC
(0.001 USDC) per call. Two calls run in parallel and complete in the same second. The payer
later disputes one of them, and the exporter discloses that call only.

**Order.** `inv_7d2e` and `inv_9a41` both completed at `09:30:02Z`. Their record digests
begin `54841bde` and `b8bdd6d3`, so `inv_7d2e` is index 0 and `inv_9a41` is index 1.
`inv_c05b` completed at `09:30:05Z` and is index 2.

**Tree.** With 3 leaves, k = 2:

```
            root
           /    \
        N01      L2
       /   \
      L0    L1
```

| Node | Value |
|---|---|
| L0 | `sha256:675fdf77455c2fc782289f859195fe5560c7f0c61d9a4f61b44144a22a6abaf4` |
| L1 | `sha256:02b8e99352a1c97ab76863a5f14ae92f52a863a2eb47c58eb6747b3d52afc857` |
| L2 | `sha256:6bebf1d4e4778231871620819d6c6ec9a7969dcb01dd9a882c13b9875d219434` |
| N01 | `sha256:ef36484f0f2dc18f930f5d50b662f5f04eaf457a2a1338e001ec8cbcbf1c1feb` |
| root | `sha256:82054f6349af3653ddf20a2dda115a5426b7eec75bc019fd8d568f0f159ff6d0` |

**Delegation artifact** (`protocol_metadata`, excerpt):

```jsonc
{
  "protocol": "x402",
  "protocol_version": "2",
  "captured_at": "2026-11-17T09:30:06Z",
  "tool_invocation": null,
  "http_payment": null,
  "call_commitment": {
    "tree_alg": "rfc9162-sha256",
    "root": "sha256:82054f6349af3653ddf20a2dda115a5426b7eec75bc019fd8d568f0f159ff6d0",
    "leaf_count": 3,
    "network": "eip155:84532",
    "asset": "0x036CbD53842c5426634e7929541eC2318f3dCF7e",
    "amount_total_atomic": "3000"
  }
}
```

**Disclosure** in the bundle:

```jsonc
{
  "kind": "call",
  "index": 1,
  "record": {
    "completed_at": "2026-11-17T09:30:02Z",
    "amount_atomic": "1000",
    "tool_invocation": {
      "mcp_server": { "name": "example-search", "version": "1.2.0",
                      "title": "Example Search", "description": "Paid web search" },
      "mcp_server_canonical_uri": "https://tools.example.com/mcp",
      "mcp_tool_name": "search",
      "mcp_arguments": { "query": "ergonomic office chair warranty" },
      "mcp_arguments_canonicalisation": "rfc8785",
      "mcp_invocation_id": "inv_9a41",
      "mcp_jsonrpc_id": 2,
      "mcp_result": {
        "hash": "sha256:fe4c6ebe56131e141b73cf9a87b651efdb51e719a7967016a2e46613431af681",
        "hash_alg": "sha256", "canonicalisation": "rfc8785", "is_error": false,
        "has_structured_content": false, "output_schema_ref": null
      }
    },
    "http_payment": {
      "x402_version": 2, "scheme": "batch-settlement", "network": "eip155:84532",
      "payer": "0x1111111111111111111111111111111111111111",
      "pay_to": "0x2222222222222222222222222222222222222222",
      "amount_atomic": "1000", "asset": "0x036CbD53842c5426634e7929541eC2318f3dCF7e",
      "transaction_hash": null, "settlement_finalized": false, "partial_settlement": false
    }
  },
  "inclusion_path": [
    "sha256:675fdf77455c2fc782289f859195fe5560c7f0c61d9a4f61b44144a22a6abaf4",
    "sha256:6bebf1d4e4778231871620819d6c6ec9a7969dcb01dd9a882c13b9875d219434"
  ]
}
```

For length, the example leaves out the x402 envelope strings (`x402_payment_required`,
`x402_payment_signature`, `x402_payment_response`) and the other §3.9.10 fields it does not
need. A producer records them as a per-call delegation would.

**Verification.** The verifier recomputes L1 from the chain's `chain_id`, `"call"`, index 1
and the record, then runs RFC 9162 §2.1.3.2 with `leaf_index` 1 and `tree_size` 3:

- `fn = 1`, `sn = 2`, `r = L1`.
- First entry, L0: `fn` is odd, so `r = SHA-256(0x01 || L0 || r)`, which is N01. Then
  `fn = 0`, `sn = 1`.
- Second entry, L2: `fn` is even and differs from `sn`, so `r = SHA-256(0x01 || r || L2)`.
  Then `fn = 0`, `sn = 0`.
- `sn` is 0 and `r` equals `root`.

The §5 entry:

```jsonc
"commitment_checks": [
  { "kind": "call", "leaf_count": 3, "leaves_disclosed": 1, "leaves_verified": 1,
    "amount_sum_checked": false, "amount_sum_matches": null }
]
```

The bundle shows that the disputed call is one of the three calls the chain committed to
before it sealed, and what was charged for it. It does not show the other two calls, or that
the three charges sum to 3000. That takes all three records.

## Open items

1. **What the single §3.6 authorization represents for a task.** Under `exact`, each call
   settles on its own, so a task chain records N settlements under one authorization. Under
   `batch-settlement`, x402 leaves redemption to the network binding. On the EVM binding
   ([scheme_batch_settlement_evm.md](https://github.com/x402-foundation/x402/blob/main/specs/schemes/batch-settlement/scheme_batch_settlement_evm.md))
   the client signs a cumulative voucher per request, the charge for each request comes back
   in the payment response, and whoever manages the vouchers (the server, or the facilitator
   in facilitator-managed mode) claims the latest voucher per channel on its own schedule.
   One claim can cover many channels, one settle sweeps many claims into a single transfer,
   and channels are long-lived, so a redemption need not line up with the end of a task.
   One option is that the sealing authorization records the task's charge:
   `stablecoin.amount_atomic` equal to `amount_total_atomic`, in the same asset and chain.
   Two questions would remain: how a later redemption is linked to the sealed chain, since
   §2.5 has no post-seal settlement type, and how §3.6 MUST-1, which compares `amount` with
   `cart.total` in ISO 4217 minor units, applies when the task's charge is less than one
   minor unit or not a whole number of them.
2. **Envelope checks on disclosed records.** Whether a verifier applies §3.9.11 to the
   sub-wrappers in a disclosed record, and where it reports the result. If it does, the
   24-hour skew rule would be measured against the record's `completed_at`, since one task
   can run longer than that. Rule 7 covers only `exact` on EVM, so a `batch-settlement`
   record would be reported `unverifiable` / `no_rule` either way.
3. **Tasks paid in more than one asset or network.** Out of scope. One commitment per asset
   would need more than one slot.
4. **Record store.** Where records live, how long they are kept and how an exporter gets them
   belong to open question 2. This draft needs only that the exporter can produce any record
   and its inclusion path, which takes all the records or the stored tree.
5. **Sum tree.** If a use case needs each disclosed amount proven to be counted in the total,
   a sum tree can be added later as a second `tree_alg` value.

## Files

| File | Purpose |
|---|---|
| `merkle-commitment-profile.md` | This document: normative text and rationale |
| `merkle-commitment-profile.schema.json` | JSON Schema (2020-12) for `call_commitment`, the call record, a disclosure and a `commitment_checks` entry |
| `merkle-commitment-profile.example.json` | The worked example as a fixture |

The example validates against the schema, and its inclusion path verifies against the root.
The proof fails for the disclosed record under another `chain_id`, at index 0 or 2, with
`kind: "line_item"`, or with `amount_atomic` changed to `"1001"`, and with the path cut to
one entry or `tree_size` 2. The schema rejects a `leaf_count` of 0, an amount with a leading
zero, a root without the `sha256:` prefix, `call_commitment` next to a non-null
`http_payment`, a `completed_at` with fractional seconds, and `amount_sum_matches` set while
`amount_sum_checked` is false.

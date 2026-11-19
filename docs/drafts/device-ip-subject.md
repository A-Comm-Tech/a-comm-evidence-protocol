# Device and IP subject labels (Sections 3.1, 3.2, 6.1, 6.4)

**Status:** draft for the next MINOR spec version. Proposal for issue #116, filed out of #105.
**Depends on:** Section 0.2 (versioning), Section 2.8 (signal classification), Section 3.9.6 (TAP),
Section 3.9.11 (verification rules), Section 3.9.18 (Web Bot Auth), Section 3.9.19 (request_binding), Section 5 (verification output),
Section 6.1 (customerHistory), Section 6.4 (Compelling-Evidence renderer), Section 8.2.1 (two planes),
the Section 6.4 change proposed on #105. This draft lands with or after #105.

rc.2 records device and network values on the Discovery and Referral artifacts and carries
them into `customerHistory.identity_keys` at export. It does not record whose device or
network a value identifies. On an agent-placed order the request often comes from the agent
platform's cloud host, so the IP and user agent are the agent's, not the cardholder's. The
Section 6.4 change proposed on #105 keeps the device or IP match as its own element next to the agent login ID.
Without a label, the renderer cannot tell a cardholder's IP from an agent's cloud IP, and
prior-transaction matching can pair two orders on a value that only shows the same agent
platform placed both.

This draft adds one optional label per value. It does not change how any value is captured,
hashed or stored.

## The label

For each value field in scope, an optional sibling field named `<field>_subject` says whose
device or network the value identifies.

| Value | Meaning |
|---|---|
| `cardholder_device` | A device or network the cardholder controls: their browser, phone or home connection. An agent running on the cardholder's own device is here too, because the value still identifies the cardholder's device. |
| `agent_runtime` | Infrastructure the agent platform runs: its cloud hosts, or a hosted or headless browser it operates. |
| `merchant_session` | The merchant's own infrastructure. The request reached the capture point from a merchant-run server (server-side checkout, a backend-to-backend call, a merchant-run proxy), so the value identifies the merchant, not the cardholder or the agent. |
| `unknown` | The capturing party cannot tell. |

Absent means `unknown`. Existing chains therefore read exactly as they do today.

**Why `_subject` and not `_source`.** `_source` already means provenance in this spec
(`facilitator_identity_source`, `discovery_source`): where a claim came from. This label says
whose device or network the value identifies. That is a different fact, and also different
from who observed it.

## Where it applies

| Location | Value field | Label field |
|---|---|---|
| Section 3.1 Discovery | `customer_device_id` | `customer_device_id_subject` |
| Section 3.1 Discovery | `customer_device_fingerprint` | `customer_device_fingerprint_subject` |
| Section 3.2 Referral | `consumer_ip_hash` | `consumer_ip_hash_subject` |
| Section 3.2 Referral | `user_agent_hash` | `user_agent_hash_subject` |
| Section 3.2 Referral | `customer_device_id` | `customer_device_id_subject` |
| Section 3.2 Referral | `customer_device_fingerprint` | `customer_device_fingerprint_subject` |
| Section 6.1 `customerHistory.identity_keys` | `ip_ref` | `ip_ref_subject` |
| Section 6.1 `customerHistory.identity_keys` | `device_id_ref` | `device_id_ref_subject` |
| Section 6.1 `customerHistory.identity_keys` | `device_fingerprint` | `device_fingerprint_subject` |

Not in scope: the KYA `aid.creation_ip` and `source_ips` (Section 3.9.16). They identify the agent by
definition and need no label.

A label is present only where its value field is present and not null. Anything else is
non-conformant.

`consumer_ip_country` (Section 3.2) is derived from the IP and carries no label of its own. A reader
takes its subject from `consumer_ip_hash_subject`.

## Signal class

The label is set by the party that captured the value: the AI platform for Section 3.1 (Section 8.7), the
merchant or its gateway for Section 3.2. A label with nothing behind it is **Asserted** (Section 2.8).

Where the capturing party saw an agent signature on the same request the value came from (a
TAP signature, Section 3.9.6, or a Web Bot Auth signature, Section 3.9.18, verified at capture), the value is
not the cardholder's network unless the capturing party knows the agent ran on the
cardholder's own device:

1. The capturing party SHOULD label such a value `agent_runtime`, or `cardholder_device`
   where it knows the agent ran on the cardholder's device.
2. The label takes the class of the evidence behind it. Where that evidence is a
   Section 3.9.19 `request_binding` without persisted request material, or a capture-time result over a
   digest-retained signature (Section 3.9.18, wire-signature retention), that class is Asserted.
   In rc.2 the Referral artifact has no request reference, so nothing in the chain ties a TAP or
   Web Bot Auth signature to the request a value came from. Every label in rc.2 is therefore
   Asserted, including one backed by a signature the capturing party saw. A signature-backed
   label is not Attested until the request reference in Open items lands.
3. A verifier MAY report a `subject_conflict` warning in its Section 5 output where a Referral value
   is labelled `cardholder_device` and the chain carries a verified `agent_identity` or
   `web_bot_auth` wrapper. The warning does not fail the chain. It tells a reviewer to check
   the label before reading the value as the cardholder's. The Section 5 result has an `errors`
   member and no warnings member, and `request_binding_mismatch` (Section 3.9.11 rule 12) has the
   same gap. This draft does not add the member: `subject_conflict` lands wherever that gap is
   closed, next to `request_binding_mismatch`.

rc.2 gives the Referral artifact no request reference, so a verifier cannot itself tie a
wrapper in the Delegation artifact's `protocol_metadata` to the request a Referral value came
from. That is why rule 3 is a warning and rule 1 sits with the capturing party. See Open items.

## Renderer rule (Section 6.4)

This is stated as what AEP's renderer does, not what a network counts. What a network counts
is the network's rule (Section 3.9.12).

This amends the Section 6.4 Compelling-Evidence renderer list.

Section 6.4 populates these elements from the raw plane (Section 8.2.1), while the label sits on
the chain plane. A raw-plane value takes the label of the chain-plane field it corresponds to:
`consumer_ip_hash_subject` for customer purchase IP, `customer_device_id_subject` for device ID,
and `customer_device_fingerprint_subject` for device fingerprint. The label is the one on the
artifact the value was taken from. Where the chain holds that value on more than one artifact
(Discovery and Referral) with different labels, the label is `unknown`, as in Section 6.1 below.

The renderer MUST NOT populate the customer purchase IP, device ID or device fingerprint
element from a value labelled `agent_runtime` or `merchant_session`. A merchant server's IP
matches every order that server handled, so it says nothing about the cardholder either.

- Customer purchase IP: left empty for that order.
- Device: Section 6.4 renders device ID or device fingerprint, never both, and that OR is
  unchanged. Where one of the two is labelled `agent_runtime` or `merchant_session`, the
  renderer MAY use the other if it carries neither label. Where neither qualifies, the element
  is left empty.
- Apart from that device ID or fingerprint choice, no element is filled from a value of
  another kind in its place.

A value labelled `cardholder_device`, `unknown`, or with no label renders as it does today.

## Prior-transaction matching (Section 6.1)

When `customerHistory` is computed at export, each `identity_keys` label MUST equal the label
on the artifact the value was taken from. Where the chain holds more than one artifact
carrying that value with different labels, the label is `unknown`.

A prior transaction MUST NOT list an IP or device element in `matched_elements` where the
disputed transaction's value for that element is labelled `agent_runtime`. Two orders placed
from the same agent platform's cloud share an IP. That match says nothing about the
cardholder.

## Versioning

The change is additive: optional payload fields, a new enum on them, and two rules that only
act on the new labels. Section 0.2's MINOR list names new optional sub-wrapper fields and new
enum values for existing optional enums. It does not name optional fields on artifact payloads
or on export output such as `customerHistory.identity_keys`,
so this change carries a one-line Section 0.2 clarification:

> New optional fields on an artifact payload or on export output, including a new enum defined on such a field, are MINOR.

The clarification and the rules land with the fields, so no version carries the label without
the rule that uses it.

## Working group

The fields are Core Specification & Schemas (Sections 3.1, 3.2, 6.1). The renderer and matching rules
are Dispute Resolution & Network Rules (Section 6.4). Filed under Core Specification & Schemas, with
Dispute Resolution reviewing the Section 6.4 part, so the schema and the rule stay in one change.

## Worked example

An agent running on its platform's cloud host buys for a cardholder. The merchant's gateway
verified a TAP signature on the Referral request at capture.

```jsonc
// Section 3.2 Referral payload (fields added by this draft marked NEW)
{
  "referral_event_id":            "4c1d2e3f-5a6b-4c7d-8e9f-0a1b2c3d4e5f",
  "consumer_ip_hash":             "sha256:9f2c...",
  "consumer_ip_hash_subject":     "agent_runtime",       // NEW, gateway saw the TAP signature
  "consumer_ip_country":          "US",
  "user_agent_hash":              "sha256:1a7e...",
  "user_agent_hash_subject":      "agent_runtime",       // NEW
  ...
}
```

At export, the renderer leaves customer purchase IP empty for this order rather than send the
agent's cloud IP as the cardholder's. No device ID or fingerprint was captured on the
Referral, so those elements are empty too, and the order's match rests on the agent login ID
element proposed on #105. `customerHistory.identity_keys.ip_ref_subject` is `agent_runtime`, so no
prior transaction is matched on IP.

The full payloads are in `device-ip-subject.example.json`.

## Open items

- A request reference on the Referral artifact (for example the `request_binding` it was
  captured under), so a verifier can tie a wrapper to a value itself and rule 3 can become a
  check rather than a warning.
- A warnings member on the Section 5 result, shared by `subject_conflict` and
  `request_binding_mismatch`.
- Whether prior-transaction matching should also skip values labelled `merchant_session`, as
  the renderer now does.
- Labels on `prior_transactions` entries, so the matching rule can check both sides.
- Whether Section 3.9.12 tier computation should count the label. This draft leaves the tier alone.

## Files

| File | Purpose |
|---|---|
| `device-ip-subject.md` | This document, normative text and rationale |
| `device-ip-subject.schema.json` | JSON Schema (2020-12) for the labels and the fields they attach to |
| `device-ip-subject.example.json` | The worked example, as a validatable fixture |

The example validates against the schema. Four negative cases are checked and rejected: a
label value outside the enum, a label without its value field, a label on a null value, and
a label on a field outside the scope above.

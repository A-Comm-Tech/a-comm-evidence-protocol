# Annotation Artifact — payload schema (§6.5)

**Status:** draft for v1.1.0. Disposition for open question #5 of the v1.0.3 comment round.
**Depends on:** §2.5 (late-arrival append), §2.8 (signal classification), §4.3 (signature),
§6.5 (named-actor rights), §8.2.1 (two-plane evidence model).

§6.5 grants any actor named in an exported chain the right to append a post-seal
annotation/rebuttal, and defers the payload schema to v1.1.0. This defines it.

## Design constraints

The annotation artifact is the first artifact type whose entire purpose is to say that
something already sealed is wrong. That makes three properties load-bearing.

**It never mutates the record.** An annotation is appended, never applied. The subject
artifact's `current_hash`, `canonical_json` and signature are untouched, and chain
verification (§5) returns the same result before and after. A reader sees the original
claim and the objection to it, in that order, with both timestamps.

**It records a claim; it does not resolve one.** No implementation may derive a truth
value from an annotation, alter `verification.valid`, or re-rank evidence strength (§3.9.12)
because an annotation exists. This is the same discipline AEP already applies to fraud
scoring: record what the instrument reported, never originate the finding.

**It carries its own signal class.** An annotation is usually the weakest evidence in the
chain — an unsigned claim by an interested party, made after the dispute began. §2.8
already has the vocabulary for that, and §6.5 already sets the rule for the parallel case
(platform attribution at Inferred or Asserted "MUST be labeled 'not verified by the named
platform'"). Annotations reuse both rather than inventing a second taxonomy.

## Chain semantics

Post-seal, accepted via the §2.5 late-arrival append path, exactly as fulfillment (§3.7)
and refund (§3.10). Annotation artifacts do NOT count toward the §2.2 expected minimum
counts, and their absence never makes a chain incomplete.

The annotator MUST already be named in the chain being annotated — that is what makes this
a named-actor right rather than an open comment channel. An annotation MAY target another
annotation (rebuttal of a rebuttal); implementations MUST accept a subject depth of at
least 2 and MAY refuse deeper chains.

Annotations are never withdrawn by deletion. A retracted claim is itself annotated with
`disposition: "withdrawn"`.

## Payload schema

```jsonc
{
  // Deterministic identifiers
  "annotation_id":  "string — UUID (required)",
  "subject_ref": {                            // what is being annotated (required)
    "artifact_type":    "string — the subject's artifact_type (required)",
    "sequence_number":  "integer — the subject's position in the chain (required)",
    "artifact_hash":    "string — the subject's current_hash, 'sha256:...' (required)"
  },

  // The objection
  "disposition": "'disputed' | 'corrected' | 'contextualized' | 'withdrawn' (required)",
  "claims": [                                  // >= 1 (required)
    {
      "field_path":     "string — JSON pointer into the subject payload, or null for
                         a claim about the artifact as a whole (required)",
      "subject_value":  "the value as sealed — MUST equal the subject artifact's value
                         at field_path (required, nullable). null where field_path is
                         null, or where the field is absent from the sealed payload;
                         absence MUST be stated in basis, since a claim about a field
                         that was never recorded is weaker than one contradicting a
                         recorded value",
      "annotated_value":"the asserted replacement, or null to dispute without
                         supplying one (required, nullable)",
      "signal_class":   "'deterministic' | 'attested' | 'inferred' | 'asserted'
                         (required, §2.8)",
      "basis":          "string — short statement of how the claim is known (required)",
      "source": {                              // OPTIONAL; defaults to the annotator
        "actor_type": "string — §4.1 actor_type enum",
        "actor_id":   "string"
      }
    }
  ],

  // Authorship of the claim, distinct from custody of the record
  "annotator": {
    "actor_type":           "string — §4.1 actor_type enum (required)",
    "actor_id":             "string — MUST match an actor_id already in the chain (required)",
    "verification_method":  "'party_signature' | 'exporter_relayed' | 'none' (required)",
    "authentication": {                        // REQUIRED when exporter_relayed; otherwise absent
      "method":           "string — how the recorder authenticated the submitting actor (required);
                           free text, RECOMMENDED: authenticated_session | signed_request | out_of_band",
      "authenticated_at": "string — ISO 8601 UTC, when it did so (required)",
      "evidence_ref":     "string — 'sha256:...' digest of the recorder's authentication record (required)"
    }
  },
  "annotator_signature": {                     // OPTIONAL — see Trust model
    "alg":       "'ed25519'",
    "key_id":    "string",
    "signature": "string — base64, over the signing object in Trust model rule 5"
  },

  // Contemporaneity — both required, never conflated
  "asserted_at": "string — ISO 8601 UTC, when the actor made the claim (required)",
  "captured_at": "string — ISO 8601 UTC, when the recorder received it (required)",

  // Cross-artifact required field
  "idempotency_key": "string — stable per-annotation key (required)"
}
```

**Permitted actor types:** `user`, `merchant`, `psp`, `agent`, `fulfillment_provider`,
`credentials_provider` — restricted to actors already named in the chain.

**Signing actor:** the annotating actor signs the artifact wrapper per §4.3 where it holds
a key with standing. Where it does not, the recording implementation signs the wrapper as
custodian and MUST set `verification_method: "exporter_relayed"`.

## Trust model

This is the part that decides what an annotation is worth, and it is deliberately an
optional field rather than a mode.

A wrapper signature (§4.3) establishes **custody**: this implementation received this claim
at this time and has not altered it since. It does not establish that the named actor made
the claim. Those are different assertions and the schema keeps them apart.

An annotation sits at one of three levels, and a reader can tell which from the record:

| Level | `verification_method` | What is established | Claim class | Export label |
|---|---|---|---|---|
| Unsigned assertion | `none` | The recorder received a claim attributed to the named actor | Asserted | "not verified by the named actor" |
| Relayed after authentication | `exporter_relayed`, with `annotator.authentication` | The recorder says it authenticated the actor, and records how | Asserted | The same label, plus the authentication method and time |
| Signed by the actor | `party_signature` | The actor's key signed the annotation | MAY be Attested | None once the signature verifies |

Normative rules:

1. Where `annotator_signature` is absent, or present and failing verification, every entry
   in `claims` MUST carry `signal_class: "asserted"`, and exports (§6) MUST label the
   annotation **"not verified by the named actor."** This extends the rule §6.5 already
   states for platform attribution.
2. Where `annotator_signature` is present and verifies against a key with standing for
   `annotator.actor_id`, claims MAY carry `signal_class: "attested"`.
3. `verification_method: "exporter_relayed"` means the recorder is attesting that the actor
   told it this — not that the actor signed it. Implementations MUST NOT present a relayed
   claim as attested.
4. An annotation MUST NOT raise the signal class of any field in the subject artifact.
   Evidence gets stronger by being signed, never by being contradicted.
5. `annotator_signature` is computed over the JCS-canonical object
   `{context, annotation_id, subject_ref, disposition, claims, annotator: {actor_type,
   actor_id}, asserted_at}`, where `context` is the fixed string `aep-annotation-v1.1.0`.
   This binds the signature to the specific subject, the actor, the disposition and the
   time, so a valid signature cannot be reused against a different artifact or with
   materially different metadata. A signature that verifies only over a narrower object,
   such as the claims alone, MUST be treated as failing verification (rule 1).
6. `verification_method: "exporter_relayed"` MUST be accompanied by
   `annotator.authentication`: the method by which the recorder authenticated the
   submitting actor, when it did so, and the digest of the recorder's own authentication
   record (`evidence_ref`), so that a reader can ask the recorder for that record.
   `authentication` appears only with `exporter_relayed`. It records the basis and does not
   raise the class: relayed claims remain Asserted (rules 1 and 3). `method` is free text
   by design. A consumer MUST NOT read the value as a measure of how strong the
   authentication was.

Rule 2 is the upgrade path. Party-held annotator keys are a cross-cutting problem — key
registration, rotation and distribution for every actor who might ever annotate — and this
schema does not attempt to solve it. It is structured so that solving it later is an
additive change: `annotator_signature` appears, `verification_method` becomes
`party_signature`, and nothing already issued breaks.

## Interaction with the two-plane model (§8.2.1)

Where the subject field is a salted hash, an annotation can dispute the **derived** value
without revealing the raw one. `delivery_address_match` is a deterministic boolean computed
against `cart.shipping_address_hash` (§3.7); an annotation may assert that the boolean is
wrong while the address that would prove it stays with the Evidence Custodian.

Implementations MUST NOT place a raw value into `annotated_value` where the subject field is
hashed. The correct annotation disputes the match result and names the custodian record that
would settle it.

## Worked example — misattributed delivery scan

The subject is the fulfillment artifact of the v1.0.3-rc.2 `b-chain` vector: UPS recorded a
delivered scan with `delivery_address_match: true`. The merchant asserts the parcel went
elsewhere, and that no proof of delivery exists. Neither claim is signed by UPS.

```jsonc
{
  "annotation_id": "a1f4c2e0-8b3d-4c7a-9e21-6f0d5b8c1a33",
  "subject_ref": {
    "artifact_type":   "fulfillment",
    "sequence_number": 8,
    "artifact_hash":   "sha256:b54b688fe50dc71229520802fab6aaa8cf95c879e30e1e6d256893f32cc030fe"
  },
  "disposition": "disputed",
  "claims": [
    {
      "field_path":      "/payload/delivery_address_match",
      "subject_value":   true,
      "annotated_value": false,
      "signal_class":    "asserted",
      "basis": "Merchant reports the parcel was delivered to an address other than the
                one confirmed at checkout. Raw address held by the Evidence Custodian
                under the §8.2.1 disclosure path; not reproduced here.",
      "source": { "actor_type": "merchant", "actor_id": "acct_vector_merchant" }
    },
    {
      "field_path":      "/payload/signature_captured",
      "subject_value":   null,
      "annotated_value": false,
      "signal_class":    "asserted",
      "basis": "Field absent from the sealed payload: the fulfillment artifact recorded
                no signature_captured value. Carrier states no recipient signature or
                delivery photograph exists for tracking number 1Zvector001. Relayed by
                the merchant; not signed by UPS.",
      "source": { "actor_type": "fulfillment_provider", "actor_id": "ups" }
    },
    {
      "field_path":      null,
      "subject_value":   null,
      "annotated_value": null,
      "signal_class":    "asserted",
      "basis": "Merchant terms require a full refund where an order is not delivered to
                the checkout shipping address.",
      "source": { "actor_type": "merchant", "actor_id": "acct_vector_merchant" }
    }
  ],
  "annotator": {
    "actor_type":          "merchant",
    "actor_id":            "acct_vector_merchant",
    "verification_method": "exporter_relayed"
  },
  "asserted_at":     "2026-09-03T14:02:00Z",
  "captured_at":     "2026-09-03T20:18:04Z",
  "idempotency_key": "cafe-grinder-annotation-001"
}
```

On export this annotation carries the label **"not verified by the named actor"** for every
claim, including the one attributed to UPS — because UPS did not sign it. The chain still
verifies. The delivered scan still stands as sealed. An adjudicator sees a Deterministic
carrier scan and an Asserted objection to it, and weighs them accordingly.

That is the intended outcome. The annotation makes the dispute legible without letting the
disputed party rewrite the record.

## Chain-level indicator

An export (§6.1) MUST carry `annotation_summary`: the number of annotation artifacts in
the chain, by disposition. When the chain has no annotations, every count is zero. The
summary is always present, so a reader can tell "no annotations" from "an exporter that does
not produce the summary".

```jsonc
"annotation_summary": { "disputed": 0, "corrected": 1, "contextualized": 0, "withdrawn": 0 }
```

Counts are by disposition only. This specification does not define an annotation as
"resolved" or "outstanding", because resolving a claim is the adjudicator's act and
recording it would originate a finding. A withdrawal is counted under `withdrawn` and the
annotation it withdraws stays under its own disposition. The summary MUST NOT be used to
compute an evidence tier (§3.9.12) or to alter `verification.valid`, consistent with the
constraint that annotations are never weighed. Its purpose is that a reader learns, without
opening every artifact, that the record contains responses or corrections.

## Open items

- A registry for `annotator.authentication.method`. Free text with recommended values in
  this draft, as for `basis`. Revisit once more than one implementation shows what is used.
- Whether `basis` should be constrained to an enum for machine consumption. Leaning no for
  v1.1.0; free text is honest about what this field is.
- Party-held annotator keys, per rule 2 above. Out of scope here and deferred. It is not yet
  tracked as a separate issue.

## Files

| File | Purpose |
|---|---|
| `annotation-artifact-v1.1.0.md` | This document — normative text and rationale |
| `annotation-artifact-v1.1.0.schema.json` | JSON Schema (2020-12) for the payload |
| `annotation-artifact-v1.1.0.example.json` | The worked example above, as a validatable fixture |

The example validates against the schema. Four negative cases are also checked and rejected:
a relayed annotation carrying `attested` claims, `verification_method: "party_signature"`
without a signature, a bare (unprefixed) subject hash, and — as the positive control for
rule 2 — the same annotation with a verifying `annotator_signature` and `attested` claims
is accepted, which is the upgrade path staying open. Two further negative cases cover the
authentication record: a relayed annotation without `annotator.authentication`, and
`annotator.authentication` present under `party_signature` or `none`.

<!-- SPDX-License-Identifier: Apache-2.0 -->
# Packet 01 — dispositions for the outstanding submissions

**Status: draft, for grading. Not yet recorded.**

Closes release gate 3. Thirteen refs across eleven submissions: the ten
recorded as outstanding in the Coverage section, plus #74, which arrived after
the window and needs a scope ruling before it can carry a ref.

Nothing in this packet changes specification text. Each disposition is a
decision about what will change and where; the edits themselves are Packets
02 onward. On grading, the Index rows below are inserted into the
COMMENT-ROUND.md Index, the Detail entries into Detail, and the Coverage
section is updated to `60 submissions received · 61 dispositioned · 0
outstanding` with the received-after-window note.

---

## Guardrail gate

| Ref | 1 neutral | 2 non-gating | 3 non-adjudicating | 4 MINOR-safe | 5 class/basis | 6 custodian-free | 7 no self-witness |
|---|---|---|---|---|---|---|---|
| I65 | yes | yes | yes | yes | yes | yes | yes |
| D66 | yes | yes | yes | yes | yes | yes | yes |
| I67 | yes | yes | **see note** | yes | yes | yes | **see note** |
| I68 | yes | yes | yes | yes | yes | yes | yes |
| I69 | **see note** | yes | **see note** | yes | yes | yes | yes |
| I70-a | yes | yes | yes | yes | n/a | yes | yes |
| I70-b | yes | yes | **see note** | yes | yes | yes | yes |
| I70-c | yes | yes | yes | yes | n/a | yes | yes |
| I70-d | yes | yes | yes | yes | n/a | yes | yes |
| I71 | yes | yes | **see note** | yes | yes | yes | yes |
| I72 | **see note** | yes | **see note** | yes | yes | yes | yes |
| D73 | yes | yes | yes | yes | yes | yes | yes |
| I74 | **see note** | yes | yes | yes | yes | yes | yes |

Rows marked *see note* are cases where the submission as filed would have
failed the check and the disposition narrows it. Each is argued in the entry.

---

## Index rows

| Ref | Source | Author | Section | Disposition | Target | Edit | Response | COI |
|---|---|---|---|---|---|---|---|---|
| [I65](#i65) | Issue #65 | @Avidmock | §3.7 / §3.5 | accept-with-modification | v1.0.3-final / v1.0.4 | not started | not recorded | |
| [D66](#d66) | Discussion #66 | @Avidmock | §3.7 / §2.8 | defer (v1.0.4) | v1.0.4 | n/a | not recorded | |
| [I67](#i67) | Issue #67 | @AgroMoo | §2.8 / §2.4 / §0.1 | accept in part / decline in part / defer | v1.0.3-final / v1.1.0 | not started | not recorded | |
| [I68](#i68) | Issue #68 | @shunhe-wang | §2.6 / §3.9.10 / §3.9.11 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I69](#i69) | Issue #69 | @AstraSyncAI | §3.9.16 / §3.9.5 / §2.6 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I70-a](#i70-a) | Issue #70 | @AstraSyncAI | §3.9.11 | accept | v1.0.3-final | not started | not recorded | |
| [I70-b](#i70-b) | Issue #70 | @AstraSyncAI | §2.5 / §6.5 | defer (v1.1.0) | v1.1.0 | n/a | not recorded | |
| [I70-c](#i70-c) | Issue #70 | @AstraSyncAI | §2.2 / Appendix B | accept | v1.0.3-final | not started | not recorded | |
| [I70-d](#i70-d) | Issue #70 | @AstraSyncAI | §4.1 | accept | v1.0.3-final | not started | not recorded | |
| [I71](#i71) | Issue #71 | @Trusteedxyz | §3.4 / §2.8 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [I72](#i72) | Issue #72 | @deepakwink | §3.9 / §2.8 | accept-with-modification | v1.0.3-final | not started | not recorded | |
| [D73](#d73) | Discussion #73 | @The-JKR | §2.4 / §3.9 | defer (v1.1.0) | v1.1.0 | n/a | not recorded | |
| [I74](#i74) | Issue #74 | @rrrodzilla | §4.3 / §5 / §6.2 | accept-with-modification | v1.0.3-final / v1.0.4 | not started | not recorded | |

---

## Detail entries

<a id="i65"></a>
### I65 · Issue #65 — §3.7 in-person services cannot populate the Fulfillment artifact

**§3.7 / §3.5** · @Avidmock · **accept-with-modification** · target v1.0.3-final / v1.0.4

Confirmed against the text. §3.7 marks `carrier` and `tracking_number`
required, and neither has a value for an appointment, a home visit, or a
mobile trade call. The §3.7 actor text anticipates the absence of a
third-party carrier and resolves the *signing* question — merchant signs both
wrapper and data — while leaving the two required *fields* in place, so the
self-logistics case is addressed for signatures and unaddressed for payload
conformance.

The submission's second observation carries more weight than its first. A
merchant who synthesises `"self"` or `"n/a"` into `carrier` puts an
unverifiable string into a field whose §2.8 class implies carrier-sourced
data. That is precisely the confusion the I35 disposition acted to prevent
when it ruled that merchant-relayed carrier JSON is Asserted and never
carrier-Attested. Leaving the fields required does not avoid the problem; it
guarantees the problem, because the schema compels the placeholder.

**This is the same edit as I20** and lands as one change rather than two. I20
reached §3.7 from reserved time-and-place commerce — lodging, vehicle rental —
and this submission reaches it from in-person services; both fail on the same
four parcel-shaped required fields, and both proposed a discriminator. A
single `fulfillment_kind` discriminator with conditionally-required fields
resolves both: `carrier` and `tracking_number` become required when and only
when a third-party carrier is used, and the location-matching fields resolve
against the cart commitment appropriate to the kind. Editors will not carry
two variants of one fix.

**Scope held to the minimal change for v1.0.3-final.** The discriminator and
the conditional requirement land now, because without them the artifact is
unemittable for an entire transaction class. The evidence fields that would
carry what a service dispute actually turns on — that the service occurred, at
that place, at that time, with both parties present — are not specified here.
The submission deliberately did not propose normative text for them and filed
Discussion #66 instead; that is the correct route under CONTRIBUTING.md §3 and
is dispositioned at D66.

The absence of a fulfillment artifact continues to imply nothing about
fulfillment, per §3.9.12. That principle is what makes the minimal fix safe to
ship ahead of the fuller profile.

Declared interest recorded: the submitter operates an appointment-booking
platform and would implement any service-fulfillment profile. The defect is
provable from the specification text independently of that interest.

<a id="d66"></a>
### D66 · Discussion #66 — service-fulfillment variant: status-chain commitment and customer counterconfirmation

**§3.7 / §2.8** · @Avidmock · **defer (v1.0.4)** · target v1.0.4

Deferred to v1.0.4 and sequenced with the Proof-of-Delivery sub-profile
already deferred there under I35, which contemplates recipient-held key
confirmation for the parcel case. A customer counterconfirmation for the
service case is the same primitive reached from the other side, and specifying
them together avoids two incompatible constructions of recipient
confirmation. The per-field §2.8 classes the proposal asks for will follow the
general provenance ladder rather than a §3.7-local rule.

The minimal fix that makes the artifact emittable for this transaction class
does not wait on this work and lands in v1.0.3-final under I65.

*Provisional: drafted from the description carried in Issue #65 rather than
from the discussion thread. To be checked against the thread before
recording.*

<a id="i67"></a>
### I67 · Issue #67 — evidence completeness, provenance and interoperability

**§2.8 / §2.4 / §0.1** · @AgroMoo · **accept in part / decline in part / defer** · target v1.0.3-final / v1.1.0

Four proposals, receiving three different answers.

**Integrity is not completeness — accepted, as an explicit non-claim.** The
distinction is correctly drawn and the specification does not currently state
it: a record can be immutable and still incomplete, and the hash chain
guarantees only that what was submitted was not altered afterwards. Text
stating that AEP makes no completeness claim lands in §0.1 for v1.0.3-final,
alongside the §3.9.12 principle it sits next to. This is a clarification of
what the specification already does not do, not a new capability, and it
protects against exactly the over-reading that would let a sealed chain be
presented as a complete account of a transaction.

**Provenance classification — accepted, routed to the existing mechanism.**
The observed/self-reported boundary is the subject of I43, already
accept-with-modification, and the vocabulary will be the one shared with
§3.9.N's `issuer_relationship`. Editors will not carry a second, parallel
provenance construct. One element of the proposed three-category model is
genuinely additive and is taken: a marker distinguishing evidence generated in
simulation, test, or reconstruction environments from evidence generated in
live execution. Cheap, and it prevents a synthetic chain from being presented
as an observed one — which the specification currently has no way to refuse.

**`participant_context` — declined.** §2.4 already names the actor classes and
the relationship vocabulary under I43 will make the participant-to-artifact
relationship inspectable. A further structure whose stated purpose is
"structured relationships between participants and evidence artifacts" for
downstream dispute resolution is, in substance, an allocation surface, and the
specification does not build those — the same ground on which the I37 weighted
confidence model was declined. The submission explicitly asks that it not
calculate liability; the editors accept that intent and still decline the
structure, because a structure's uses are not controlled by its author's
intent.

**Cross-domain evidence portability — deferred to v1.1.0.** AEP is
transaction-scoped by design, and §3.1's Discovery scoping is a deliberate
limit rather than an omission. The right vehicle is D12-a's split of the
specification into a transaction-neutral core plus profiles, already deferred
to v1.1.0; a portable evidence object model is a profile question and cannot
be answered before that split exists.

*Note on guardrail 7.* This submission asks for the independent-observer
boundary more directly than any other in the round, and the editors' working
rule — that a party acting as agent in a transaction cannot be the sole
attestor of its own conduct, while a party that is observer and custodian
without being the agent is unconflicted — is not in the specification text.
Whether that rule becomes normative in v1.0.3-final is tracked as an open
editorial item, not settled by this disposition.

<a id="i68"></a>
### I68 · Issue #68 — x402 payer authorization and facilitator identity are conflated

**§2.6 / §3.9.10 / §3.9.11** · @shunhe-wang · **accept-with-modification** · target v1.0.3-final · *security*

Confirmed, and the defect is in the specification rather than in a reading of
it. §2.6 gives the x402 key-discovery path as `facilitator_signer` recovered
from the EIP-712 signature; §3.9.11 rule 7 recovers the signer from
`x402_payment_signature` and asserts it equals `payer`. Both sentences
describe the same recovery and name its output as two different actors. A
signature over a payment payload establishes that the payer authorized the
payment. It does not, by being that signature, establish who verified or
submitted it.

The four divergent implementations the submission enumerates all follow from
the text, and one of them — reporting both payment authorization and
facilitator identity as verified when only the payer signature was checked —
is an evidence-integrity failure, not merely an interoperability defect. A
recipient reads a verified facilitator identity that nothing verified.

**Accepted:** payment-payload signature recovery is corrected to payer
authorization throughout §2.6 and §3.9.11; facilitator identity is carried and
verified separately, with its declared source and resulting §2.8 class; and
rail roles that the schemes distinguish — transaction submitter, operator,
capture authorizer — are preserved under separate names, with equality
between roles recordable where demonstrated and never assumed. Guardrail 5
applies directly: a recipient must be able to read which actor claim each
successful check satisfied.

**Modified in two respects.** The proposed field names are not adopted as
given; the submission itself notes the names matter less than the separation,
and the naming will follow the verification-result schema rather than sit
beside it. And the separation is not implementable until that schema can
express a state meaning *no rule was defined* — the same dependency on I15
that I60 carries.

**This lands with I60, not separately.** I60 adjudicated rule 7 as the
complete specification of x402 signature verification in v1.0.x. That
adjudication stands; what changes is the label on what rule 7 recovers.
Landed as two independent edits, the two dispositions would restate the
conflation this one removes. The vectors requested here — payer and
facilitator distinct, payer and facilitator and capture authorizer all
distinct, valid payer signature with unverified facilitator declaration, and
the negative case of an implementation equating the two — are added to the
v1.0.3-final vector set and are the conformance check that this fix holds.

<a id="i69"></a>
### I69 · Issue #69 — identity-transport slot is vendor-shaped where §3.9.N is issuer-neutral

**§3.9.16 / §3.9.5 / §2.6** · @AstraSyncAI · **accept-with-modification** · target v1.0.3-final

The submission asks the editors a direct question — whether the
identity-transport layer is assumed to hold multiple issuer profiles or to
converge on one — and the log answers it directly: **multiple.** The
identity-transport layer is multi-issuer, on the same construction §3.9.N
already uses.

The inconsistency is real and is against the editors' own newer design.
§3.9.16 states that `SKYFIRE_KYA` denotes a protocol family that is
multi-issuer by design and does not denote a single issuer, and the field
naming does not carry that intent: the generic category name `kya_identity`
occupies the identity-transport slot while the schema inside it is one
issuer's claim structure. §3.9.N, accepted three weeks earlier under D4-a,
carries `issuer` plus an `issuer_relationship` enum and describes the
relationship rather than naming the vendor. Two layers doing a related job
answer the same question differently.

The consequence the submission identifies is the one that decides this.
A credential inside the slot reaches B-tier paired with a session binding; a
credential with identical cryptographic properties that cannot fit the slot
reaches D-tier. **Tier then tracks schema fit rather than evidence strength**,
which is not a defensible position for a specification whose §3.9.12
disclaimers rest on tiers describing verifiability. The I26 disposition
relieves part of the fallthrough — a `verified_identity` sub-wrapper now makes
Delegation present in a GENERIC chain, so an issuer outside the slot is no
longer unable to record identity anywhere — but relief from the worst case is
not the same as the slot being neutral.

**Accepted, by the additive path.** The slot gains an issuer field and a
profile discriminator, with the existing field set retained as one named
profile so that chains written against rc.2 stay valid and no re-versioning
occurs; §3.9.11's issuer-specific rule becomes profile-dispatched. This keeps
the change MINOR under §0.2. The alternative — restructuring the wrapper's
internals in place — would be cleaner and is not worth a MAJOR.

This is also the mechanism open question 3 requires for an independently
versioned extension registry at v1.1.0, and settling it here rather than there
means the registry inherits a neutral slot instead of retrofitting one.

Declared interest recorded: the submitter operates an agent-identity service
and would implement the slot. The submission is argued as a consistency
question against the editors' own design and proposes no text favouring the
submitter's credential format; the inconsistency is verifiable from §3.9.16
and §3.9.N alone.

*Apache §5 note: this submission is marked as not a Contribution. The
resulting text will be authored independently from the identified defect
rather than adapted from the submission's suggested direction.*

<a id="i70-a"></a>
### I70-a · Issue #70 — §3.9.11 rule 2 skew bound conflicts with pre-issued mandates

**§3.9.11** · @AstraSyncAI · **accept** · target v1.0.3-final

Correct, and the reading the submission proposes is the intended one. Rule 2's
24-hour bound between sub-wrapper timestamps and the delegation artifact's
`captured_at` is aimed at per-request envelopes — TAP, Web Bot Auth,
`request_binding` — where a stale timestamp is evidence of replay. An AP2
autonomous-mode Open Checkout Mandate is signed in advance of the transaction
it authorizes, potentially by weeks, and §3.9.7 carries it; read strictly,
rule 2 fails a conformant AP2-Autonomous chain and makes the §3.9.12 A-tier
path unreachable in the common case.

The current phrasing puts the general bound first and the per-protocol
carve-out second, which is what produces the strict reading. v1.0.3-final
states the rule the other way round: pre-issued credentials are bound by their
own `exp`, and the 24-hour bound applies to per-request envelopes. The same
treatment is applied to §3.9.N's `expires_at` when that text lands under D4-a,
so the question the submission anticipates does not arise a second time.

<a id="i70-b"></a>
### I70-b · Issue #70 — no artifact records the dispute or its outcome

**§2.5 / §6.5** · @AstraSyncAI · **defer (v1.1.0)** · target v1.1.0

Deferred to v1.1.0 and sequenced with the §6.5 annotation artifact, whose
late-append mechanics it shares.

The structural argument is the strongest cross-cutting observation received in
this round and is recorded here because it will outlive the deferral. This
round contains at least three separate arguments that the §3.9.12 tier ladder
mis-ranks something — I59 on auth-capture settlement timing, I37 proposing a
weighted alternative, I16 on what a recipient can evaluate at all — and the
corpus contains no mechanism by which the ladder could ever be tested against
observed outcomes. A post-seal artifact carrying reason code, submission
reference and resolution would give it an empirical basis, and would let
disagreements of this class eventually be settled by data rather than by
argument.

The deferral is about construction, not merit. A dispute-outcome artifact sits
one step from the thing this specification does not do, and the line has to be
drawn in the text before the artifact exists rather than after: AEP would
record that a dispute occurred, under which reason code, and how it resolved,
as facts with their own provenance, and would derive nothing from them. No
tier, no score, and no recomputation of any existing field may consume the
outcome. Specifying that boundary is v1.1.0 work and it is not safe to rush it
into an additive release.

<a id="i70-c"></a>
### I70-c · Issue #70 — §2.2 prose and Appendix B disagree on expectedArtifactCount

**§2.2 / Appendix B** · @AstraSyncAI · **accept** · target v1.0.3-final

Confirmed. §2.2's parenthetical states the function returns 8/7/6/5 as of
v1.0.2; Appendix B returns those values only when `protocol !== 'GENERIC'` and
7/6/5/5 otherwise. The surrounding §2.2 prose describes the GENERIC case
correctly, so this is a reconciliation of the parenthetical rather than a
substantive disagreement — but two passages stating different things about the
same function is exactly the defect the disposition log exists to close.
Editorial, lands in v1.0.3-final.

Reconciled together with the I26 change, which makes Delegation present in a
GENERIC chain when a `verified_identity` sub-wrapper is populated and
therefore alters the GENERIC counts conditionally. Landing the two
independently would reintroduce the mismatch this item removes.

<a id="i70-d"></a>
### I70-d · Issue #70 — §4.1 hash-input comment omits `refund` from the artifact_type enum

**§4.1** · @AstraSyncAI · **accept** · target v1.0.3-final

Confirmed. The inline comment in the §4.1 formula lists eight artifact types;
Appendix B's `ArtifactType` includes `refund`, and §2.7 and §3.8 both carry
it. Editorial, lands in v1.0.3-final with the other foundation corrections.

<a id="i71"></a>
### I71 · Issue #71 — `policy_version` is a self-asserted label

**§3.4 / §2.8** · @Trusteedxyz · **accept-with-modification** · target v1.0.3-final

Accepted. The observation that carries it is that everything else in the chain
is hash-bound and the policy identifier is not: a verifier reading a version
string cannot check that any rule set corresponds to it, cannot detect a
relabel after the fact, and cannot compare two artifacts claiming the same
version. In a specification whose purpose is tamper-evidence, the one field
that says which controls were in force is the one field that cannot be
checked.

The construction is right for the constraint. Rule parameters are merchant
confidential and in aggregate describe how to evade them, so shipping the rule
set is not available; a hash over a canonicalized rule set answers *which
policy* without disclosing it. Version and hash are both retained, for the
reason the submission gives — the hash proves what the policy was, the version
says which one to ask for — and both resolve from the same record, so a
verifier is not left sweeping a producer's history or resolving "the most
recent one" against a signed artifact that meant a specific one.

**Two constraints are normative rather than advisory.**

1. The commitment covers the hashed rule-set bytes and nothing else. It MUST
   NOT be read as evidence that those rules were applied to produce `outcome`,
   `rules_triggered` or `risk_score` in a given evaluation. Committing to a
   rule set is not evidence an engine ran it, and the submission is right to
   write that scope into the proposal rather than leave it implied.
2. The signal class is Attested per §2.8, not Deterministic. A recipient
   holding the disclosed rule set can recompute the hash; one who has not
   received that disclosure can verify the signature and treat the hash as an
   opaque comparable commitment. Those are different verifier contracts and
   the field's class states which applies.

**Not adopted as an answer to open question 4.** The submission observes that a
compact signed summary a rail can consume needs a content-addressed
identifier, and that is correct as far as it goes, but the
network-consumable attestation is I28 and D22's work and is bounded by the
D4-b decline: no artifact this specification defines becomes a settlement
precondition. A content-addressed policy identifier is descriptive and is
accepted on that basis alone.

Lands with the §3.4 edit carrying I49, I50, I51, I53 and I54. All six
submissions describe one defect from six directions — the artifact records a
verdict without recording the provenance or mode of the evaluation that
produced it — and §3.4 is rewritten once.

<a id="i72"></a>
### I72 · Issue #72 — enrollment provenance behind a verified identity

**§3.9 / §2.8** · @deepakwink · **accept-with-modification** · target v1.0.3-final

Accepted, and folded into the `verified_identity` sub-wrapper accepted under
I26, whose text has not yet been written. Adding the fields now costs one
edit rather than two and avoids a wrapper that ships and is immediately
extended.

The gap is correctly stated. A record reading `verified: true` is identical in
two materially different cases: a credential enrolled under supervision with
documentary identity checks, and one enrolled unsupervised by the shopper
minutes earlier in the same session, where the verification confirms only that
the person matches a template they had just created. §3.9.12 credits the
verification without knowing anything about the enrolment behind it.

The ask is the right one and is the reason this is accepted rather than
deferred: the submission asks that the basis become inspectable, not that the
weighting change. That is the settled line running through I33, I35, I56 and
I43 — a field exists so a recipient can read the basis for a classification
rather than infer it — and this is the same principle applied to
identity. All fields optional and nullable, each classified under §2.8, each
provider populating what it holds. Nothing existing changes and no
implementation must add anything to stay conformant.

**Two modifications.** The enrolling party's assurance level is carried as a
foreign label — the issuing scheme's own value, recorded as stated — and AEP
neither maps it to a tier nor ranks schemes against each other; carrying a
label is description, mapping it would be adjudication. And the fields are
specified against the §2.4 `identity_verifier` role rather than against any
named provider, so that any party performing the role can populate them.

Declared interest recorded: the submitter operates an identity verification
service that would populate these fields.

<a id="d73"></a>
### D73 · Discussion #73 — organisation-issued commercial authority and external determinations as witnessed references

**§2.4 / §3.9** · @The-JKR · **defer (v1.1.0)** · target v1.1.0

Deferred to v1.1.0 and sequenced with the B2B principal work already routed
there: D12-b's organization slot for B2B principals and D12-c's generalization
of `mit_mandate_ref` to a governing-document pointer. The three describe one
profile and specifying them separately would produce three overlapping ways to
reference an authority a natural person did not issue.

*Provisional: drafted from the submission title and its relationship to D12-b
and D12-c rather than from the discussion thread. To be checked against the
thread before recording, in particular whether the external-determinations
element is separable from the commercial-authority element and warrants its
own ref.*

<a id="i74"></a>
### I74 · Issue #74 — transparency-log anchoring has no receipt format

**§4.3 / §5 / §6.2** · @rrrodzilla · **accept-with-modification** · target v1.0.3-final / v1.0.4 · *received after window — taken in scope*

**Scope ruling first.** Received 3 September, twenty days after the window
closed. Taken in scope for v1.0.3-final, on the ground that it reports a
defect in text this round is still editing rather than proposing a capability:
§4.3 already ships three anchoring methods and §5 and §6.2 already key
verification classes off them. Shipping v1.0.3-final with a verification class
whose meaning is implementation-defined, having been told it is, is not a
defensible use of the remaining window. Recorded in Coverage as
received-after-window so the process stays legible; this ruling is not a
precedent that the window is open.

The defect is confirmed. Of §4.3's three methods, the first has a defined
format in RFC 3161 and the third is self-describing; the second names no log
entry format, no receipt format, no inclusion-proof structure, and no rule for
what a verifier checks. Two conforming verifiers can therefore disagree on
whether a bundle is anchored — one accepting an opaque log identifier, the
other requiring an inclusion proof — and a verification class the
specification defines becomes implementation-defined in practice. The
submission was found by an independent verifier implementation, which is the
evidence that matters: there was nothing to verify against.

**Accepted for v1.0.3-final:** a normative receipt structure for the
transparency-log method, carried in an optional `anchors[]` element with the
digest committed to, the log identity, and the inclusion-proof coordinates;
and a verifier rule stating that the anchored class requires a valid inclusion
proof over the sealed hash, so the class stops depending on which verifier
reads it.

**Modified in three respects.**

1. **No named implementation.** The submission proposes a specific
   transparency service and links a specific repository. The receipt structure
   is specified in AEP's own terms against RFC 9162, which is a published RFC
   and safely incorporable under §0.1. A named implementation cannot be the
   slot, in this layer any more than in §3.9.16.
2. **SCITT is non-normative.** The submission's payload-binding construction
   is sound, but §0.1 incorporates referenced documents at the version
   published as of this specification's publication date, and an IETF draft is
   a moving target. SCITT interoperability is recorded as a non-normative
   implementation note, and AEP does not take a dependency on a draft that can
   change under it.
3. **Consistency proofs defer to v1.0.4.** The proposed second class —
   requiring a checkpoint consistency proof in addition to an inclusion
   receipt — is a real strengthening and is not needed to close the
   ambiguity. It defers with the key-discovery and revocation work under I36.

**The terminology collision is accepted and is not cosmetic.** Transparency-log
practice uses *witness* for a party co-signing a log checkpoint; this
specification uses it for the cross-protocol observation record, and "AEP is a
witness, not a competitor" is the sentence the positioning rests on. §4.3
adopts *checkpoint co-signer* for the log-side party.

The submitter offers an independent implementation and conformance vectors,
including the negative cases — altered seal, receipt from a different log,
gapped checkpoint. Those are taken for the v1.0.3-final vector set. An
independently written verifier is the strongest available check that the
receipt structure is verifiable by someone who did not write it, and this item
should not close without one.

Depends on I3: what the receipt commits to is the sealed hash, and I3 is
where the bundle signature and the chain-level field envelope get defined.
Ordering is foundation first.

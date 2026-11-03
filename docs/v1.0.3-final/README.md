<!-- SPDX-License-Identifier: Apache-2.0 -->
# v1.0.3-final — execution plan

Working plan for closing the v1.0.3 comment round. This file is process, not
specification: nothing here is normative, and nothing here changes a
disposition already recorded in [COMMENT-ROUND.md](../../COMMENT-ROUND.md).

## Where the round stands

The comment window ran 13 July – 14 August 2026 and is closed. Sixty
submissions were received. The disposition log records 56 refs covering 50 of
them, and the four release gates track what remains.

| Gate | State |
|---|---|
| 1 · Conformance vectors published at `vectors/v1.0.3` | (a), (b), (h) + keys published; (c) blocked on I3; (d)–(g), (i)–(m) pending |
| 2 · Reference-implementation alignment items closed | **not tracked in this repository** — see *Unknowns* below |
| 3 · All comment dispositions recorded | 10 outstanding, plus #74 received after the window |
| 4 · Every accepted disposition carried into spec text | **41 refs read `Edit: not started`; 1 reads `landed`** |

Gate 4 is the work. A disposition is a decision; it is not the change.

## The organizing principle

**Edit by primitive, not by issue.** The 41 owed edits are not 41 independent
changes. They land on a small number of shared primitives that reviewers
reached from different directions:

```
§3.9.11  10 refs   §2.8    8 refs   §3.4   6 refs   §4.3   5 refs
§3.6      4 refs   §3.9    4 refs   §5     4 refs   §6.1   4 refs   §8.2.1  4 refs
```

Eight submissions independently ask for a §2.8 classification fix. Patched one
at a time they produce eight subtly different ladders and a specification that
contradicts itself. Written once as a general rule, all eight resolve
consistently and the later ones become one-line references.

The same holds for verification. Ten submissions want §3.9.11 rules, but four
of them — I15, I21, I58, I60 — are asking one question: *how does a recipient
tell "checked and passed" from "no rule existed"?* That needs one answer in
§5, after which nine wrapper-level rules have somewhere to land.

So: shared primitive first, per-issue application second. That is what keeps
the answers complementary rather than merely individually defensible.

## Guardrail gate

Seven checks. Every pull request that touches specification text carries a
completed gate table in its description. A `no` on any row blocks the merge.

| # | Check | Why it exists |
|---|---|---|
| 1 | **Vendor-neutral.** Does it name a role where a vendor name is not required? A named protocol may be carried as one profile among several; it may never *be* the slot. | The witness-not-competitor posture. Live in #69 and #74. |
| 2 | **Non-gating.** Does it avoid creating a settlement precondition or an obligation on a rail? | §9.2 / §9.3. Settled by D4-b; preserved in the I40 reframe. Pulled at by I58, I59, I60, #68. |
| 3 | **Non-adjudicating.** Does it avoid assigning weight, outcome, or liability? | §3.9.12. Settled by the I37 decline. Pulled at by I59, I43, #67, #70-b. |
| 4 | **MINOR-safe.** Does it leave hash-input semantics unchanged? | §0.2 puts canonical-JSON and hash-semantics changes in MAJOR. I17 is the sole erratum exception, and only because the prose misdescribes the RFC it already incorporates. |
| 5 | **Class stated, basis inspectable.** Does every new field declare its §2.8 signal class, with the *basis* for that class readable from a field rather than inferred? | The settled I33 → I35 → I56 → I43 line. Apply it; do not relitigate it. |
| 6 | **Custodian-free core.** Does it leave core-chain conformance achievable without operating a raw plane or an Evidence Custodian? | I7, and the separation-of-roles commitment in GOVERNANCE.md. |
| 7 | **No self-witnessing.** Does it avoid letting a party be the sole attestor of its own agent's conduct? A participant acting as agent needs an observer; a party that is the observer and vault maintainer, and not the agent, is unconflicted. | Editorial policy, **not currently in the specification**. See *Unknowns*. |

## Critical path

Dispositioning the outstanding submissions is a prerequisite for the edits they
feed — but only two of them gate the foundation work, so this does not
serialize into phase-after-phase.

```
#70-a,c,d ─┐
#74 ───────┴─▶ FOUNDATION ──────────────────────────────┐
               I17 · I3 · §2.2/App-B counts · §4.1 enum  │
                                                         ▼
#69 ─▶ D (wrapper slot shape)  ─┐                    EXPORT (§6)
#67 ─▶ B (provenance ladder)  ──┼─▶ PRIMITIVES ─┐    I16 · I7 · I34 · I40
I15 ─▶ C (verification result) ─┘               │    I18 · D22 · #74 anchors[]
                                                ▼
#71 ─▶ 4a §3.4 policy        ─┐
#72 ─▶ 4c §3.9 wrappers      ─┼─▶ ARTIFACT TRACKS (parallel)
#68 ─▶ 4d settlement / x402  ─┤
#65, D66 ─▶ 4b §3.7 fulfil.  ─┘
```

Three hard edges:

- **I3 gates vector (c)** and gates I16's exporter identity and supersession.
- **#68 must land with I60.** #68 corrects the premise — payer is not
  facilitator — that I60's rule-7 analysis rests on. Landed separately they
  contradict each other in §2.6 and §3.9.11.
- **I15 gates I21, I58, I59, I60 and #68.** Each needs a verification-result
  state meaning *no rule was defined*; none of them is implementable until §5
  can carry one.

Two edges are softer than they look: the §3.4 track (4a) and the §3.7 track
(4b) touch no primitive that the others need, so they can run at any point
after their dispositions are graded.

## Working cadence

Two people. The unit of work is a **packet**: one document, one grading pass,
one merge.

1. **Draft.** A packet is produced here containing the proposed text, a
   completed guardrail-gate table, and an explicit list of what changes in the
   repository.
2. **Grade.** The packet is reviewed outside this loop and returned with
   corrections.
3. **Complete.** The graded text is implemented verbatim, pushed to a branch,
   and opened as a pull request. Corrections are applied as given; where a
   correction conflicts with a recorded disposition or a guardrail row, that is
   raised rather than silently reconciled.

Packets are sized so a grading pass is one sitting. A packet never mixes a
disposition with the spec edit it authorizes — deciding and changing stay
separate, which is the distinction gate 4 exists to enforce.

| Packet | Contents | Unblocks |
|---|---|---|
| 01 | Dispositions for the 11 outstanding submissions | Gate 3; foundation and all four artifact tracks |
| 02 | Foundation edits — I17, I3, §2.2/Appendix B counts, §4.1 enum | Gate 1 (vector c); export track |
| 03 | Primitive B — §2.8 provenance ladder | 8 refs + #67, #72 |
| 04 | Primitive C — §5 verification result schema | 5 refs + #68 |
| 05 | Primitive D — §3.9 wrapper slot shape | #69; open question 3 |
| 06–09 | Artifact tracks 4a–4d | 22 refs |
| 10 | Export and recipient-relative verification | 9 refs |
| 11 | Vectors and CI | Gate 1 |
| 12 | Appendix D changelog, gate close-out, tag | Gates 1–4 |

## Decisions owed

Four, held outside the packets because they are not editorial:

1. **#69 / Primitive D** — does the identity-transport layer hold multiple
   issuer profiles, as §3.9.N already does, or converge on one? The submission
   asks the editors this directly and the log should answer it in those terms.
2. **I18 safe harbour** — narrow §6.5 to a non-attribution rule, or keep the
   broader evidentiary exclusion? Legal and strategic. The second question in
   that submission — whether a use restriction belongs in a conformance clause
   of an Apache-2.0 specification headed for a standards body — bears directly
   on the GOVERNANCE.md trajectory and should be settled before v1.0.3-final
   rather than inherited by whoever receives the specification.
3. **I3** — make the reference implementation's behavior normative, define the
   bundle signature, or both. Option 1 alone leaves vector (c) blocked.
4. **#74 scope** — received 3 September, after the window. Recommendation and
   rationale are in Packet 01.

## Unknowns

- **Gate 2 is unverifiable from here.** "Reference-implementation alignment
  items closed (spec §3.2/§3.5/§3.6 migration notes)" names work in a
  repository this one does not contain. Nothing here can observe its state, and
  no issue tracks it. It needs an owner and a visible tracking item, or it will
  be the gate that is discovered open on release day.
- **Guardrail 7 is unwritten.** The no-self-witnessing rule is applied as
  editorial policy but appears nowhere in the specification: there is no
  self-attestation language, no conflict-of-interest rule, and "observer"
  occurs once, in a §9 positioning table. Three submissions — #43, #67 and I7 —
  turn on exactly this boundary. Either it becomes normative text in
  v1.0.3-final or the gate is enforcing a rule implementers cannot read.
- **Discussions #66 and #73** are dispositioned provisionally in Packet 01;
  both drafts were written from adjacent submissions rather than the threads
  themselves and are marked for review against them.
- **Commenter responses.** 45 of 56 refs read `Response: not recorded`. The
  log's own text makes this a legitimacy question — a decline the commenter
  accepted and a decline the commenter contests are different public facts.
  Soliciting these is cheap and independent of every other workstream.

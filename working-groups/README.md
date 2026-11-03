# Working groups

Working groups (WGs) are where focused drafting, review, and decision-making happen. They feed the one normative specification; they do not fork it.

> **Status: provisional.** The group structure, scopes, and leads below are a starting proposal. They take effect when ratified by a vote of participating members (see [GOVERNANCE.md](../GOVERNANCE.md)). Charters stay marked *Provisional* until then.

## Groups

| Folder | Scope |
| --- | --- |
| [`core-spec-schemas/`](core-spec-schemas/CHARTER.md) | Artifact schemas, hash-chain primitives, serialization |
| [`dispute-network-rules/`](dispute-network-rules/CHARTER.md) | Exportable bundles vs. network dispute rules, representment pipelines, reason codes |
| [`identity-delegation/`](identity-delegation/CHARTER.md) | Consent structures, biometric authorization bindings, credential attestation envelopes |
| [`tokenization-privacy/`](tokenization-privacy/CHARTER.md) | Secure enclave, tokenization vault, and credential isolation integration standards |
| [`reference-implementations/`](reference-implementations/CHARTER.md) | Open-source client libraries and the conformance matrix |

## How the pieces relate

- `spec/` is the single normative source and one SemVer line. Only editors merge into it.
- `vectors/` defines conformance. Any group may contribute vectors; editors merge them.
- `profiles/` is reserved for versioned profiles that reference a core spec version. It stays empty until a group is chartered to publish one.
- Each group folder holds its own `drafts/`, `decisions/`, and `meetings/`.

## Lifecycle of a change

1. **Discussion.** Open a Discussion in the group's category before drafting text (CONTRIBUTING.md).
2. **Group draft.** Text lands in the group's `drafts/` through a pull request.
3. **Group approval.** The group records its decision in `decisions/`.
4. **Comment round.** Public review with a disposition for every comment (COMMENT-ROUND.md).
5. **Editor merge.** Editors fold approved text into a versioned release under spec §0.2.

Changes that touch hash or signature semantics are MAJOR. They need sign-off from `core-spec-schemas` and matching vectors from `reference-implementations` before merge.

## Operating rules

- **Cadence:** each group meets biweekly. Agendas are posted ahead and minutes are published in the group's `meetings/` folder.
- **Leads:** each group has a lead chosen by vote, who may be an outside expert or an executive from a participating organization. Terms and disclosures go in the charter.
- **Openness:** anyone may read the spec, join discussions, and file issues.
- **Neutrality:** the neutrality rules in GOVERNANCE.md and CONTRIBUTING.md apply in full. Folder names, charters, and minutes do not name vendors or rank implementations.
- **DCO:** every commit is signed off, as everywhere in this repository.

## Starting or changing a group

Copy [`_template/`](_template/) and propose the new charter in a pull request. Chartering, merging, or retiring a group is a member vote.

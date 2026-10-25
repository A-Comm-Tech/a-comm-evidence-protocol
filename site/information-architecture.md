# AEP public site: information architecture

Status: **draft for review.** Planning material only. Not normative; if anything here disagrees with the specification, the specification wins.

Specification referenced: A-Comm Evidence Protocol (AEP) Draft v1.0.3-rc.2, published 2026-07-02, Apache 2.0.

## 1. Purpose of the site

Explain, in plain terms, the gap AEP addresses and how the protocol closes it:

> When a consumer agent buys from your store, you cannot see who it was, what it was authorized to do, or whether the interaction was tampered with.

The site explains the lifecycle, the mechanism (canonicalize, link, seal, verify), who governs the specification, and how to read, implement and challenge it. It does not sell anything.

Voice: precise, short sentences, no exclamation points, no em dashes. Observational verbs: records, witnesses, captures, verifies.

## 2. Sitemap

```
/                              Home
/lifecycle                     The eight segments, overview
  /lifecycle/discovery
  /lifecycle/referral
  /lifecycle/intent
  /lifecycle/delegation
  /lifecycle/policy
  /lifecycle/cart
  /lifecycle/authorization     (the seal)
  /lifecycle/fulfillment       (fulfillment and refund, post-seal)
/how-it-works                  Canonicalization, hash chain, seal, verification, DisputeBundle
/demo                          Simulated chain update (illustrative data only)
/protocols                     Protocols AEP records, neutral list
/governance                    Who is in charge, who is not, how decisions are made
/implement                     Role quickstarts, test vectors, conformance
/participate                   Issues, discussions, comment rounds, DCO, security reports
/faq                           Evidence vs. liability, privacy, what AEP is not
/spec          -> external     Current draft (versioned URL) and repository
/legal                         License, trademark notice, privacy notice for the site
/llms.txt  /robots.txt  /sitemap.xml
```

Not in the sitemap: member lists, partner or customer logos, endorsements, rankings, pricing.

## 3. Navigation

Primary (max seven): Lifecycle, How it works, Protocols, Governance, Implement, Spec (external), GitHub (external).

Primary action: "Read the draft specification". Secondary: "See a chain update".

Footer: Spec, GitHub, Governance, Security, License, Privacy.

## 4. Pages: purpose and outline

### Home `/`
Purpose: state the problem, show the lifecycle, show the mechanism, say who governs.
1. Hero: the problem sentence; four-sentence mechanism; two actions.
2. Lifecycle map: eight segments left to right, seal at authorization, fulfillment and refund post-seal. Conditional segments tagged.
3. Simulated chain update (see `/demo`).
4. How it works: canonicalize, link, seal, verify and export.
5. Protocols AEP records: four example rows, link to `/protocols`.
6. Who is in charge, who is not.
7. One quote: "For agentic commerce to scale, it needs to be observable, traceable, and auditable." (Krystal Gracier)
8. What the standard covers.
9. Participate: spec status, implement, contribute.

### Lifecycle `/lifecycle` and segment pages
Purpose: one page per segment. Outline per segment page:
- What the artifact records, in one sentence.
- When it is present (always, conditional, or post-seal), per the transaction type.
- Key fields, linked to the spec section.
- Signal class of typical fields (deterministic, attested, inferred, asserted).
- What it does not prove.
- Previous and next segment.

### How it works `/how-it-works`
Purpose: the mechanism, without marketing.
- RFC 8785 JSON Canonicalization.
- SHA-256 chain with `previous_hash` inside each hash input.
- Ed25519 seal at authorization; `sealed_bundle_hash`.
- Post-seal appends (fulfillment, refund, annotations) and why they never rewrite the seal.
- External anchoring of the sealed hash.
- Verification steps and verification classes.
- DisputeBundle export.
- Link to test vectors.

### Demo `/demo`
Purpose: show a chain growing and breaking when tampered with.
- Client-side only, no network calls, fixed sample JSON.
- Hashes may be computed in the browser, and are always labelled illustrative.
- Persistent label: "Illustrative. Simulated data. These hashes do not correspond to any real transaction."
- Controls: step, play, alter a field, reset. Motion respects `prefers-reduced-motion`.

### Protocols `/protocols`
Purpose: list the protocols and standards AEP wraps, neutrally.
- Columns: name, version referenced by the spec, status in AEP (wrapped or provisional), primary-source URL.
- Alphabetical. No logos, no comparison columns, no qualitative judgments.
- Entries the spec marks provisional are labelled provisional.
- Version and URL taken from spec §2.6 and re-checked against the primary source before publication.

### Governance `/governance`
Purpose: say plainly who is in charge and who is not.
- Today: maintainer group, stewarded during the bootstrap phase by A-Comm Technologies, Inc. (per `GOVERNANCE.md`).
- How decisions are made: public review, comment rounds, public dispositions, versioning policy (spec §0.2).
- Path: external maintainers, published charter, standards-body liaison.
- A multi-stakeholder consortium is proposed and not yet formed. This page says so until a charter is published.
- Not in charge: AEP does not decide disputes, does not assign liability, does not require any company's product, and requires no registration, fee or permission.
- Neutrality rules: no vendor endorsements, no partnership announcements, no competitive rankings, no liability assertions.

### Implement `/implement`
Purpose: get an implementer to a verified chain.
- Quickstarts: merchant, agent platform, PSP and acquirer (from `docs/implementers/`).
- Test vectors (`vectors/`).
- Minimal Merchant Profile and Minimal Platform Profile.
- Independent implementations, listed without ranking, only after they exist.

### Participate `/participate`
Purpose: how to comment, file errata, propose changes, report security issues.
- Issues and discussions; comment rounds and disposition log (`COMMENT-ROUND.md`).
- Apache 2.0 and DCO sign-off (`CONTRIBUTING.md`).
- Security reports (`SECURITY.md`).
- Code of Conduct.

### FAQ `/faq`
- Is AEP a liability framework? No. Evidence tiers describe verifiability only (spec §3.9.12).
- Does AEP replace any commerce or payment protocol? No. It records them.
- What personal data is in a chain? The two-plane model and the Evidence Custodian role (spec §8.2.1).
- Who decides a dispute? The applicable network, issuer or dispute process.

### Legal `/legal`
- Specification license: Apache 2.0, including the patent grant.
- Trademark notice: text to match the specification's license section.
- Site privacy notice.

## 5. What the site may claim

Every product or capability claim is classified before it is published.

| Claim | Class | Basis |
|---|---|---|
| Typed artifacts for the eight lifecycle segments | In the specification | Spec §3 |
| Hash chain, RFC 8785 canonicalization, SHA-256 with previous hash in the input | In the specification | Spec §4 |
| Ed25519 seal at authorization; post-seal appends | In the specification | Spec §3.10, §4.3 |
| Verification and DisputeBundle export | In the specification | Spec §5, §6 |
| Signal classification (deterministic, attested, inferred, asserted) | In the specification | Spec §2.8 |
| Two-plane privacy model, Evidence Custodian role | In the specification | Spec §8.2.1 |
| External anchoring of the sealed hash | In the specification | Spec §4.3 |
| Evidence tiers describe verifiability, not liability | In the specification | Spec §3.9.12 |
| Wrapped protocols and versions | In the specification | Spec §2.6 |
| Evidence vault, permissioned datasets, knowledge graphs, commerce signals | Not defined by the specification | Not presented as AEP features. If ever mentioned, labelled as a separate company offering |
| Adoption, members, customers, partners, transaction volumes | Not supported | Not published |
| Prevents fraud, assigns fault, guarantees outcomes | Not supported | Not published |

Rules:
- Demo hashes are always labelled illustrative.
- No logos of other organizations, no endorsements, no rankings.
- Protocols are named only with name, version and primary-source URL, or marked provisional.
- The only quote is the one in §4 (Home).

## 6. Proposed build and deploy plan

- **Stack:** static HTML, CSS and vanilla JavaScript. No framework, no build step. One directory per page with `index.html`.
- **Styles:** a single `tokens.css`; light mode with page `#ededee`, ink `#000000`, accent `#5b4bd6`; respect `prefers-reduced-motion`.
- **Demo:** one self-contained script using the Web Crypto API (`crypto.subtle.digest('SHA-256')`) over fixed sample JSON. No network calls.
- **Accessibility:** semantic headings, visible focus, color contrast at WCAG 2.2 AA, the lifecycle map available as an ordered list.
- **Crawlers:** `robots.txt`, `sitemap.xml`, `llms.txt` pointing to the spec and repository.
- **Checks:** a CI job that fails on exclamation points in copy, unlabelled hashes, and banned terms.
- **Hosting:** a static host with clean URLs and HTTPS, on the project domain.
- **Phases:** 1) IA and homepage wireframe (this directory). 2) Content drafts reviewed against the spec. 3) Build. 4) Accessibility and claims review. 5) Publish.

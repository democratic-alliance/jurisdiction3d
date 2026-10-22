# Jurisdiction3D: Verifiable Jurisdiction and Authority for Cross-Border Digital Interactions

**Proposal draft v0.1** · [Democratic Alliance](https://github.com/democratic-alliance) · built on the [Democratic International Architecture (DIA)](DIA.md).         
**Format:** NGI-style transatlantic proposal (modelled on the NGI Sargasso call structure; see the status note below).       
**Companion document:** *[Jurisdiction3D Technical Specification v0.3-draft](TECH-SPEC.md)*.      

> **Status note (read first).** NGI Sargasso's fifth open call closed on 25 November 2024, and the programme announced that it would launch no further calls. This text is therefore written as a **funder-agnostic, NGI-style proposal**. Sections can be reused for a successor NGI, Horizon Europe, or other EU–US/Canada cooperation call once its template and criteria are known. Items in `[BRACKETS]` are placeholders that only the applicant can fill. Nothing in this draft should be submitted before the target call's actual form, limits and eligibility rules have been checked.

---

## 1. Summary (target: ~200 words)

People and organizations increasingly act across several overlapping rule systems at once: national law, contracts, platform policies, protocol and DAO rules, professional and ethical codes. Today no open, machine-verifiable way exists to answer three basic questions about a cross-border digital action: **which rules apply, who has the authority to impose them, and what evidence supports that answer.**

Jurisdiction3D is an open-source reference model, specification and toolkit that represents jurisdiction as an explicit, inspectable object. It models a *competence region* in three dimensions (domain, applicability, scale), checks that delegated authority never exceeds its source, and resolves rule conflicts with a deterministic procedure that returns either a justified result or an explicit **"Open conflict"**. It never silently picks a winner. Every step is recorded in a verifiable trace, and proofs can be made with selective disclosure so that validity can be checked without exposing personal data.

The project will deliver a stable specification, a reference implementation, privacy-preserving proof components, a public conformance suite, and a transatlantic pilot, all under open licences and aimed at standards communities.

---

## 2. The problem

### 2.1 What is missing

- **Identity systems answer "who", not "under what authority".** A verified identity says nothing about whether the party acting on it is entitled to impose a rule, and in which domain.
- **Cross-border actions fall under several rule sets at the same time.** A single token sale, data transfer or service restriction can touch the law of two states, a contract, a protocol's governance and a platform's policy. Systems tend to hide this by choosing one set of rules implicitly.
- **Authority is implicit and unbounded in practice.** Control of a lower layer (e.g. infrastructure or a protocol) can end up acting as authority over a higher one, and it is difficult to demonstrate this when it happens.
- **Conflicts are treated as errors to be hidden.** Users, auditors and courts receive an outcome without a reconstructible reason.

### 2.2 Why it matters for the Next Generation Internet

An internet that is trustworthy, human-centric and respects data sovereignty needs the *rules of the interaction* to be as inspectable as the data. Without it, trust remains institutional ("trust the platform"), and cross-border interoperability stops at the data format and never reaches the legal and normative layer.

---

## 3. Fit with NGI priorities

| NGI Sargasso theme (as published) | How Jurisdiction3D addresses it |
|---|---|
| **Trust** | Evidence-weighted authority chains; every result carries a reproducible trace; claims without sufficient evidence cannot create authority |
| **Data sovereignty** | The individual (the "Subject") is the persistent anchor; rules act on *actions* at the boundary, never on the person's protected personal domain; privacy by architecture |
| **Digital identity** | Separates identity, authority, jurisdiction, rule and evidence as distinct primitives; pairwise pseudonymous Subject identifiers; compatible with DIDs |
| **Decentralised technologies / standards** | Explicit treatment of protocol and DAO rules (layer L4) next to law and contracts; open schemas intended for standards bodies |
| **Internet architecture renovation** | A neutral coordination layer between jurisdictions, *not* a new jurisdiction |
| **Transatlantic cooperation** | Conflict-of-laws style problems between EU, US and Canadian regimes are the primary test bed |

---

## 4. Concept and approach

### 4.1 Core ideas (from the DIA framework)

1. **Subject first, relationships second, objects third.** The individual exists before the systems that describe, govern or serve them. A database record, account, wallet or credential is a representation, not the person.
2. **Authority is relational and explicit.** For each relationship the model records the source of authority, scope, duration, powers, limits and a review mechanism.
3. **Jurisdiction is not identity.** Identity persists across relationships; jurisdiction is contextual to each one.
4. **No silent escalation.** A delegated authority can never exceed its parent's scope, powers or time. Control over one layer does not imply control over a more protected one.
5. **Conflict is data.** Where rules clash and no published procedure decides between them, the output is an explicit `Open` result routed to a dispute mechanism.
6. **The model is not a sovereign.** It does not decide what is just, lawful or ethical. It computes which rule prevails *under the policy tables a forum has itself published*.

### 4.2 The three-dimensional model

A norm's competence is a *region* in a space with three axes:

- **D (Domain):** what is regulated (a taxonomy with three roots: fundamental rights, institutions, economy and resources);
- **S (Applicability):** to whom and via which connecting factors (territory, residence, establishment, effects, consent, protocol address, …);
- **Sc (Scale):** micro, local, cross-border, global.

The normative layers (state law, contract, ethical/religious norms, protocol rules, platform policy) are **not** a fourth axis. They are a *type of source of authority*, kept orthogonal to the region. Time is handled as a validity interval on each record.

Because regions are boxes, containment and intersection are cheap, exact checks. This turns "no privilege escalation" from a principle into a property that software can verify.

### 4.3 Conflict resolution

A deterministic cascade resolves clashes in a fixed order: admissibility of the rule → protected-core exclusion → mandatory over voluntary rules → recognised precedence → proximity of connection → specificity → time → explicit voluntary hierarchy → **Open**. Each step is recorded. The political parameters (precedence tables, weights, definition of the protected core) are published, versioned *forum policies*, so differences between forums become visible rather than buried in code.

### 4.4 Verifiability without exposure

Core statements, such as "this authority is validly delegated", "this action falls inside that authority's scope" or "this rule version applied at this time", are provable with selective disclosure or zero-knowledge techniques, so a verifier learns the fact without learning the underlying personal or commercial data.

---

## 5. Objectives and expected results

| # | Objective | Measurable result |
|---|---|---|
| O1 | Stabilise the specification | Specification v1.0-rc: complete JSON Schemas for all record types, versioned and published under an open licence |
| O2 | Show that the model is implementable and deterministic | Open-source reference implementation of region logic, authority validation and the conflict-resolution function; byte-identical traces for identical inputs |
| O3 | Make verification privacy-preserving | At least one concrete proof suite for three statements: containment (`R_child ⊆ R_parent`), chain validity, and membership of an action in an authority's region |
| O4 | Make the model testable by third parties | Public conformance suite of ~40 test vectors (+ machine-readable fixtures) and a corpus study on whether two of the three axes can be merged |
| O5 | Validate across jurisdictions | Transatlantic pilot with at least two jurisdictions' sample rule sets (see §7) |
| O6 | Feed standards and open-source communities | At least [N] contributions or discussion documents to relevant standards communities; public documentation and a developer guide |

---

## 6. Work plan

> Durations and effort are placeholders. Adjust to the target call's project length and funding ceiling (the last Sargasso call offered up to €100,000 per project).

| WP | Title | Months | Lead | Main outputs |
|---|---|---|---|---|
| WP1 | Specification hardening | 1–4 | `[EU partner]` | Complete schemas (Action, Decision, Challenge, Snapshot, Forum Policy, Trace); resolved spec-decision list; v1.0-rc |
| WP2 | Reference implementation | 2–6 | `[EU partner]` | Library for regions, `Valid`, `Competent`, `resolve`; trace and snapshot hashing; CLI and API |
| WP3 | Privacy-preserving proofs | 3–7 | `[US/CA partner]` | Proof suite for P1–P3; benchmark and threat model |
| WP4 | Conformance and corpus study | 4–8 | `[joint]` | Fixtures for all test vectors; corpus test on axis independence; published report |
| WP5 | Transatlantic pilot | 6–9 | `[joint]` | Two jurisdictions' rule samples encoded; end-to-end scenarios; lessons learned |
| WP6 | Standards, community, dissemination | 1–9 | `[joint]` | Contributions to standards groups; documentation; talks and workshops |

### Milestones

| ID | Month | Milestone |
|---|---|---|
| M1 | 4 | Specification v1.0-rc published; all schemas validated against test vectors |
| M2 | 6 | Reference implementation passes ≥ 90% of the conformance suite |
| M3 | 7 | Proof suite demonstrated on three statements |
| M4 | 9 | Pilot report, final conformance results, v1.0 decision |

---

## 7. Transatlantic collaboration and pilot

The model's central test is a **cross-jurisdiction collision**. The pilot encodes the same scenario under at least two regimes and checks whether the resolution procedure produces a justified result or an honest `Open`.

**Reference scenario.** A participant in a decentralised organisation established in a European state sells a digital asset to a buyer in a US or Canadian jurisdiction through a smart contract. Three or more rule sets apply at once: the seller's state, the buyer's state, the protocol's governance rules, and the contract between the parties. The pilot shows, step by step:

- which rules are admissible and why;
- where a mandatory local rule overrides a voluntary one;
- where two equal-ranked local rules clash and the answer depends on the forum's published weights, so that the result is correctly `Open`;
- how a participant who is *affected* but not a party can challenge the result.

**Stress test.** The specification also includes a contested real-world event (a restriction of satellite communications service in a conflict zone) as a conformance test. It is used **only** to test model behaviour on claims that cannot be verified from public data, and the project draws no conclusions about the event itself.

**Division of work:** `[US/CA partner]` leads proofs and the North American rule samples; `[EU partner]` leads specification, implementation and EU rule samples; both co-lead the pilot and standards work. **A US or Canadian partner is required by the programme model and has not yet been identified.** `[PLACEHOLDER: name, organisation, role, letter of intent]`

---

## 8. Open source, standards and sustainability

- **Licensing:** specification and schemas under `[CC BY 4.0]`; code under `[Apache-2.0 / MPL-2.0]`; all test data public. `[Confirm with legal advice]`
- **Standards engagement:** relevant targets include identity and credential standards communities and internet-governance forums. Specific groups and a contribution plan will be fixed after consultation with the partners. `[PLACEHOLDER]`
- **Governance of the specification:** public issue tracker, semantic versioning, an explicit process for approving changes to normative decisions.
- **Sustainability:** the model is deliberately *infrastructure*, not a service. After the project, maintenance can be carried by the community and by the applicant `[describe commitment]`.
- **Policy tables stay with forums.** The project publishes example tables but does not claim to own or define them.

---

## 9. Impact

| Audience | Expected benefit |
|---|---|
| Individuals | Can see which rules apply to an action, under whose authority, and how to challenge it |
| Developers of identity, DAO and platform systems | A shared vocabulary and test suite for authority and jurisdiction |
| Regulators and courts | Inspectable, versioned authority and rule records; reproducible reasoning for cross-border matters |
| Standards bodies | A concrete, implemented proposal for the normative layer of cross-border interoperability |
| Researchers | A formal, falsifiable model, with the open questions listed and testable |

**Indicators (to be agreed with the funder):** conformance pass rates by independent implementations, number of independent contributors, external citations or adoption, standards contributions accepted for discussion.

---

## 10. Risks and mitigation

| Risk | Likelihood / impact | Mitigation |
|---|---|---|
| The model is perceived as a new "super-jurisdiction" | Medium / High | The architecture, the text and the code say explicitly that it is not: mandatory local law prevails; the model computes, it does not decide |
| Two of the three axes turn out not to be independent | Medium / Medium | Corpus study (WP4) is planned; schemas allow merging in a later version |
| The resolution procedure is order-dependent when precedence is not transitive | Medium / Medium | Acyclicity checks, canonical processing order, `Open` on non-transitive cases; formal analysis in WP4 |
| Forum policy tables are politically contested | High / Medium | Tables are published, versioned and attributable; the project does not set them |
| Proof components are too slow or complex | Medium / Medium | Start with a minimal statement set; allow Merkle-based proofs where zero-knowledge is overkill |
| Misuse for surveillance or profiling | Low / High | Rules act on actions, not on persons; pairwise identifiers; linking identities is itself an authorised, logged action; threat model in WP3 |
| Missing transatlantic partner | **Open** | Use the programme's brokerage or matchmaking channels; target partners among identity, privacy and legal-informatics groups |
| Real-world test events are sensitive or change | Medium / Low | Treat as test vectors only; state facts as of a date; no findings about the event |

---

## 11. Ethics, data protection and responsible use

- The project works on **rule structures and synthetic or public data**. No personal data is required for the research plan. `[Confirm with data-protection advice]`
- AI systems, where used for classification, produce **evidence for a human or authority to attest**. They are never the source of a decision.
- The model never claims to determine what is lawful, just, halal or ethical. It records which framework was selected, by whom and in which version.
- A threat model for surveillance misuse and for the combination of identifiers is a deliverable.

---

## 12. Applicant and team

- **Applicant:** Democratic Alliance. `[PLACEHOLDER: legal form, country of registration, eligibility category (e.g. NGO / non-profit), registration number]`
- **Team:** `[PLACEHOLDER: names, roles, relevant experience, publications, prior open-source work]`
- **Advisory input (optional):** `[legal-informatics, privacy-engineering, standards-process advisers]`

---

## 13. Budget (illustrative; replace with the call's template)

The numbers below only show a plausible *shape* for a project with a €100,000 ceiling. They are not estimates of real costs.

| Category | Share | Indicative amount | Used for |
|---|---|---|---|
| Specification and schema work | 20% | €20,000 | WP1 |
| Software engineering | 30% | €30,000 | WP2 |
| Cryptography and privacy engineering | 20% | €20,000 | WP3 |
| Conformance, corpus study, pilot | 15% | €15,000 | WP4–5 |
| Standards, dissemination, travel | 10% | €10,000 | WP6 |
| Project management and audit | 5% | €5,000 | all |
| **Total** | 100% | **€100,000** | |

`[Per-partner split, personnel rates and eligible-cost rules to be set from the target call.]`

---

## 14. Before submitting: checklist

- [ ] Find an **open call** that fits (the Sargasso series has ended). Candidates to investigate: successor NGI funding, other Horizon Europe cascade or direct calls, and national or private programmes for open-source trust infrastructure.
- [ ] Obtain and follow that call's **template, page limits, criteria and eligibility rules**.
- [ ] Secure the **US or Canadian partner** if the call requires one; get a letter of intent.
- [ ] Confirm the **legal entity and eligibility category** of the applicant.
- [ ] Complete the missing schemas and fixtures (Technical Specification T-1, T-2) so the proposal can point to working artefacts.
- [ ] Decide licences; get legal review.
- [ ] Prepare a short public repository and demo; funders reward evidence of existing work.
- [ ] Have the proposal read by someone outside the project for clarity.

---

## Appendix A: Mapping to the Technical Specification

| Proposal element | Specification section |
|---|---|
| Three-dimensional model | §2 (the jurisdiction space) |
| No silent escalation | §3.1.4, §5.2 (`Valid`) |
| Conflict resolution | §6 (cascade, reference algorithm) |
| Trace and auditability | §7 (trace Σ) |
| Proof statements P1–P7 | §8 |
| Service interface | §9 |
| Conformance suite | §11 |
| Open decisions (SD-1…5) and issues (OI-1…8) | §12 |

## Appendix B: Sources checked for this draft

Call status and programme details were taken from public pages on NGI Sargasso (the fifth call's closing date and the "final call" announcement on the NGI community site, call listings on EU funding portals). They may be out of date; confirm on the official programme site before relying on them.

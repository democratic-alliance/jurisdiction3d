[Jurisdiction3D](README.md) | [DIA Stack](DIA.md) | [DIA Regulator](regulator.md) | Tech Spec | [Funding](FUNDING.md)

# Jurisdiction3D — Technical Specification

**Version:** 0.3.1-draft    
**Organization:** [Democratic Alliance](https://github.com/democratic-alliance)       
**Infrastructure:** Democratic International Architecture (DIA)         
**Based on:** *Jurisdiction3D - Formal Model* v0.2, [DIA Stack](DIA.md), [Regulator Object](regulator.md), [DIA International Manifesto](README.md).            
**Status:** Draft for review. Not normative until approved.      

> **L0 defines the subject.
> Jurisdiction constrains the action.
> Rules define the constraints.
> DIA proves what happened.**

---

## 0. Scope and conventions

### 0.1 Purpose

This document turns the Jurisdiction3D formal model into an implementable specification: data structures, validation predicates, the conflict-resolution function, trace and snapshot formats, proof hooks, service interfaces, and a conformance suite.

Jurisdiction3D **does not decide what is lawful, just, halal, or ethical**. It computes which rule prevails *under the policy tables a forum has published*, and it records every step so the result can be verified and challenged.

### 0.2 Normative language

The key words MUST, MUST NOT, SHOULD, SHOULD NOT and MAY are used as in RFC 2119.

### 0.3 Spec decisions

Places where this specification goes beyond the v0.2 formal model are marked **[SD-n]** and collected in §12. They need explicit approval before the spec is treated as normative.

### 0.4 Architectural invariants this spec must preserve

| # | Invariant | Where enforced |
|---|---|---|
| 1 | Local Authority | §6.3 step 2 |
| 2 | Delegation ≠ surrender | §5.3 `Valid` |
| 3 | Non-Escalation | §5.3, §3.1.4 |
| 4 | Protected L0 | §3.1.1, §6.3 step 1 |
| 5 | Boundary Enforcement | §4.3 (rules apply to Actions, not Subjects) |
| 6 | Normative Pluralism | §4.2 `layer` |
| 7 | Provenance | §4.4, §4.5 |
| 8 | Auditability | §6.5, §7 |
| 9 | Contestability | §4.9, §6.6 |
| 10 | Minimal Authority | §5.5 |
| 11 | Privacy | §8 |
| 12 | AI Accountability | §4.7, §4.10 |

### 0.5 Encoding and identifiers

- **Serialization:** JSON (RFC 8259). Implementations MAY offer YAML for authoring but MUST canonicalize to JSON before hashing.
- **Canonicalization:** JSON Canonicalization Scheme (RFC 8785).
- **Hash:** SHA-256, written `sha256:<hex>`.
- **Time:** RFC 3339 UTC timestamps. Validity intervals are half-open `[from, to)`; `to: null` means open-ended.
- **Identifiers:** opaque, globally unique strings with a type prefix, e.g. `auth:…`, `rule:…`, `obj:…`, `sub:…`, `ev:…`, `rel:…`, `act:…`, `dec:…`, `chal:…`. A Subject identifier MUST be a DID or a pairwise pseudonymous identifier (§8.4). It MUST NOT be a national ID number.
- **Versioning:** every Rule, Authority, taxonomy and policy table carries a monotonically increasing integer `version`. A new version is a new immutable record. Records MUST NOT be edited in place.

---

## 1. Layer model recap

Jurisdiction3D answers three independent questions about every norm:

| Question | Symbol | Type |
|---|---|---|
| Where does the competence lie? | `J = (D, S, Sc)` | position in space 𝒥 |
| What kind of source of authority is it? | `λ ∈ {1..5}` | stack layer |
| When is it valid? | `V = [t0, t1)` | interval |

- `λ` is the DIA stack layer: 1 Local Jurisdiction, 2 Contract, 3 Religious/Ethical, 4 Protocol/DAO, 5 Application/Organization.
- **L0 is not a value of λ.** The Subject is not a source of norms. L0 is the reference point: every region is described relative to a Subject through axis S, and `D0` is the protected domain adjacent to the Subject.
- Time is **not** an axis of 𝒥 in this version (§12, OI-7).

---

## 2. The jurisdiction space 𝒥

𝒥 = Dom × App × Scl.

### 2.1 Axis D — Dom (domain taxonomy)

Dom is a **forest** with three fixed roots:

```text
D0  Fundamental rights      D0.privacy, D0.liberty, D0.bodily-integrity, …
D1  Institutions            D1.courts, D1.registration, D1.elections, D1.defence, …
D2  Economy and resources   D2.finance.crypto, D2.property, D2.labour, D2.communications, …
```

**Representation.** A domain is a dotted path. `d ⊑ d'` ("d is a sub-domain of d'") holds iff `d'` is a prefix of `d` at a segment boundary.

```text
D2.finance.crypto ⊑ D2.finance          true
D2.finance        ⊑ D2.finance.crypto   false
D2.finance        ⊑ D2.property         false
D2.finance        ⊑ D0                  false
```

**Rules.**

1. Closure is **downward only**. Competence over `D2.finance` covers `D2.finance.crypto`, never a sibling, never a different root.
2. D0, D1 and D2 are never nested in one another.
3. A taxonomy is a published, versioned artifact: `{ taxonomy_id, version, nodes[], issuer, hash }`. Every Region, Rule and Authority MUST reference the taxonomy version it uses. Comparing records across taxonomy versions requires an explicit, published mapping; otherwise the comparison result is `Unknown` (treated as *not contained*, §3.1.4).

**D0 default (Invariant 4).**

- A **delegated** Authority (`parent ≠ null`) can only hold D0 competence if its parent holds it (follows from containment).
- A **root** Authority whose D component includes any `D0.*` path MUST carry a `d0_grant` reference: the evidence record of the legitimate normative process (statute, treaty, court order) that grants it. Without `d0_grant`, such an Authority is invalid. **[SD-1]**
- D0 is not immunity from mandatory law. L1 may hold explicit D0 powers. The point is that D0 powers are never implicit.

### 2.2 Axis S — App (applicability space)

`S` is the set of contexts to which a norm applies. For machine checking, S MUST be given in **structured form**:

```json
{ "subject_classes": ["legal_entities"], "factors": ["establishment", "effects"] }
```

**Connecting factors** (closed set K):

```text
territory, nationality, residence, establishment, place_of_performance,
effects, consent_choice, membership, account, protocol_address
```

**Semantics.** A context `u` satisfies `S` iff the acting or affected subject belongs to at least one listed subject class **and** at least one listed factor holds in `u`.

**Subject-class lattice.** Subject classes are drawn from a published, versioned lattice with a declared subsumption order (e.g. `natural_persons ⊑ all_subjects`, `legal_entities ⊑ all_subjects`). `⊑` here is the lattice order.

**Containment (sound, not complete):**

```text
S1 ⊆ S2  ⟸  classes(S1) ⊑-covered-by classes(S2)  ∧  factors(S1) ⊆ factors(S2)
```

where "covered by" means every class in S1 is `⊑` some class in S2.

**Opaque predicates.** An implementation MAY accept free-form predicates for S, but a Region with an opaque S has `containment = Unknown`, which is treated as **not contained** (fail-closed, §3.1.4). Opaque S is therefore unusable as a delegated Authority or Rule region. It is allowed only for informational records.

> "Authoritarian" or "transitional" counterparties are **not** part of S. Recognition is a property of the Authority (`status`, §4.4).

### 2.3 Axis Sc — Scl (scale)

```text
0 micro (contract parties, family, small community)
1 local (municipality, region of one state)
2 cross-border (more than one L1 jurisdiction)
3 global (universal protocols and treaties)
```

A norm's Sc is the **highest level at which it claims to act or is enforced**. Representation: a set of integers from `{0,1,2,3}`, normalised to a sorted list. Containment is set inclusion.

### 2.4 Region and projections

A **Region** is a box:

```text
R = D × S × Sc ⊆ 𝒥
```

```json
{
  "taxonomy": { "id": "dom-taxonomy", "version": 4 },
  "D":  ["D2.finance.crypto"],
  "S":  { "subject_classes": ["legal_entities"], "factors": ["establishment"] },
  "Sc": [1]
}
```

An **Action projection** is a point:

```text
j = (d, u, sc)
d ∈ Dom,  u = { subject_classes[], factors_holding[] },  sc ∈ Scl
```

Membership `j ∈ R` holds iff all three hold:

1. `d ⊑ d'` for some `d' ∈ R.D`;
2. `u` satisfies `R.S` (§2.2);
3. `sc ∈ R.Sc`.

**Operations** (all componentwise; see §5.1 for algorithms):

- `R1 ⊆ R2`: containment.
- `R1 ∩ R2`: intersection, itself a Region. Empty intersection means no collision is possible.
- Empty-D check: a Region with `D = []` is empty.

---

## 3. Core algorithms

### 3.1 Region operations

#### 3.1.1 D containment

```text
D1 ⊆ D2  ⇔  ∀ d ∈ D1 : ∃ d' ∈ D2 : d ⊑ d'
```

#### 3.1.2 D intersection

```text
D1 ∩ D2 = { d1 | d1 ∈ D1, ∃ d2 ∈ D2: d1 ⊑ d2 } ∪ { d2 | d2 ∈ D2, ∃ d1 ∈ D1: d2 ⊑ d1 }
```

then remove duplicates and any element that is a strict sub-domain of another element in the result (antichain normalisation).

#### 3.1.3 S and Sc

- `S1 ∩ S2`: class set = pairwise meets in the lattice; factor set = `factors(S1) ∩ factors(S2)`.
- `Sc1 ∩ Sc2`: set intersection.

#### 3.1.4 Fail-closed rule

Any containment check that cannot be completed (opaque S, unmapped taxonomy versions, missing lattice) returns `Unknown`. `Unknown` MUST be treated as **not contained** by `Valid`, `Competent` and `applies`, and MUST be logged to Σ. A system MUST NOT turn `Unknown` into `true`.

This is the machine form of Non-Escalation: when in doubt, authority is not extended.

### 3.2 Reference pseudocode

```python
def contains_D(D_outer, D_inner):
    return all(any(is_sub(d, o) for o in D_outer) for d in D_inner)

def region_contains(R_outer, R_inner, ctx):
    if not same_taxonomy_or_mapped(R_outer, R_inner, ctx):
        return Unknown
    if not contains_D(R_outer.D, R_inner.D):
        return False
    if S_contains(R_outer.S, R_inner.S, ctx.lattice) is not True:
        return False                         # includes Unknown -> fail closed
    return set(R_inner.Sc) <= set(R_outer.Sc)

def in_region(j, R, ctx):
    d, u, sc = j
    return (any(is_sub(d, o) for o in R.D)
            and satisfies(u, R.S, ctx.lattice)
            and sc in R.Sc)
```

---

## 4. Data model

All types are immutable records once published. Field names are normative.

### 4.1 Subject

```json
{ "id": "sub:…", "did": "did:…", "attributes": { } }
```

A natural person. Legal entities, accounts, wallets and credentials are **Objects**, linked to the Subject through Relationships (Invariant: *no Object is the Subject*). `attributes` is a commitment container (§8): it MAY hold hashes or commitments rather than clear values.

### 4.2 Object

```json
{
  "id": "obj:…",
  "kind": "State | Court | Regulator | Contract | Community | Protocol | DAO | Organization | Application",
  "layer": 1,
  "name": "…"
}
```

`layer` is λ ∈ {1..5}. An Object becomes a **Regulator** (`regulator.md`) not by `kind` alone but by holding an Authority record (§4.4).

### 4.3 Action

```json
{
  "id": "act:…",
  "actor": "sub:…",
  "act_type": "…",
  "target": "…",
  "time": "2026-10-14T00:00:00Z",
  "context": { },
  "projections": [
    { "d": "D2.communications", "u": { "subject_classes": [], "factors_holding": [] }, "sc": 2 }
  ]
}
```

- `projections` has **at least one** element.
- Rules act on Actions at the boundary where intention becomes externally relevant (Invariant 5). **No Rule, Authority or protocol primitive may take a Subject's L0 as its target.**
- The projections are produced by a *classifier*. A classifier MAY be an AI system, but its output is **Evidence** (§4.7), not a Decision. A human or Authority attests the projection set that is used in resolution. **[SD-2]**

### 4.4 Authority

```json
{
  "id": "auth:…",
  "version": 1,
  "holder": "obj:…",
  "source": "statute | treaty | delegation | contract | membership | protocol_governance | court_order",
  "source_evidence": ["ev:…"],
  "parent": "auth:… | null",
  "R_de_jure":  { "…Region…" },
  "R_de_facto": { "…Region…" },
  "powers": ["create_rule", "interpret", "supervise", "certify", "sanction", "adjudicate"],
  "limits": [ ],
  "V": { "from": "2025-01-01T00:00:00Z", "to": null },
  "revocable": true,
  "status": "recognized | partially_recognized | disputed | effective_control_only",
  "review_mechanism": "ref:… | null",
  "d0_grant": "ev:… | null",
  "provenance_hash": "sha256:…"
}
```

**Derived values.**

```text
R(α)   := R_de_jure(α)             -- Competent, Valid, applies always use de jure
Gap(α) := R_de_facto(α) ∖ R_de_jure(α)
```

**Status and Gap are recorded, not adjudicated.** Recognition is a normative question for a recognising forum. The model writes the discrepancy to Σ; it does not resolve it.

The mapping to `regulator.md`'s ten questions:

| `regulator.md` question | Field |
|---|---|
| Who is the regulator | `holder` |
| What authority | `powers` |
| Where / scope | `R_de_jure` |
| When | `V` |
| Why | `source`, `source_evidence` |
| Which rules | Rules with `issuer = id` |
| What powers | `powers` |
| What limitations | `limits` |
| Review / appeal / termination | `review_mechanism`, `revocable` |
| Who is regulated | Relationships where `authority = id` |

### 4.5 Rule

```json
{
  "id": "rule:…",
  "version": 3,
  "issuer": "auth:…",
  "R_r": { "…Region…" },
  "modality": "O | P | F",
  "act_pattern": { "act_type": "…", "target": "…" },
  "condition": { "expr": "…" },
  "mandatory": true,
  "layer": 1,
  "V": { "from": "…", "to": null },
  "provenance_hash": "sha256:…"
}
```

- `modality`: O = obligation, P = permission, F = prohibition.
- `condition` is a side-effect-free expression in a published, versioned, sandboxed language (candidates: CEL, JSONLogic). Evaluation MUST be deterministic and terminate. **[SD-3]**
- **Competence (ultra vires check):**

```text
Competent(r) ⇔ R_r ⊆ R(issuer(r))  ∧  V(r) ⊆ V(issuer(r))
```

### 4.6 Relationship

```json
{
  "id": "rel:…",
  "subject": "sub:…",
  "object": "obj:…",
  "authority": "auth:…",
  "kind": "…",
  "rules": ["rule:…"],
  "R_rel": { "…Region…" },
  "V": { "from": "…", "to": null },
  "state": "proposed | active | suspended | terminated | challenged | disputed",
  "role": "party | affected",
  "collision_clause": "ref:… | null"
}
```

Implements the DIA chain: Subject → Object → Relationship → Scope → Source of authority → Rights/obligations → Validity conditions.

- `role = affected`: the Subject bears consequences of actions taken under the Relationship without being a party. It gives no party rights, but (i) the `effects` factor on axis S applies, and (ii) it grants standing to Challenge (§4.9).
- `state = disputed`: the *existence* of the relationship is contested (e.g. whether a party actually uses a given service). Its rules do not disappear; they pass through the cascade with a flag.
- `collision_clause`: for λ ≥ 2 relationships, the explicit voluntary hierarchy used by cascade step 7.

### 4.7 Evidence

```json
{
  "id": "ev:…",
  "claim": "…",
  "claim_type": "identity | authority | consent | event | rule_version | delegation | revocation",
  "issuer": "obj:… | sub:…",
  "time": "…",
  "proof": { "type": "signature | merkle_path | zk_proof | attestation", "data": "…" },
  "disclosure": "full | selective | commitment_only",
  "weight_class": "cryptographic | institutional | public_party_statement | anonymous_attribution",
  "interested": false,
  "links": ["ev:…"]
}
```

**Evidence ≠ Decision.** Evidence supports claims; it has no normative effect by itself.

**Weight rules.**

| `weight_class` | May build authority chains / competence? | Logged in Σ? |
|---|---|---|
| `cryptographic` | Yes | Yes |
| `institutional` | Yes | Yes |
| `public_party_statement` | **No**: claim only | Yes |
| `anonymous_attribution` | **No**: claim only | Yes |

`interested = true` (issuer is a party to the dispute) lowers weight but never removes the record.

**Evidence needed:**

- `Valid(α)`, `Competent(r)` and any delegation proof: `cryptographic` or `institutional`.
- Voluntary rules (λ ≥ 2): evidence of `consent`.
- **AI-produced analysis** is `Evidence` with `issuer` = the AI system's Object and is never a Decision (Invariant 12).

### 4.8 Decision

```json
{
  "id": "dec:…",
  "action": "act:…",
  "applicable": ["rule:…"],
  "conflicts": [{ "r1": "rule:…", "r2": "rule:…", "projection": 0 }],
  "result": "Resolved | Open",
  "evidence": ["ev:…"],
  "issuer": "auth:…",
  "procedure": "ref:…",
  "trace": "sigma:…",
  "challenges": ["chal:…"],
  "state": "final | challenged"
}
```

`issuer` MUST be an Authority. A system output that is not issued by an Authority is a *computation record* (a resolution report), never a Decision (Invariant 12). A Decision with an open Challenge has `state = challenged`.

### 4.9 Challenge

```json
{
  "id": "chal:…",
  "target": { "type": "Evidence | Authority | Rule | Decision", "id": "…" },
  "ground": "…",
  "filed_by": "sub:… | obj:…",
  "standing": "party | affected | representative",
  "time": "…",
  "status": "filed | accepted | rejected | withdrawn | upheld"
}
```

Challenges are part of the record. They are appended, never deleted. `representative` standing covers a state or body acting for affected Subjects.

### 4.10 Resolution report (non-normative record)

The output of `Res` is a **report**, not a Decision. It carries the result, the trace Σ and the snapshot hash. A Decision may *adopt* a report by reference.

---

## 5. Validation predicates

### 5.1 Region containment and intersection

As in §3. All implementations MUST return `Unknown` where §3.1.4 requires it.

### 5.2 Delegation chain

```text
Valid(α) ⇔
    parent(α) = null  ?  root_ok(α)
                      :  R(α) ⊆ R(parent)  ∧  P(α) ⊆ P(parent)  ∧  V(α) ⊆ V(parent)
                         ∧  Valid(parent)  ∧  ¬revoked(α)
                         ∧  ∃ ev ∈ delegation_evidence(α): weight_class ∈ {cryptographic, institutional}

root_ok(α) ⇔ α.source ∈ ForumPolicy.root_sources
             ∧ ∃ ev ∈ source_evidence(α): weight_class ∈ {cryptographic, institutional}
             ∧ (D0 ∩ R(α).D = ∅  ∨  α.d0_grant ≠ null)         -- [SD-1]
```

- `root_sources` is a **forum policy** parameter. Default: every `source` value except `delegation`.
- Cycle in the parent chain → `Valid = False`, logged.
- Chain depth limit: forum policy, default 16.

### 5.3 Rule competence

`Competent(r)` as in §4.5. Rules with `Competent = False` are *excluded* at cascade step 0 and logged with their reason.

### 5.4 Spillover and Gap

```text
Spillover(α, a)  ⇔  ∃ i : j_i(a) ∉ R(α)         -- the action reaches outside α's de jure region
Gap(α) ≠ ∅                                         -- α can act in places where its authority is not sourced
```

Neither invalidates the action automatically. Both MUST be written to Σ and give grounds for a Challenge. Whether an effect is ultra vires is decided by the body named in `review_mechanism`.

### 5.5 Minimal Authority check

A requested grant `G` against a role `Req` passes iff `Req.R ⊆ G.R` and `Req.P ⊆ G.P`. Implementations SHOULD expose the **difference** `G ∖ Req` so that excess authority can be removed (Invariant 10).

### 5.6 Applicability

```text
applies(α, a, t, i) ⇔ j_i(a) ∈ R(α) ∧ t ∈ V(α)
```

---

## 6. Conflict resolution

### 6.1 Inputs

```text
Res(a, t, Snapshot, ForumPolicy) → Resolved(R*, Σ) | Open(Candidates, Route, Σ)
```

#### Snapshot

A frozen, content-addressed view of every record the computation can read:

```json
{
  "snapshot_id": "snap:…",
  "as_of": "2026-10-14T00:00:00Z",
  "roots": {
    "rules": "sha256:…",
    "authorities": "sha256:…",
    "evidence": "sha256:…",
    "relationships": "sha256:…",
    "taxonomy": "sha256:…",
    "subject_lattice": "sha256:…",
    "forum_policy": "sha256:…"
  },
  "hash": "sha256:…"
}
```

Each root is a Merkle root over the canonical JSON of the corresponding record set. `Res` MUST NOT read anything outside the Snapshot.

#### ForumPolicy

The political parameters of the forum. They are **not truths of the model**; different forums may publish different ones. They are signed, versioned records:

```json
{
  "id": "policy:…",
  "version": 1,
  "forum": "obj:…",
  "root_sources": ["statute", "treaty", "court_order", "contract", "membership", "protocol_governance"],
  "phi0": "ref:…",
  "precedence_rec": [ { "over": "…", "under": "…" } ],
  "proximity_weights": { "territory": , "residence": 0.2, "effects": 0.2, "…": 0.0 },
  "max_chain_depth": 16,
  "dispute_mechanism": "ref:… | null",
  "status_handling": "flag"
}
```

- **Φ0** (`phi0`): the protected-core definition, supplied by a recognised body (e.g. a constitutional court or treaty). The model **applies** Φ0 and never defines it.
- **≻_rec**: a set of recognised precedence norms (treaties, conflict-of-laws rules, jurisdiction clauses).
- **`proximity_weights`**: weights `w_k` per connecting factor.

> A forum's choice of these tables is itself a political act. Publishing them is how that act becomes visible.

### 6.2 Applicable rules and conflict

For projection `i`:

```text
A(a,t,i) = { r ∈ Snapshot.rules |
             Competent(r) ∧ Valid(issuer(r))
           ∧ j_i(a) ∈ R_r ∧ t ∈ V(r) ∧ φ_r(a) }

Conflict(r1, r2, a, i) ⇔ r1, r2 ∈ A(a,t,i) ∧ modalities incompatible for act(a)
    incompatible: (O, F), (P, F), or (O, O) with mutually exclusive content
```

Each projection runs through the cascade separately. Rules of different projections never conflict with each other (they sit at different points of 𝒥); the final result combines the projection results (§6.4).

### 6.3 The cascade

For each conflicting pair, steps are applied in order. The **first step that yields a single winner ends the pair.** Every step is written to Σ.

| # | Step | Rule | Data source |
|---|---|---|---|
| 0 | Admissibility | Exclude rules without competence (de jure), with invalid chain, without sufficient-weight delegation evidence, or (voluntary rules) without consent evidence. Flag spillover on projections outside `R(issuer)`. | `Valid`, `Competent`, Evidence weights |
| 1 | D0 core | Exclude rules that violate the protected core defined by Φ0. | `phi0` |
| 2 | Mandatory over voluntary | A mandatory L1 rule (`mandatory = true`) beats rules of layers λ ≥ 2. | Invariant 1 |
| 3 | Recognised precedence | Apply explicit seniority norms: treaties, conflict-of-laws, jurisdiction clauses. | `precedence_rec` |
| 4 | Proximity | Between different L1 jurisdictions: `π(r,a) = Σ_k w_k · [factor k holds for r in a]`. Strictly higher π wins. | `proximity_weights` |
| 5 | Specificity (*lex specialis*) | Strictly narrower D wins; if D is equal, strictly narrower S wins. | `⊑`, `⊆` |
| 6 | Time | Same issuer and same region: later version wins; **no retroactivity**. | `V(r)` |
| 7 | Voluntary hierarchy | For λ ≥ 2: the explicit collision clause in the Relationship. | `Relationship.collision_clause` |
| 8 | Open | No step yields a winner: result is `Open`, routed to `dispute_mechanism`. | `review_mechanism` |

**Notes.**

1. Step 1 does not decide *what* D0 contains. It applies a Φ0 supplied by a recognised body.
2. Rules from Authorities with `status ≠ recognized` are **not discarded automatically**. They pass through with a flag; recognition is the forum's decision.
3. A `disputed` Relationship passes through with a flag. If the outcome depends on whether it exists and no sufficient-weight Evidence is available, the result is `Open`, never an assumption in favour of either party.
4. Step 3's `precedence_rec` MUST be acyclic. A cycle returns `Open` before any pair is processed.

### 6.4 Result composition and the unattributed case

- **Resolved** is returned only if every projection resolves **and** the projection results are compatible (no `Conflict` relation across projections for the same `act_type`/`target`).
- Otherwise `Open`, naming the problematic projection.
- **Unattributed action [SD-4].** If a projection has `A = ∅` **because** candidate rules were excluded at step 0 (spillover, invalid chain, insufficient evidence), and the action was performed under a claimed authority, the projection result is `Open` with `reason = "no_admissible_basis"`, not `Resolved(∅)`. An empty result is allowed only when no rule was excluded for these reasons.

### 6.5 Reference algorithm

```python
def resolve(a, t, snap, policy):
    results, trace = [], []
    if not acyclic(policy.precedence_rec):
        return Open(candidates=[], reason="cyclic_precedence", route=policy.dispute_mechanism,
                    trace=trace, snapshot=snap.hash)

    for i, j in enumerate(projections(a)):
        excluded = []
        A = []
        for r in snap.rules:
            ok, why = admissible(r, j, t, a, snap, policy)         # step 0
            (A if ok else excluded).append(r if ok else (r, why))
        trace += log_step0(i, A, excluded)                          # includes spillover flags

        A = [r for r in A if not violates_core(r, policy.phi0)]     # step 1
        trace += log_step1(i, A)

        if not A and any_excluded_for_authority(excluded):          # SD-4
            return Open(candidates=excluded, reason="no_admissible_basis",
                        proj=i, route=policy.dispute_mechanism, trace=trace, snapshot=snap.hash)

        result = set(A)
        for (r1, r2) in canonical_order(conflict_pairs(A, a, j)):   # sorted by (min id, max id)
            winner = None
            for step in (mandatory_over_voluntary, recognised_precedence,
                         proximity, specificity, temporal, voluntary_hierarchy):
                winner = step(r1, r2, a, j, snap, policy)           # r1 | r2 | None
                trace.append(log(i, step.__name__, r1, r2, winner))
                if winner:
                    break
            if winner is None:
                return Open(candidates=(r1, r2), proj=i, route=policy.dispute_mechanism,
                            trace=trace, snapshot=snap.hash)        # step 8
            result.discard(r2 if winner is r1 else r1)
        results.append(result)

    if not compatible(results):
        return Open(candidates=results, route=policy.dispute_mechanism,
                    trace=trace, snapshot=snap.hash)
    return Resolved(union(results), trace=trace, snapshot=snap.hash)
```

### 6.6 Properties an implementation MUST satisfy

1. **Determinism.** Same `(a, t, Snapshot, ForumPolicy)` → same result and same Σ (byte-identical after canonicalization).
2. **No silent choice.** `Open` is a first-class result ("conflict is data").
3. **Auditability.** Σ lists every step, every evidence reference and every rule version.
4. **Contestability.** Every step can be targeted by a Challenge (§4.9).
5. **Non-normativity.** `Res` computes which rule prevails *under the forum's tables*. It does not decide what is just.
6. **Evidence weight.** Claims without sufficient-weight Evidence never build an authority chain. If an outcome depends on them, the result is `Open`.
7. **Multi-projection.** `Resolved` requires all projections resolved and compatible.
8. **Order stability.** Conflict pairs are processed in canonical order. See OI-6 for the known transitivity gap.

---

## 7. Trace Σ

### 7.1 Format

```json
{
  "sigma_id": "sigma:…",
  "action": "act:…",
  "as_of": "2026-10-14T00:00:00Z",
  "snapshot": "sha256:…",
  "policy": { "id": "policy:…", "version": 1 },
  "entries": [
    {
      "n": 1,
      "projection": 0,
      "step": "admissibility",
      "inputs": [{ "rule": "rule:…", "version": 3 }],
      "outcome": "excluded",
      "reason": "spillover | no_competence | invalid_chain | insufficient_evidence | no_consent",
      "evidence": ["ev:…"],
      "flags": ["authority_status:disputed"]
    }
  ],
  "findings": [
    { "type": "gap", "authority": "auth:…", "region": { } },
    { "type": "missing_review_mechanism", "authority": "auth:…" },
    { "type": "unverifiable_rule_version", "rule": "rule:…" }
  ],
  "result": "Resolved | Open",
  "hash": "sha256:…"
}
```

### 7.2 Requirements

- Σ MUST be append-only during a computation and immutable afterwards.
- Σ MUST contain **version and hash** for every rule it mentions (Invariant 7).
- `findings` records *absences that matter*: a missing review mechanism, a missing version history, an unverifiable change. An absence is a finding, not a verdict.
- Σ SHOULD be anchored: its hash MAY be published to an append-only log. The anchoring service is outside this spec.

---

## 8. Privacy and proof hooks

Principle: **maximum verifiability with minimum necessary disclosure** (Invariant 11). Mechanisms are implementation-agnostic. The statements below define *what* must be provable.

### 8.1 Disclosure levels

`full` / `selective` / `commitment_only` (field on Evidence). A verifier MAY demand a higher level only if the forum policy for that verification type allows it.

### 8.2 Provable statements

| ID | Statement | Reveals | Hides |
|---|---|---|---|
| P1 | `R_child ⊆ R_parent` | the fact of containment | the regions themselves (optional) |
| P2 | `Valid(α)`: a chain exists to a root source | root identity and chain length bounds | intermediate holders (optional) |
| P3 | `j(a) ∈ R(α)` | membership | context `u(a)` |
| P4 | `t ∈ V(α)` | membership | exact validity bounds |
| P5 | `rule_version(r)` is `v` at time `t` | version and hash | rule body (optional) |
| P6 | Consent evidence exists for `(σ, rule)` | existence | content of consent |
| P7 | `α` is not revoked at `t` | non-revocation | revocation list |

Implementations MAY use zero-knowledge proofs, Merkle proofs, selective-disclosure credentials or cryptographic commitments. A forum policy names the accepted proof suites.

### 8.3 Commitments

Regions SHOULD be committable: each component (D set, S pair, Sc set) hashed with a per-record salt into a Merkle tree so that P1/P3 can be proven against a published root.

### 8.4 Subject pseudonymity

- Subject identifiers SHOULD be pairwise pseudonymous per Object (a distinct DID per relationship) so Objects cannot trivially correlate a Subject across systems.
- Linking identifiers is itself an *action* that must be authorised by the Subject, or by an Authority with the explicit power and scope, and is logged (Invariant 4).

---

## 9. Service interface

Transport-agnostic. Each operation is a pure function of its request plus a Snapshot. JSON over HTTPS is the reference binding.

| Operation | Request | Response |
|---|---|---|
| `validateAuthority` | `authority_id`, `snapshot` | `{ valid: bool\|Unknown, chain[], findings[] }` |
| `checkRule` | `rule_id`, `snapshot` | `{ competent: bool\|Unknown, exclusions[] }` |
| `contains` | `region_a`, `region_b` | `true \| false \| Unknown` |
| `intersect` | `region_a`, `region_b` | `Region` |
| `projectionsOf` | `action` | candidate `projections[]` + producing evidence (AI output is Evidence) |
| `resolve` | `action`, `as_of`, `snapshot`, `policy` | resolution report `{ result, sigma }` |
| `fileChallenge` | `Challenge` | challenge id; affected Decision becomes `challenged` |
| `getTrace` | `sigma_id` | Σ (with the caller's disclosure level) |
| `proveStatement` | `P1..P7` + parameters | proof object |
| `verifyProof` | proof object | `{ valid: bool, statement }` |

**Example: `resolve`**

```json
// request
{
  "action": "act:2026-0001",
  "as_of": "2026-10-14T00:00:00Z",
  "snapshot": "snap:7f3a…",
  "policy": "policy:forum-a-v1"
}

// response
{
  "result": "Open",
  "reason": "no_admissible_basis",
  "projection": 1,
  "route": null,
  "sigma": "sigma:c91e…",
  "snapshot": "sha256:7f3a…"
}
```

**Errors.** `422` unsupported taxonomy mapping; `409` snapshot hash mismatch; `400` malformed region. **No error code may express "authority granted"; granting always requires a positive proof.**

---

## 10. JSON Schema (reference, draft 2020-12)

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://democratic-alliance.github.іо/jurisdiction3d/v/schema.json",
  "$defs": {
    "id": { "type": "string", "pattern": "^[a-z]+:[A-Za-z0-9._:-]+$" },
    "hash": { "type": "string", "pattern": "^sha256:[0-9a-f]{64}$" },
    "domainPath": { "type": "string", "pattern": "^D[0-2](\\.[a-z0-9-]+)*$" },
    "interval": {
      "type": "object",
      "required": ["from"],
      "properties": {
        "from": { "type": "string", "format": "date-time" },
        "to": { "type": ["string", "null"], "format": "date-time" }
      },
      "additionalProperties": false
    },
    "factor": {
      "enum": ["territory","nationality","residence","establishment",
               "place_of_performance","effects","consent_choice",
               "membership","account","protocol_address"]
    },
    "region": {
      "type": "object",
      "required": ["taxonomy", "D", "S", "Sc"],
      "properties": {
        "taxonomy": {
          "type": "object",
          "required": ["id", "version"],
          "properties": { "id": { "type": "string" }, "version": { "type": "integer", "minimum": 1 } }
        },
        "D": { "type": "array", "items": { "$ref": "#/$defs/domainPath" }, "uniqueItems": true },
        "S": {
          "type": "object",
          "required": ["subject_classes", "factors"],
          "properties": {
            "subject_classes": { "type": "array", "items": { "type": "string" }, "minItems": 1 },
            "factors": { "type": "array", "items": { "$ref": "#/$defs/factor" }, "minItems": 1, "uniqueItems": true }
          },
          "additionalProperties": false
        },
        "Sc": { "type": "array", "items": { "enum": [0, 1, 2, 3] }, "minItems": 1, "uniqueItems": true }
      },
      "additionalProperties": false
    },
    "power": { "enum": ["create_rule","interpret","supervise","certify","sanction","adjudicate"] },
    "authority": {
      "type": "object",
      "required": ["id","version","holder","source","source_evidence","parent",
                   "R_de_jure","R_de_facto","powers","V","status","provenance_hash"],
      "properties": {
        "id": { "$ref": "#/$defs/id" },
        "version": { "type": "integer", "minimum": 1 },
        "holder": { "$ref": "#/$defs/id" },
        "source": { "enum": ["statute","treaty","delegation","contract","membership","protocol_governance","court_order"] },
        "source_evidence": { "type": "array", "items": { "$ref": "#/$defs/id" } },
        "parent": { "oneOf": [{ "$ref": "#/$defs/id" }, { "type": "null" }] },
        "R_de_jure": { "$ref": "#/$defs/region" },
        "R_de_facto": { "$ref": "#/$defs/region" },
        "powers": { "type": "array", "items": { "$ref": "#/$defs/power" }, "uniqueItems": true },
        "V": { "$ref": "#/$defs/interval" },
        "status": { "enum": ["recognized","partially_recognized","disputed","effective_control_only"] },
        "review_mechanism": { "type": ["string","null"] },
        "d0_grant": { "oneOf": [{ "$ref": "#/$defs/id" }, { "type": "null" }] },
        "provenance_hash": { "$ref": "#/$defs/hash" }
      }
    },
    "rule": {
      "type": "object",
      "required": ["id","version","issuer","R_r","modality","act_pattern","mandatory","layer","V","provenance_hash"],
      "properties": {
        "id": { "$ref": "#/$defs/id" },
        "version": { "type": "integer", "minimum": 1 },
        "issuer": { "$ref": "#/$defs/id" },
        "R_r": { "$ref": "#/$defs/region" },
        "modality": { "enum": ["O","P","F"] },
        "act_pattern": { "type": "object" },
        "condition": { "type": "object" },
        "mandatory": { "type": "boolean" },
        "layer": { "enum": [1,2,3,4,5] },
        "V": { "$ref": "#/$defs/interval" },
        "provenance_hash": { "$ref": "#/$defs/hash" }
      }
    },
    "evidence": {
      "type": "object",
      "required": ["id","claim","claim_type","issuer","time","proof","disclosure","weight_class","interested"],
      "properties": {
        "claim_type": { "enum": ["identity","authority","consent","event","rule_version","delegation","revocation"] },
        "disclosure": { "enum": ["full","selective","commitment_only"] },
        "weight_class": { "enum": ["cryptographic","institutional","public_party_statement","anonymous_attribution"] },
        "interested": { "type": "boolean" },
        "proof": {
          "type": "object",
          "required": ["type"],
          "properties": { "type": { "enum": ["signature","merkle_path","zk_proof","attestation"] } }
        }
      }
    },
    "relationship": {
      "type": "object",
      "required": ["id","subject","object","authority","kind","rules","R_rel","V","state","role"],
      "properties": {
        "state": { "enum": ["proposed","active","suspended","terminated","challenged","disputed"] },
        "role": { "enum": ["party","affected"] }
      }
    }
  }
}
```

> The `action`, `decision`, `challenge`, `snapshot`, `forum_policy` and `sigma` schemas follow the field lists in §4, §6 and §7 and are to be added in the next iteration. **[Open task T-1]**

---

## 11. Conformance suite

An implementation is **conformant** if it produces the stated outcome for every vector below and satisfies §6.6. Vectors are given in prose-table form; machine-readable fixtures (JSON) are to be generated from this table. **[Open task T-2]**

### 11.1 Region and chain tests

| ID | Setup | Expected |
|---|---|---|
| TV-01 | `D2.finance.crypto ⊑ D2.finance` | `true` |
| TV-02 | `D2.finance ⊆ D2.finance.crypto` | `false` (closure only downward) |
| TV-03 | `D2.finance ⊆ D2.property` | `false` (siblings) |
| TV-04 | `D2.x ⊆ D0` | `false` (roots not nested) |
| TV-05 | Region `A ∩ B` where `D(A) = D2.finance`, `D(B) = D2.property` | `D = []` → empty, no collision possible |
| TV-06 | Child Authority with `R(child) ⊄ R(parent)` | `Valid = False` (escalation) |
| TV-07 | Child Authority with `P(child) ⊄ P(parent)` | `Valid = False` |
| TV-08 | Child Authority with `V(child) ⊄ V(parent)` | `Valid = False` |
| TV-09 | Parent chain contains a cycle | `Valid = False`, logged |
| TV-10 | Root Authority with `D0.privacy` in D and `d0_grant = null` | `Valid = False` **[SD-1]** |
| TV-11 | Opaque S in a delegated Authority | `Unknown` → `Valid = False`, logged (fail-closed) |
| TV-12 | Delegation evidence only `public_party_statement` | `Valid = False` |

### 11.2 Cascade tests

| ID | Setup | Expected |
|---|---|---|
| TV-20 | Rule with `R_r ⊄ R(issuer)` | Excluded at step 0 (ultra vires), logged |
| TV-21 | Mandatory L1 `F` vs voluntary L2 `P` | L1 prevails at step 2 |
| TV-22 | L1 (UA) `P` vs L1 (X) `F`, no ≻_rec, π(UA) > π(X) | UA rule prevails at step 4 |
| TV-23 | Same as TV-22 but π equal; equal D and S; same issuer? No | Falls through steps 5–7; if none decide → `Open` (step 8) |
| TV-24 | Same issuer, same region, v1 vs v2, both valid at t | v2 prevails (step 6); no retroactivity before v2 `from` |
| TV-25 | `precedence_rec` contains A ≻ B and B ≻ A | `Open(reason = cyclic_precedence)` |
| TV-26 | Rule violating Φ0 | Excluded at step 1 |
| TV-27 | Two projections, results compatible | `Resolved` |
| TV-28 | Two projections, one `Open` | `Open`, projection named |
| TV-29 | Relationship `disputed`, outcome depends on its existence, no sufficient evidence | `Open`, not assumption |
| TV-30 | Same inputs twice | Byte-identical Σ |

### 11.3 TV-40 — The reference scenario (Starshield/Starlink, October 2026)

This vector uses the case from §10 of the formal model. It is included because it exercises `Open`, spillover, Gap, claims-vs-evidence and `affected` standing at once. It is a **test of the model's behaviour on a contested event**, not a finding of fact. All facts are as reported in open sources as of 18.10.2026 and may change.

**Setup (abstracted).**

| Element | Value |
|---|---|
| Action | Speed restriction on terminals in motion, territory UA |
| Projection j1 | `(D2.communications, u{territory:UA, account, establishment:US, effects:UA}, 2)` |
| Projection j2 | `(D1.defence, u{effects:UA}, 2)` |
| α_provider | holder = commercial provider; `R_de_jure` covers `D2.communications` only; `source = contract` |
| α_state | holder = state; competent in `D1.defence`; no recorded delegation to the provider |
| Evidence about who decided | three incompatible statements from interested parties → all `public_party_statement` |
| Evidence about which system | contested → `Relationship(UA ↔ Starshield).state = disputed` |
| Technical change logs, contract terms | not available in the Snapshot |
| `review_mechanism` | none known |
| Affected Subjects | operators and civilians: `role = affected` |

**Expected output.**

1. j1: provider's rule is admissible (inside `R_de_jure`) → processed normally.
2. j2: **Spillover** flagged for α_provider. α_state is competent in D1 but no Evidence of delegation to the provider with `weight_class ∈ {cryptographic, institutional}` → excluded as `insufficient_evidence`.
3. §6.4 **[SD-4]**: `A(j2) = ∅` *because* candidates were excluded → `Open(reason = no_admissible_basis, projection = 1)`.
4. Overall result: `Open`. `route = null` → a **finding** `missing_review_mechanism`.
5. Findings also include `gap` for α_provider and `unverifiable_rule_version` (no version record for the change).
6. The three public statements are written to Σ as claims. None enters a chain.
7. Affected Subjects have `standing = affected` for a `fileChallenge`.
8. The model records **no** conclusion on motive, legality, or who is right.

---

## 12. Spec decisions and open items

### 12.1 Spec decisions (proposed, require approval)

| ID | Decision | Reason |
|---|---|---|
| SD-1 | Root Authorities with D0 competence require `d0_grant` | D0 powers must never be implicit (Invariant 4) |
| SD-2 | Action projections come from a classifier whose output is Evidence; a human/Authority attests the set used | Keeps AI as Evidence (Invariant 12) |
| SD-3 | `condition` in a sandboxed, deterministic expression language | Determinism and termination |
| SD-4 | `A = ∅` from step-0 exclusions on a claimed-authority action → `Open(no_admissible_basis)` | Makes the v0.2 case result precise; prevents "nothing applies, so nothing wrong" |
| SD-5 | `Unknown` containment is always treated as *not contained* | Fail-closed Non-Escalation |

### 12.2 Open issues (carried from the formal model, §9)

| ID | Issue | Status |
|---|---|---|
| OI-1 | Are S and Sc orthogonal? Test on a real-norm corpus; if Sc is ~functionally dependent on S, merge | Open. Schema keeps them separate, may change |
| OI-2 | Who maintains the Dom taxonomy; cross-jurisdiction versioning | Open. This spec only requires versioned taxonomies and mappings |
| OI-3 | Publication format and governance of weights, `≻_rec`, Φ0 | Open. ForumPolicy (§6.1) is a first proposal |
| OI-4 | Procedure by which a forum decides recognition of an Authority | Open. The model records `status` and `Gap` only |
| OI-5 | Is L3 (voluntary norms) a stack layer or a Relationship type? | Open |
| OI-6 | Transitivity of the pairwise cascade; formal proof or a restricted class of admissible tables | Open. Mitigation: canonical pair order, acyclicity check, `Open` on non-transitive cases |
| OI-7 | Time as a full axis (4D) for retrospective cases | Open |
| OI-8 | Formalization of Φ0 without turning the model into a new sovereign | Open |

### 12.3 Next tasks

| ID | Task |
|---|---|
| T-1 | Complete the JSON Schemas for Action, Decision, Challenge, Snapshot, ForumPolicy, Σ |
| T-2 | Generate machine-readable fixtures for TV-01…TV-40 |
| T-3 | Reference implementation of §3 and §5 (containment, `Valid`, `Competent`) |
| T-4 | Reference implementation of §6 with Σ output and snapshot hashing |
| T-5 | Define at least one concrete proof suite for P1–P3 |
| T-6 | Corpus test for OI-1 (S/Sc orthogonality) |

---

## Appendix A — Worked containment example

```json
{
  "authority": {
    "id": "auth:example-001",
    "R_de_jure": {
      "taxonomy": { "id": "dom-taxonomy", "version": 4 },
      "D": ["D2.finance"],
      "S": { "subject_classes": ["legal_entities"], "factors": ["establishment", "effects"] },
      "Sc": [0, 1]
    }
  },
  "rule": {
    "id": "rule:example-017",
    "R_r": {
      "taxonomy": { "id": "dom-taxonomy", "version": 4 },
      "D": ["D2.finance.crypto"],
      "S": { "subject_classes": ["legal_entities"], "factors": ["establishment"] },
      "Sc": [1]
    }
  }
}
```

`Competent(rule:example-017)`:

- D: `D2.finance.crypto ⊑ D2.finance` ✓
- S: classes `legal_entities ⊑ legal_entities` ✓; factors `{establishment} ⊆ {establishment, effects}` ✓
- Sc: `{1} ⊆ {0,1}` ✓
- V: assumed contained ✓

→ `Competent = true`. Had the rule listed `Sc: [2]`, containment would fail and the rule would be excluded at step 0 with `reason = no_competence`.

## Appendix B — Mapping to `regulator.md`

| `regulator.md` concept | Spec element |
|---|---|
| Regulator (authority-bearing Object) | Object + Authority |
| Regulated Subject | Subject linked through a Relationship |
| Authority source | `Authority.source`, `source_evidence` |
| Regulatory domain / geographic / temporal scope | `R_de_jure` (D, S, Sc), `V` |
| Applicable rules | Rules with `issuer = Authority.id` |
| Powers / enforcement | `powers`, `limits` |
| Review / appeal / termination | `review_mechanism`, `revocable`, Challenge |
| Regulator ≠ Jurisdiction | A jurisdiction is a Region/L1 source; a Regulator is an Authority holder inside it |
| Regulator ≠ Sovereign Subject | No Rule targets L0; Authority is bounded by source, scope, purpose |

## License
Apache 2.0

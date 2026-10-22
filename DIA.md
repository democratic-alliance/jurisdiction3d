[Jurisdiction3D](README.md) | DIA Stack | [DIA Regulator](regulator.md) | [Tech Spec](TECH-SPEC.md) | [Funding](FUNDING.md)

# DIA — Foundational Architecture

This document is organized into three top-level sections: 
* the [ontological model](1-dia-jurisdiction-stack) 
* the [normative framework](2-dia-normative-framework) 
* and the [architectural charter](3-dia-rchitectural-charter). 

They are connected but have distinct purposes and grounds of justification.

# 1. DIA Jurisdiction Stack

### 1. The Subject Comes First

Every jurisdictional system begins with a **Subject**.

The Subject is the individual person: the bearer of identity, agency, rights, interests, obligations, and the capacity to enter relationships.

The Subject is not an application, an organization, a contract, a protocol, or a jurisdiction.

Those are **Objects and normative environments surrounding the Subject**.

DIA begins with this distinction:

> **The Subject exists before the systems that describe, govern, contract with, or serve the Subject.**

---

### 2. The World Is a Stack of Relationships

A person does not exist inside a single jurisdictional system.

A person simultaneously participates in multiple systems:

- states and local jurisdictions;
- contracts;
- organizations;
- protocols;
- DAOs;
- ethical and religious communities;
- applications and platforms.

Each system defines a particular relationship with the Subject.

Therefore DIA does not model jurisdiction as a single vertical chain.

It models it as a **set of Relationships between a Subject and multiple Objects**.

```text
                         OBJECTS
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
        State           Contract             DAO
          │                 │                 │
          └──────────────┬──┴──┬──────────────┘
                         │     │
                         SUBJECT
                            │
             ┌──────────────┴──────────────┐
             │                             │
          Ethics                      Application
```

The Subject remains the reference point.

---

### 3. The Six Layers

DIA represents these relationships through six conceptual layers.

#### L0 — Subject / Fundamental Personal Layer

**The Subject.**

The individual is the primary entity in the model.

L0 represents:

- personal identity;
- agency;
- autonomy;
- interests;
- capacities;
- personal boundaries;
- the ability to enter or terminate relationships where legally possible.

L0 is not a jurisdiction.

It is the **entity to which jurisdictional relationships attach**.

---

#### L1 — Local Jurisdiction

**The Subject ↔ State/Jurisdiction relationship.**

A jurisdiction is an external normative system that applies to the Subject according to legally recognized connecting factors.

It can create mandatory rights and obligations.

The Subject does not necessarily choose this relationship.

Therefore L1 is fundamentally different from a voluntary application account or contractual relationship.

---

#### L2 — Contractual Obligations

**The Subject ↔ Contract relationship.**

A contract creates a defined relationship between parties.

Its authority derives from the agreement together with the legal framework that recognizes and limits it.

A contract therefore does not redefine the Subject.

It defines **what obligations and rights arise between the Subject and another party**.

---

#### L3 — Religious / Ethical Norms

**The Subject ↔ Normative Community relationship.**

Ethical, philosophical, cultural, and religious systems may influence how the Subject chooses to act.

Their authority may derive from:

- personal conviction;
- community membership;
- tradition;
- belief;
- voluntary commitment.

DIA therefore distinguishes normative commitment from legally compulsory jurisdiction.

---

#### L4 — Protocol / DAO Rules

**The Subject ↔ Protocol relationship.**

A protocol establishes machine-readable rules governing participation in a technical system.

A DAO may additionally establish governance mechanisms through which participants create, modify, or enforce protocol rules.

The protocol does not become the Subject.

The Subject participates in the protocol under a defined set of technical and governance relationships.

---

#### L5 — Application / Organization Policies

**The Subject ↔ Organization/Application relationship.**

Applications, platforms, employers, communities, and organizations establish operational rules for participation.

These rules may determine:

- access;
- permissions;
- roles;
- moderation;
- available functionality;
- organizational procedures.

They operate within their defined scope.

An application account does not become the person's identity.

It represents **one relationship between the Subject and an Object**.

---

### 4. Objects Do Not Become the Subject

A fundamental DIA principle is:

> **No Object should be mistaken for the Subject.**

A government database is not the person.

A passport is not the person.

A contract is not the person.

A wallet address is not the person.

A DAO membership is not the person.

An application account is not the person.

A digital identity record is not the person.

They are representations, relationships, credentials, or containers of authority associated with the Subject.

```text
Subject
   │
   ├── represented by → Identity
   ├── governed by    → Jurisdiction
   ├── obligated by   → Contract
   ├── participates in→ Protocol
   ├── belongs to     → Organization
   └── interacts with → Application
```

DIA therefore separates **identity of the Subject** from **representations of the Subject**.

---

### 5. Authority Is Relational

Authority does not exist merely because an Object claims it.

DIA asks:

> **What is the Relationship between this Object and this Subject, and what gives that Relationship its authority?**

For every Relationship, DIA should be able to represent:

```text
Subject
   ↓
Object
   ↓
Relationship
   ↓
Scope
   ↓
Source of authority
   ↓
Rights / obligations
   ↓
Validity conditions
```

This makes authority explicit rather than implicit.

---

### 6. Jurisdiction Is Not Identity

A jurisdiction may determine what rules apply to a Subject.

It does not determine the complete identity of the Subject.

Likewise, an organization may recognize a person as an employee, a protocol may recognize an address as a participant, and an application may recognize an account as a user.

These are different predicates over the same Subject.

Therefore:

> **Identity is persistent across relationships; jurisdiction is contextual to relationships.**

---

### 7. Multiple Jurisdictions Can Coexist

The Subject may simultaneously have relationships with multiple jurisdictions and normative systems.

These relationships may overlap.

They may also conflict.

DIA therefore does not assume:

```text
Person → One Jurisdiction
```

Instead:

```text
                         ┌─ Jurisdiction A
                         │
                         ├─ Jurisdiction B
                         │
SUBJECT ─────────────────┼─ Contract A
                         │
                         ├─ Protocol A
                         │
                         ├─ Organization A
                         │
                         └─ Application A
```

The central problem becomes not identity assignment but **relationship resolution**.

---

### 8. Conflict Must Be Explicit

When two Objects impose incompatible rules, DIA should not silently choose one.

It should represent the conflict.

```text
Subject
   │
   ├── Object A
   │     └── Rule A
   │
   └── Object B
         └── Rule B

Rule A ≠ Rule B
       ↓
   Conflict
       ↓
Resolution mechanism
```

Resolution may depend on:

- jurisdiction;
- contractual hierarchy;
- temporal precedence;
- scope;
- consent;
- protocol governance;
- applicable law;
- recognized dispute mechanisms.

The important principle is:

> **Conflict is data, not an error to be hidden.**

---

### 9. The Subject Is Not Owned by the Stack

Higher layers may represent, govern, constrain, or serve the Subject.

They do not thereby acquire ownership of the Subject.

The distinction is fundamental:

> **A system may possess authority over a relationship without possessing the person who participates in that relationship.**

DIA consequently treats personal identity as the anchor from which relationships are established, rather than as an asset belonging to whichever system currently contains the person's data.

---

### 10. From Identity Management to Relationship Management

Traditional identity systems tend to ask:

> "Who is this person?"

DIA asks an additional question:

> **"What relationships does this Subject currently have with the systems surrounding them?"**

The resulting model is:

```text
                 OBJECT
                   │
              Relationship
                   │
                   ▼
SUBJECT ─────────────────────
                   ▲
              Relationship
                   │
                 OBJECT
```

Identity establishes **who the Subject is**.

DIA additionally establishes:

- who recognizes the Subject;
- under what authority;
- for what purpose;
- within what scope;
- for what period;
- with what obligations;
- with what rights;
- and under what conditions the relationship changes or terminates.

---

### 11. Subject Principle

DIA therefore rests on a simple architectural principle:

> **Subject first. Relationships second. Objects third.**

The Subject is the persistent reference.

Objects are contextual.

Relationships define authority.

Jurisdictions define applicable rules.

Contracts define negotiated obligations.

Protocols define machine-enforceable participation.

Organizations define institutional relationships.

Applications define operational access.

None of these should be confused with the Subject itself.

---

### 12. The International Layer

DIA's purpose is not necessarily to create another jurisdiction above existing jurisdictions.

Its purpose is to provide a **neutral coordination layer for relationships that already cross jurisdictional boundaries**.

A person may simultaneously exist within several legal, contractual, organizational, technological, and normative environments.

DIA provides a common model for representing those relationships without requiring them to collapse into a single authority.

Thus:

> **DIA is not a super-state.**
>
> **DIA is a international relationship layer.**

It connects Subjects to the many systems in which they participate while preserving the distinction between the Subject and the Objects that govern, represent, or interact with them.

---

### Core Axiom

> **The Subject is not a layer of the system.**
>
> **The Subject is the reference point from which the system's relationships are understood.**

And therefore:

```text
             THE WORLD OF OBJECTS

   Jurisdiction ─┐
   Contract ─────┤
   Protocol ─────┤
   Organization ─┼─── Relationships ─── SUBJECT
   Application ──┤
   Norms ────────┘

                    L0
                 THE SUBJECT
```

**DIA begins with the person, not the institution.**

---

# 2. DIA Normative Framework

## Purpose and Scope

The DIA Normative Framework justifies and defines rights, obligations, legitimate authority, and conflict-resolution principles. It builds on the ontological model in the DIA Jurisdiction Stack but does not treat architectural consistency or institutional recognition as sufficient proof of normative legitimacy.

The Subject is the individual person who bears identity, agency, interests, rights, and obligations. Relationships with states, contracts, normative communities, protocols, organizations, and applications may establish different rights and obligations, but the existence of a relationship does not by itself establish that every exercise of authority within it is legitimate.

## Foundational Distinctions

The framework must preserve these distinctions:

- **Existence and recognition:** Recognition acknowledges a Subject or a claim about the Subject; it does not create the Subject's existence.
- **Relationship and legitimacy:** A relationship may exist without every claim of authority within it being justified.
- **Capability and authorization:** The technical ability to perform an action does not establish permission to perform it.
- **Authorization and legitimacy:** Institutional authorization does not, by itself, establish normative legitimacy.
- **Structural validity and normative permissibility:** A structurally valid operation may still violate a right or obligation.
- **Moral obligation and formal agreement:** An obligation need not arise from a contract, and the existence of a contract does not prove that every term is legitimate.
- **Conflict and error:** Conflicting rules must be represented and evaluated explicitly rather than silently discarded.

## Normative Justification Requirements

Every rights constraint or normative rule adopted by DIA should document:

1. **Grounding:** The argument explaining why the right or obligation exists.
2. **Scope:** The Subjects, relationships, contexts, and circumstances to which it applies.
3. **Content:** The conduct that is required, permitted, prohibited, or conditionally permitted.
4. **Authority:** The source and limits of any authority to interpret, apply, or enforce the rule.
5. **Conflict handling:** The procedure for identifying and resolving conflicts with other rights, obligations, or rules.
6. **Review and remedy:** How decisions can be challenged, corrected, or remedied.
7. **Operationalization:** Which parts can be expressed as machine-checkable constraints and which require human interpretation or adjudication.
8. **Limitations:** Known uncertainties, exceptions, assumptions, and risks of misapplication.

## Ethical Sources

Biblical tradition may be one source of ethical inspiration, alongside philosophical, legal, cultural, and other ethical traditions. Ethical inspiration is not, by itself, a sufficient justification for imposing a rule on every Subject or jurisdiction.

A normative rule must be supported by an explicit argument appropriate to its intended scope. DIA must not silently turn the doctrine of one tradition into a universal technical requirement, nor claim that a technical invariant proves a moral proposition.

## Relationship to Architecture

The normative framework and the architectural invariants are connected but distinct:

- The **Normative Framework** justifies rights and obligations and defines how they should be interpreted.
- The **Architectural Charter** states architectural requirements and formal invariants.
- The implementation may enforce selected normative rules through authorization checks or other formal constraints when their meaning and applicability can be specified adequately.

The enforcement of a rule proves, at most, that the implementation applied the encoded rule under the specified conditions. It does not independently prove that the rule is just.

## Conflict Resolution

When rules or claims conflict, DIA should represent the conflict explicitly and preserve the relevant context. Resolution may depend on scope, applicable jurisdiction, the source and limits of authority, consent where relevant, temporal precedence, contractual relationships, governance procedures, applicable law, or a recognized dispute-resolution process.

No single ordering rule should be assumed to resolve every conflict. The applicable resolution method and its justification must be explicit, reviewable, and appropriate to the type of conflict.

## Status

This section establishes the intended structure and foundational distinctions of the DIA Normative Framework. It is not yet a complete or universally justified catalogue of rights. Individual rights, obligations, and conflict-resolution rules require separate specification and justification.

---

# 3. DIA Architectural Charter

## Purpose and Scope

The DIA Architectural Charter defines architectural requirements and formal invariants that make DIA implementations verifiable. It is grounded in explicit system requirements, not directly derived from a religious or philosophical doctrine.

A principle in this Charter is an architectural commitment until its scope, predicates, applicability conditions, failure semantics, and verification method have been specified. A principle must not be described as a formally verified invariant merely because it appears in this document.

## The Twelve Architectural Principles

### 1. Identity Integrity

The system must distinguish Subjects according to an explicitly defined identity model. It must not silently conflate distinct Subjects or treat representations as distinct Subjects when the applicable identity model establishes their equivalence.

**Requires:** Subject model, identity scope, equivalence rules, and identity-claim semantics.

### 2. Referential Integrity

References must satisfy the resolution rules applicable to their type and context. A syntactically well-formed identifier is not sufficient if the referenced object does not resolve under the applicable rules.

**Requires:** Reference types, resolution rules, and behavior for missing, revoked, or unresolved references.

### 3. Semantic Integrity

Relations must preserve their explicitly defined meaning. Distinct relations—such as ownership, possession, custody, and authorization—must not be silently substituted for one another.

**Requires:** Relation vocabulary, type system, semantic definitions, and transformation rules.

### 4. Dependency Integrity

Required dependencies must satisfy the structural constraints defined for the relevant graph or relation type. Acyclicity applies where required by the dependency model; it must not be imposed indiscriminately on every relation.

**Requires:** Graph semantics, dependency classification, and cycle-handling rules.

### 5. Historical Integrity

The system must distinguish recorded events, current state, and later interpretations. Corrections, reversals, disputes, and invalidations must not silently falsify the relevant history. Historical integrity does not require indefinite retention of every datum; privacy, minimization, and deletion requirements must be specified separately.

**Requires:** Event model, correction and supersession semantics, retention rules, and deletion behavior.

### 6. Deterministic Validation

Equivalent representations of the same relevant state must produce equivalent validation results under the same applicable invariant set and validation context.

**Requires:** State model, representation-equivalence relation, explicit validation inputs, and version selection.

### 7. Error and Invalidity Separation

The system must distinguish structural invalidity from operational failure and must not infer culpability solely from either. Network failure, unavailable services, invalid transitions, and unauthorized operations are different conditions.

**Requires:** Error taxonomy, transition model, failure behavior, and recovery semantics.

### 8. Explicit Invariant Evolution

Changes to applicable invariants must be identifiable and governed by explicit versioning and compatibility rules. A new version must not silently redefine the validity of historical operations without specifying the consequences.

**Requires:** Version identifiers, migration rules, compatibility criteria, and change governance.

### 9. Rights-Constraint Separation

Rights and normative restrictions must be represented separately from purely structural validity conditions. Structural validity must not be treated as proof that an operation respects all applicable rights.

**Requires:** Explicit interfaces between invariant validation, normative policy evaluation, and authorization.

### 10. Agreement Integrity

Agreements must have explicit semantics, participants, terms, and lifecycle rules appropriate to their intended use. The existence of an agreement must be distinguished from its current status, enforceability, and normative legitimacy.

**Requires:** Agreement model, lifecycle state machine, amendment and termination rules, and applicable authorization rules.

### 11. Authority Separation

Technical capability, institutional authority, authorization, structural validity, and normative legitimacy must remain distinct. Technical control does not by itself establish permission, and institutional authorization does not by itself establish normative legitimacy.

**Requires:** Authority model, authorization rules, and a defined relationship to the Normative Framework.

### 12. Verifiable Accountability

Decisions and state transitions must provide evidence appropriate to the system's accountability requirements. Auditability must not be confused with truth, correctness, or legitimacy; evidence supports only the claims warranted by its verification model.

**Requires:** Evidence model, verification procedures, provenance rules, and accountability requirements.

## Formalization and Conformance

Each formal invariant must specify:

- the objects and relations to which it applies;
- the conditions under which it applies;
- its formal predicate or validation procedure;
- required behavior when the condition fails;
- dependencies on other invariants;
- versioning and compatibility requirements;
- its verification method; and
- known limitations.

A DIA implementation conforms to a principle only to the extent that the corresponding requirements have been implemented and verified. Principles that remain underspecified are architectural commitments, not established formal guarantees.

## Governing Rule

No implementation detail, institutional decision, or technical capability may silently redefine the meaning of a foundational principle. Architectural changes must follow explicit change-management rules. Normative changes must follow the justification and governance procedures defined by the Normative Framework.

Architectural consistency alone does not establish the legitimacy of a system or the justice of the rules it enforces.

## Status

This Charter is a proposed architectural specification. The twelve principles require further refinement into individually testable requirements and formal invariants where appropriate.

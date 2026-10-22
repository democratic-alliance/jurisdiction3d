Jurisdiction3D | [DIA Stack](DIA.md) | [DIA Regulator](regulator.md) | [Tech Spec](TECH-SPEC.md) | [Funding](FUNDING.md)
# DIA Jurisdiction3D Manifesto
### Version 0.1.1 — Draft

> **L0❤️ defines the Subject.  
> Jurisdiction constrains the action.  
> DIA verifies the boundary.**

---

## 0. Purpose

Modern society operates across multiple normative systems simultaneously.

A person may be subject to the law of a state, contractual obligations, professional rules, organizational policies, religious or ethical principles, and the rules of digital protocols.

These systems increasingly interact across borders.

Yet most existing institutions assume a simpler world: one person, one jurisdiction, one authority, one trusted intermediary.

**DIA International** proposes another model.

It is a protocol architecture for interaction between people, organizations, digital systems, and jurisdictions that:

- preserves the authority of applicable local law;
- allows multiple normative systems to coexist;
- separates identity, authority, rules, and evidence;
- makes delegated authority explicit and bounded;
- provides verifiable evidence of what happened;
- prevents unauthorized escalation of delegated privileges;
- enables cross-jurisdictional procedures without creating a jurisdiction above existing jurisdictions.

DIA International is not a replacement for the state.

It is not a new sovereign.

It is not an attempt to abolish jurisdiction.

It is an interoperability layer between jurisdictions, norms, identities, and actions.

---

# I. Fundamental Principles

## 1. Local Jurisdiction Is Real

Every legally relevant action occurs within some applicable legal context.

DIA International does not replace:

- national law;
- local law;
- courts;
- regulators;
- administrative authorities;
- constitutional orders;
- mandatory legal requirements.

Where mandatory local law applies, it remains authoritative.

DIA provides interoperability with jurisdiction.

It does not claim supremacy over jurisdiction.

> **Compatibility, not substitution.**

---

## 2. Delegation Is Not Surrender

A person may delegate authority without surrendering ownership of the underlying identity, values, rights, or protected personal domain.

Delegation must be:

- explicit;
- scoped;
- time-bounded where appropriate;
- revocable where legally possible;
- auditable;
- non-escalating.

An authority delegated for one purpose must not automatically become authority for another.

> **Authority must have a scope.**

---

## 3. No Privilege Escalation

A delegated authority must never silently acquire greater privileges than those explicitly granted.

If an entity receives authority over `L4`, this does not imply authority over `L3`, `L2`, `L1`, or `L0`.

Formally:

```text
Granted(A, Lₙ) ≠ Granted(A, L₀...Lₙ₋₁)
```

unless such authority has been explicitly established by a legitimate normative process.

> **Delegated authority cannot manufacture its own authority.**

---

# II. The Normative Stack

DIA International recognizes that different kinds of rules are not interchangeable.

A possible normative stack is:

```text
L5 — Application / Organization Policies
L4 — Protocol / DAO Rules
L3 — Religious / Ethical Norms
L2 — Contractual Obligations
L1 — Local Jurisdiction
L0 — Fundamental Personal Layer
```

These layers may interact.

They must not be confused.

A contract is not automatically legislation.

A DAO rule is not automatically a court order.

A religious norm is not automatically state law.

A local regulation is not automatically a protocol rule.

Each rule must carry its provenance, authority, scope, and applicability.

---

# III. L0 — The Protected Layer

L0 represents the fundamental personal domain of the subject.

It may include:

- personhood;
- fundamental identity;
- bodily integrity;
- fundamental autonomy;
- protected privacy;
- deeply held values and beliefs;
- other fundamental attributes recognized by the applicable legal and normative framework.

L0 is **not a jurisdiction above the law**.

L0 does not create immunity from mandatory legal obligations.

Instead, L0 establishes a boundary between:

**the existence and fundamental identity of the subject**

and

**actions performed by that subject in the external world.**

This distinction is fundamental.

> **L0 is protected.  
> Actions are jurisdictionally constrained.**

The purpose of jurisdiction is not to obtain unrestricted root access to the person.

The purpose of jurisdiction is to establish which rules govern legally relevant actions.

---

# IV. Jurisdiction at the Boundary

The central architectural distinction of DIA International is:

```text
L0 ≠ Action
L0 ≠ Legal Immunity
Control ≠ Constraint
```

A jurisdiction does not need unrestricted control over the internal state of a person in order to regulate an external action.

For example:

```text
Person
  │
  ├── L0: protected personal domain
  │
  └── Action
        │
        ├── jurisdiction
        ├── applicable rules
        ├── authority
        └── evidence
```

The legal system operates primarily at the boundary where an intention becomes an externally relevant action.

DIA provides evidence that this boundary was crossed according to defined rules.

> **L0 defines the subject.  
> Jurisdiction constrains the action.  
> DIA verifies the boundary.**

---

# V. Normative Pluralism

DIA International assumes that human interactions may involve several normative systems simultaneously.

For example:

```text
Mandatory Local Law
        ↓
Contract
        ↓
Organizational Rules
        ↓
Religious / Ethical Framework
        ↓
Protocol Rules
```

These systems may coexist without being treated as equivalent.

A participant may voluntarily choose to operate according to a halal/haram framework, an ethical code, professional standards, or another normative system.

Such a framework may govern a relationship when properly adopted and applicable.

However:

> A voluntarily adopted norm does not automatically become state law.

Where a voluntary rule conflicts with mandatory local law, the applicable mandatory law remains controlling.

DIA does not itself determine whether something is halal, haram, ethical, or unethical.

Instead, DIA should be able to record:

- which normative framework was selected;
- who defined or authorized it;
- which version was used;
- which rules were applied;
- when they were applied;
- to which participants and actions they applied.

---

# VI. Proof Before Trust

Traditional institutions frequently depend on trusted intermediaries.

DIA attempts to reduce this dependency.

The objective is not:

> “Trust the system.”

The objective is:

> **“Verify what the system claims happened.”**

A DIA-compatible interaction should, where technically and legally possible, provide verifiable evidence of:

- identity;
- authority;
- consent;
- applicable rules;
- relevant events;
- provenance;
- timestamps;
- decisions;
- delegation;
- revocation;
- modifications;
- procedural steps.

Trust may remain necessary.

But trust should not be required where cryptographic or independently verifiable evidence can replace it.

---

# VII. Auditability Is Constitutional

Auditability is not merely a compliance feature.

For a jurisdictional protocol, auditability is a fundamental property.

A participant should be able to determine:

```text
WHO
  acted

UNDER WHICH AUTHORITY
  they acted

UNDER WHICH RULES
  they acted

ON WHAT EVIDENCE
  they acted

WHAT ACTION
  was performed

WHAT DECISION
  resulted

AND WHY THAT AUTHORITY
  was applicable
```

The goal is not necessarily to expose every piece of information.

The goal is to make the **validity of the process verifiable** while minimizing unnecessary disclosure.

> **Verifiability without unnecessary exposure.**

---

# VIII. Identity Is Not Authority

Knowing who someone is does not automatically determine what they are authorized to do.

Therefore:

```text
Identity ≠ Authority
Authority ≠ Jurisdiction
Jurisdiction ≠ Rule
Rule ≠ Evidence
Evidence ≠ Decision
```

DIA should maintain these concepts as distinct primitives.

An identity system must not silently become an authorization system.

An authorization system must not silently become a jurisdiction.

A jurisdiction must not silently become an unrestricted surveillance mechanism.

---

# IX. Consent Is Not the Only Source of Authority

Consent is important but insufficient as a universal foundation.

Authority may arise from different sources, including:

- explicit delegation;
- contract;
- law;
- court or administrative process;
- organizational governance;
- protocol governance;
- other recognized normative mechanisms.

DIA must therefore represent not merely:

> “Did the person consent?”

but:

> **“Why does this actor have authority to perform this action?”**

This distinction becomes critical when authority is imposed by law or exercised under a recognized institutional process.

---

# X. AI and Automated Adjudication

AI may participate in:

- evidence analysis;
- identity verification;
- rule matching;
- anomaly detection;
- procedural assistance;
- translation;
- legal research;
- decision support;
- generation of explanations.

AI may assist a jurisdiction.

AI must not silently become the source of jurisdictional authority.

Every consequential automated decision should have an identifiable:

- authority;
- applicable rule set;
- evidence basis;
- procedural context;
- scope;
- accountability mechanism.

The system must distinguish between:

```text
AI generated analysis
```

and

```text
legally authoritative decision
```

These are not inherently the same thing.

---

# XI. Cross-Jurisdictional Operation

International does not mean “above national”.

It means that an interaction may cross jurisdictional boundaries without requiring the participants to pretend those boundaries do not exist.

For example:

```text
Person A
   │
Jurisdiction A
   │
   ├── DIA
   │
   ├── International Procedure
   │
   ├── DIA
   │
Jurisdiction B
   │
Person B
```

The protocol should identify:

1. which jurisdiction applies;
2. which rules apply;
3. which authority is recognized;
4. which procedures are available;
5. how evidence can be transferred;
6. how decisions can be recognized or challenged.

DIA International therefore aims at **jurisdictional interoperability**, not jurisdictional replacement.

---

# XII. Separation of Control and Constraint

One of the central design principles is the separation between **control** and **constraint**.

A system may constrain an action without controlling the entire subject.

For example:

```text
Constraint:
“You may not perform Action X under Rule Y.”

is fundamentally different from:

Control:
“We must possess unrestricted control over your internal state
in order to prevent Action X.”
```

DIA favors the first model wherever legally and technically possible.

This allows mandatory rules to remain enforceable while reducing unnecessary intrusion into protected personal domains.

---

# XIII. Minimal Authority

Every authority should have the minimum privileges necessary to perform its legitimate function.

```text
Required Privilege ⊆ Granted Privilege
```

and preferably:

```text
Granted Privilege ≈ Required Privilege
```

Authority should therefore be:

- minimal;
- explicit;
- scoped;
- inspectable;
- revocable where appropriate;
- resistant to privilege escalation.

This principle applies equally to:

- humans;
- organizations;
- courts;
- governments;
- DAOs;
- smart contracts;
- AI agents;
- automated services.

No category of actor receives unlimited authority merely because of its identity.

---

# XIV. The Right to Know the Rule

A participant should not be subjected to an opaque normative process when the system can reasonably disclose the rules governing the interaction.

Where appropriate, the participant should be able to determine:

```text
Which rule?
Which version?
Which authority?
Which jurisdiction?
Which scope?
Which evidence?
Which procedure?
Which decision?
Which appeal or challenge mechanism?
```

Rules should be versioned.

Changes should be attributable.

Applicability should be determinable.

Historical decisions should remain interpretable under the rules that governed them.

---

# XV. The Right to Challenge

A verifiable system must also be contestable.

Auditability without contestability can produce a perfectly auditable injustice.

Therefore, a DIA-compatible jurisdiction should provide mechanisms for:

- disputing identity claims;
- disputing authority;
- disputing evidence;
- disputing rule applicability;
- disputing procedural violations;
- correcting errors;
- appealing or escalating decisions where applicable.

The system must preserve not only the history of decisions, but also the history of disagreement.

> **A challenge is part of the record, not an error in the record.**

---

# XVI. Privacy by Architecture

DIA should not equate auditability with universal visibility.

The architectural objective is:

```text
Maximum verifiability
+
Minimum necessary disclosure
```

Possible mechanisms include:

- selective disclosure;
- zero-knowledge proofs;
- cryptographic commitments;
- pseudonymous identifiers;
- capability-based authorization;
- provenance proofs;
- independently verifiable attestations.

The system should prove what needs to be proven without automatically revealing everything that could be revealed.

---

# XVII. The State Is a Participant, Not a Root Process

Within DIA International, a state may possess legitimate authority.

That authority must nevertheless be represented explicitly.

The architecture should not model the state as an invisible universal root process with unrestricted access to every layer.

Instead:

```text
State Authority
      │
      ▼
Applicable Jurisdiction
      │
      ▼
Legally Relevant Action
```

rather than:

```text
State
  │
  └── root access
        │
        └── everything
```

The difference is architectural, not merely philosophical.

Authority becomes:

- attributable;
- scoped;
- auditable;
- contestable;
- technically representable.

---

# XVIII. The Principle of Non-Escalation

No actor should obtain higher privileges merely because it controls a lower layer.

Formally:

```text
Control(Lₙ) ⟹̸ Control(Lₙ₋₁)
```

unless an explicit legitimate rule establishes that relationship.

In particular:

```text
Control(L4) ⟹̸ Control(L0)
```

and:

```text
Control(L1) ⟹̸ unrestricted access to L0
```

This principle should be treated as an architectural invariant.

---

# XIX. DIA as Evidence Infrastructure

DIA is not intended to become the ultimate authority.

DIA is infrastructure for:

- identity;
- evidence;
- provenance;
- authorization;
- verification;
- audit trails;
- procedural state;
- cryptographic attestations.

The protocol should make it possible to answer:

> “What happened?”

> “Who performed the action?”

> “Under what authority?”

> “According to which rules?”

> “What evidence supports the claim?”

> “Can an independent party verify the result?”

DIA should not answer normative questions that belong to the applicable normative authority.

It should make those decisions **traceable and verifiable**.

---

# XX. The Constitutional Invariants

The following principles form the initial invariant set of DIA International:

### Invariant 1 — Local Authority
DIA does not replace applicable local jurisdiction.

### Invariant 2 — Delegation
Delegation does not equal surrender.

### Invariant 3 — Non-Escalation
Delegated authority cannot silently expand its own scope.

### Invariant 4 — Protected L0
The fundamental personal layer is not an unrestricted target of external control.

### Invariant 5 — Boundary Enforcement
Mandatory rules may constrain legally relevant actions without requiring unrestricted control over L0.

### Invariant 6 — Normative Pluralism
Different normative systems must remain distinguishable.

### Invariant 7 — Provenance
Every authoritative rule must have an identifiable source and version.

### Invariant 8 — Auditability
Consequential actions must be independently verifiable where feasible.

### Invariant 9 — Contestability
A decision must be capable of being challenged through an applicable procedure.

### Invariant 10 — Minimal Authority
Every actor should receive no more authority than necessary for its legitimate function.

### Invariant 11 — Privacy
Verification should minimize unnecessary disclosure.

### Invariant 12 — AI Accountability
AI may assist decision-making but cannot silently become the source of authority.

---

# XXI. The Core Formula

The architecture can be summarized as:

```text
        PERSON
          │
          ▼
         L0
  Protected Personal Layer
          │
          │
     ┌────┴────┐
     │         │
     ▼         ▼
  IDENTITY   ACTION
     │         │
     │         ▼
     │    JURISDICTION
     │         │
     │         ▼
     │        RULES
     │         │
     └────┬────┘
          ▼
         DIA
          │
          ▼
     PROOF / AUDIT
```

Or, more simply:

> **L0 defines the subject.**  
> **Jurisdiction constrains the action.**  
> **Rules define the constraints.**  
> **DIA proves what happened.**

---

# XXII. What This Manifesto Does Not Claim

DIA International does not claim:

- to abolish states;
- to abolish courts;
- to create a universal sovereign;
- to make protocol rules superior to mandatory law;
- to make religious norms equivalent to legislation;
- to make cryptography a substitute for legitimate authority;
- to make AI a sovereign decision-maker;
- to make every human action permanently auditable;
- to eliminate disagreement;
- to eliminate the need for institutions.

Its ambition is narrower and more technical:

> **Make jurisdiction, authority, identity, rules, actions, and evidence interoperable without collapsing them into one system of unrestricted control.**

---

# XXIII. Closing Principle

The central problem is not:

> **Who should control everything?**

The central problem is:

> **How can legitimate authority operate without becoming unlimited authority?**

DIA International begins from the assumption that these are separable problems.

A person can be subject to law without surrendering their entire personal domain.

An organization can receive authority without receiving unlimited power.

A state can enforce its jurisdiction without becoming a universal root process.

A protocol can enforce rules without becoming a sovereign.

An AI can assist decisions without becoming their ultimate source.

A cryptographic system can establish proof without establishing legitimacy by itself.

Therefore:

> **Authority must be explicit.**  
> **Jurisdiction must be bounded.**  
> **Delegation must be scoped.**  
> **Rules must be attributable.**  
> **Evidence must be verifiable.**  
> **Privacy must be architectural.**  
> **Challenge must remain possible.**  
> **And no delegated authority may silently become root.**

**DIA International v0.1.1**

[Jurisdiction3D](README.md) | [DIA Stack](DIA.md) | DIA Regulator | [Tech Spec](TECH-SPEC.md) | [Funding](FUNDING.md)

# DIA. Regulator

## Definition of a Regulator

A **Regulator** is an Object that possesses recognized authority to establish, interpret, supervise, or enforce rules within a defined domain and scope.

A regulator is therefore not a special type of Subject.

It is an **authority-bearing Object** participating in a relationship with one or more Subjects.

```text
SUBJECT
   │
   │ subject to / interacts with
   ▼
REGULATOR
   │
   ├── defines rules
   ├── interprets rules
   ├── supervises compliance
   ├── issues permissions or certifications
   └── applies authorized sanctions
```

The authority of a regulator must have an identifiable **source**.

That source may include:

- legislation;
- delegated governmental authority;
- institutional mandate;
- contractual delegation;
- protocol governance;
- membership rules;
- another recognized source of authority.

Therefore DIA distinguishes:

> **Authority** — the recognized capacity to impose or administer rules.

> **Regulator** — the Object exercising that authority within a defined scope.

> **Regulated Subject** — the Subject to whom the regulatory relationship applies.

---

## Regulatory Relationship

DIA represents regulation as an explicit relationship rather than an intrinsic property of either party:

```text
Subject
   │
   │ regulated by
   ▼
Regulator
   │
   ├── Authority Source
   ├── Regulatory Domain
   ├── Geographic Scope
   ├── Temporal Scope
   ├── Applicable Rules
   ├── Powers
   └── Enforcement Mechanisms
```

This allows DIA to distinguish between:

```text
WHO          → Subject
WHO REGULATES→ Regulator
WHY          → Source of Authority
WHAT         → Regulatory Rules
WHERE        → Jurisdiction / Scope
WHEN         → Temporal Validity
HOW          → Enforcement Mechanism
```

---

## Regulator ≠ Jurisdiction

A **Jurisdiction** defines the legal or normative domain in which rules may apply.

A **Regulator** is an entity exercising regulatory authority within some domain.

The distinction is therefore:

```text
Jurisdiction
     │
     │ establishes / recognizes
     ▼
Regulatory Authority
     │
     ▼
Regulator
     │
     │ regulates
     ▼
Subject
```

A jurisdiction may contain multiple regulators.

A regulator may operate within only part of a jurisdiction.

A regulator may also have authority that crosses territorial boundaries where such authority is legally or contractually recognized.

---

## Regulator ≠ Organization

Not every organization is a regulator.

An organization may simply provide services, employ people, operate infrastructure, or establish membership rules.

It becomes a regulator when it possesses recognized authority to create, administer, supervise, or enforce rules over a defined domain.

Thus:

```text
Organization
     │
     ├── Service Provider
     ├── Employer
     ├── Platform
     ├── Association
     └── Regulator
```

"Regulator" describes a **relationship and authority function**, not merely an organizational type.

---

## Regulator ≠ Sovereign Subject

DIA preserves the distinction between the Subject and the authority governing the Subject.

A regulator may exercise authority over a Subject within its legitimate scope.

It does not thereby become the Subject's owner.

The fundamental relationship remains:

```text
        REGULATOR
             │
       exercises authority
             │
             ▼
          SUBJECT
```

The regulator's authority is therefore **bounded by its source, scope, purpose, and applicable rules**.

---

## Regulatory Authority as a First-Class Object

DIA treats regulatory authority as explicit data.

A regulatory relationship should be capable of answering:

1. **Who** is the regulator?
2. **Who** is the regulated Subject?
3. **What** authority does the regulator possess?
4. **Where** does that authority apply?
5. **When** is it valid?
6. **Why** does the authority exist?
7. **Which rules** does it administer?
8. **What powers** are granted?
9. **What limitations** constrain those powers?
10. **What mechanism** exists for review, appeal, or termination?

This transforms the concept of jurisdiction from an implicit background assumption into an explicit, inspectable relationship.

---

## Core Regulatory Axiom

> **A regulator is not the source of the Subject.**
>
> **A regulator is an authority-bearing Object whose relationship with the Subject has a defined source, scope, and duration.**

DIA therefore models regulation as:

```text
SUBJECT
   │
   │ regulated-by
   ▼
REGULATOR
   │
   ├── authority ← SOURCE
   ├── rules
   ├── scope
   ├── duration
   └── enforcement
```

The fundamental principle remains:

> **Subject first. Relationships second. Objects third. Authority must be explicit.**

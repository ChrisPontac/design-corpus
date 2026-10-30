---
slug: condition-inclusion-dsl
type: pattern
status: active
description: Declarative DSL for grouped condition/constraint inclusion rules, usable for both form validation and driving conditional editing behaviour.
tags: dsl, declarative-rules, forms, validation, permitting, environmental-regulation, conditional-editing
---

# Condition-inclusion DSL

A declarative language for expressing which elements of a form-plus-template system must, or must not,
be present together — used both to validate a completed form and, optionally, to drive the live editing
experience. First captured in the context of a permit-conditions template system (application answers
checked against template-supplied constraints); the mechanism itself is domain-agnostic.

## Terminology caveat

Worth distinguishing from, and not confusing with, a separate and simpler mechanism: automatic inclusion
("merge") rules, which decide which elements apply at all based on an orthogonal property of the subject
(e.g. "if the site is in Wales, include condition X, otherwise condition Y"). The DSL below is about
consistency constraints *among already-included* elements, not about which elements get included in the
first place. "Condition inclusion constraints" may be the more precise name for this DSL, specifically to
avoid being confused with the merge rules — not settled, worth revisiting if the ambiguity recurs.

## Model

**Elements** are either:
- a *constraint*, identified by a `cId`, belonging to the template side, or
- a *question/answer*, identified by an `aId`, belonging to the form side — an answer may carry a value.

**Groups** collect elements from exactly one source (an Answer group or a Constraint group — never mixed).
Membership is many-to-many: an element can belong to zero, one, or many groups.

```
"Group" <user-supplied name> "includes" <element id>
```

**Validity rules** come in two kinds:
- *Conditional rule* = name + condition clause + constraint clause ("if ... then it must be that ...")
- *Constraint rule* = name + constraint clause only (an unconditional "must be" requirement)

**Compound rules** combine validity rules with `AND`, `OR`, `XOR`, and parentheses.

**Action statements** separately declare which rules, beyond being checked at submit-time, should also
drive the editor's live behaviour (e.g. progressive disclosure). This separation matters: the same rule
grammar serves two distinct purposes — terminal validation, and (only when referenced by an action
statement) dynamic UI behaviour — and a rule is not assumed to do the second just because it does the first.

## Condition / constraint clause grammar

The same grammar is used for both the condition and the constraint half of a conditional rule (introduced
by "If" and "Then it must be that" respectively):

```
["!"] (
    "Every member of Group" <group name>
  | "Any member of Group" <group name>
  | "Exactly" <n> "members of Group" <group name>
  | "Element" <element id>
) ["exists" | <comparator> <literal>]   -- comparator+literal only valid for answer elements
```

Examples:
```
If Every member of Group Apple exists
If Any member of Group Kiwi exists
If !Every member of Group Banana exists
If Element cId.22 exists
If Element aId.66 has value "No"
If !Element cId.33 has value "Grebe"
```

## Representation is separable from the model

The grammar above is one rendering (regular-expression-like text). The underlying model — elements,
groups, validity rules, compound rules, action statements — is representation-independent; a graphical
rule builder over the same model is equally valid. Worth remembering when choosing how to expose this to
end users: the DSL/grammar is an implementation convenience, not the design itself.

## Why this generalises

Any system with (a) a set of answerable questions and (b) a set of template-supplied constraints that must
be jointly satisfied, where validity rules may also need to drive dynamic form behaviour rather than only
gate submission, fits this shape.

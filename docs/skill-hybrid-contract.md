# SKILL Hybrid Contract

## Purpose

This document defines the shared contract for hybrid skill packages that sit between pure instruction skills and runtime-assisted capability packages.

It is intended to be consumed by:

- upstream authoring tools that generate hybrid packages
- downstream package/runtime tools that validate, install, sync, and inspect them

This document extends, but does not replace, the base manifest-first `SKILL` contract.

---

## 1. What a Hybrid Package Is

A hybrid package is a `SKILL` package that combines:

- canonical manifest metadata in `SKILL.toml`
- human/agent instructions in `SKILL.md`
- runtime-facing workflow assets such as command docs, helper scripts, and templates

Its defining characteristic is that the package introduces a structured boundary between:

- AI interpretation and decision-making
- deterministic runtime helpers
- mutable external state

---

## 2. Hybrid Packages Are Additive, Not a Separate Base Format

The base package shape remains:

- `SKILL.toml`
- `SKILL.md`

Hybrid behavior is an extension layer on top of the base contract, not a replacement contract.

Existing skill-only packages remain valid packages.

---

## 3. Canonical Hybrid Metadata Lives in `SKILL.toml`

If hybrid behavior is declared, it must be declared in canonical manifest metadata rather than inferred from directory shape alone.

Directory presence may support validation, but it must not be the only source of truth.

---

## 4. The First-Stage Hybrid Contract Must Stay Minimal

The first shared hybrid contract should cover only the minimum needed for interoperability:

- whether the package is hybrid-aware
- what command namespace it owns
- which command verbs it declares
- where mutable runtime data belongs
- what knowledge/runtime-specific metadata is required for validation

Anything beyond that should remain out of contract until needed by more than one tool.

---

## 5. Hybrid Fields Must Describe Public Meaning, Not Generator Internals

Hybrid metadata must capture stable package semantics, not temporary implementation details of a scaffolder.

Examples of appropriate contract data:

- package kind
- command namespace
- allowed command verbs
- storage mode
- storage scope
- default entry template

Examples of inappropriate contract data unless standardized:

- transient generator bookkeeping
- internal prompt flow settings
- implementation-only convenience flags

---

## 6. Command Namespace Is a Contract Surface

The command namespace is not cosmetic.

It is the public naming boundary for hybrid workflow entrypoints and must be treated as a shared contract field.

The namespace must be:

- explicit
- collision-checkable
- stable enough for tooling to reason about

If a package declares namespace `research`, a downstream tool must be able to understand that its command family belongs under the `research-*` surface.

---

## 7. Command Verbs Are Declared, Not Implied

Hybrid packages must declare which verbs belong to the package command surface.

First-stage verbs may include:

- `save`
- `search`
- `load`

If other verbs are introduced later, they must be added intentionally through contract evolution, not inferred ad hoc.

---

## 8. Hybrid Kind Must Identify the Package Family

Hybrid packages should declare a stable `kind` so that downstream tools can distinguish different hybrid families without guessing from names or directories.

First-stage example:

- `knowledge`

Future kinds may exist, but should not be added until they require distinct package behavior across more than one tool.

---

## 9. Storage Mode Must Be Explicit

If a hybrid package requires mutable runtime state, its storage mode must be explicit in the contract.

The default shared contract should prefer:

- external data directory storage

Development-only or project-relative storage should be treated as explicit opt-in behavior, not the silent default.

---

## 10. Hybrid Contract Must Preserve the AI / Runtime Boundary

The contract should make it clear that:

- AI owns semantic interpretation, structuring, and judgment
- runtime helpers own deterministic file and command behavior

The contract must not allow scripts to become the hidden owner of semantic truth.

---

## 11. Knowledge Metadata Must Support Validation, Not Overfit the First Tool

Knowledge-oriented metadata should be chosen because it is meaningful to multiple tools.

Examples:

- supported entry types
- declared index files
- package-relative default entry template

These fields should support validation, inspection, and authoring consistency across tools.

---

## 12. Hybrid Packages Must Keep Runtime Assets Separate from Mutable Knowledge

Hybrid packages may ship the workflow assets needed to interact with runtime state.

They must not treat mutable knowledge data itself as part of the installable package payload.

The package may contain templates and command definitions.

It must not treat captured user knowledge as package content.

---

## 13. Backward Compatibility Must Be Additive

Hybrid support should be introduced without breaking skill-only packages.

Downstream tools that do not yet fully implement hybrid behavior should at minimum:

- parse safely when possible
- fail clearly when necessary
- avoid inventing their own unofficial semantics

---

## 14. Hybrid Contract Requires Ownership Clarity

The hybrid layer introduces shared questions that must not remain implicit, including:

- who defines command exposure rules
- who validates namespace collisions
- who installs runtime command helpers
- who diagnoses missing runtime assets
- who is installer of record

Hybrid support is incomplete until these ownership questions are explicitly documented.

---

## 15. Hybrid Contract Exists to Enable Parallel Tool Evolution

The purpose of the hybrid contract is to let multiple tools evolve in parallel without diverging on package meaning.

It should make it possible for:

- one tool to generate hybrid packages
- another tool to validate and install them
- both tools to agree on what those packages are

---

## Minimum Shared Surfaces

At minimum, a stable hybrid contract should cover:

1. hybrid package declaration
2. hybrid kind
3. command namespace
4. command verbs
5. storage mode and default scope
6. package-relative templates or knowledge metadata needed for validation

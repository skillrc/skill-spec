# SKILL Contract Index

## Purpose

This index is the entry point for the shared `SKILL` contract layer.

It is intended for:

- upstream authoring tools
- downstream package/runtime tools
- future shared schema or validation crates
- review sessions that need a single place to understand the current contract stack

---

## Contract Stack

### 1. Principles
- [`skill-contract-principles.md`](./skill-contract-principles.md)

Defines the constitutional rules for the shared contract layer:

- contract before implementation
- canonical manifest ownership
- package asset vs mutable state separation
- ownership clarity
- stability before convergence

---

### 2. Hybrid Contract
- [`skill-hybrid-contract.md`](./skill-hybrid-contract.md)

Defines the shared extension contract for hybrid packages:

- hybrid package meaning
- command namespace as contract surface
- command verbs
- storage mode and scope
- hybrid metadata discipline

---

### 3. Runtime Asset Boundary
- [`skill-runtime-asset-boundary.md`](./skill-runtime-asset-boundary.md)

Defines what belongs to the installable runtime package surface and what must remain mutable external state.

---

### 4. Ownership Matrix
- [`skill-ownership-matrix.md`](./skill-ownership-matrix.md)

Defines ownership boundaries across:

- schema
- generation
- validation
- installation
- command exposure
- diagnostics

---

## Reading Order

Recommended order for review:

1. `skill-contract-principles.md`
2. `skill-hybrid-contract.md`
3. `skill-runtime-asset-boundary.md`
4. `skill-ownership-matrix.md`

This order moves from constitutional rules to package meaning to deployment boundary to responsibility assignment.

---

## Current Status

These documents should be treated as the shared contract layer for ongoing upstream/downstream coordination.

Suggested status model for future updates:

- `proposed`
- `confirmed`
- `deferred`
- `rejected`

Until a dedicated status header is added to each document, treat this contract layer as:

> **active working contract, under consolidation**

---

## How Other Projects Should Use This Layer

### Upstream authoring tools
Should use this layer to decide:

- what package fields to emit
- which hybrid fields are canonical
- which files belong to runtime assets
- what is fallback vs authoritative behavior

### Downstream package/runtime tools
Should use this layer to decide:

- what package fields to parse and validate
- what runtime assets may be installed or synced
- how mutable state must be separated
- which responsibilities belong to runtime tooling

---

## Scope Boundary

This index covers the shared contract layer only.

It does **not** replace:

- project-specific implementation plans
- generator UX decisions
- package-manager internals
- migration plans inside individual repositories

Those may evolve independently as long as they remain aligned with this contract layer.

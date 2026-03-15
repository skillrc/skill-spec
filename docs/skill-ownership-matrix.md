# SKILL Ownership Matrix

## Purpose

This document defines ownership boundaries for the shared `SKILL` contract and its surrounding package lifecycle.

It is intended to reduce ambiguity between:

- upstream authoring tools
- downstream package/runtime tools
- future shared spec or schema layers

Ownership here means: who is authoritative for defining, generating, validating, installing, or diagnosing a given surface.

---

## Ownership Model

Three ownership layers are assumed:

1. **Spec layer** — defines the contract itself
2. **Authoring layer** — generates package content that conforms to the contract
3. **Runtime/management layer** — consumes package content and performs package operations

If a responsibility belongs to more than one layer, the type of ownership must be distinguished clearly.

---

## 1. Contract / Schema Ownership

### Canonical owner
- shared spec layer

### Meaning
- defines field names
- defines field meaning
- defines required vs optional structure
- defines versioned compatibility expectations

### Non-owners
- authoring tools may not privately redefine schema semantics
- runtime tools may not privately reinterpret schema semantics

---

## 2. Manifest Generation Ownership

### Canonical owner
- upstream authoring tools

### Meaning
- generate `SKILL.toml`
- generate default values and scaffolding
- ensure emitted package shape conforms to spec

### Constraint
- generation ownership does not imply schema ownership

---

## 3. Manifest Validation Ownership

### Canonical owner
- shared spec layer defines validation rules
- both upstream and downstream tools may implement validation against that spec

### Meaning
- authoring tools validate before package finalization
- runtime/package tools validate before install, sync, or diagnostics

### Constraint
- validation logic may exist in multiple tools
- validation meaning must come from one contract source

---

## 4. Base Package Asset Ownership

### Canonical owner
- upstream authoring tools generate package assets
- downstream package tools install and sync package assets

### Meaning
- authoring side owns creation
- management side owns deployment operations

### Shared rule
- both sides must agree on which assets belong to the runtime package surface

---

## 5. Mutable Runtime State Ownership

### Canonical owner
- end user / runtime environment

### Meaning
- mutable knowledge entries
- indexes
- user-generated runtime data

### Constraint
- neither authoring tools nor package managers should redefine mutable user state as immutable package content

---

## 6. External Data Directory Contract Ownership

### Canonical owner
- shared spec layer

### Runtime enforcement owner
- downstream package/runtime tools

### Authoring support owner
- upstream authoring tools

### Meaning
- spec defines what the contract means
- authoring tools emit packages that follow it
- runtime tools validate and enforce it where applicable

---

## 7. Command Namespace Ownership

### Canonical owner
- shared spec layer

### Generation owner
- upstream authoring tools

### Collision / exposure validation owner
- downstream package/runtime tools once they manage command exposure

### Meaning
- namespace syntax and semantics come from the contract
- scaffolding emits a namespace
- runtime tooling may later validate collisions and exposure rules

---

## 8. Command Exposure Ownership

### Current recommendation
- transitional / evolving ownership

### Near-term interpretation
- generated helper scripts or install conveniences may expose commands for development use

### Long-term target
- downstream package/runtime tools should become installer of record for runtime command exposure if that behavior is standardized

### Constraint
- convenience exposure must be marked non-authoritative until ownership is formally transferred

---

## 9. Installer of Record Ownership

### Current state
- must be documented explicitly per package/runtime phase

### Long-term recommendation
- downstream package/runtime tools should own installation semantics once hybrid/runtime packaging is stable

### Constraint
- upstream-generated install scripts should be treated as fallback or development convenience unless explicitly declared authoritative

---

## 10. Diagnostics Ownership

### Canonical owner for diagnostic meaning
- shared spec layer defines what “healthy” means at the contract level

### Operational owner
- downstream package/runtime tools

### Meaning
- package managers and doctor-style tools should check asset presence, state placement, compatibility, and runtime health

### Constraint
- authoring tools may provide authoring-time validation but should not become runtime health authorities by default

---

## 11. Compatibility Version Ownership

### Canonical owner
- shared spec layer

### Generation owner
- upstream authoring tools emit compatibility metadata

### Enforcement owner
- downstream runtime/package tools interpret and enforce compatibility where needed

---

## 12. Fallback and Migration Ownership

### Canonical owner
- shared spec layer defines whether a mechanism is authoritative, fallback, transitional, or deprecated

### Meaning
- migration behavior must be named clearly
- fallback installers must be labeled as fallback
- compatibility metadata must not silently become canonical

---

## 13. Ownership Ambiguity Is a Blocker, Not a Detail

If a surface does not have a clear owner, that is not a minor documentation gap.

It is a contract blocker.

Examples:

- no one owns command collision checks
- no one owns external data directory enforcement
- no one owns runtime asset classification

These must be resolved before convergence work accelerates.

---

## Summary Matrix

| Surface | Spec Layer | Authoring Tool | Runtime / Management Tool |
|---------|------------|----------------|---------------------------|
| Manifest schema | Owns meaning | Consumes | Consumes |
| Manifest generation | Defines rules | Owns generation | Reads |
| Manifest validation | Defines rules | Validates before emit | Validates before install/use |
| Runtime asset classification | Defines rules | Emits accordingly | Installs/syncs accordingly |
| Mutable runtime state | Defines separation | Must not package as immutable payload | Must preserve and not overwrite incorrectly |
| Command namespace semantics | Defines rules | Emits namespace | Validates/exposes when standardized |
| Command exposure | Defines authority rules | Temporary fallback only if declared | Long-term installer-of-record target |
| Diagnostics meaning | Defines what health means | Optional authoring checks | Owns operational diagnostics |

# SKILL Contract Principles

## Purpose

This document defines the governing principles for the shared `SKILL` contract used by:

- upstream authoring tools such as `opencode-skill-create`
- downstream package/runtime tools such as `opencode-skill-management`

It is a principles document, not an implementation guide.

---

## 1. Contract Before Implementation

The `SKILL` contract must be defined before upstream and downstream tools extend behavior around it.

No tool may treat its local implementation as the source of truth for package semantics.

---

## 2. One Contract, Multiple Tools

Authoring tools and package-management tools may have different responsibilities, but they must share the same package contract.

- authoring tools produce packages
- management tools consume, install, sync, and validate packages

They share a contract, not a product boundary.

---

## 3. `SKILL.toml` Is Canonical

Machine-readable package truth lives in `SKILL.toml`.

`SKILL.md` is the human/agent instruction surface.

Compatibility metadata may exist in derived forms, but must not replace the canonical manifest.

---

## 4. Narrow Entrance, Explicit Meaning

Every field admitted into the contract must be:

- structurally valid
- semantically defined
- unambiguous in ownership
- stable enough to be consumed by more than one tool

If a field cannot be explained clearly, it is not ready for the shared contract.

---

## 5. Upstream Generates, Downstream Interprets

The upstream tool may generate package files and defaults.

The downstream tool may parse, validate, install, sync, and diagnose those packages.

Neither side may silently redefine the contract in a way the other side cannot see.

---

## 6. Package Assets and Mutable Runtime State Must Be Separate

The contract must distinguish between:

- installable package assets
- mutable runtime data

Installable assets are versioned, movable, cacheable, and replaceable.

Mutable runtime state is user or project data and must not be treated as part of the immutable package payload.

---

## 7. External Data Directories Are the Default for Mutable State

If a package introduces mutable knowledge or runtime-managed user content, that data must default to an external data directory contract.

Installed package directories should be treated as read-mostly deployment artifacts, not as mutable data stores.

---

## 8. Ownership Must Be Explicit

For every meaningful contract surface, ownership must be named.

This includes at least:

- schema ownership
- generation ownership
- validation ownership
- install ownership
- command exposure ownership
- runtime diagnostics ownership

Unowned behavior is unstable behavior.

---

## 9. Convenience Is Not Authority

Generated helper scripts, fallback installers, migration compatibility layers, and development conveniences may exist, but they must be marked as non-authoritative when they are not the long-term source of truth.

Convenience must not silently become contract.

---

## 10. Naming Must Reveal Role

Contract names must reveal what they govern.

Good names reduce architectural confusion:

- `command_namespace`
- `data_dir_contract`
- `runtime_assets`
- `mutable_state`
- `compat`

Vague names increase drift between tools.

---

## 11. Change Must Be Reviewable

Every contract change should answer:

1. What changed?
2. Why did it change?
3. Which tools are affected?
4. Is it backward compatible?
5. What is now authoritative?

The contract must evolve through reviewable changes, not through folklore.

---

## 12. Stability Before Convergence

Before repositories, binaries, or CLIs converge, the contract layer must stabilize first.

Contract instability is a reason to delay product or codebase convergence, not a reason to merge faster.

---

## 13. Composition Over Centralization

The `SKILL` contract should enable multiple tools to interoperate through a clear shared shape.

It should not force all responsibilities into one tool, one binary, or one boundary prematurely.

Shared contract first. Shared implementation later, if justified.

---

## 14. Discussion and Decision Must Be Distinct

The contract layer must distinguish among:

- proposed rules
- confirmed rules
- deferred questions
- rejected alternatives

Discussion notes are not contract truth.

---

## 15. Contract Exists to Keep Evolution Clear

The purpose of the `SKILL` contract is not paperwork.

Its purpose is to ensure that:

- upstream tools do not generate drifting package semantics
- downstream tools do not reinterpret packages arbitrarily
- multiple projects can move in parallel without losing coherence
- future shared crates or workspaces have a stable base to stand on

---

## Short Form

1. Contract before implementation.
2. One contract, multiple tools.
3. `SKILL.toml` is canonical.
4. Package assets are not mutable runtime state.
5. External data dirs own mutable state by default.
6. Ownership must be explicit.
7. Convenience is not authority.
8. Naming must reveal role.
9. Changes must be reviewable.
10. Stability before convergence.

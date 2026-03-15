# SKILL Runtime Asset Boundary

## Purpose

This document defines the boundary between:

- installable runtime package assets
- mutable runtime or knowledge state

Its purpose is to prevent upstream authoring tools and downstream package/runtime tools from treating deployable artifacts and mutable user data as the same thing.

---

## 1. Runtime Assets Are Deployable Package Content

Runtime assets are files that belong to the package as a deployable artifact.

They are expected to be:

- versioned
- installable
- cacheable
- replaceable
- safe to copy or sync across runtime targets

---

## 2. Mutable State Is Not Package Content

Mutable state is user or project data created or updated during package use.

It is expected to change over time and must not be treated as part of the immutable package payload.

---

## 3. First-Class Runtime Assets

Unless a more specific contract overrides them, the following should be treated as first-class runtime assets when present:

- `SKILL.toml`
- `SKILL.md`
- `commands/`
- `scripts/`
- `templates/`
- other explicitly declared support files needed by runtime workflows

These belong to the package delivery surface.

---

## 4. Mutable Knowledge and Runtime Data Must Live Outside the Package Tree

Captured knowledge entries, indexes, user-created notes, and other mutable state must not default to living inside the installed package tree.

Installed package directories are deployment boundaries, not persistent mutable data stores.

---

## 5. External Data Directory Is the Default Mutable Boundary

When mutable state is required, the default contract should place it in an external data directory resolved through explicit rules.

This keeps:

- package installation deterministic
- sync behavior predictable
- cache/store semantics clean
- user data independent from reinstall/redeployment

---

## 6. Development Layout and Installed Layout Must Not Be Confused

A source project may contain:

- authoring docs
- tests
- local development fixtures
- generated examples
- project-local mutable data for development

That does not make all such files runtime assets.

The runtime asset boundary must be defined by contract, not by mere repository co-location.

---

## 7. Installation Must Preserve the Asset Boundary

Installers, sync tools, and package managers must preserve the distinction between:

- files that should be installed or synced
- files that should never be treated as package payload

If a tool installs mutable user data as if it were part of the package, it has crossed the boundary incorrectly.

---

## 8. Runtime Helpers May Read Package Assets but Must Not Assume Package Mutability

Scripts and command helpers may read:

- templates
- command docs
- package-relative support files

They must not rely on the installed package directory as their default write target for mutable runtime data.

---

## 9. Templates Are Assets, Not State

Templates belong to the package because they define reusable starting structures.

Instantiated files created from those templates belong to mutable runtime state, not to the package artifact itself.

This distinction must remain explicit.

---

## 10. Validation Should Check Asset Presence and State Separation Separately

Downstream validation and diagnostics should answer two different questions:

1. Are the required runtime assets present?
2. Is mutable state stored in the correct external location?

Conflating these two checks causes architectural confusion.

---

## 11. Package Managers Should Treat Runtime Assets as Replaceable

Because runtime assets are package content, package managers should be free to:

- reinstall them
- replace them
- re-sync them
- rebuild their deployed view from cache or source

This is only safe if mutable state is outside that boundary.

---

## 12. Source Repositories May Be Richer Than Installed Packages

A source repository can contain more material than what is installed.

That is acceptable and expected.

The contract must define the runtime asset surface clearly enough that tooling can know which subset is authoritative for runtime installation.

---

## 13. Boundary Violations Must Be Called Out Explicitly

The following are boundary violations:

- writing user knowledge into the installed package tree by default
- treating generated indexes as immutable package content
- assuming repository-local docs are runtime package assets without declaration
- allowing package managers to overwrite mutable user state during reinstall

---

## 14. This Boundary Exists to Keep Package Operations Safe

The runtime asset boundary is what allows package operations to remain safe and deterministic.

Without it, install/sync/update behavior becomes dangerous because tools cannot tell the difference between:

- what may be replaced
- what must be preserved

---

## Short Form

1. Runtime assets are deployable package content.
2. Mutable state is not package content.
3. Installed package trees are not default write targets.
4. External data directories own mutable state by default.
5. Templates are assets; instantiated knowledge is state.
6. Validation must check asset presence and state placement separately.

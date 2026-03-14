# SKILL Package Specification

Version: `1.0.0`

## 1. Purpose

This specification defines a manifest-first package format for AI skills. The goal is to make skill metadata easy for tooling to parse, validate, lock, cache, and display while keeping the actual instruction content readable in Markdown.

## 2. Design Principles

1. **Single source of truth** — canonical metadata lives in `SKILL.toml`, not in Markdown frontmatter.
2. **Human content stays human** — `SKILL.md` contains the skill instructions for agents and authors.
3. **Backward-compatible rollout** — tooling may temporarily read legacy YAML frontmatter, but that format is compatibility-only.
4. **Executable standards** — schemas and validators should be able to check packages automatically.
5. **Version-aware distribution** — semantic versions express release intent, while package managers may lock exact commits and content hashes.

## 3. Package Layout

A compliant skill package contains:

```text
skill-package/
├── SKILL.toml
├── SKILL.md
└── supporting/      # optional
```

### Canonical files

- `SKILL.toml` — machine-readable manifest
- `SKILL.md` — human/agent-readable instructions

### Compatibility notes

- During migration, `SKILL.md` MAY include a reduced YAML frontmatter block.
- If both exist, `SKILL.toml` MUST take precedence.
- Tools SHOULD warn when they must fall back to legacy frontmatter.

## 4. Manifest Format

`SKILL.toml` is the canonical metadata document.

### Example

```toml
manifest_version = "1.0"

[skill]
name = "git-workflow"
version = "1.2.0"
description = "Use when planning safe git commits, history cleanup, or atomic change grouping"
type = "technique"
category = "engineering"
boundary = "Focused on local git hygiene, commit planning, and safe history editing"
maturity = "beta"
last_verified = "2026-03-14"
tags = ["git", "workflow", "commits"]
topics = ["version-control", "code-hygiene"]
non_goals = ["Teaching Git fundamentals"]

[source]
repository = "https://github.com/example/opencode-skill-git-workflow"
license = "MIT"

[compat]
min_opencode_version = "0.1.0"
```

## 5. Required Fields

### Top-level

#### `manifest_version`
Manifest schema version.

- Type: string
- Example: `"1.0"`
- Purpose: allows tooling to negotiate compatibility and schema evolution.

### `[skill]`

#### `name`
Machine-friendly skill identifier.

- Type: string
- Format: lowercase kebab-case
- Example: `git-workflow`

#### `version`
Semantic version for the skill package.

- Type: string
- Format: semantic version
- Example: `1.2.0`

#### `description`
A short trigger-oriented description.

- Type: string
- Recommended style: begin with `Use when ...`

#### `type`
Defines the instructional mode of the skill.

Allowed values:
- `technique`
- `pattern`
- `reference`
- `discipline`

#### `category`
Primary management category for the skill.

Recommended values:
- `engineering`
- `frontend`
- `backend`
- `devops`
- `testing`
- `security`
- `research`
- `writing`
- `workflow`
- `product`
- `general`

#### `boundary`
One sentence describing the intended coverage of the skill.

#### `maturity`
Lightweight lifecycle and trust signal.

Allowed values:
- `draft`
- `alpha`
- `beta`
- `stable`
- `deprecated`

#### `last_verified`
Date the skill was last verified against the expected workflow.

- Type: string
- Format: `YYYY-MM-DD`
- Example: `2026-03-14`

#### `tags`
Short keywords for search and clustering.

- Type: array of strings
- Recommended format: lowercase kebab-case
- Example: `tags = ["git", "workflow"]`

#### `topics`
Higher-level themes used for grouping related skills.

- Type: array of strings
- Recommended format: lowercase kebab-case
- Example: `topics = ["version-control"]`

## 6. Optional Fields

### `[skill]`

#### `non_goals`
Problems the skill explicitly does not try to solve.

- Type: array of strings

### `[source]`

Optional source metadata describing the repository, homepage, or license.

Recommended keys:
- `repository`
- `license`
- `homepage`

### `[compat]`

Optional compatibility information for runtimes and package managers.

Recommended keys:
- `min_opencode_version`
- `min_skillmine_version`

## 7. `SKILL.md` Content Document

`SKILL.md` contains the actual instructional content.

Recommended structure:

```markdown
# Skill Title

## Overview

## When to Use

## Core Technique / Pattern / Reference / Rule

## Common Mistakes

## See Also
```

`SKILL.md` SHOULD remain content-first. Tooling MAY derive a minimal compatibility frontmatter block if required by older runtimes, but full metadata SHOULD NOT be duplicated manually.

## 8. Validation Rules

Tools SHOULD validate at least:

1. `manifest_version` is supported.
2. `skill.name` matches `^[a-z0-9]+(-[a-z0-9]+)*$`.
3. `skill.version` is valid semver.
4. `skill.description` is not empty and preferably starts with `Use when`.
5. `skill.type` is one of the allowed values.
6. `skill.category` is one of the recommended values or a recognized extension.
7. `skill.maturity` is one of the allowed values.
8. `skill.last_verified` matches `YYYY-MM-DD`.
9. `skill.tags` and `skill.topics` are non-empty arrays.
10. `SKILL.md` exists beside `SKILL.toml`.

## 9. Versioning and Distribution

- Authors SHOULD manage `skill.version` manually using semantic versioning.
- Releases SHOULD be tagged in git using a corresponding version tag such as `v1.2.0`.
- Package managers MAY resolve version constraints to exact commits and record those in lockfiles.
- Content-addressable caches MAY use tree hashes independent of release tags.

## 10. Migration from Legacy Frontmatter

If you have a legacy skill with only `SKILL.md` frontmatter:

1. Create `SKILL.toml` using the existing metadata.
2. Add `version` and `last_verified`.
3. Keep `SKILL.md` focused on content.
4. Allow package managers to warn when operating in legacy fallback mode.

## 11. Evolution Path

1. Start with manifest-aware skill creation.
2. Add schema validation in authoring and management tools.
3. Add indexing and browsing tools.
4. Add richer registry features only after package contracts stabilize.

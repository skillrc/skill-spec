# SKILL Specification Draft

Version: `0.1-draft`

## 1. Purpose

This specification defines a lightweight metadata format for SKILL files. The goal is to improve discoverability, boundary clarity, and maintainability without overloading the skill authoring experience.

## 2. Design Principles

1. **Management before scoring** — metadata should help organize and govern skills before trying to rank them.
2. **Boundary clarity** — every skill should clearly state what it covers.
3. **Lightweight authorship** — the schema must remain practical for interactive generators.
4. **Forward compatibility** — future validation and quality tooling should be possible without breaking the format.

## 3. File Format

A SKILL file begins with YAML frontmatter followed by Markdown content.

```yaml
---
name: git-workflow
description: Use when planning safe git commits, history cleanup, or atomic change grouping
type: technique
category: engineering
tags:
  - git
  - workflow
topics:
  - version-control
boundary: "Focused on local git hygiene, commit planning, and safe history editing"
maturity: beta
non_goals:
  - Teaching Git fundamentals
---
```

## 4. Required Fields

### `name`
Machine-friendly skill identifier.

### `description`
A short trigger-oriented description. Recommended style: begin with `Use when ...`.

### `type`
Defines the instructional mode of the skill.

Allowed values:
- `technique`
- `pattern`
- `reference`
- `discipline`

### `category`
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

### `boundary`
One sentence describing the intended coverage of the skill.

### `tags`
Short keywords for search and clustering. Recommended format: lowercase kebab-case.

These are required output fields. Generators should suggest them automatically from the skill inputs and let authors confirm or edit them.

### `topics`
Higher-level themes used for grouping related skills.

These are required output fields. Generators should suggest them automatically from the skill inputs and let authors confirm or edit them.

### `maturity`
Lightweight lifecycle and trust signal.

Allowed values:
- `draft`
- `alpha`
- `beta`
- `stable`
- `deprecated`

## 5. Optional Fields

### `non_goals`
Problems the skill explicitly does not try to solve. This field narrows interpretation and reduces misuse.

## 6. Recommended Authoring Guidance

- Keep the `boundary` concrete rather than aspirational.
- Use `non_goals` to prevent vague or over-broad skills.
- Prefer one primary `category`, several narrow `tags`, and at least one meaningful `topic`.
- Let tooling draft `tags` and `topics`, then keep the author in the review loop.
- Use `maturity` instead of a numeric score in early-stage ecosystems.

## 7. Deferred Ideas

The following ideas are intentionally left out of v0.1:

- numeric quality scores
- automatic ranking formulas
- mandatory authorship metadata
- dependency graphs between skills

These can be introduced later if the ecosystem proves they are needed.

## 8. Example Evolution Path

1. Start with metadata-aware skill creation.
2. Add linting for missing or weak metadata.
3. Add indexing and browsing tools.
4. Consider derived quality signals only after governance patterns stabilize.

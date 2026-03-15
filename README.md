OPENCODE-SKILL-SPEC(5)

```text
NAME
       opencode-skill-spec - manifest-first package standard for
       AI skill libraries

SYNOPSIS
       A skill package contains:

              SKILL.toml
              SKILL.md
```

```text
DESCRIPTION
       Most skill systems start as loose Markdown files with a name,
       description, and instructions. That works for small libraries, but
       breaks down once discoverability, maintenance, compatibility, and
       tooling matter.

       Problem pattern:

       +------------------+-------------------------------------------+
       | Problem          | Symptom                                   |
       +------------------+-------------------------------------------+
       | Can't find       | Search misses relevant skills             |
       | Can't assess     | No lifecycle or trust signal              |
       | Can't group      | Similar capabilities scatter              |
       | Can't prevent    | Out-of-scope use is common                |
       | Can't manage     | Tooling must parse Markdown heuristically |
       +------------------+-------------------------------------------+

       This repository defines a manifest-first contract so skill packages
       are machine-readable, governable, and interoperable across generators,
       validators, installers, and package-management tooling.
```

```text
FILE FORMAT
       Canonical package layout:

       +------------------+-------------------------------------------+
       | File             | Role                                      |
       +------------------+-------------------------------------------+
       | SKILL.toml       | Canonical machine-readable manifest       |
       | SKILL.md         | Human/agent instruction content           |
       +------------------+-------------------------------------------+

       Compatibility metadata MAY still appear in SKILL.md during migration,
       but SKILL.toml is authoritative.
```

```text
SCHEMA SURFACE
       Required core fields:

       +----------------------+----------------------------------------+
       | Field                | Meaning                                |
       +----------------------+----------------------------------------+
       | manifest_version     | Manifest schema version                |
       | skill.name           | Stable machine identifier              |
       | skill.version        | Published package version              |
       | skill.description    | Trigger phrase                         |
       | skill.type           | Instruction mode                       |
       | skill.category       | Primary management bucket              |
       | skill.tags           | Search keywords                        |
       | skill.topics         | Thematic grouping                      |
       | skill.boundary       | One-sentence scope contract            |
       | skill.maturity       | Lifecycle stage                        |
       | skill.last_verified  | Verification date                      |
       +----------------------+----------------------------------------+

       Optional first-stage surfaces:

       +------------------+-------------------------------------------+
       | Field            | Meaning                                   |
       +------------------+-------------------------------------------+
       | skill.non_goals  | Explicit exclusions                       |
       | [source]         | Repository / license metadata             |
       | [compat]         | Runtime compatibility metadata            |
       +------------------+-------------------------------------------+
```

```toml
EXAMPLE

manifest_version = "1.0"

[skill]
name = "git-workflow"
version = "1.2.0"
description = "Use when planning safe git commits"
type = "technique"
category = "engineering"
boundary = "Focused on local git hygiene"
maturity = "beta"
last_verified = "2026-03-14"
tags = ["git", "commits", "repository"]
topics = ["version-control", "code-hygiene"]
non_goals = ["Teaching git basics"]
```

```text
DESIGN PRINCIPLES
       1. Management before scoring
              Metadata exists to organize and govern before ranking.

       2. Manifest-first truth
              Tooling should not need Markdown parsing heuristics to manage
              packages.

       3. Boundary clarity
              Every skill should say what it covers and what it does not.

       4. Progressive maturity
              Use lifecycle stages instead of arbitrary quality scores.

       5. Authoring vs runtime separation
              A schema that is easy to govern is not automatically easy to
              author; tooling may generate defaults while preserving schema
              requirements.
```

```text
CONTRACT LAYER
       Shared contract documents live under docs/ and currently include:

       +--------------------------------------+---------------------------+
       | Document                             | Purpose                   |
       +--------------------------------------+---------------------------+
       | docs/contract-index.md               | Entry point               |
       | docs/skill-contract-principles.md    | Constitutional rules      |
       | docs/skill-hybrid-contract.md        | Hybrid package contract   |
       | docs/skill-runtime-asset-boundary.md | Asset vs state boundary   |
       | docs/skill-ownership-matrix.md       | Ownership responsibilities|
       +--------------------------------------+---------------------------+
```

```text
REPOSITORY LAYOUT
       opencode-skill-spec/
       +-- README.md
       +-- schema/
       |   +-- skill-manifest-v1.schema.json
       +-- docs/
       |   +-- quick-start.md
       |   +-- schema-reference.md
       |   +-- authoring-rationale.md
       |   +-- contract-index.md
       |   +-- skill-contract-principles.md
       |   +-- skill-hybrid-contract.md
       |   +-- skill-runtime-asset-boundary.md
       |   '-- skill-ownership-matrix.md
       '-- examples/
           '-- example-skill/
```

```text
QUICK START
       1. Read docs/schema-reference.md for field semantics.
       2. Read docs/authoring-rationale.md for generation policy.
       3. Validate generated packages against schema/skill-manifest-v1.schema.json.
       4. Use docs/contract-index.md when coordinating upstream/downstream tools.
```

```text
FILES
       schema/skill-manifest-v1.schema.json
              JSON Schema for manifest validation.

       docs/schema-reference.md
              Field-by-field contract explanation.

       docs/authoring-rationale.md
              Why required schema does not imply manual author entry.

       docs/contract-index.md
              Entry point for the shared contract stack.
```

```text
STATUS
       Draft v0.1.

       The base schema is stable enough for tooling experiments. The shared
       hybrid/runtime contract layer is under active consolidation.
```

```text
SEE ALSO
       docs/schema-reference.md
       docs/authoring-rationale.md
       docs/contract-index.md
       opencode-skill-create(1)
       skillmine(1)
```

```text
AUTHORS
       OpenCode contributors

LICENSE
       MIT
```

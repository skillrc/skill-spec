OPENCODE-SKILL-SPEC(5)

NAME
       opencode-skill-spec - Manifest-first package standard for
       AI skill libraries

SYNOPSIS
       A skill package contains a machine-readable manifest plus a human-
       readable instruction document:

              SKILL.toml
              SKILL.md

DESCRIPTION
       Most AI skill systems start simple:

              name + description + content

       But at scale (>50 skills), chaos emerges:

       +------------------+-------------------------------------------+
       | Problem          | Symptom                                   |
       +------------------+-------------------------------------------+
       | Can't find       | Searching "commits" misses git skills     |
       | Can't assess     | Is this experimental or production?       |
       | Can't group      | Similar capabilities scattered            |
       | Can't prevent    | Skills used for wrong problems            |
       | Can't maintain   | Abandoned skills accumulate               |
       +------------------+-------------------------------------------+

       This specification defines a manifest-first package contract that makes
       skills discoverable, manageable, and trustworthy without forcing tools
       to parse Markdown as their source of truth.

FILE FORMAT
       Skill packages use a manifest-first layout:

       +------------------+-------------------------------------------+
       | Section          | Format                                    |
       +------------------+-------------------------------------------+
       | Manifest         | TOML in SKILL.toml                        |
       | Content          | Markdown in SKILL.md                      |
       | Encoding         | UTF-8, LF line endings                    |
       | Compatibility    | Legacy YAML frontmatter MAY be supported  |
       +------------------+-------------------------------------------+

       Complete example:

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

              # Git Workflow

              ## Overview
              Brief description...

REQUIRED FIELDS

       +------------------+----------+-----------------------------------+
       | Field            | Type     | Purpose                           |
       +------------------+----------+-----------------------------------+
       | manifest_version | string   | Manifest schema compatibility     |
       | skill.name       | string   | Machine-friendly identifier       |
       | skill.version    | string   | Skill release version (semver)    |
       | skill.description| string   | "Use when..." trigger phrase      |
       | skill.type       | enum     | technique/pattern/reference/etc   |
       | skill.category   | enum     | Primary management bucket         |
       | skill.tags       | array    | Search keywords                   |
       | skill.topics     | array    | Thematic groups                   |
       | skill.boundary   | string   | One-sentence scope definition     |
       | skill.maturity   | enum     | draft/alpha/beta/stable/etc       |
       | skill.last_verified | date  | Last verification date            |
       +------------------+----------+-----------------------------------+

OPTIONAL FIELDS

       +------------------+----------+-----------------------------------+
       | Field            | Type     | Purpose                           |
       +------------------+----------+-----------------------------------+
       | skill.non_goals  | array    | Explicit exclusions               |
       | [source]         | table    | Repository/license metadata       |
       | [compat]         | table    | Runtime compatibility metadata    |
       +------------------+----------+-----------------------------------+

CANONICAL VS COMPATIBILITY FORMAT

       Canonical metadata lives in SKILL.toml.

       SKILL.md contains the actual skill instructions for humans and agents.

       Legacy YAML frontmatter in SKILL.md MAY still be emitted or consumed
       during migration, but it is considered derived compatibility metadata,
       not the authoritative source of truth.

FIELD DETAILS

       manifest_version
              Manifest schema version understood by tooling.
              Example: 1.0

       skill.name
               Machine-friendly identifier.
               Format: lowercase letters, numbers, hyphens.
               Max 50 characters.
               Example: git-workflow, code-review

       skill.version
              Semantic version for the published skill package.
              Example: 1.2.0

       skill.description
               Human-readable trigger phrase.
               Recommended: Start with "Use when..."
               Length: 10-30 words.
               Example: "Use when planning safe git commits"

       skill.type
               Defines instructional mode.

              +---------------+-------------------------------------------+
              | Value         | Use For                                   |
              +---------------+-------------------------------------------+
              | technique     | Step-by-step procedures                   |
              | pattern       | Mental models, approaches                 |
              | reference     | Documentation, syntax guides              |
              | discipline    | Rules, requirements, checklists           |
              +---------------+-------------------------------------------+

       skill.category
               Primary management bucket.

              +---------------+-------------------------------------------+
              | Value         | Domain                                    |
              +---------------+-------------------------------------------+
              | engineering   | Software engineering workflows            |
              | frontend      | UI, UX, browser work                      |
              | backend       | APIs, services, server logic              |
              | devops        | Infrastructure, deployment                |
              | testing       | Unit, integration, E2E testing            |
              | security      | Security reviews, hardening               |
              | research      | Investigation, discovery                  |
              | writing       | Documentation, communication              |
              | workflow      | Process, collaboration                    |
              | product       | Product thinking, strategy                |
              | general       | Cross-functional                          |
              +---------------+-------------------------------------------+

       skill.tags
               Searchable keywords for discovery.
               Format: TOML array of lowercase, kebab-case strings.
               Count: 3-5 recommended.
               AI-generated by default, user-confirmed.

               Example:
                     tags = ["git", "commits", "repository"]

       skill.topics
               Thematic groups for browsing.
               Format: TOML array of kebab-case strings.
               Count: 1-3 recommended.
               AI-generated by default, user-confirmed.

               Example:
                     topics = ["version-control", "code-hygiene"]

       skill.boundary
               One-sentence scope definition.
              Format: "Focused on [concrete scope]"
              Must be specific enough to judge fit.

              Good: "Focused on local git hygiene and history cleanup"
              Bad:  "Helps with coding tasks"

       skill.maturity
               Lifecycle stage.

              +-----------+-----------------------------------------------+
              | Stage     | Meaning                                       |
              +-----------+-----------------------------------------------+
              | draft     | Early idea, needs shaping                     |
              | alpha     | Experimental but usable                       |
              | beta      | Tested, mostly stable                         |
              | stable    | Recommended for production                    |
              | deprecated| Kept for reference only                       |
              +-----------+-----------------------------------------------+

       skill.last_verified
              Date the skill was last verified against the expected workflow.
              Example: 2026-03-14

       skill.non_goals
               Explicit exclusions.
               Prevents misuse by stating what skill does NOT do.

               Example:
                     non_goals = ["Teaching git fundamentals", "Remote branch policies"]

DESIGN PRINCIPLES

       1. Management before scoring
          Metadata organizes before ranking.

       2. AI-assisted authoring
          Required fields generated by tooling,
          confirmed by humans.

       3. Boundary clarity
          Every skill states what it covers
          AND what it does not.

       4. Progressive maturity
          Lifecycle stages (draft->stable)
          instead of arbitrary scores.

EXAMPLES

       Example 1: Git Workflow (Technique)

              ---
              name: git-workflow
              description: Use when planning safe git commits
              type: technique
              category: engineering
              tags:
                - git
                - commits
                - repository
              topics:
                - version-control
                - code-hygiene
              boundary: "Focused on local git hygiene"
              maturity: beta
              ---

       Example 2: Code Review (Pattern)

              ---
              name: code-review
              description: Use when reviewing code quality
              type: pattern
              category: engineering
              tags:
                - code-review
                - quality
              topics:
                - quality-engineering
              boundary: "Covers structured code review"
              maturity: beta
              ---

REPOSITORY STRUCTURE

       opencode-skill-spec/
       ├── README.md              This file
       ├── SPEC.md                Core specification
       ├── LICENSE                MIT License
       ├── CHANGELOG.md           Version history
       ├── CONTRIBUTING.md        Contribution guidelines
       ├── docs/
       │   ├── quick-start.md     5-minute tutorial
       │   ├── schema-reference   Field documentation
       │   ├── comparison.md      Alternative approaches
       │   ├── authoring-rationale.md  Design philosophy
       │   └── tags-topics.md     Tags vs topics guide
       └── examples/
           └── example-skill.md   Realistic example

STATUS

       Draft v0.1

       Core schema is stable.
       Seeking community feedback on tooling recommendations,
       migration guides, and best practices by skill type.

SEE ALSO

       SPEC.md(5)            Complete technical specification
       docs/quick-start.md(5) 5-minute tutorial
       docs/comparison.md(5)  Alternative approaches
       docs/schema-reference.md(5) Field-by-field reference

AUTHORS

       OpenCode Contributors

LICENSE

       MIT

BUGS

       Report issues at:
       https://github.com/yourusername/opencode-skill-spec/issues

NOTES

       For generator implementations, see docs/quick-start.md
       For validation rules, see docs/schema-reference.md

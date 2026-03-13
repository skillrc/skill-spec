OPENCODE-SKILL-SPEC(5)

NAME
       opencode-skill-spec - Management-oriented metadata standard for
       AI skill libraries

SYNOPSIS
       A SKILL file contains YAML frontmatter followed by Markdown content:

              ---
              name: git-workflow
              description: Use when planning safe git commits
              type: technique
              category: engineering
              tags:
                - git
                - commits
              topics:
                - version-control
              boundary: "Focused on local git hygiene..."
              maturity: beta
              ---

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

       This specification defines a lightweight metadata layer that makes
       skills discoverable, manageable, and trustworthy without adding
       authoring friction.

FILE FORMAT
       SKILL files use YAML frontmatter + Markdown body:

       +------------------+-------------------------------------------+
       | Section          | Format                                    |
       +------------------+-------------------------------------------+
       | Frontmatter      | YAML between --- delimiters               |
       | Content          | Markdown after second ---                 |
       | Encoding         | UTF-8, LF line endings                    |
       | Extension        | .md (Markdown)                            |
       +------------------+-------------------------------------------+

       Complete example:

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
              non_goals:
                - "Teaching git basics"
              ---

              # Git Workflow

              ## Overview
              Brief description...

REQUIRED FIELDS
       +------------------+----------+-----------------------------------+
       | Field            | Type     | Purpose                           |
       +------------------+----------+-----------------------------------+
       | name             | string   | Machine-friendly identifier       |
       | description      | string   | "Use when..." trigger phrase      |
       | type             | enum     | technique/pattern/reference/etc   |
       | category         | enum     | Primary management bucket         |
       | tags             | array    | Search keywords                   |
       | topics           | array    | Thematic groups                   |
       | boundary         | string   | One-sentence scope definition     |
       | maturity         | enum     | draft/alpha/beta/stable/etc       |
       +------------------+----------+-----------------------------------+

OPTIONAL FIELDS
       +------------------+----------+-----------------------------------+
       | Field            | Type     | Purpose                           |
       +------------------+----------+-----------------------------------+
       | non_goals        | array    | Explicit exclusions               |
       +------------------+----------+-----------------------------------+

FIELD DETAILS
       name
              Machine-friendly identifier.
              Format: lowercase letters, numbers, hyphens.
              Max 50 characters.
              Example: git-workflow, code-review

       description
              Human-readable trigger phrase.
              Recommended: Start with "Use when..."
              Length: 10-30 words.
              Example: "Use when planning safe git commits"

       type
              Defines instructional mode.

              +---------------+-------------------------------------------+
              | Value         | Use For                                   |
              +---------------+-------------------------------------------+
              | technique     | Step-by-step procedures                   |
              | pattern       | Mental models, approaches                 |
              | reference     | Documentation, syntax guides              |
              | discipline    | Rules, requirements, checklists           |
              +---------------+-------------------------------------------+

       category
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

       tags
              Searchable keywords for discovery.
              Format: lowercase, kebab-case.
              Count: 3-5 recommended.
              AI-generated by default, user-confirmed.

              Example:
                     tags:
                       - git
                       - commits
                       - repository

       topics
              Thematic groups for browsing.
              Format: kebab-case, domain-oriented.
              Count: 1-3 recommended.
              AI-generated by default, user-confirmed.

              Example:
                     topics:
                       - version-control
                       - code-hygiene

       boundary
              One-sentence scope definition.
              Format: "Focused on [concrete scope]"
              Must be specific enough to judge fit.

              Good: "Focused on local git hygiene and history cleanup"
              Bad:  "Helps with coding tasks"

       maturity
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

       non_goals
              Explicit exclusions.
              Prevents misuse by stating what skill does NOT do.

              Example:
                     non_goals:
                       - "Teaching git fundamentals"
                       - "Remote branch policies"

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

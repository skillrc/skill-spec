# Quick Start Guide

Get started with the OpenCode Skill Specification in 5 minutes.

---

## Option 1: I'm Creating a New Skill

### Step 1: Prepare Your Inputs

Before generating, know:
- **Name**: What will you call it? (e.g., `git-workflow`, `code-review`)
- **Description**: When should someone use this? Start with "Use when..."
- **Type**: What kind of skill is it?
  - `technique` — Step-by-step procedure
  - `pattern` — Mental model or approach
  - `reference` — Lookup, syntax, docs
  - `discipline` — Rules to follow
- **Category**: Primary bucket
  - `engineering`, `frontend`, `backend`, `devops`, `testing`, `security`, `research`, `writing`, `workflow`, `product`, `general`
- **Boundary**: One sentence on what it covers

### Step 2: Use a Generator

Use a tool that supports this spec (like `opencode-create-skill`):

```bash
opencode-create-skill
```

The tool will:
1. Ask for your inputs
2. Generate suggested `tags` and `topics` using AI
3. Show you the suggestions
4. Let you accept or edit them
5. Create the complete SKILL.md

### Step 3: Review and Refine

Check the generated metadata:

```yaml
---
name: git-workflow
description: Use when planning safe git commits and repository cleanup
type: technique
category: engineering
tags:
  - git
  - commits
  - repository
topics:
  - version-control
boundary: "Focused on local git hygiene and repository maintenance"
maturity: draft
---
```

**Ask yourself:**
- [ ] Do the tags feel like keywords I'd search for?
- [ ] Do the topics feel like themes this belongs to?
- [ ] Is the boundary specific enough?
- [ ] Should I add non-goals?

### Step 4: Add Non-Goals (Optional but Recommended)

Add explicit exclusions:

```yaml
non_goals:
  - "Teaching Git fundamentals"
  - "Managing remote branch policies"
```

### Step 5: Write Content

Add your skill content below the frontmatter:

```markdown
# Git Workflow

## Overview
Brief description of what this skill provides.

## When to Use
- Before committing changes
- When cleaning up local history

## Core Technique

### Step-by-Step
1. Stage related changes together
2. Write atomic commit messages
3. Review before pushing

## Common Mistakes
### Mistake 1: Mixing unrelated changes
**Problem:** Hard to revert or review
**Fix:** Split into separate commits
```

---

## Option 2: I'm Migrating Existing Skills

### For Each Existing Skill

#### 1. Add Required Fields

Start with what you have:
```yaml
---
name: my-skill
description: Use when...
---
```

Add the missing required fields:
```yaml
---
name: my-skill
description: Use when...
type: technique          # Choose best fit
category: engineering    # Choose best fit
boundary: "Focused on..." # Extract from description
maturity: draft          # Start here
tags: []                 # Will generate
topics: []               # Will generate
---
```

#### 2. Generate Tags and Topics

Use an AI tool or generator:

```bash
# Example using a hypothetical tool
skill-enrich my-skill.md
```

Or manually based on:
- What keywords describe this skill?
- What theme does it belong to?

#### 3. Set Initial Maturity

Be honest:
- Just created? → `draft`
- Works but untested? → `alpha`
- Tested somewhat? → `beta`
- Used in production? → `stable`

#### 4. Review

Ask:
- Are tags specific enough?
- Is the boundary clear?
- Should I add non-goals?

---

## Option 3: I'm Building a Tool

### For Skill Generators

**Must support:**
1. Collect: `name`, `description`, `type`, `category`, `boundary`
2. Generate: `tags`, `topics` using AI/rules
3. Present: Show suggestions to user
4. Confirm: Let user accept or edit
5. Output: Complete SKILL.md with all required fields

**Recommended flow:**
```
User Input → AI Suggestion → User Review → Final Output
```

### For Skill Linters

**Check for:**
```python
required_fields = ['name', 'description', 'type', 'category', 
                   'tags', 'topics', 'boundary', 'maturity']

valid_types = ['technique', 'pattern', 'reference', 'discipline']
valid_maturity = ['draft', 'alpha', 'beta', 'stable', 'deprecated']
```

**Warn about:**
- Empty tags/topics lists
- Generic tags (`code`, `software`)
- Vague boundaries ("helps with coding")
- Missing non-goals for `discipline` type

### For Skill Catalogs

**Index by:**
- Category (primary filter)
- Topics (thematic browsing)
- Tags (keyword search)
- Maturity (trust filter)

**Display:**
- Name + description
- Maturity badge
- Tags (clickable filters)
- Boundary (preview)

---

## Common Patterns

### Pattern 1: Technique Skill

```yaml
---
name: atomic-commits
description: Use when organizing changes into independently reversible commits
type: technique
category: engineering
tags:
  - git
  - commits
  - organization
topics:
  - version-control
  - code-hygiene
boundary: "Covers splitting changes, writing commit messages, and maintaining clean history"
maturity: beta
---
```

### Pattern 2: Pattern Skill

```yaml
---
name: fail-fast
description: Use when designing error handling that surfaces problems immediately
type: pattern
category: engineering
tags:
  - errors
  - reliability
  - design
topics:
  - reliability-patterns
  - defensive-programming
boundary: "Describes the fail-fast principle and when to apply it"
maturity: stable
---
```

### Pattern 3: Reference Skill

```yaml
---
name: postgres-commands
description: Use when writing PostgreSQL and need quick syntax reference
type: reference
category: backend
tags:
  - postgres
  - sql
  - database
  - reference
topics:
  - databases
  - sql-reference
boundary: "Provides common PostgreSQL commands, functions, and query patterns"
maturity: stable
---
```

### Pattern 4: Discipline Skill

```yaml
---
name: no-console-in-prod
description: Use when reviewing code that must not have console.log in production
type: discipline
category: engineering
tags:
  - linting
  - production
  - quality
topics:
  - code-quality
  - production-readiness
boundary: "Enforces removal of debug logging before production deployment"
maturity: stable
non_goals:
  - "Detecting all types of bugs"
  - "Replacing comprehensive testing"
---
```

---

## Validation Checklist

Before considering a skill complete:

### Required Fields
- [ ] `name` — Valid format, ≤50 chars
- [ ] `description` — Starts with "Use when", specific
- [ ] `type` — One of: technique, pattern, reference, discipline
- [ ] `category` — Clear primary bucket
- [ ] `tags` — 3-5 specific keywords
- [ ] `topics` — 1-3 relevant themes
- [ ] `boundary` — One sentence, starts with "Focused on"
- [ ] `maturity` — One of: draft, alpha, beta, stable, deprecated

### Quality Checks
- [ ] Tags are specific (not `code`, `software`, `development`)
- [ ] Topics are meaningful (not just category repeats)
- [ ] Boundary is concrete (you can tell if your problem fits)
- [ ] For `discipline` type: non-goals added
- [ ] For `beta` or `stable`: boundary has been tested

### Content Checks
- [ ] Content below frontmatter is helpful
- [ ] Examples are realistic
- [ ] Common mistakes are addressed
- [ ] Related skills are listed (if applicable)

---

## Troubleshooting

### "My tags are too generic"

**Problem:** AI suggested `code`, `software`, `development`

**Fix:** Manually replace with specific terms:
- ❌ `code` → ✅ `python`
- ❌ `software` → ✅ `testing`
- ❌ `development` → ✅ `debugging`

### "My boundary is vague"

**Problem:** "Focused on helping with code quality"

**Fix:** Make it specific:
- ❌ "Helps with code quality"
- ✅ "Covers static analysis, complexity checks, and test coverage gaps"

### "I don't know what maturity to set"

**Guidelines:**
- `draft` — Just wrote it, not tested
- `alpha` — Works for me, might break for others
- `beta` — Several people used it successfully
- `stable` — Used in production, no major issues
- `deprecated` — There's a better alternative now

**When in doubt:** Start with `draft`, promote as it proves itself.

### "My topics seem off"

**Check:** Are they themes or just keywords?

- ❌ Keywords: `git`, `commits`, `commands`
- ✅ Themes: `version-control`, `developer-tools`, `automation`

**Fix:** Topics should answer "what domain does this belong to?"

---

## Next Steps

1. **Create your first skill** using the steps above
2. **Review the full spec** in [SPEC.md](../SPEC.md)
3. **Compare approaches** in [Comparison](comparison.md)
4. **Understand the design** in [Authoring Rationale](authoring-rationale.md)
5. **Contribute** improvements back to the spec

---

## Getting Help

- **Questions about fields?** → [Schema Reference](schema-reference.md)
- **Why design it this way?** → [Authoring Rationale](authoring-rationale.md)
- **Tags vs Topics confusion?** → [Tags and Topics Guide](tags-topics.md)
- **Comparing to alternatives?** → [Comparison](comparison.md)

---

**You're ready. Go build something findable.**

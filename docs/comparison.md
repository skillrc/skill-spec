# Comparison with Alternatives

How the OpenCode Skill Specification compares to other approaches.

---

## Comparison Table

| Approach | Discovery | Quality Signal | Governance | Authoring Friction | Scale |
|----------|-----------|----------------|------------|-------------------|-------|
| **Minimal (name + desc)** | ❌ Poor | ❌ None | ❌ None | ✅ Zero | ❌ Fails at 20+ |
| **Score-Based** | ⚠️ Okay | ⚠️ Gameable | ⚠️ Subjective | ⚠️ High | ⚠️ Problematic |
| **Taxonomy-Heavy** | ✅ Good | ⚠️ Complex | ✅ Strong | ❌ Very High | ✅ Good |
| **This Spec** | ✅ Good | ✅ Maturity | ✅ Strong | ✅ Low | ✅ Good |

---

## Approach 1: Minimal (Name + Description Only)

**Example:**
```yaml
name: git-workflow
description: Use when working with git
```

**Problems at Scale:**

1. **Can't search effectively**
   - Searching "commits" won't find this
   - No topic browsing
   - No category filtering

2. **Can't assess quality**
   - Is this well-tested or experimental?
   - Should I trust it for production?
   - Who maintains it?

3. **Can't prevent misuse**
   - Does it cover remote branches? Maybe, maybe not
   - Will it teach me git basics? Unclear
   - Scope is implicit

4. **Can't maintain inventory**
   - 50 skills all look similar
   - Duplicates are invisible
   - Abandoned skills accumulate

**Verdict:** Works for <10 skills. Fails at scale.

---

## Approach 2: Score-Based Quality

**Example:**
```yaml
name: git-workflow
quality_score: 8.5
popularity: 92
trust_level: 4.2
```

**Problems:**

1. **Scores are meaningless**
   - What does "8.5" mean? Better than 8.0, worse than 9.0?
   - Is 7.0 good or bad?
   - Different scores mean different things to different people

2. **Gaming is inevitable**
   - Authors optimize for score, not usefulness
   - Vanity metrics over real quality
   - Score inflation over time

3. **False precision**
   - Quality isn't one-dimensional
   - A "9.0" skill might be wrong for your use case
   - Scores hide context

4. **Authoring burden**
   - How do I know what score to assign?
   - Do I need user data I don't have?
   - Creates pressure to inflate

**Verdict:** Numbers feel objective but create more problems than they solve.

---

## Approach 3: Heavy Taxonomy

**Example:**
```yaml
name: git-workflow
classification:
  primary_domain: version_control
  secondary_domains:
    - collaboration
    - code_quality
  competency_level: intermediate
  prerequisites:
    - git_basics
    - command_line
  related_standards:
    - git_conventional_commits
  skill_matrix:
    technical_depth: 7
    breadth: 4
    collaboration: 6
```

**Problems:**

1. **Authoring is exhausting**
   - Too many decisions to make
   - Requires deep system knowledge
   - Discourages contribution

2. **Maintenance nightmare**
   - Taxonomies change
   - Classifications drift
   - Dependencies break

3. **Precision without accuracy**
   - All that detail might be wrong
   - Hard to validate
   - False sense of rigor

4. **Tools get complex**
   - Complex UI needed
   - Validation rules everywhere
   - Onboarding is painful

**Verdict:** Over-engineered. The cost exceeds the value.

---

## This Specification: Management-Oriented, AI-Assisted

**Key Insight:**
> Good governance doesn't require exhaustive taxonomy. It requires clear boundaries and discoverability.

### How We Solve the Problems

#### Discovery (vs Minimal)
**Our approach:**
```yaml
tags:
  - git
  - commits
  - repository
topics:
  - version-control
  - code-hygiene
```

- **Tags** for keyword search
- **Topics** for browsing
- **Category** for management
- AI suggests, user confirms

**Result:** Findable without manual tagging burden.

---

#### Quality Signal (vs Scores)
**Our approach:**
```yaml
maturity: beta
```

**Stages:**
- `draft` → "Might not work, shape it"
- `alpha` → "Try it, expect issues"
- `beta` → "Good enough for most"
- `stable` → "Production-ready"
- `deprecated` → "Don't use for new work"

**Why stages win:**
- Clear meaning
- Actionable
- Hard to game
- Natural lifecycle

---

#### Governance (vs Heavy Taxonomy)
**Our approach:**
```yaml
category: engineering
boundary: "Focused on local git hygiene..."
non_goals:
  - "Teaching git fundamentals"
```

**Just enough structure:**
- One category for bucketing
- Boundary for scope
- Non-goals for exclusions

**Result:** Manageable without exhaustion.

---

#### Authoring Friction (vs All Approaches)
**Our approach:**

1. User provides:
   - `name`
   - `description`
   - `type`
   - `category`
   - `boundary`

2. AI generates:
   - `tags`
   - `topics`

3. User confirms or edits

**Result:**
- **Schema requires** metadata (for governance)
- **Authoring doesn't** require typing it all (low friction)
- **AI suggests** reasonable defaults (fast)
- **Human confirms** (accurate)

---

## Specific Comparisons

### Tags/Topics vs Keywords

|  | Keywords | Tags + Topics |
|--|----------|---------------|
| **Level** | Flat | Hierarchical |
| **Purpose** | Search | Search + Browse |
| **Count** | Unlimited | Limited (intentional) |
| **Curation** | Manual | AI-assisted |

**Why two layers?**
- Tags = keywords (find me things about X)
- Topics = themes (show me related capabilities)

### Maturity vs Score

|  | Score | Maturity |
|--|-------|----------|
| **Meaning** | Unclear | Clear stage |
| **Actionable** | No | Yes |
| **Gaming** | Easy | Hard |
| **Maintenance** | Constant | Lifecycle-based |
| **Context** | Ignored | Front and center |

### Boundary vs Scope Section

|  | Scope Section | Boundary |
|--|---------------|----------|
| **Location** | Body text | Frontmatter |
| **Length** | Paragraphs | One sentence |
| **Machine readable** | No | Yes |
| **Enforceable** | No | Can lint |

---

## When to Use Each Approach

### Use Minimal (Name + Desc) When:
- You have <10 skills
- Personal use only
- No sharing planned
- No maintenance needed

### Use Score-Based When:
- You have massive user data
- Statistical ranking matters
- Gaming can be monitored
- Users demand "top 10" lists

### Use Heavy Taxonomy When:
- Enterprise compliance requires it
- Skills map to formal curricula
- Detailed reporting needed
- You have dedicated librarians

### Use This Spec When:
- You want governance without bureaucracy
- Skills will grow to 50-500
- Team collaboration matters
- AI assistance available
- You want findability without manual tagging

---

## Real-World Trade-offs

### "But scores feel more precise!"

Precision without meaning is misleading.

- "8.5/10" sounds precise but tells you nothing
- "beta" is vague-sounding but tells you exactly what to expect

### "But we need more categories!"

Resist the urge. One primary category forces clarity.

- Multiple categories = classification debates
- One category + tags/topics = flexible but structured

### "But AI might get tags wrong!"

Yes, which is why humans confirm.

- AI suggests (fast)
- Human reviews (accurate)
- Better than manual typing (tedious)

---

## Migration Path

### From Minimal
1. Add `type`, `category` (pick best fit)
2. Add `boundary` (one sentence from description)
3. Set `maturity: draft`
4. Generate `tags/topics` with AI
5. Review and refine

### From Score-Based
1. Map scores to maturity:
   - < 6.0 → `alpha`
   - 6.0-8.0 → `beta`
   - 8.0+ → `stable`
2. Add missing required fields
3. Drop scores (they were misleading anyway)

### From Heavy Taxonomy
1. Pick ONE `category`
2. Convert detailed classification to `tags`
3. Summarize scope in `boundary`
4. Simplify, simplify, simplify

---

## Summary

| What You Want | Use This |
|---------------|----------|
| Findability | Tags + Topics |
| Quality signal | Maturity stages |
| Scope clarity | Boundary + Non-goals |
| Low friction | AI-assisted authoring |
| Governance | Required schema fields |
| Scale | All of the above |

**This spec optimizes for the sweet spot:**
> Enough structure for governance, not so much that authoring becomes a burden.

---

## See Also

- [Authoring Rationale](authoring-rationale.md) — Why we designed it this way
- [Schema Reference](schema-reference.md) — Complete field documentation
- [Quick Start](quick-start.md) — Get started in 5 minutes

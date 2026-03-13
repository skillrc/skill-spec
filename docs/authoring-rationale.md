# Authoring Rationale

## Why this document exists

The SKILL specification distinguishes between two concerns:

1. what metadata must exist in the final skill file
2. how that metadata is produced during authoring

This distinction matters because a schema that is easy to govern is not automatically easy to author.

## Required schema does not mean manual entry

`tags` and `topics` are required fields because every skill should be discoverable at both:

- the keyword level (`tags`)
- the thematic level (`topics`)

But requiring those fields in the final file does **not** mean the human author should always type them from scratch.

## Why AI/tooling should generate defaults

AI or structured tooling can derive first-pass tags/topics from:

- skill name
- description
- category
- boundary

This improves:

- consistency across the skill library
- speed of authoring
- discoverability quality
- taxonomy hygiene

## Why human confirmation still matters

Automatic suggestions are useful, but they are not authoritative.

Authors should be able to:

- accept the generated values
- edit them manually
- override them completely

This keeps the system efficient without removing author judgment.

## Recommended policy

- `tags` and `topics` must exist in the output schema
- generators should suggest them automatically by default
- authors should confirm or revise them before the file is finalized

This policy balances governance with usability.

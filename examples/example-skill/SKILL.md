# Git Workflow

## Overview

Use a structured process to split changes into atomic commits, preserve safety when rewriting local history, and keep version control operations easy to review.

## When to Use

- You need to organize unrelated changes before committing
- You need to clean up local history before creating a pull request
- You want repeatable rules for safe git operations

## Boundary

Focused on local git hygiene, commit planning, and safe history editing.

## Non-Goals

- Teaching Git fundamentals
- Managing remote repository policy

## Core Technique

1. Inspect the working tree and recent history.
2. Split changes into independently reversible groups.
3. Commit foundations before dependents.
4. Rewrite only local history unless explicitly approved.

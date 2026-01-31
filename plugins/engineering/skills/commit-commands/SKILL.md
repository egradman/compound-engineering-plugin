---
name: commit-commands
description: This skill should be used when the user wants to commit staged or unstaged changes. It provides two commands - a single commit command and a bucket commit command that groups related changes into multiple commits.
---

# Commit Commands

This skill provides git commit workflows accessible via slash commands.

## Available Commands

### `/commit`

Creates a single git commit from all current changes. Analyzes the diff, proposes a commit message, and gives the user an opportunity to edit the message before committing.

### `/bucket-commit`

Analyzes all current changes and proposes multiple commits that group related changes together. Presents the proposed buckets to the user for approval, allowing them to move changes between buckets or skip items before committing.

## When to Use

- `/commit` - Standard single commit for a cohesive set of changes
- `/bucket-commit` - When the working tree contains multiple unrelated changes that should be separate commits

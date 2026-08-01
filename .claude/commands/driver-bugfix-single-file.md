---
name: driver-bugfix-single-file
description: Workflow command scaffold for driver-bugfix-single-file in linux.
allowed_tools: ["Bash", "Read", "Write", "Grep", "Glob"]
---

# /driver-bugfix-single-file

Use this workflow when working on **driver-bugfix-single-file** in `linux`.

## Goal

Fixing a bug or issue in a single driver source file, often with a detailed commit message explaining the root cause, symptoms, and fix.

## Common Files

- `drivers/*/*.c`
- `drivers/*/*/*.c`

## Suggested Sequence

1. Understand the current state and failure mode before editing.
2. Make the smallest coherent change that satisfies the workflow goal.
3. Run the most relevant verification for touched files.
4. Summarize what changed and what still needs review.

## Typical Commit Signals

- Identify the bug and its root cause in the driver source file.
- Edit the relevant driver source file to fix the bug.
- Write a detailed commit message explaining the bug, how it was fixed, and referencing any relevant bug reports or Fixes tags.
- Commit the change.

## Notes

- Treat this as a scaffold, not a hard-coded script.
- Update the command if the workflow evolves materially.
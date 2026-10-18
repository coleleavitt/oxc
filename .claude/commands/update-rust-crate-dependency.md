---
name: update-rust-crate-dependency
description: Workflow command scaffold for update-rust-crate-dependency in oxc.
allowed_tools: ["Bash", "Read", "Write", "Grep", "Glob"]
---

# /update-rust-crate-dependency

Use this workflow when working on **update-rust-crate-dependency** in `oxc`.

## Goal

Updates a Rust crate dependency to a newer version, typically via Renovate or similar automation.

## Common Files

- `Cargo.lock`

## Suggested Sequence

1. Understand the current state and failure mode before editing.
2. Make the smallest coherent change that satisfies the workflow goal.
3. Run the most relevant verification for touched files.
4. Summarize what changed and what still needs review.

## Typical Commit Signals

- Update the version of the crate in Cargo.toml (sometimes implicit via lockfile)
- Regenerate Cargo.lock
- Commit Cargo.lock (and possibly Cargo.toml)
- Include changelog or release notes in commit message

## Notes

- Treat this as a scaffold, not a hard-coded script.
- Update the command if the workflow evolves materially.
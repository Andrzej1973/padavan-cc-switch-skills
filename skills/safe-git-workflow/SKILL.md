---
name: safe-git-workflow
description: Use a cautious, auditable Git workflow for firmware repositories, preserving user changes and avoiding destructive operations.
---

# Safe Git Workflow

## Purpose
Perform Git operations with clear scope, preservation of user work, and verifiable results.

## Hard rules
- Before any change, inspect `git status --short --branch`, current branch, HEAD SHA, and relevant diff.
- Never discard, overwrite, reset, clean, rebase, amend, or force-push user changes unless the user explicitly authorizes the specific operation.
- Do not run `git reset --hard`, `git clean -fd`, or force-push as routine cleanup.
- Never stage unrelated files, secrets, build outputs, or generated firmware images without explicit intent.
- Do not commit or push unless requested.
- Keep changes to the requested repository and scope.

## Workflow
1. Identify repository root and confirm the expected project.
2. Record branch, HEAD SHA, upstream, working-tree state, staged changes, and untracked files.
3. Read relevant files and history before editing.
4. Make the smallest coherent change; preserve unrelated user modifications.
5. Inspect `git diff --check`, staged diff, unstaged diff, and untracked files.
6. Run relevant tests and record outcomes.
7. Before a commit, show the exact files and diff summary; commit only when requested.
8. After operations, verify branch/HEAD/status and report any remaining changes.

## Conflict handling
- Do not choose one side blindly.
- Inspect conflict markers and both histories; understand intended behavior.
- Preserve unrelated hunks and user work.
- Run tests after resolution.
- If intent cannot be established, stop and ask.

## Required output
- Initial and final status
- Commands run
- Files affected
- Tests and results
- Any operation skipped for safety

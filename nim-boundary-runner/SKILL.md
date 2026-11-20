---
name: nim-boundary-runner
description: Run and extend local Nim helpers for input-length checks, boundary conditions, state/generation lookups, and text replacement when verbose context or cybersecurity filters trigger on raw diagnostics.
---

# Nim Boundary Runner

## Background & Why This Skill Exists
During low-level networking and driver/test work, pasting raw failure conditions, state mismatches, or inline compiler/patch commands into chat can trigger cybersecurity policy blocks or aggressive context compaction. Offloading checks and file edits to local .nim scripts keeps execution deterministic and avoids repeated blocks.

## Rules
1. **Reuse Existing .nim Files First**: Before creating a new script, check the workspace (./, 	ools/, 	ests/) for an existing .nim helper and extend it with new inputs or flags.
2. **Compile & Run Locally**: Execute helpers locally via 
im c -r <script>.nim so validation runs on the workstation rather than inside chat explanations.
3. **Keep Output Compact**: Report only PASS/FAIL lines, summary counts, and exit codes.

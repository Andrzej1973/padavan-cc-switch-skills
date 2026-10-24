---
name: firmware-build-verify
description: Build Padavan firmware and verify the correct target artifact, metadata, checksum, and build evidence.
---

# Firmware Build Verify

## Purpose
Produce and verify a reproducible, target-correct Padavan firmware build, preferably in CI for the named board.

## Safety
- Do not flash a physical router.
- Do not delete build outputs or change build configuration without explicit permission.
- Do not print secrets from CI variables, config files, or logs.
- A successful compile is not proof of bootability, runtime correctness, or hardware compatibility.

## Workflow
1. Record repository URL, branch, commit SHA, dirty/clean status, target board/profile, toolchain, kernel version, and build configuration.
2. Inspect the documented build instructions and CI workflow before running commands.
3. Prefer the repository's supported GitHub Actions workflow; use local builds only if the required environment is available.
4. Capture workflow URL/run ID, job conclusions, relevant logs, and exact build command.
5. Enumerate generated artifacts and verify filenames, file types, sizes, timestamps, and target identifiers against project documentation.
6. Compute SHA-256 for each release artifact. Record hashes; do not invent or copy a hash from a different build.
7. Check that the artifact is not empty/truncated and that packaging/container format matches the expected upgrade path.
8. Confirm expected features/configuration are present through build config, image metadata, or binary inspection where practical.
9. Compare artifact size and key build outputs against a known-good baseline if available.
10. Produce a release manifest with commit SHA, target, artifact name, byte size, SHA-256, CI link, and verification limitations.

## Required output
- Build status with evidence
- Target/profile and commit
- Artifact inventory and SHA-256
- Checks performed and results
- Warnings, skipped checks, and limitations
- Separate statements for compile success, artifact validation, and hardware validation

## Acceptance criteria
Never mark release-ready if the target is ambiguous, the artifact is missing/empty, the checksum is absent, or CI evidence cannot be tied to the exact commit.

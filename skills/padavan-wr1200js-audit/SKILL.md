---
name: padavan-wr1200js-audit
description: Lead the primary Padavan 4.4.198 audit, fixes, security review, CI verification, and release-readiness assessment for YOUHUA WR1200JS (MT7621).
---

# YOUHUA WR1200JS Padavan Audit

## Purpose and priority

This Skill governs the primary project: comprehensive audit, correction, stabilization, and release-readiness work for Padavan 4.4.198 on YOUHUA WR1200JS (MT7621).

The user's authoritative project document is `Повний технічний аудит, виправлення та стабілізація.txt`. Read and follow its current repository copy or user-provided copy. Do not silently replace its priorities with this Skill. If the document is unavailable, state that limitation and ask for it rather than inventing its exact requirements.

MIMINI is a separate, secondary workstream. It must never displace the WR1200JS priority.

## Non-negotiable safety

- Start read-only: inspect repository URL, branch, HEAD, `git status --short --branch`, staged/unstaged/untracked files, and relevant diffs.
- Preserve all user changes. Never use destructive cleanup, force push, history rewrite, automatic merge, or branch deletion.
- Do not commit, push, tag, publish firmware, or merge unless the user explicitly authorizes that operation.
- Never flash or reboot a physical router, alter NVRAM, factory data, bootloader, or partition layout without explicit, specific approval.
- Never expose credentials, private keys, Wi-Fi passwords, tokens, or secrets in output or commits.
- Do not claim a test, CI run, hardware validation, or fix succeeded unless evidence confirms it.
- Do not change toolchain, kernel baseline, SoC support, or platform architecture without a documented compatibility analysis and authorization where required.

## Initial repository review

1. Confirm the active branch, HEAD SHA, clean/dirty state, upstream, and any in-progress CI.
2. Read project instructions and status documents, including README, `AGENTS.md`, project status, handoff, build configs, CI workflows, and relevant docs.
3. Identify the exact target board and confirm MT7621-specific assumptions.
4. Map changed files to their callers, configuration sources, init lifecycle, runtime consumers, and tests.
5. Keep conclusions tied to the inspected commit and file versions.

## Priority work areas

Follow the authoritative project document's priority order. The known critical areas include:

### VPN Profile Manager IPv6 policy routing

Review the complete path through `tools/vpn-profiles/route-apply.c`, headers, tests, `profilectl.c`, config planning, and config application.

- Keep IPv4 and IPv6 operations family-specific.
- Use IPv6 route/rule operations for IPv6 resources (for example, `ip -6 route` and `ip -6 rule`).
- Verify dual-stack behavior, default routes, prefix validation, rollback, cleanup, and idempotence.
- Never delete routes, rules, addresses, or tables owned by another profile or subsystem.
- Test IPv4, IPv6, dual stack, invalid prefixes, command failures, partial failures, cleanup, rollback, and repeated invocation.

### VPN profile shutdown and cleanup

Review `wr_down()` and the results of route, address, and rule cleanup operations.

- Do not report successful shutdown when cleanup failed or its state is unknown.
- Preserve enough state for safe retry/recovery.
- Handle partial failure without deleting resources owned by other profiles.
- Add tests for each partial-failure path.

### Release and documentation consistency

Reconcile README, `AGENTS.md`, project status/handoff, runtime docs, CAKE/SQM documentation, `sources.lock.json`, build configs, and release policy.

- Preserve the documented baseline decisions.
- Mark superseded guidance clearly; do not erase useful history.
- Verify actual image composition rather than relying only on documentation.

### CI/CD and artifact integrity

Inspect relevant GitHub Actions, build scripts, build configs, packaging, and checksums.

- Confirm default image composition and target correctness.
- Ensure experimental options do not leak into the default image.
- Tie CI and artifacts to the exact commit SHA.
- Check artifact names, sizes, formats, and SHA-256.
- Do not claim release readiness from a successful compile alone.

### Security and runtime integration

Audit WebUI/CGI/shell/filesystem/network boundaries, including command injection, CSRF, path traversal, permissions, secrets, DNS leakage, IPv4/IPv6 firewalling, WAN exposure, USB/IP, NAT/HWNAT, and relevant services.

Review service lifecycle and interoperability for the components present in this repository, including VPN, DNS, proxy, overlay networking, QoS/IFB, USB, and hardware/software packet acceleration. Do not assume a feature is enabled or supported without tracing configuration to runtime.

### Kernel and reproducible-build review

- Review the existing vendor kernel baseline and verifiable security backports; do not casually replace or bump the kernel.
- Review source locks, licensing, toolchain inputs, and reproducibility claims.
- Do not mark a build self-contained or reproducible without executing and documenting the required verification.

## CI pause and secondary MIMINI work

- If WR1200JS work is waiting for CI or another external check, record the exact commit and pending check.
- Do not change files or shared components that could invalidate that pending check.
- During the wait, an independent MIMINI task may proceed only in a separate branch/worktree and only if it cannot affect the pending WR1200JS result.
- Return to WR1200JS promptly when the check completes; inspect the result and continue the primary task.
- Keep reports, branches, and commits for the two workstreams separate.

## Testing and evidence

- Prefer focused automated tests and static checks relevant to the change.
- Record exact commands, exit codes, and the commit tested.
- Label hardware-only tests `NOT RUN` when no physical WR1200JS is available.
- Distinguish source inspection, host tests, cross-compilation, CI, boot validation, and real hardware/network tests.
- For every defect, record reproduction/evidence, impact, minimal fix, and regression coverage.

## Release-readiness decision

Use only evidence-backed states:
- `READY FOR RELEASE`
- `READY FOR CONTROLLED TESTING`
- `NOT READY FOR RELEASE`

Do not use `READY FOR RELEASE` while required IPv6 routing/cleanup, CI, security, image composition, artifact integrity, or recovery evidence remains unverified.

## Required handoff

Report:
1. Repository, branch, HEAD, and working-tree state.
2. Primary objective and current priority.
3. Findings grouped by severity with file/line evidence.
4. Changes made and why.
5. Tests/CI run, exact results, and tests not run.
6. Pending CI or blockers.
7. Release-readiness state.
8. The next safe, narrowly scoped action.

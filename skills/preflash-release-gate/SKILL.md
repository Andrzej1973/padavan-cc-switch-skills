---
name: preflash-release-gate
description: Make an evidence-backed GO/NO-GO decision before flashing Padavan firmware to physical hardware.
---

# Preflash Release Gate

## Purpose
Act as the final independent gate before any physical router flash. Default to NO-GO when critical evidence is missing.

## Hard safety rules
- Never flash, reboot, reset, or change the physical router.
- Never authorize or perform flashing on the user's behalf. Only the user can explicitly approve the physical flash after reviewing the report.
- A successful build is not hardware validation.
- Do not treat planned tests as completed tests.
- If there is no verified recovery path, classify the release as NO-GO.

## Required gates
### A. Source integrity
- Confirm repository, branch, exact commit SHA, dirty/clean status, full diff, and no unintended changes.
- Confirm the build used the same commit being reviewed.
- Check secrets, credentials, private keys, debug backdoors, and unintended files.

### B. Target and artifact
- Confirm exact board/model and hardware revision compatibility from project-supported evidence.
- Confirm image format, size, target identifier, packaging, and upgrade method.
- Verify SHA-256 from the actual artifact and retain the value.
- Ensure the artifact is downloadable and not truncated.

### C. CI and tests
- Verify GitHub Actions conclusion and link for the exact commit.
- Record exact commands, logs, exit codes, and test results.
- Run static checks and automated tests available in the repository.
- Explicitly list missing hardware, RF, throughput, reboot, and upgrade/downgrade tests.

### D. Network/security behavior
- Review firewall and routing behavior, including IPv4 and IPv6 where enabled.
- For IoT isolation, verify intended 2.4 GHz-only exposure, separate addressing, Internet access, and blocks to primary LAN and router administration.
- Review DHCP/DNS access and established/related return traffic.
- Verify manual WAN port remapping, NAT, hardware NAT/flow offload, Wi-Fi, and other existing features for regressions.

### E. Recovery and release evidence
- Identify documented recovery/rollback procedure, required tools, and known failure modes.
- Ensure a known-good firmware and configuration backup are available where applicable.
- Provide artifact name, size, SHA-256, CI URL, commit SHA, and compatibility evidence.

## Decision policy
- GO: every mandatory evidence gate passes; still requires explicit user approval to flash.
- CONDITIONAL: only non-critical checks remain, with a clear list and user acceptance; never use this for unknown target compatibility or missing recovery.
- NO-GO: wrong/uncertain target, failed CI, unexplained diff, missing artifact/hash, isolation/security uncertainty, regression in critical networking, or unverified recovery plan.

## Required report
1. Decision: GO / CONDITIONAL / NO-GO
2. Evidence for each gate, including links and command output
3. Artifact identity and SHA-256
4. Critical risks and untested assumptions
5. Recovery plan
6. Exact next action required from the user

Never say "safe to flash" solely because compilation succeeded.

---
name: padavan-router-profile-manager
description: Manage router hardware profiles across Padavan firmware repositories. Identify board and SoC differences, select applicable skills, protect hardware-specific configuration, and validate assumptions before firmware changes.
---

# Padavan Router Profile Manager

## Mission

Manage reusable hardware profiles for multiple routers and firmware repositories. Reuse common skills while preserving the differences between models, board revisions, and SoC platforms.

## Core rules

- Never assume two routers are compatible because they use the same firmware family.
- Never assume a model name uniquely identifies a board revision.
- Never transfer GPIO, Ethernet port mapping, switch configuration, Wi-Fi calibration, hardware NAT, flow offload, or boot settings between devices without verification.
- Never modify a firmware repository unless the user has identified it as the intended target.
- Never flash a physical router without explicit user approval.
- Keep proprietary source code and confidential vendor data protected.
- Clearly separate verified facts, assumptions, and unknowns.

## Router profile fields

For each router, record as available:

1. Manufacturer and exact model
2. Hardware revision and board identifier
3. SoC and CPU architecture
4. Flash and RAM capacity
5. Ethernet MAC, PHY, switch, and port mapping
6. WAN/LAN configuration and VLAN requirements
7. Wi-Fi chipset, driver, firmware, and calibration requirements
8. Hardware NAT and flow offload implementation
9. Kernel version and source provenance
10. Toolchain and firmware build configuration
11. Known vendor patches and proprietary components
12. Recovery method and known limitations
13. Evidence sources and verification status

Do not invent missing values. Mark unknown fields as unknown.

## Repository handling

- Identify the target repository before any changes.
- Record its URL, branch, commit, and working-tree status.
- Keep router profiles separate from firmware source code.
- Reuse common skills only after checking their applicability.
- Use model-specific instructions only for verified hardware differences.
- Do not change unrelated repositories or branches.

## Workflow

1. Identify the router and board revision.
2. Locate or create its hardware profile.
3. Verify profile details against repository files, board documentation, or reliable hardware evidence.
4. Select applicable network, kernel, build, regression, and release skills.
5. Identify missing information and incompatible assumptions.
6. Before changes, establish a baseline and a recoverable working branch.
7. After changes, report affected hardware features and validation evidence.
8. Update the profile when new facts are verified.

## Validation

Distinguish source review, successful build, and hardware verification.

A successful build does not prove that Ethernet, WAN mapping, Wi-Fi, hardware NAT, flow offload, or recovery works on the device.

Use these statuses:

- UNKNOWN: insufficient evidence
- NEEDS REVIEW: assumptions or conflicts remain
- BUILD VERIFIED: the relevant build completed successfully
- HARDWARE VERIFIED: relevant tests were performed on the exact device and evidence was recorded

## Stop conditions

Stop and request clarification when the exact board is unknown, profile evidence conflicts, a hardware-specific patch is missing, a change may affect WAN mapping or acceleration, or recovery is uncertain.

## Final report

State the target router, board revision, repository and branch, applicable skills, verified hardware facts, assumptions, changes made, test results, and unresolved risks.
---
# Padavan Kernel Upgrade 4.4.198 to 4.4.302

## Mission
Safely migrate Padavan Linux kernel from 4.4.198 to 4.4.302 while preserving vendor patches and proprietary drivers.

## Safety
- Never replace the modified Padavan kernel tree with pristine upstream sources.
- Never discard vendor patches just to make patches apply.
- Review every conflict manually.
- Never flash a physical router without explicit user approval.

## Baseline
Identify the repository, commit, target board, SoC, toolchain, kernel configuration, and source provenance. Inventory vendor patches and proprietary modules.

## Migration
Compare upstream 4.4.198 and 4.4.302 separately from the modified Padavan tree. Port patches incrementally and document conflicts, adaptations, and configuration changes.

## Validation
Record build logs, warnings, artifacts, module dependencies, and regression results. Test WAN mapping, hardware NAT, flow offload, Wi-Fi, and recovery on the exact hardware.

Build success does not prove hardware compatibility. Do not flash without explicit approval.

## Deliverables
Provide a patch ledger, configuration diff, build evidence, regression report, unresolved risks, and recovery plan.

Use HARDWARE VERIFIED only after actual target-device tests with recorded evidence.

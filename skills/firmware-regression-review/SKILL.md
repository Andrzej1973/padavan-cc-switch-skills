---
name: firmware-regression-review
description: Review Padavan firmware changes for regressions in networking, UI, build integration, and existing router features.
---

# Firmware Regression Review

## Purpose
Review a change set in a Padavan firmware repository for behavioral, security, compatibility, and performance regressions.

## Safety and scope
- Review first; do not modify code unless asked.
- Inspect the full diff and surrounding call paths, not just the changed lines.
- Treat build success as one signal, not proof of correctness.
- Preserve user-established features, including manual WAN port reassignment and hardware NAT/flow offload unless explicitly superseded.

## Review checklist
1. Determine base/head commits, changed files, generated files, untracked files, and repository cleanliness.
2. Trace every changed configuration key through defaults, web UI, validation, persistence, init scripts, and runtime consumers.
3. Check backward compatibility for existing config files, upgrades, factory defaults, and missing/invalid values.
4. Check firewall order, interface names, routing, NAT, DHCP/DNS, IPv4/IPv6, and fail-closed isolation behavior.
5. Check Wi-Fi radio targeting; do not accidentally expose an IoT SSID on 5 GHz if only 2.4 GHz is intended.
6. Review web UI input validation, escaping, localization, responsive layout, and consistent enable/disable behavior.
7. Review shell scripts for quoting, exit status, idempotence, races, temporary files, permissions, and safe failure handling.
8. Check build-system integration, package selection, conditional compilation, image size, and target-specific assumptions.
9. Inspect possible impact on throughput features, hardware NAT, flow offload, and WAN/LAN port mapping.
10. Run relevant tests or static checks that are available and safe. Clearly label unavailable hardware tests.
11. Report only actionable findings, each with severity, file/line, failure scenario, evidence, and suggested minimal fix.

## Required output
- Summary and overall risk
- Findings sorted by severity
- Regression matrix for existing features
- Tests run and not run
- Questions or assumptions that block a confident conclusion

## Severity
- Critical: likely bricking, major security exposure, or loss of core networking
- High: likely loss of connectivity, isolation breach, or broken upgrade path
- Medium: significant feature regression with a workaround
- Low: limited defect or usability issue
- Informational: non-blocking observation

Do not mark a finding as fixed unless the fix is present and verified.

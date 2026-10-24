---
name: padavan-network-audit
description: Audit Padavan router networking, Wi-Fi, VLANs, firewall, NAT, and isolation without changing device state.
---

# Padavan Network Audit

## Purpose
Perform evidence-based network audits for Padavan firmware, especially MediaTek MT7621-based devices such as Xiaomi Mi Router WR1200JS.

## Operating rules
- Start in read-only mode. Do not edit files, commit, deploy, restart services, or change router settings unless the user explicitly authorizes that exact action.
- Never flash firmware or issue destructive router commands.
- Distinguish confirmed facts, hypotheses, and unknowns. Do not infer runtime behavior from configuration alone.
- Preserve existing WAN/LAN port remapping and hardware NAT/flow offload unless the task explicitly requires changing them.
- Do not expose credentials, private keys, Wi-Fi passwords, tokens, or public IP details in reports.

## Workflow
1. Identify repository, branch, commit, target board, SoC, kernel version, build system, and relevant configuration files.
2. Trace the path from UI/configuration through init scripts and network/firewall setup to runtime services.
3. Map interfaces, bridges, VLANs, DHCP/DNS, routes, NAT, firewall chains, Wi-Fi radios, and management services.
4. For an IoT SSID, verify that it is 2.4 GHz only if requested, receives its own address pool/subnet, can reach the Internet, and cannot reach primary LAN clients or router administration services. Check IPv4 and IPv6 paths if enabled.
5. Check that established/related return traffic is handled intentionally and that DHCP/DNS needed by IoT clients still work.
6. Check startup persistence, reload behavior, configuration migration/defaults, and failure-safe behavior.
7. Run available static tests or test scripts only when safe and relevant. Do not claim a runtime test happened unless it did.
8. Report findings by severity with file paths, line numbers, evidence, assumptions, and minimal remediation options.

## Required output
- Scope and commit inspected
- Network topology and relevant files
- Findings: Critical / High / Medium / Low / Informational
- Reachability matrix (IoT -> Internet, primary LAN, router UI/SSH/DNS/DHCP)
- Tests run, exact commands, exit codes, and tests not run
- Regressions and unresolved questions

## Acceptance criteria
No claim of isolation is accepted without tracing both allow and deny paths. No claim of hardware NAT performance is accepted from source inspection alone.

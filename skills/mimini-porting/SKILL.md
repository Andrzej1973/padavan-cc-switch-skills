---
name: mimini-porting
description: Audit and develop an isolated Padavan 4.4.198 port for Xiaomi Mi WiFi Mini R1CM, preserving compatible stock features and integrating cross-band repeating plus USB RAW and LPR printing.
---

# Xiaomi Mi WiFi Mini R1CM Porting

## Purpose and project priority

Develop a separate Padavan 4.4.198 port for Xiaomi Mi WiFi Mini R1CM (MIMINI) as a secondary workstream. The primary project remains YOUHUA WR1200JS (MT7621), governed by the project's main audit instructions and the `padavan-wr1200js-audit` Skill.

Work on MIMINI only when the primary work is safely waiting (for example, on CI) or when explicitly assigned. Never let MIMINI changes delay or invalidate primary WR1200JS work.

## Isolation and safety

- Use a dedicated branch or worktree for MIMINI.
- Keep MIMINI commits, CI, artifacts, and reports separate from WR1200JS.
- Do not alter shared files while a WR1200JS CI run is pending if those changes could affect its result.
- Inspect Git status, branch, HEAD, and diffs before any edit; preserve user changes.
- Do not commit, push, merge, tag, publish, or flash without explicit authorization.
- Never flash or reboot the physical router or alter bootloader, factory data, NVRAM, or partitions without specific approval.
- Do not assume MT7621 code or drivers can be reused unchanged on MIMINI.

## Initial inventory

Before editing, inspect the current repository and existing MIMINI work. Review, where present:

- `configs.build/mi-mini.config`
- `docs/MI-MINI-AUDIT-2026-11-07.md`
- `docs/mi-mini-port-status.md`
- `docs/MI-MINI-DEPENDENCY-REVIEW.md`
- `boards.json`
- MT7620/RT2860 wireless driver candidates
- factory adapters and platform resource probes
- APCLI/STA concurrency analysis
- existing `p910nd`, LPR/LPD candidates, USB printer support, and build dependencies

Treat these files as evidence to re-check, not proof that the feature works.

Confirm the actual board revision and hardware from repository-supported evidence: SoC, RAM, flash, radio chips, USB controller/port, GPIO/platform data, boot flow, kernel configuration, and drivers. Mark unknown hardware facts as unknown; do not infer them from the product name alone.

## Preserve Padavan feature parity

The goal is a full Padavan port, not a stripped-down image containing only the new features.

1. Inventory all stock Padavan features and build options in the baseline source.
2. Trace each feature through configuration, WebUI, init scripts, kernel modules, daemons, and runtime dependencies.
3. Create and maintain `docs/MI-MINI-FEATURE-PARITY.md`.
4. Record for every feature: baseline presence, MIMINI status, hardware/software dependencies, evidence, tests, and reason for any exclusion.
5. Use these statuses consistently:
   - `PRESERVED`: compatible stock feature retained.
   - `IMPLEMENTED`: new feature implemented.
   - `PARTIAL`: incomplete implementation.
   - `BLOCKED`: a specific blocker prevents progress.
   - `NOT PORTED`: not yet ported.
   - `NOT TESTED`: no adequate validation evidence.
6. Audit, at minimum, routing, WAN/LAN, DHCP, DNS, NAT, firewall, IPv4/IPv6, Wi-Fi AP/STA/WISP, supported guest networks, WebUI/admin, VPN components, USB storage/filesystems, USB printing, logs/diagnostics, QoS, NTP, DDNS, and all other features actually present in the baseline.
7. Preserve every stock feature that is compatible with the MIMINI hardware and drivers. Do not remove a working feature merely to simplify the port.
8. If a feature cannot be ported, document the concrete hardware, driver, memory, flash, or software reason. Do not label an unverified feature as preserved.

A WebUI menu item or successful compile is not proof of runtime functionality.

## Cross-band Wi-Fi repeater

Target behavior: one integrated firmware image supports an automatic cross-band repeater, subject to verified radio/driver capability.

- Uplink on 5 GHz and AP on 2.4 GHz.
- Uplink on 2.4 GHz and AP on 5 GHz.
- Scan and select the upstream SSID; configure credentials through the existing Classic WebUI.
- Support automatic band selection and a manual override.
- Persist settings across reboot and recover after upstream signal loss.
- Report actual uplink/AP bands and connection state.

Verify APCLI/STA plus AP concurrency for the exact MT7620A/MT7612E drivers and board revision. Do not infer concurrency merely because two radio chips exist. Trace interface creation, channel constraints, driver capabilities, authentication, bridging/routing, and failure recovery. If the requested simultaneous mode is unsupported, report the exact blocker and viable alternative; do not simulate success.

## Printing: preserve stock support and add protocols

Printing is a first-class feature-parity requirement. First inspect and preserve all compatible stock Padavan print mechanisms, USB printer drivers/kernel modules, hotplug scripts, service lifecycle, WebUI controls, and USB subsystem behavior.

### USB printer detection and lifecycle

- Detect attach/detach and re-enumeration reliably.
- Handle device nodes and permissions safely.
- Start only services whose prerequisites are present.
- Stop cleanly on detach and recover on reconnect.
- Avoid races, stale processes, and conflicting access to the printer.
- Preserve USB storage and other compatible USB-device support.

### RAW printing: TCP 9100

Use the repository's compatible `p910nd` implementation when appropriate.

- Provide RAW TCP 9100 access from LAN.
- Show service/printer state in Classic WebUI.
- Start/stop with printer availability and user configuration.
- Recover from USB detach/reconnect.
- Do not expose the port to WAN by default.

### LPR/LPD: TCP 515

Add a genuine LPR/LPD implementation; `p910nd` alone is not LPR/LPD.

- Provide LPR/LPD on TCP 515 with a configured queue.
- Verify client interoperability for the supported Windows and Linux configurations.
- Implement queue/job handling and error reporting appropriate to the selected daemon.
- Integrate start/stop and USB detach/reconnect behavior.
- Expose service and queue state in Classic WebUI.
- Restrict access to LAN by default; do not open TCP 515 to WAN.

### Concurrent print paths

The intended image supports compatible stock printing, RAW 9100, and LPR/LPD 515 for the same physical USB printer.

- Establish whether the selected daemons can safely share the device or require a common spool/serialization layer.
- Prevent concurrent writers from corrupting jobs or device state.
- Avoid claiming a shared queue unless the implementation actually provides one.
- Measure RAM, flash, CPU, and dependency costs before adding a spooler or daemon.
- Test simultaneous submissions and sequential jobs, detach/reconnect, cancellation/error paths, and service shutdown.
- Keep all print ports LAN-only unless the user explicitly authorizes a different policy.

## Classic WebUI integration

Preserve the existing Padavan Classic WebUI, authentication, localization, and configuration conventions.

Add controls/status for:
- Repeater enable/disable, scan, SSID, credentials, automatic/manual band mode, actual bands, and connection state.
- USB printer presence.
- RAW TCP 9100 enable/disable and status.
- LPR TCP 515 enable/disable, queue, and status.
- Print errors and queue status where supported.

Do not create a parallel WebUI framework or break existing page IDs, form names, backend endpoints, or configuration semantics.

## Resource and security review

- Account for limited flash and RAM; quantify added binaries, libraries, modules, and runtime memory where possible.
- Prefer existing compatible components; avoid unnecessary heavyweight dependencies.
- Validate all WebUI inputs and shell arguments.
- Do not log Wi-Fi passwords or print-job contents.
- Keep RAW 9100 and LPR 515 restricted to LAN by default.
- Review IPv4 and IPv6 firewall paths.
- Ensure services do not bind to WAN interfaces unintentionally.
- Check permissions, temporary files, queue paths, and symlink/path traversal risks.

## Build and testing

Create focused host tests and static checks where possible.

Test:
- Board configuration and image composition.
- Feature-parity inventory and preservation of existing features.
- Radio interface setup and band selection logic.
- APCLI/STA concurrency assumptions against driver evidence.
- Repeater persistence and reconnect logic.
- USB attach/detach and re-enumeration.
- Stock print path, RAW 9100, LPR/LPD 515.
- Queue/job integrity and concurrent submissions.
- Firewall restrictions and WebUI validation.
- Service start/stop, reboot persistence, and failure recovery.
- Image size and runtime resource use.

Separate evidence levels: source inspection, host tests, cross-build, CI, boot test, and real hardware/network test. If the physical MIMINI is unavailable, mark hardware tests `NOT RUN`; never present host tests as hardware validation.

## Documentation and reporting

Maintain separate MIMINI records, including:
- `docs/MI-MINI-FEATURE-PARITY.md`
- `docs/mi-mini-port-status.md`
- `docs/MI-MINI-DEPENDENCY-REVIEW.md`
- focused repeater and printing test reports

Report repository/branch/commit, changed files, preserved stock features, new features, test commands/results, untested items, blockers, resource impact, and next safe step.

## Completion criteria

Do not call MIMINI complete until:
- The exact board and build target are confirmed.
- A separate MIMINI image builds and its artifact is identified.
- The feature-parity matrix is complete and all exclusions are justified.
- Compatible stock Padavan features, including USB and stock printing, are preserved.
- Repeater capability is demonstrated or a concrete hardware/driver blocker is documented.
- RAW 9100 and genuine LPR/LPD 515 are integrated and tested.
- Concurrent print behavior is validated.
- Classic WebUI and LAN-only firewall policy are verified.
- Automated and hardware test statuses are reported honestly.

Use `PASS`, `FAIL`, `NOT RUN`, and `BLOCKED` precisely. A successful build alone is not completion.

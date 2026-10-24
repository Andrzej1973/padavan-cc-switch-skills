---
name: embedded-linux-kernel-review
description: Review embedded Linux kernel and vendor-tree changes for compatibility, security, buildability, and router performance.
---

# Embedded Linux Kernel Review

## Purpose
Review kernel changes in embedded router firmware, including vendor trees and older Linux 4.4-based systems, with attention to compatibility and performance.

## Rules
- Do not recommend replacing a vendor-patched kernel with a raw upstream kernel without a complete compatibility analysis and explicit user approval.
- Preserve target-specific drivers, board support, device tree/platform data, wireless stack, switch/PHY integration, and vendor patches.
- Never claim a kernel version bump is drop-in compatible based only on version numbers.
- Do not alter or backport patches without permission when the user asked for review only.

## Workflow
1. Identify exact source tree, base commit, target SoC/board, architecture, toolchain, kernel config, and vendor patch stack.
2. Compare configuration and patch deltas; classify changes by subsystem.
3. Review API/ABI changes affecting drivers, networking, netfilter, MIPS/MT7621 platform support, filesystems, crypto, wireless, and build tools as applicable.
4. Check for required companion changes in drivers, headers, firmware blobs, userspace tools, and build scripts.
5. Assess security fixes and CVEs only against verifiable affected ranges and patch presence; avoid assuming a version number proves vulnerability or remediation.
6. Inspect memory use, stack, locking, IRQ context, DMA/cache assumptions, error paths, and lifetime/race hazards.
7. Evaluate networking performance implications, including packet paths, GRO/GSO, checksum offload, hardware NAT/flow offload, switch drivers, and PHY behavior where relevant.
8. Verify configuration compatibility and build integration for the exact target.
9. Run available compile/static tests if authorized and practical; do not claim boot/runtime validation without hardware or a faithful test environment.
10. Recommend the smallest viable backport or patch series rather than a broad kernel replacement when appropriate.

## Required output
- Compatibility verdict and confidence
- Subsystem-by-subsystem findings with file/line evidence
- Build and test evidence
- Security notes with verified sources when discussing current CVEs
- Performance risks and expected tradeoffs
- Missing hardware validation
- Minimal remediation plan

## Acceptance criteria
Separate source-level plausibility, successful compilation, boot validation, and real traffic/performance testing as distinct evidence levels.

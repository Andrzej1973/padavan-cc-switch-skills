# Padavan Firmware Skills Pack for CC Switch / Codex

Ten project-focused agent Skills for auditing and developing Padavan router firmware safely.

## Included Skills

- `padavan-network-audit` — networking, firewall, Wi-Fi and IoT isolation audit
- `firmware-build-verify` — CI build and firmware artifact verification
- `firmware-regression-review` — regression review across networking, UI and build integration
- `padavan-neon-ui` — Neon-style router web UI implementation/review
- `preflash-release-gate` — evidence-backed pre-flash GO/NO-GO gate
- `safe-git-workflow` — preserve user changes and use auditable Git operations
- `embedded-linux-kernel-review` — embedded/vendor kernel compatibility review
- `context-efficiency` — reduce redundant context and repeated work without compromising verification
- `padavan-wr1200js-audit` — primary YOUHUA WR1200JS (MT7621) audit and release-readiness workflow
- `mimini-porting` — isolated Xiaomi Mi WiFi Mini R1CM port with stock feature parity, repeater, and USB printing

## Layout

Each skill is a direct child of `skills/` and contains `SKILL.md`, the standard Agent Skills entry point.

```text
padavan-cc-switch-skills/
├── README.md
└── skills/
    ├── padavan-network-audit/SKILL.md
    ├── firmware-build-verify/SKILL.md
    ├── firmware-regression-review/SKILL.md
    ├── padavan-neon-ui/SKILL.md
    ├── preflash-release-gate/SKILL.md
    ├── safe-git-workflow/SKILL.md
    ├── embedded-linux-kernel-review/SKILL.md
    ├── context-efficiency/SKILL.md
    ├── padavan-wr1200js-audit/SKILL.md
    └── mimini-porting/SKILL.md
```

## Add this repository in CC Switch

1. Open **Skills → Repository Management → Add Repository**.
2. Enter the repository details below.
3. Refresh the Skills discovery list and install the Skills you want for Codex.

| Setting | Value |
|---|---|
| Owner | `Andrzej1973` |
| Repository | `padavan-cc-switch-skills` |
| Branch | `main` |
| Subdirectory | `skills` |

The new Skills become available from the `main` branch after the pull request adding them has been merged.

## Safety design

- Skills default to read-only inspection and require explicit permission for changes where appropriate.
- No Skill authorizes flashing a physical router.
- The pre-flash gate distinguishes CI/build evidence from actual hardware validation.
- Skills explicitly preserve WAN port remapping and hardware NAT/flow offload unless the user approves a change.

## Validation note

These are agent instruction files, not independently validated firmware tooling. They guide an agent; they do not themselves run tests or guarantee correctness. Review each Skill before using it in a production workflow.

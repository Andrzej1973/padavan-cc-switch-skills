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

## Add as a repository in CC Switch
1. Publish this folder as a **public GitHub repository** (instructions below).
2. In CC Switch, open **Skills → Repository Management → Add Repository**.
3. After the changes are merged into `main`, enter your GitHub owner, repository name, branch `main`, and subdirectory `skills`.
4. Refresh the Skills discovery list, then install the skills you want for Codex.

CC Switch's repository manager uses the owner/name/branch/subdirectory fields. The intended values are:
- Owner: `<your-github-username>`
- Name: `padavan-cc-switch-skills`
- Branch: `main`
- Subdirectory: `skills`

## Publish on GitHub (Windows)
This repository contains the Skills pack. New additions should be merged into `main` before selecting `main` in CC Switch. No firmware repository is changed by this Skills pack.

1. Download and extract `padavan-cc-switch-skills.zip`.
2. Open PowerShell in the extracted `padavan-cc-switch-skills` folder.
3. Create a **new empty public repository** named `padavan-cc-switch-skills` in your GitHub account using GitHub's website. Do not initialize it with a README, license, or `.gitignore` because this folder already contains a README.
4. Run:

```powershell
git init
git branch -M main
git add README.md skills
git diff --cached --check
git status --short
git commit -m "Add Padavan firmware agent skills"
git remote add origin https://github.com/<YOUR_GITHUB_USERNAME>/padavan-cc-switch-skills.git
git push -u origin main
```

Replace `<YOUR_GITHUB_USERNAME>` with your GitHub username. Review `git status` before committing. These commands only publish this new skills folder; they do not touch your firmware repository.

## Safety design
- Skills default to read-only inspection and require explicit permission for changes where appropriate.
- No skill authorizes flashing a physical router.
- The pre-flash gate distinguishes CI/build evidence from actual hardware validation.
- Skills explicitly preserve WAN port remapping and hardware NAT/flow offload unless the user approves a change.

## Validation note
These are newly authored starter Skills, not independently validated firmware tooling. They guide an agent; they do not themselves run tests or guarantee correctness. Review before using them in a production workflow.

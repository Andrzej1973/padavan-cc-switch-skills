---
name: padavan-neon-ui
description: Design and review a Padavan web UI in the established Neon visual style while preserving usability and compatibility.
---

# Padavan Neon UI

## Purpose
Implement or review UI changes using the project's agreed Neon style without breaking Padavan's existing web interface conventions.

## Rules
- First inspect existing CSS, templates, JavaScript conventions, localization, and the previously agreed Neon implementation. Reuse existing tokens/components instead of inventing a parallel design system.
- Do not replace broad UI files or reformat unrelated code.
- Do not change networking behavior as a side effect of a visual change.
- Avoid remote font, script, or CSS dependencies unless explicitly approved; router UI should work offline.
- Keep the interface usable on desktop and mobile, readable in light/dark conditions supported by the project, and accessible with keyboard/focus states.

## Workflow
1. Identify the actual page/template, shared styles, JS, localization files, and build integration.
2. Establish existing visual tokens: background, panels, text, accent, borders, focus, status, spacing, radius, and typography.
3. Match the established Neon style: restrained luminous accents, clear hierarchy, readable contrast, and subtle effects rather than excessive glow.
4. Preserve existing form names, element IDs, backend endpoints, config variables, and submit semantics.
5. Validate enabled/disabled/loading/error states, labels, units, help text, and validation feedback.
6. Escape untrusted dynamic text; do not inject raw configuration values into HTML.
7. Keep CSS scoped to the target page/component and avoid broad selectors that alter unrelated router pages.
8. Run available lint/build/UI checks and inspect generated assets or screenshots if available.

## Required output
- Files changed or reviewed
- Visual decisions and reused tokens
- Behavior preserved
- Accessibility/responsiveness checks
- Tests and limitations

## Acceptance criteria
A visual change must not alter configuration semantics, service behavior, Wi-Fi band selection, firewall policy, or persisted settings.

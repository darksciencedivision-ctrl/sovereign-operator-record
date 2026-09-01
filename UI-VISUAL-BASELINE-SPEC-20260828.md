# UI VISUAL BASELINE SPEC — N-14 · 2026-08-28
## Target: the operator-approved capture `design/UI-TARGET-20260828.png` (shell build 2026-08-21 theme)

The operator has designated the attached capture as the required look of the shell UI. Builder task: make the candidate worktree shell (HEAD lineage) render in visual parity with this target. First step is a determination: render the worktree shell and screenshot-compare — if it already matches (the theme may be present in the baseline; THEME-BASELINE-v3.md and the Gate-5/5b visual set live in the tree), the item closes as a verified visual-parity check; if it diverges, implement to this spec.

## Layout (from the capture)
Top chrome: left brand block — diamond glyph logo, "SOVEREIGN WORKSPACE" over "SYSTEMS CONTROL" (small caps, letterspaced). Center breadcrumb bar: `SWS › SWS-UI-001 v1.2 › build <date> › <HOSTNAME>` in a pill container. Right: DISCOVERY · THEME-BASELINE · directive links + live clock (HH:MM:SS).
**PRE-FLIGHT panel** (full width, first): dependency rows with status dots — Ollama (model count), py -3.12 (version), node (version), npm, ports 5175/8700/5180 (free/shell) — plus a right-aligned summary "N/7 available · N unavailable". Dot colors: green ok, red failed, amber external.
**Module grid**: two columns. Cards: SOVEREIGN | SOW (row 1), Token Center full-width or paired (row 2), Debate Table | Distillery (row 3). Each card: name (caps, monospace), one-line description, status line with colored dot + label (green Ready, gray Stopped, red "Failed: EXIT", amber "External (not shell-owned)"), Last Check time, Endpoint URL, button row [Start·Stop·Restart·Open·Run Startup Test·Logs] — primary-action buttons filled accent blue, secondary outlined. Distillery card additionally: Snapshot id, Open questions (linked OQ ids), Handoff docs links, runtime status line in italic.
**LOGS panel** (full width, bottom): header "LOGS", right-aligned "no module selected" placeholder.
Footer: left `SWS-UI-001 v1.2 – Sovereign Workspace Shell`, right `local dashboard – no external assets`.

## Style tokens (approximated from the capture; builder verifies against THEME-BASELINE-v3.md before inventing values)
Background: near-black navy (#05080f-ish) with a subtle starfield/earth-limb image occupying the left/edges behind panels. Panels: translucent dark slate (#0b1220 ~85% opacity), 1px faint border, ~8px radius. Type: monospace family throughout (matches existing shell). Accent: azure blue (≈#2f6bff) for primary buttons/links/brand. Status: green ≈#38d97a, red ≈#ff4d4d, amber ≈#e8a33d, muted gray for Stopped. High contrast maintained (existing contrast evidence h16 applies — do not regress it).

## Hard constraints (non-negotiable)
1. **No external assets** — the footer says it and CSP enforces it: the background image ships as a local static file (or embedded data URI) served by the shell itself; `default-src 'self'` CSP unchanged; no fonts/scripts/images from any network host.
2. No inline scripts/styles that would weaken the existing CSP; keep the current security-header posture byte-for-byte.
3. No backend/route changes ride along — this is presentation only; any JS change is confined to rendering.
4. Dark theme is the shipped look (launcher already enforces OS dark mode); light-variant behavior per existing THEME-BASELINE rules.
5. Keyboard navigation and the rendered-DOM test surface (Gate-5b visual set) must still pass; add/refresh the visual regression evidence with before/after screenshots.
6. The pre-flight npm probe failure visible in the target capture is NOT part of this item — it is punch item N-15 (detection correctness), fixed separately so this item stays presentation-only.

## Acceptance
Side-by-side screenshot of the worktree shell vs `design/UI-TARGET-20260828.png` at 1440-class width: same structure, same status vocabulary and colors, no external requests in the network log, CSP headers unchanged, shell suite still green. Evidence into the item bundle; CANDIDATE for reviewer.

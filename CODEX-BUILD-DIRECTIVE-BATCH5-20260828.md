# CODEX WORK DIRECTIVE — BATCH 5 · SWS-REM-DIR-20260828
## Builder: codex · ENTRY 005 (log SHA-256 after recording: `9732a4b631302f25294283c2b5f3e463…`) · zero placeholders

**Operator:** paste between the markers into Codex, working directory `D:\producttion software 2\`, full file access.

---BEGIN PASTE---

You are the BUILDER seat `codex (local harness)`, executing Batch 5 under SWS-REM-DIR-20260828 R2 + Annex A, authorized by OPERATOR-INSTRUCTIONS.log ENTRY 005 (`OPEN ... BATCH-5: OP-1 OP-3 OP-4 N-13b + OP-2(dossier-only)`). **All standing rules R-1..R-10 from the Batch 4-E R2 directive apply verbatim** (read `CODEX-BUILD-DIRECTIVE-BATCH4EXT-R2-20260828.md` §1 first): write set, encoding rules, no invented APIs, coupled values, shell-suite side-effect restoration, test hygiene, minimal diffs, per-item discipline, park-don't-improvise, no network/spend/model-loads. CANDIDATE only, no reviewer PASS, no operator questions.

BOOT: verify ENTRIES 001–005 in the log; directive hash `9061c2e8…`; worktree HEAD `a945d997617b43251d600ef63ac459d2f72e01fb`, clean porcelain. Read `PUNCH-LIST-V3-20260828.md` Lane OP. Mismatch → BOOT-FAILURE.md, stop.

## QUEUE — strict order; the manifest/gate item is LAST

### 1 · N-13b — execute the adapter rebase (carried from Batch 4-E)
`tools/release/rebase_adapters.py` refused atomically because the three llama.cpp target paths have never existed. Add per-adapter optional handling: adapters listed in a new `"optional_adapters"` array in `shell/config/install.json` (populate with `["llamacpp"]`) are SKIPPED-WITH-RECORD when their targets are absent, never batch-fatal; non-optional adapters keep the hard refusal. Update `test_rebase_adapters.py` for the skip path. Then EXECUTE the rebase against this worktree. Post-conditions captured: `rg "Product Software|Users/Sslaw" shell/modules/*.json` → hits only in `llamacpp.json` (skipped, recorded) and `.pre-rebase` backups; second run = no-op; tests green. NOTES restate: module venvs still unprovisioned (T-2/E-3) — paths correct ≠ modules startable. Commit.

### 2 · OP-3 — SOW terminal cap: 5 → minimum 8
Locate the concurrent-pane/session cap (R-3: read the source — likely a constant or policy in `modules/sow/apps/desktop/main.js` or the session/registry layer; `rg -n "\b5\b"` is not a method — find the named limit). Fail-before: cite the cap's line and its enforcement. Raise the limit to 8 (if the cap interacts with layout logic, supervision slots, or recovery snapshots, those coupled sites move in the same commit, R-4, each listed). Pass-after: a test creating 8 concurrent sessions succeeds and a 9th behaves per the (possibly retained) new cap's design; desktop `node --test` suite green (measured); falsification-harness re-pin if main.js changed (follow its documented protocol, R-4). Commit.

### 3 · OP-4 — Token Center embedded live in the shell UI
Goal: the shell's Token Center section renders the RUNNING app (loopback iframe of `http://127.0.0.1:8765/`) in place, not a click-out; when Token Center is down, a graceful fallback panel (status + Start button) — never a broken frame.
Security constraints, non-negotiable: (a) shell CSP gains exactly `frame-src http://127.0.0.1:8765` — nothing wider; every other directive byte-identical (prove by header diff). (b) Token Center currently forbids framing (M-1 hardening) — grant a SCOPED exception: its anti-framing header(s) must allow ancestor `http://127.0.0.1:5180` ONLY (CSP `frame-ancestors http://127.0.0.1:5180`; remove any blanket `X-Frame-Options: DENY` in favor of that directive), all other M-1 protections untouched — its 31-test suite must stay green, with a NEW test asserting framing is allowed from 5180's origin form and that the CSRF/token flow still works when framed (the embedded app fetches its own same-origin token — verify the client path doesn't depend on top-level context). (c) No shell backend route changes beyond serving the updated page. Fail-before: current markup (click-out) + TC's framing refusal captured. Pass-after: template/CSS diff, header diffs both sides, shell suite green (R-5 restoration applied), TC suite green + new framing test. Commit.

### 4 · OP-1 — proper launcher
The interim `Sovereign Workspace.bat` (custody root) is the operator's stopgap. Product version: `tools/release/install_shortcut.ps1` (NEW) — creates a Desktop + Start-Menu shortcut via WScript.Shell COM targeting `powershell.exe -NoProfile -ExecutionPolicy Bypass -File "<worktree>\Start-Shell.ps1"`, working dir = worktree root, window style normal, icon from a NEW local `shell/static/sovereign.ico` (generate from the existing brand glyph asset in the shell's static tree; if none exists as ico, convert the logo SVG/PNG already shipped — local assets only). Idempotent (re-run replaces). This script is INSTALL TOOLING: it writes shortcuts to the user's Desktop/Start Menu when the OPERATOR runs it — you do NOT run it (R-1 write set); you test it with a `-TargetDir` override pointed INSIDE the worktree temp area and assert the .lnk is created with correct fields (read back via COM). Document one-line usage in NOTES for the operator. Commit.

### 5 · OP-2 — SOVEREIGN frontier-provider DESIGN DOSSIER (no code)
Write `bundles\BATCH5\OP-2\FRONTIER-PROVIDER-DOSSIER.md`: (1) current state — SOVEREIGN's model list source (SYSTEM_MANIFEST.json authority per M-6), its loopback-only Ollama client and why frontier providers break that boundary; (2) candidate mechanism — reuse SOW's existing provider-CLI adapters (`modules/sow/adapters/frontier/{claude_code,codex,grok_build}.py`) as subprocess providers, mapped into SOVEREIGN roles, vs. a direct-API alternative — trade-offs of each; (3) the decisions only the operator can make: spend authority (OD-5 currently DENY — this feature is inert without a new grant), key custody (no credentials in logs/artifacts — cite the packaging gate classes), role eligibility (which SOVEREIGN roles may use hosted models vs must stay local), and evidence/telemetry routing (Token Center already collects for these providers — cite its collector matrix); (4) a proposed implementation plan sized in commits, executable in a future batch after those rulings. NO product bytes change for this item. Bundle only.

### 6 · LAST — manifest/gate closure (standing rule)
Regenerate module provenance + RELEASE-MANIFEST.json against the final tree (sow bytes changed in OP-3; tokencenter in OP-4; shell in 1/3/4). Re-run ALL gates per the Batch 4-E §2.9c invocation list + the release-tool aggregate; everything green or fix-within-scope and regenerate again. Commit.

## OUTPUT
`bundles\BATCH5\<id>\` per item; `bundles\BATCH5\BATCH-REPORT.md` (items, commits, measured counts, T-2 additions: operator runs `install_shortcut.ps1`; live embed check of OP-4 in the running shell; OP-2 rulings needed); append a BATCH-5 section to `bundles\LOOP-RUN-REPORT.md`. Clean porcelain, no FF FE, end reports with:
`BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`

Execute BOOT now, then the queue in order. Stop after item 6.

---END PASTE---

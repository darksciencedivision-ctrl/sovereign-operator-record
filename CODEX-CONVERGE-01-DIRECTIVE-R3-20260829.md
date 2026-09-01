# CODEX WORK DIRECTIVE — CONVERGE-01 · REVISION R3
## SWS-REM-DIR-20260828 · builder: codex · ENTRY 006 + ENTRY 007 · supersedes R2 (kept as history)
## R3 adds: phase C0 (adopt-and-verify the in-progress UI work), N-16 (runtime evidence dirties the tree), and a BOOT rule that distinguishes contamination from work

**Operator:** paste between the markers into Codex, working directory `D:\producttion software 2\`, full file access. Long run.

---BEGIN PASTE---

You are the BUILDER seat `codex (local harness)`, executing CONVERGE-01 (R3) under SWS-REM-DIR-20260828 R2 + Annex A, authorized by OPERATOR-INSTRUCTIONS.log ENTRY 006 as amended by ENTRY 007. Your prior BOOT refusal on the dirty worktree was CORRECT and is recorded as such; ENTRY 007 adjudicates that state and grants phase C0 to absorb it. Run-to-convergence: no stops between items/phases; park-and-continue per E-1..E-6; exit only when the §CONVERGENCE CHECKLIST has no blank row. **Standing rules R-1..R-10 from `CODEX-BUILD-DIRECTIVE-BATCH4EXT-R2-20260828.md` §1 apply verbatim.** CANDIDATE only; no reviewer PASS; no operator questions.

### Network grant (unchanged from R2)
ONLY `registry.npmjs.org`, `pypi.org` + `files.pythonhosted.org`, and the Electron dist host used by the pinned `electron` package. Installs are LOCK-DRIVEN ONLY: `npm ci` (never `npm install`); pip from the module's resolved lock (`--require-hashes` where the lock carries hashes, else the lock's pinned `==` versions, recording that hashes were absent). No install from any URL, git ref, or local path not named in a committed lock. No billed APIs, no provider calls, no model downloads, no other hosts.

### BOOT (R3 — three-way, not two-way)
Verify: ENTRIES 001–007 in the log; directive `9061c2e8…`; annex `acae8fb8…`; HEAD `a945d997617b43251d600ef63ac459d2f72e01fb`. Then classify the working tree against ENTRY 007's inventory:
- **Expected-adoptable** (ENTRY 007 names these): modified `RELEASE-MANIFEST.json`, `shell/BUILD-MANIFEST.txt`, `shell/static/app.css`, `shell/static/workspace-bg.svg`; untracked `shell/static/workspace-bg-v2.png` and `evidence/startup-tests/tokencenter-*.json`. → proceed to C0.
- **Anything else modified or untracked** → BOOT-FAILURE.md naming the unexpected paths, stop. (Unknown changes are still a hard stop; only ENTRY 007's named set is pre-adjudicated.)
- Note: `git status` on this mount can be slow — always allow it to complete; an empty result from a killed command is NOT a clean tree.
Write `bundles\CONVERGE01\CHECKPOINT.json` = `{"phase":"BOOT","head":"a945d99","completed":[]}`. **Resume rule:** if CHECKPOINT.json already records completed phases, verify each recorded commit exists and re-enter at the first incomplete phase.

## PHASE C0 — adopt-and-verify the in-progress work + N-16 (NEW, runs first)
1. **Inventory & classify** every dirty path into: (a) UI work product, (b) manifest artifacts, (c) product-generated runtime evidence. Record hashes and mtimes. Bundle: `bundles\CONVERGE01\C0\`.
2. **Verify the UI work against the N-14 hard constraints before adopting it** (`UI-VISUAL-BASELINE-SPEC-20260828.md`): zero external references in `app.css`, `workspace-bg.svg`, and any asset they pull (validator pre-check found `app.css:70 → url("/static/workspace-bg-v2.png")`, local — confirm independently and completely); CSP and security headers byte-identical to HEAD (diff the header-emitting code paths and prove it); presentation-only (no backend/route/JS-behavior changes hiding in the diff — read the full diff, don't skim); contrast/keyboard behavior not regressed. **If any constraint fails, do NOT adopt that file: park it with a dossier and restore that path from HEAD.** Record the 1,524,053-byte PNG's SHA-256 as a served asset.
3. **N-16 fix (product defect):** the shell writes `evidence/startup-tests/*.json` into the tracked tree at runtime, so normal product use dirties the release candidate and BOOT-fails every future run. Relocate that write target to a gitignored runtime lane (preferred — read the writer first, R-3) and/or ignore the path; then extend `.gitignore` accordingly and add a package-boundary-gate assertion that runtime-evidence classes never enter an archive. The four existing artifacts are product output, not source: preserve them into the C0 bundle as evidence, then remove them from the worktree. Regression test: exercising the startup-test path leaves `git status` clean.
4. **Adopt:** commit the verified UI work (`app.css`, `workspace-bg.svg`, `workspace-bg-v2.png`) as `C0 N-14b: adopt in-progress visual baseline work` with the constraint-verification evidence; commit the N-16 fix separately. Do NOT hand-adopt `RELEASE-MANIFEST.json`/`BUILD-MANIFEST.txt` — restore both from HEAD and let C6/manifests-last regenerate them from final bytes (that is the whole point of the standing rule).
5. Shell suite green (measured, R-5 restoration applied) before leaving C0. Checkpoint.

## PHASE C1 — Batch-5 feature queue (specs verbatim from `CODEX-BUILD-DIRECTIVE-BATCH5-20260828.md`)
N-13b (optional-adapter skip + EXECUTE the rebase — ENTRY 007 notes the captured startup-test argv still targets `D:/Product Software/...`, independent confirmation the rebase has not run) · OP-3 (terminal cap → min 8) · OP-4 (Token Center embedded live; scoped `frame-src`/`frame-ancestors` exactly as specified) · OP-1 (`install_shortcut.ps1` + local icon; you do NOT run it — test via `-TargetDir` inside the worktree temp) · OP-2 (FRONTIER-PROVIDER-DOSSIER, bundle only, no code). One commit per item; `bundles\CONVERGE01\C1-<id>\`. Checkpoint.

## PHASE C2 — environment provisioning
1. **Disk preflight:** require ≥12 GB free on the worktree volume; short → park all of C2 with the measured figure and continue to C3. Record free space.
2. **Locks are read-only inputs:** read each module's lock first; a missing, partial, or self-inconsistent lock → PARK that module's provisioning with a dossier; NEVER regenerate or edit a lock. No tracked lockfile bytes change in this phase.
3. Provision into gitignored env dirs, lock-driven per the network grant: SOVEREIGN `.venv` (WORKSPACE-RESOLVED-LOCK), Debate `.venv`, SOW Python env (supplies jsonschema — unblocks the full pytest suite and the parked T2 leg), Distillery `.venv`, Token Center (stdlib, verify only), node deps via `npm ci`. Capture installer output, `pip freeze`/`npm ls`, and a version-vs-lock diff; ANY drift → park-with-dossier, never silent acceptance.
4. **Supply-chain disposition:** `npm audit` / `pip-audit` (or the equivalent the tool actually supports — R-3) per environment; critical/high → fix only if the lock permits, else park as a NAMED accepted risk for the release notes. Re-run the secrets scan tree-wide. Confirm every env dir is gitignored (extending `.gitignore` is the only tracked change C2 may commit).
5. Per-module import/self-check needing only its venv (no services, no models). Checkpoint.

## PHASE C3 — release identity
1. `LICENSE` (root + per module): `Copyright (c) 2026 Samuel Lawson / Dark Science Division. All rights reserved.` + `Interim license record; canonical license text to be supplied by the operator before external distribution.`
2. `THIRD-PARTY-NOTICES.md` (root): bundled OSS dependencies + licenses, built from C2 lock/audit data.
3. `VERSION.json` (root): `{"system":"sovereign-workspace","version":"1.0.0-rc.1","source_commit":"<filled at C6>","modules":{…from RELEASE-MANIFEST…}}`. `rc.1` because ratification has not occurred; plain `1.0.0` is minted by the operator at promotion, never by you.
4. `SBOM.json` (CycloneDX-style or a documented equivalent built from locks + C2 snapshots + a native-binary inventory with SHA-256 for every Electron/node-pty/`.exe`/`.node`/`.pyd`/`.dll`).
5. Per-module release archives: `git archive` each module path at HEAD → `release-artifacts/<module>-<version>-src.zip` + `.sha256` + annotated tag `rel/<module>/<version>`. Debate: confirm its pin equals the existing `d03ba417…` identity; do not re-cut its upstream artifact.
6. **Rollback of record:** do not manufacture one — name the frozen RESET baseline `SOVEREIGN_RESET_BASELINE_20260827T193540Z.zip` (`00ae9eef…`) as the previous-baseline rollback artifact in the release notes.
7. Commit; `release-artifacts/` gitignored, hashed at C6. Checkpoint.

## PHASE C4 — install path + self-host PROXY test
Build under `tools/release/`: `build_release.ps1`, `install.ps1 -Dest <dir> [-TargetDir <shortcut dir>]`, `verify_install.ps1 -Dest`, `uninstall.ps1 -Dest`. `install.ps1` writes an **install manifest** listing every path it creates, including out-of-Dest shortcuts; `uninstall.ps1` removes exactly those and proves the manifest is fully consumed — the honest claim is "removes what it recorded."
**Self-host proxy test** into `D:\producttion software 2\install test area\` (spaces, empty), `-TargetDir` inside that area: build → install → verify → uninstall, captured; the installed shell boots and serves `/` from the installed location (loopback GET, clean stop). **State the limitation explicitly:** this shares the host's Python/node/Ollama/global state, so it proves install/verify/uninstall mechanics and layout, NOT clean-room independence — a fresh-VM clean-room install is a RESIDUAL reviewer/T-2 item. Clean up the test area after evidence capture. Checkpoint.

## PHASE C5 — deterministic program at final bytes
After the LAST code/tooling commit, with measured counts and a per-module determinism label:
- DETERMINISTIC-HERMETIC: SOW full pytest (now provisioned — record measured vs the 2382/0/1 host-coupled contract), SOW desktop `node --test`, terminal suite, shell suite (R-5 restoration), tokencenter, release-tool tests, all gates.
- Debate / SOVEREIGN / Distillery: run ONLY legs that pass from their venv with NO Ollama models and NO live services; every model- or service-dependent leg PARKS as E-6 with that exact cause. Do not load a model; do not claim coverage you did not run.
Zero unexplained warnings/orphans or park-with-dossier. Any failure traced to C0–C4 changes is fixed within scope and C5 re-runs from the top. Checkpoint.

## PHASE C6 — seal (manifests last)
Regenerate provenance + `RELEASE-MANIFEST.json` (now also enumerating LICENSE / THIRD-PARTY-NOTICES / VERSION / SBOM / release-artifacts / the adopted UI assets). Fill `VERSION.json` `source_commit`. Re-cut the two candidate ZIPs from `git archive` of the final commit into `release-artifacts/` (+ sidecars), cold-extract and hash-verify each. Build `REVIEW-PACKAGE-CONVERGE01.zip` (patches since `a945d99`, all CONVERGE01 bundles minus >5 MB files, custody docs) + sidecar. Write `RELEASE-NOTES-DRAFT.md` (versions; changes since baseline; rollback artifact; EVERY accepted limitation/park by name: audit-accepted risks, Grok/G28 no-spend, Distillery status-only + deferred live testing + OP-6 UI, H-5/OD-20, MT-09, G26, live-acceptance legs, OP-2 rulings, upstream W-4, canonical license text pending, true clean-room pending). Write `CONVERGE-REPORT.md` (commit chain, per-phase results, measured totals, filled checklist). Final commit; append CONVERGE-01 to `bundles\LOOP-RUN-REPORT.md`.

## CONVERGENCE CHECKLIST (mechanical exit gate)
Every row must read DONE-CANDIDATE, PARKED(reason), or DEFERRED(operator) — no blanks:
`[ ] C0 UI adoption  [ ] C0 N-16 fix  [ ] C1 N-13b  [ ] C1 OP-3  [ ] C1 OP-4  [ ] C1 OP-1  [ ] C1 OP-2-dossier  [ ] C2 provisioning(all modules)  [ ] C2 supply-chain disposition  [ ] C3 LICENSE+notices  [ ] C3 VERSION+SBOM  [ ] C3 archives+tags  [ ] C4 install tooling  [ ] C4 self-host proxy test  [ ] C5 all deterministic suites  [ ] C5 gates green  [ ] C6 manifests regenerated  [ ] C6 ZIPs re-cut+cold-verified  [ ] C6 review package  [ ] C6 release-notes draft`
Any PARKED/DEFERRED row names its blocker and appears in the report's residual queue — which must contain ONLY reviewer verdicts, operator physical acts/rulings, and operator-deferred items. A blank row, or a residual item that is builder-hermetic, means you are NOT converged: loop back and build it.

End every report with: `BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`
Execute BOOT now. Run C0→C6 to convergence, then stop.

---END PASTE---

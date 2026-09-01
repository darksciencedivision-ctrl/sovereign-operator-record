# CODEX WORK DIRECTIVE — CONVERGE-01 · REVISION R2 (hardened)
## SWS-REM-DIR-20260828 · builder: codex · ENTRY 006 · supersedes CONVERGE-01 R1 (kept as history)
## Hardening rationale: CONVERGE-01-HARDENING-REVIEW-20260828.md (W-1..W-10)

**Operator:** paste between the markers into Codex, working directory `D:\producttion software 2\`, full file access. Long run.

---BEGIN PASTE---

You are the BUILDER seat `codex (local harness)`, executing CONVERGE-01 (R2) under SWS-REM-DIR-20260828 R2 + Annex A, authorized by OPERATOR-INSTRUCTIONS.log ENTRY 006. Run-to-convergence: no stops between items/phases; park-and-continue per E-1..E-6; exit ONLY when the §CONVERGENCE CHECKLIST has no blank row. **Standing rules R-1..R-10 from `CODEX-BUILD-DIRECTIVE-BATCH4EXT-R2-20260828.md` §1 apply verbatim.** CANDIDATE only; no reviewer PASS; no operator questions.

### Network grant (ENTRY 006, tightened per W-1)
Network is permitted ONLY to: `registry.npmjs.org`, `pypi.org` + `files.pythonhosted.org`, and the Electron dist host used by the pinned `electron` package. Installs are LOCK-DRIVEN ONLY: `npm ci` (never `npm install`), and pip from the module's resolved lock (`--require-hashes` when the lock carries hashes; otherwise pinned `==` versions from the lock, and record that hashes were absent). NO install from any URL, git ref, or local path not already named in a committed lock. No billed APIs, no provider calls, no model downloads, no other hosts. A registry refusal or a host outside this list is E-1-adjacent: park the affected item, continue.

### BOOT
ENTRIES 001–006 in the log; directive `9061c2e8…`; annex `acae8fb8…`; worktree HEAD `a945d997617b43251d600ef63ac459d2f72e01fb`, clean porcelain. Read `PUNCH-LIST-V3-20260828.md` Lane OP, `CODEX-BUILD-DIRECTIVE-BATCH5-20260828.md` (C1 item specs), and `CONVERGE-01-HARDENING-REVIEW-20260828.md`. Write `bundles\CONVERGE01\CHECKPOINT.json` = `{"phase":"BOOT","head":"a945d99","completed":[]}`. Mismatch → BOOT-FAILURE.md, stop. **Resume rule:** if CHECKPOINT.json already shows completed phases from a prior run, verify each recorded commit exists and re-enter at the first incomplete phase — do not redo completed phases.

## PHASE C1 — Batch-5 feature queue (specs verbatim from the Batch-5 directive)
N-13b (optional-adapter skip + EXECUTE rebase) · OP-3 (terminal cap → min 8) · OP-4 (Token Center embedded live; scoped CSP `frame-src`/`frame-ancestors` exactly as the Batch-5 spec states) · OP-1 (`install_shortcut.ps1` + local icon; you do NOT run it — test via `-TargetDir` inside the worktree temp) · OP-2 (FRONTIER-PROVIDER-DOSSIER, bundle only). One commit per item; `bundles\CONVERGE01\C1-<id>\`. Checkpoint.

## PHASE C2 — environment provisioning (W-1,2,3,9)
1. **Disk preflight:** require ≥ 12 GB free on the worktree volume; short → park all of C2 with the measured figure, continue to C3 (which does not need venvs). Record free space.
2. **Locks are read-only inputs (W-2):** for each module read its lock/requirements FIRST. A missing, partial, or self-inconsistent lock → PARK that module's provisioning with a dossier; NEVER regenerate or edit a lock. No tracked lockfile byte changes in this phase.
3. Provision into gitignored env dirs only, lock-driven per the network grant: SOVEREIGN `.venv` (WORKSPACE-RESOLVED-LOCK), Debate `.venv`, SOW Python env (supplies jsonschema — unblocks full pytest + the parked T2 leg), Distillery `.venv`, Token Center (stdlib — verify only), node deps via `npm ci` per committed package-lock. Capture installer output + `pip freeze`/`npm ls` + a version-vs-lock diff; ANY drift from lock → park-with-dossier, don't accept silently.
4. **Supply-chain disposition (W-3):** run `npm audit` / `pip-audit` (or equivalent the tool actually supports — R-3) per environment; critical/high → fix only if the lock permits, else park as a NAMED accepted risk for release notes. Re-run the secrets scan across the tree. Confirm every env dir is gitignored (extend `.gitignore` only if a path escapes — that's the sole tracked change C2 may commit).
5. Per-module import/self-check that needs only its venv (no services, no models). Checkpoint.

## PHASE C3 — release identity (W-10, + third-party notices)
1. `LICENSE` (root + per module): `Copyright (c) 2026 Samuel Lawson / Dark Science Division. All rights reserved.` + `Interim license record; canonical license text to be supplied by the operator before external distribution.`
2. `THIRD-PARTY-NOTICES.md` (root): enumerate bundled OSS dependencies + their licenses from the C2 lock/audit data — an all-rights-reserved product that ships OSS deps MUST carry these notices.
3. `VERSION.json` (root): `{"system":"sovereign-workspace","version":"1.0.0-rc.1","source_commit":"<filled at C6>","modules":{…from RELEASE-MANIFEST…}}`. `rc.1` because ratification has not occurred; the plain `1.0.0` identity is minted by the operator at promotion, never by you.
4. `SBOM.json` (CycloneDX-style or a documented equivalent you build from lock + C2 snapshots + a native-binary inventory with SHA-256 for every Electron/node-pty/`.exe`/`.node`/`.pyd`/`.dll` in the tree).
5. Per-module release archives: `git archive` each module path at HEAD → `release-artifacts/<module>-<version>-src.zip` + `.sha256` + annotated tag `rel/<module>/<version>`. Debate: confirm its pin equals the existing `d03ba417…` identity in notes; do not re-cut its upstream artifact.
6. **Rollback of record (W-10):** do NOT manufacture a rollback package — name the frozen RESET baseline `SOVEREIGN_RESET_BASELINE_20260827T193540Z.zip` (`00ae9eef…`) as the previous-baseline rollback artifact in the release notes.
7. Commit (LICENSE, THIRD-PARTY-NOTICES, VERSION.json, SBOM, tags; `release-artifacts/` gitignored, hashed at C6). Checkpoint.

## PHASE C4 — install path + self-host PROXY test (W-4, W-5)
Build under `tools/release/`: `build_release.ps1` (artifact set from a clean `git archive`), `install.ps1 -Dest <dir> [-TargetDir <shortcut dir>]`, `verify_install.ps1 -Dest`, `uninstall.ps1 -Dest`. `install.ps1` writes an **install manifest** listing EVERY path it creates, including out-of-Dest shortcuts (W-5); `uninstall.ps1` removes exactly the manifest's paths and proves the manifest is fully consumed — the honest claim is "removes what it recorded," not "touched nothing outside Dest."
**Self-host proxy test** into `D:\producttion software 2\install test area\` (spaces, empty), with `-TargetDir` pointing INSIDE that area so shortcuts stay contained: build → install → verify → uninstall, all captured; installed shell boots and serves `/` from the installed location (loopback GET, clean stop). **State the limitation explicitly (W-4):** this shares the host's Python/node/Ollama/global state and therefore PROVES the install/verify/uninstall mechanics and layout, and does NOT prove true clean-room independence — a fresh-VM clean-room install is a RESIDUAL reviewer/T-2 item. Clean up the test area after evidence capture. Checkpoint.

## PHASE C5 — deterministic program at final bytes (W-6)
After the LAST code/tooling commit, run with measured counts and a per-module determinism label:
- SOW full pytest (now provisioned) — record measured vs the 2382/0/1 host-coupled contract. SOW desktop `node --test`; terminal suite; shell suite (R-5 restoration); tokencenter; release-tool tests; all gates. These are DETERMINISTIC-HERMETIC.
- Debate, SOVEREIGN, Distillery: run ONLY the legs that pass from their venv with NO Ollama models and NO live services; every model- or service-dependent leg is PARKED as E-6 with that exact cause — do not load a model, do not claim coverage you didn't run.
Zero unexplained warnings/orphans or park-with-dossier. Any failure traced to C1–C4 changes is fixed within scope and C5 re-runs from the top. Checkpoint.

## PHASE C6 — seal (manifests last)
Regenerate provenance + RELEASE-MANIFEST (also enumerating LICENSE/THIRD-PARTY-NOTICES/VERSION/SBOM/release-artifacts hashes). Fill VERSION.json `source_commit`. Re-cut the two candidate ZIPs from `git archive` of the final commit into `release-artifacts/` (+ sidecars), cold-extract each and hash-verify. Build `REVIEW-PACKAGE-CONVERGE01.zip` (patches since `a945d99`, all CONVERGE01 bundles minus >5 MB files, custody docs) + sidecar. Write `RELEASE-NOTES-DRAFT.md` (versions; changes since baseline; the rollback artifact; EVERY accepted limitation/park by name: audit-accepted risks, Grok/G28 no-spend, Distillery status-only + deferred live testing + OP-6 UI, H-5/OD-20, MT-09, G26, live-acceptance legs, OP-2 rulings, upstream W-4, canonical license text pending, true clean-room pending). Write `CONVERGE-REPORT.md` (commit chain, per-phase results, measured totals, and the §CONVERGENCE CHECKLIST filled). Final commit; append CONVERGE-01 to `bundles\LOOP-RUN-REPORT.md`.

## CONVERGENCE CHECKLIST (mechanical exit gate — W-7)
The loop exits only when EVERY row reads DONE-CANDIDATE, PARKED(reason), or DEFERRED(operator) — no blanks:
`[ ] C1 N-13b  [ ] C1 OP-3  [ ] C1 OP-4  [ ] C1 OP-1  [ ] C1 OP-2-dossier  [ ] C2 provisioning(all modules)  [ ] C2 supply-chain disposition  [ ] C3 LICENSE+notices  [ ] C3 VERSION+SBOM  [ ] C3 release archives+tags  [ ] C4 install tooling  [ ] C4 self-host proxy test  [ ] C5 all deterministic suites  [ ] C5 gates green  [ ] C6 manifests regenerated  [ ] C6 ZIPs re-cut+cold-verified  [ ] C6 review package  [ ] C6 release-notes draft`
Any PARKED/DEFERRED row must name its blocker and appear in the report's residual queue — which must contain ONLY reviewer verdicts, operator physical acts/rulings, and operator-deferred items. If any row is blank or any residual item is builder-hermetic, you are NOT converged: loop back and build it.

End every report with: `BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`
Execute BOOT now. Run C1→C6 to convergence, then stop.

---END PASTE---

# CONVERGE-01 — COMPLETE SELF-CONTAINED DIRECTIVE (R2)
## For: grok-4.6 on OpenCode · SWS-REM-DIR-20260828 · ENTRIES 006 / 007 / 008 / 009
## Verifies all prior work (10 commits, two builder seats), then finishes C4 → C5 → C6 to convergence. Nothing external to read.

**Operator:** open OpenCode with Grok 4.6, project directory `D:\producttion software 2\`, full file access. Paste everything between the markers.

---BEGIN PASTE---

You are the BUILDER seat `grok-4.6 (opencode)`, completing CONVERGE-01 under directive SWS-REM-DIR-20260828, authorized by OPERATOR-INSTRUCTIONS.log ENTRIES 006, 007, 008 and 009. Two builder seats have already worked this loop — codex (7 commits) and a prior grok session (3 commits) — each halting on a harness usage limit, not on failure. Your job: **independently verify everything already built, finish the interrupted phase, and run to convergence without stopping.** This document is complete; you need no other directive.

Discipline: never stop between items or phases; never ask the operator anything; when something cannot be done, PARK it with a written dossier and continue; exit only when the §7 checklist has no blank row. Everything you produce is CANDIDATE. You never write reviewer PASS. You never promote anything.

---

## §1 · STANDING RULES — binding for the whole run

**S-1 Write set.** Write ONLY inside `D:\producttion software 2\release-worktree\`, `D:\producttion software 2\release-planning\bundles\`, and (for the C4 proxy test only) `D:\producttion software 2\install test area\`. Read anything. NEVER write to: the frozen ZIPs or their `.sha256` sidecars in `D:\producttion software 2\`, any existing custody document in `release-planning\` (directives, punch lists, adjudications, OPERATOR-INSTRUCTIONS.log), `release-planning\audit-codex\`, or `D:\Product Software\` (read-only source of record).

**S-2 Encoding.** NEVER write or restore a source file with PowerShell redirection (`>`), `Out-File`, or `Set-Content` — PowerShell 5.1 emits UTF-16 and silently corrupted a file earlier in this program. Use Python `open(path,'w',encoding='utf-8')` / `'wb'`, or `cmd /c "... > file"`. After every item confirm no changed file begins with bytes `FF FE` or `FE FF`; new text files are UTF-8 without BOM, LF endings.

**S-3 No invented APIs.** Before calling a function, asserting a signature, using a config key, or passing a CLI flag — read its definition in the tree. If source contradicts assumption, source wins and NOTES records the correction.

**S-4 Coupled values move together.** Any file pinning a hash, version, or count of another file (`shell/BUILD-MANIFEST.txt`, `RELEASE-MANIFEST.json`, `tools/release/fixture_allowlist.json`, `tools/mutation/pane_input_bypass_mutations.js` PINNED_BASELINE, config-integrity pins, `pytest.ini` contracts, install manifests) is updated in the SAME commit as the change that invalidated it, following that file's own documented re-pin protocol.

**S-5 Test-suite side effects.** The shell suite REWRITES `shell/BUILD-MANIFEST.txt` and `evidence/hardening/h13..h16-*.txt`. After ANY shell-suite run: `git status`; restore evidence-lane rewrites byte-exact via `cmd /c "git show HEAD:<path> > <path>"` then hash-verify; only intended changes may remain.

**S-6 Test hygiene.** `PYTHONDONTWRITEBYTECODE=1` or `python -B` for every Python run. Delete any `__pycache__`/`.pyc`/`.pytest_cache` you create before committing.

**S-7 Minimal diff.** Nothing outside the item's named files except what S-4 forces; list every forced coupling in NOTES.

**S-8 Per-item discipline.** fail-before evidence → minimal fix → pass-after evidence → affected suite with MEASURED counts (never expected) → changed-file list → `git status` clean-except-intended → ONE commit `<PHASE> <item>: <summary>` → bundle `release-planning\bundles\CONVERGE01\<item>\` with `NOTES.md`.

**S-9 Park, never improvise.** Criteria you cannot check mechanically, an unexpected tracked-byte difference, or a correct action that exceeds scope → PARK with a dossier naming the blocker, continue the queue. Never repair outside scope.

**S-10 Prohibitions.** No billed API calls. No provider/LLM calls. No GPU compute. No model downloads or loads. No launching operator-owned services (the C4 loopback boot of the installed shell is the sole exception, and it is stopped immediately). No gate promotion. No reviewer PASS.

**S-11 Network grant (narrow).** ONLY `registry.npmjs.org`, `pypi.org`, `files.pythonhosted.org`, and the Electron dist host used by the pinned `electron` package — and ONLY for lock-driven installs into worktree or proxy-install environments. `npm ci`, NEVER `npm install`. Python packages only from a module's committed lock (`--require-hashes` where hashes exist; otherwise the lock's pinned `==` versions, recording that hashes were absent). NEVER install from a URL, git ref, or local path not named in a committed lock. Any other host: park the affected item.

**S-12 A red suite is not an authorization.** A suite is satisfied when (a) every failure traces to a named pre-existing baseline or parked cause, and (b) NO failure is attributable to work done in this loop. You do NOT make a red suite green by importing source from outside the candidate, by editing test assertions, or by touching a HELD item. Those PARK for the reviewer.

**S-13 Held items stay held.** The operator's alone; not yours to advance, revert, or build upon: H-5 / OD-20 canonical-lineage ruling (commit `e2e8e55`); OD-23 assertion-inversion adjudication; H-6 composed-system proof; MT-09 signature; G26 attended debug; all live-acceptance and display legs; OP-2 implementation (dossier only, already written); OP-6 Distillery UI; true clean-room install.

**S-14 No assertion edits without authority.** Never modify a test assertion to accommodate observed behavior. A test failing because implementation changed is EVIDENCE — park it with a dossier naming both sides. The only permitted assertion edits are those a NEW feature you build in this loop requires; name and justify each in NOTES.

---

## §2 · BOOT

Verify all of the following; any mismatch → write `release-planning\bundles\CONVERGE01\BOOT-FAILURE.md` naming it, and STOP.

1. `release-planning\OPERATOR-INSTRUCTIONS.log` contains ENTRIES 001 through 009.
2. SHA-256 of `release-planning\SWS-REM-DIR-20260828-CANDIDATE-R2.md` = `9061c2e8a40bcff4258f77692ff0e23ffebde98eaa638446a22a7aa914b3691d`.
3. SHA-256 of `release-planning\LOOP-PROTOCOL-20260828.md` = `acae8fb8b9b4d673f28ae5b6d4b74fbfed0c86e0e70bfa1f86638237219bae44`.
4. In `release-worktree`: `git rev-parse HEAD` begins `a45fb98`; branch `main`. **The mount can be slow — always let `git status` finish; empty output from a killed command is NOT a clean tree.**
5. These ten commits exist in this order after `a945d99`: `1f0a59c`, `65bd58f`, `e2e8e55`, `e03811e`, `1397a14`, `e55cde9`, `ad8c4c7` (codex seat) then `0a63826`, `0722692`, `a45fb98` (prior grok seat).
6. `git status --porcelain`: if EMPTY, proceed. If it shows leftovers from the interrupted C4 proxy run (untracked `.runtime/` content, `__pycache__`, or artifacts under `install test area\`), that is EXPECTED — record them in `bundles\CONVERGE01\V\boot-residue.md`, clean only the untracked non-tracked-tree residue (never a tracked file), and proceed. Any MODIFIED TRACKED file is a hard stop.

Then overwrite `release-planning\bundles\CONVERGE01\CHECKPOINT.json`:
`{"phase":"V","head":"a45fb98","completed":[{"phase":"C0","commits":["1f0a59c","65bd58f","e2e8e55"]},{"phase":"C1","commits":["e03811e","1397a14","e55cde9","ad8c4c7","0a63826"]},{"phase":"C2","commits":[],"status":"partial-park"},{"phase":"C3","commits":["0722692"]},{"phase":"C4","commits":["a45fb98"],"status":"tooling-committed-proxy-cycle-incomplete"}]}`
Keep it current at every phase boundary.

---

## §3 · PHASE V — VERIFY ALL PRIOR WORK (before building anything)

You are independently re-deriving two other seats' claims, including a prior session of your own. Do not trust reports; check bytes and re-run suites. Evidence → `bundles\CONVERGE01\V\`. **Findings are RECORDED, not repaired** — unless S-12/S-13 explicitly permit, a discrepancy parks and continues.

**V-1 Encoding sweep.** For every file changed in `a945d99..a45fb98`: confirm none begins `FF FE`/`FE FF` and none contains NUL bytes. Record the file count.

**V-2 UI adoption (`1f0a59c`).** `shell/static/app.css`, `workspace-bg.svg`, `workspace-bg-v2.png` committed; ZERO external references (`http://`, `https://`, protocol-relative) in the CSS/SVG; background referenced as a local path; shell CSP and security-header code byte-identical to `a945d99` (diff those paths explicitly). Record the PNG's SHA-256 and size.

**V-3 N-16 runtime evidence (`65bd58f`).** `.runtime/` in `.gitignore`; no `evidence/startup-tests/tokencenter-*.json` tracked or untracked; the startup-test writer in `shell/src/` targets a gitignored runtime path. **Behavioral proof:** drive the writer hermetically (unit test or direct call — do NOT launch a module service), then confirm `git status` is still clean.

**V-4 N-13b rebase (`e03811e`).** Exactly one adapter — `shell/modules/llamacpp.json` — still contains `D:/Product Software` or `C:/Users/Sslaw`, recorded as an intentional optional skip; the other six point into the worktree; `.pre-rebase` backups exist; `tools/release/rebase_adapters.py --dry-run` is now a NO-OP.

**V-5 OP-3 terminal cap (`1397a14`).** Visible-pane limit is 8. Re-run the SOW desktop suite (`node --test test/*.test.js` from `modules/sow/apps/desktop`) and the terminal suite; record MEASURED counts (prior claims: 1104 desktop, 225 terminal).

**V-6 OP-4 Token Center embed (`e55cde9`).** Shell CSP gained `frame-src` scoped to `http://127.0.0.1:8765` and nothing wider; Token Center's framing policy allows ancestor `http://127.0.0.1:5180` ONLY; no other M-1 protection weakened (diff header code against `a945d99`). Re-run the Token Center suite; record MEASURED counts (prior claim: 32).

**V-7 OP-1 shortcut installer (`ad8c4c7`).** `tools/release/install_shortcut.ps1` + test exist; `shell/static/sovereign.ico` is a local asset. Re-run its test with `-TargetDir` INSIDE a worktree temp lane. **Do NOT run it against the real Desktop or Start Menu.** Confirm no shortcut exists outside the worktree.

**V-8 Manifests untouched by feature commits.** `RELEASE-MANIFEST.json` and `shell/BUILD-MANIFEST.txt` unchanged across `a945d99..a45fb98` (`git log --name-only` over those two paths must be empty). They are regenerated from final bytes at C6 only.

**V-9 The HELD commit `e2e8e55` — verify, do not act.** Confirm `modules/sow/control_plane/canonical_registry.py` = `88c2362ad731d2e433beabc65329ab75f44349a70e2493671903bcc5b6df4161` and `modules/sow/adapters/local/llamacpp.py` = `b2484b887ca4087070ffbdbb8abc86b88fe58d63c5c2aa4ae5d15009ce418b29`, and that both are byte-identical to the copies inside the frozen RESET baseline ZIP (`SOVEREIGN_RESET_BASELINE_20260827T193540Z.zip`, sha256 `00ae9eefc7dada37943bb08a305020c73e2c029a6398858e8bba27078c349a7c`) — read from the ZIP without extracting it anywhere permanent. These are the subject of the operator's HELD OD-20 ruling (S-13): **neither extend nor revert.** Also enumerate every assertion `e2e8e55` changed across the seven shell test files into `bundles\CONVERGE01\V\assertion-inventory.md` (file, old, new, one row each) for the reviewer under OD-23.

**V-10 OP-2 dossier (`0a63826`).** Confirm it changed ZERO product bytes (only `bundles\CONVERGE01\C1-OP-2\`). Read the dossier and confirm it names the operator decisions (spend/OD-5, credential custody, role eligibility, telemetry) rather than assuming any.

**V-11 C2 provisioning and its parks.** Read `bundles\CONVERGE01\C2\`. Confirm by inspection: SOVEREIGN and Debate `.venv` exist and their installed versions match their locks exactly (re-derive the version-vs-lock diff yourself); Node dependencies installed via `npm ci`; **and confirm the two parks are real, not convenience** — verify from the files themselves that SOW's Python requirements express ranges rather than a resolved closure, and that Distillery has no Python dependency lock. Record both as named release-note limitations: *these are genuine reproducibility defects in the candidate, not environment problems.* Confirm no tracked lockfile byte changed in C2 and every environment directory is gitignored.

**V-12 C3 release identity (`0722692`).** Confirm present and well-formed: root and per-module `LICENSE`; `THIRD-PARTY-NOTICES.md`; `VERSION.json` carrying `1.0.0-rc.1`; `SBOM.json`. Verify the five annotated tags exist (`rel/debate/v1.2.1-hardening`, `rel/distillery/1.1.0rc3`, `rel/sovereign/SOVEREIGN_ENTERPRISE_PRODUCTION_20260813_142520`, `rel/sow/0.1.0`, `rel/tokencenter/record-dated-2026-08-26`) and that each module archive under `release-artifacts/` has a matching `.sha256`. **Known gap to confirm and close in C4:** there is no `rel/shell/...` tag or shell release archive — the shell is a release object too. If confirmed missing, create the shell archive + sidecar + annotated tag as the first act of C4 (S-4: it belongs to the release identity set), or PARK with a reason if the shell's version identity cannot be determined from `VERSION.json`/`BUILD-DIRECTIVE`.

**V-13 C4 tooling (`a45fb98`).** Confirm `build_release.ps1`, `install.ps1`, `verify_install.ps1`, `uninstall.ps1` exist under `tools/release/`; read each fully (S-3). Confirm `install.ps1` writes an install manifest enumerating every path it creates including out-of-Dest shortcuts, and `uninstall.ps1` removes exactly the manifest set and proves consumption. Note the two defects the prior session fixed (native `-c` quoting under PowerShell; import working directory) and confirm both fixes are present in the committed bytes.

**V-14 Shell suite baseline.** Run the full shell suite (`py -3.12 -B -m unittest discover -s shell/tests`), apply S-5 restoration immediately after, record MEASURED counts. Prior claims: 165/165 after the C0 reconciliation, 166 after OP-4. Whatever you measure is the baseline you carry under S-12.

Write `bundles\CONVERGE01\V\VERIFICATION-REPORT.md`: one row per check — VERIFIED / DISCREPANCY / UNVERIFIABLE — with the bytes or output behind each. Checkpoint. **Continue even if you find discrepancies.**

---

## §4 · PHASE C4-FINISH — complete the interrupted proxy cycle

The prior session committed the tooling and got partway through the final replay before halting. Finish it.

1. **Shell release object** — if V-12 confirmed it missing: `git archive` the shell path at HEAD → `release-artifacts/shell-<version>-src.zip` + `.sha256` + annotated tag `rel/shell/<version>` (version from `VERSION.json`). Commit with the C4 work.
2. **Reset the proxy destination.** `D:\producttion software 2\install test area\` must start EMPTY — remove any residue from the interrupted run (it is scratch, not evidence; capture a listing into the bundle first).
3. **Run the complete cycle, every step captured to `bundles\CONVERGE01\C4\`:** `build_release.ps1` → `install.ps1 -Dest "<install test area>" -TargetDir "<inside that area>"` (so no shortcut escapes) → `verify_install.ps1 -Dest` (hash-verify the full manifest) → boot the installed shell through its documented launcher, one loopback `GET /`, then clean stop → `uninstall.ps1 -Dest` which must consume the exact manifest set and leave the area empty.
4. **The exact-set guard is a feature, not an obstacle.** If uninstall refuses because verification or launch created unrecorded artifacts, do NOT loosen the guard — fix the *producer* so it writes to disposable temp storage or is itself recorded in the manifest, then replay the cycle from the amended commit. The prior session's fixes (Debate verification logs → temp; launcher run with bytecode disabled) are precedent; apply the same pattern to anything new.
5. **State the limitation in NOTES:** this host already has Python, node and Ollama, so the cycle proves install/verify/uninstall MECHANICS and layout — it does NOT prove clean-room independence. A fresh-VM clean-room install remains a residual operator/reviewer item.
6. Clean up the test area after evidence capture. Commit (amend or add; one coherent C4 commit set). Checkpoint.

---

## §5 · PHASE C5 — deterministic program at final bytes

Run ONLY after the last code/tooling commit. Record MEASURED counts with a determinism label per suite. S-12 governs "satisfied".

**Must run (deterministic-hermetic):** SOW desktop `node --test`; SOW terminal suite; shell suite (S-5 restoration after); Token Center suite; all `tools/release/test_*.py`; and every gate — package boundary against a clean `git archive` extraction, provenance cross-hash, release-manifest check, governance BOM, model consistency, innerHTML sink audit.

**SOW full pytest:** attempt it. If the SOW Python environment is unprovisioned because of the V-11 lock defect, PARK it with that exact cause and record that the `pytest.ini` `2382/0/1` contract remains UNVERIFIED in this loop — do not synthesize a lock to make it runnable (S-11, S-9).

**Must NOT run:** any Debate, SOVEREIGN, or Distillery leg requiring Ollama models or a live service — PARK with that exact cause. Do not load a model. Do not claim coverage you did not run.

Zero unexplained warnings or orphan processes, or park with a dossier. Any failure attributable to work done in THIS loop is fixed in scope and C5 restarts from the top; pre-existing baseline failures are recorded, not fixed. Checkpoint.

---

## §6 · PHASE C6 — seal (manifests last; nothing after)

1. Regenerate the three module provenance records and `RELEASE-MANIFEST.json` against the FINAL tree — now also enumerating LICENSE, THIRD-PARTY-NOTICES, VERSION.json, SBOM.json, every release archive and sidecar (including the shell object), the adopted UI assets, and the C4 install tooling. Prior records superseded per the existing convention; previous files preserved.
2. Fill `VERSION.json`'s `source_commit` with the final commit.
3. Re-cut the candidate ZIPs from `git archive` of the final commit into `release-artifacts/` with `.sha256` sidecars; cold-extract each to a temp area and hash-verify.
4. Build `release-planning\REVIEW-PACKAGE-CONVERGE01.zip` (+ `.sha256`): every patch since `a945d99`, all CONVERGE01 bundles except files >5 MB, and the custody documents. Note excluded large files as reproducible from the commit.
5. Write `bundles\CONVERGE01\RELEASE-NOTES-DRAFT.md` — versions, changes since baseline, the named rollback artifact (`SOVEREIGN_RESET_BASELINE_20260827T193540Z.zip`, `00ae9eef…`), and EVERY accepted limitation and park BY NAME, including at minimum: **SOW Python dependencies are unpinned ranges and Distillery has no Python lock — reproducible provisioning is not achieved for those two modules**; SOW full pytest contract unverified this loop (if parked); the OD-20 lineage ruling outstanding; the OD-23 assertion inventory; the pre-existing shell-suite baseline; C2 supply-chain accepted risks; no-spend limitations (Grok registry leg, G28); Distillery status-only plus deferred live testing plus deferred UI (OP-6); H-6; MT-09; G26; live-acceptance legs; OP-2 awaiting rulings; upstream Distillery W-4 tagging (outside write boundary); canonical license text pending; true clean-room install pending.
6. Write `bundles\CONVERGE01\CONVERGE-REPORT.md`: full commit chain from `a945d99`, per-phase results across all three builder sessions, all measured totals, the §7 checklist filled, and the RESIDUAL QUEUE.
7. Final commit. Append a CONVERGE-01 section to `bundles\LOOP-RUN-REPORT.md` without rewriting anything above it.

---

## §7 · CONVERGENCE CHECKLIST — the exit gate

Every row must read DONE-CANDIDATE, PARKED(reason) or DEFERRED(operator). No blanks.

`[ ] V verification report  [ ] V-9 assertion inventory  [ ] V-11 C2 parks confirmed real  [ ] C4 shell release object  [ ] C4 full proxy cycle  [ ] C4 test area cleaned  [ ] C5 deterministic suites  [ ] C5 gates  [ ] C5 SOW full pytest (run or parked)  [ ] C6 manifests regenerated  [ ] C6 ZIPs re-cut + cold-verified  [ ] C6 review package  [ ] C6 release-notes draft  [ ] C6 converge report`

The RESIDUAL QUEUE must contain ONLY reviewer verdicts, operator physical acts and rulings, and operator-deferred items. Include these verbatim if unresolved: `OD-20 ruling on the e2e8e55 lineage import`, `OD-23 assertion-inversion adjudication`, `SOW + Distillery dependency-lock defects`, `pre-existing shell-suite baseline disposition`, `19 CANDIDATE gates + Gate-5/5b reviewer backlog`. If any checklist row is blank, or any residual item is something a builder could have done hermetically, **you are not converged — loop back and build it.**

End every report with exactly: `BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`

Execute §2 BOOT now. Then V → C4-FINISH → C5 → C6, to convergence. Stop only when §7 has no blank row.

---END PASTE---

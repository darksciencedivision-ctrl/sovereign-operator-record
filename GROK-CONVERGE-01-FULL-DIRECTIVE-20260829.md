# CONVERGE-01 — COMPLETE SELF-CONTAINED DIRECTIVE
## For: grok-4.6 on OpenCode · SWS-REM-DIR-20260828 · ENTRIES 006 / 007 / 008
## Verifies the prior builder's work, then runs to convergence. Nothing external to read.

**Operator:** open OpenCode with Grok 4.6, project directory `D:\producttion software 2\`, full file access. Paste everything between the markers. Long run — let it work.

---BEGIN PASTE---

You are the BUILDER seat `grok-4.6 (opencode)`, executing CONVERGE-01 to completion under directive SWS-REM-DIR-20260828, authorized by OPERATOR-INSTRUCTIONS.log ENTRIES 006, 007 and 008. A prior builder seat (codex) completed part of this loop and halted on a harness usage limit — not a failure, not an exception. Your job: **independently verify what it did, then build everything that remains, without stopping.** This document is complete; you do not need to read any other directive.

Run-to-convergence discipline: you do not stop between items or phases; you never ask the operator anything; when something cannot be done you PARK it with a written dossier and continue; you exit only when the §10 checklist has no blank row. Everything you produce is CANDIDATE. You never write reviewer PASS. You never promote anything.

---

## §1 · STANDING RULES — binding for the whole run

**S-1 Write set.** You may write ONLY inside `D:\producttion software 2\release-worktree\` and `D:\producttion software 2\release-planning\bundles\`. Read anything. NEVER write to: the frozen ZIPs or their `.sha256` sidecars in `D:\producttion software 2\`, any existing custody document in `release-planning\` (directives, punch lists, adjudications, OPERATOR-INSTRUCTIONS.log), `release-planning\audit-codex\` (frozen evidence), or `D:\Product Software\` (read-only source of record, never a target).

**S-2 Encoding.** NEVER write or restore a source file with PowerShell redirection (`>`), `Out-File`, or `Set-Content` — PowerShell 5.1 emits UTF-16 and silently corrupted a file earlier in this program. Use Python `open(path,'w',encoding='utf-8')` / `'wb'`, or `cmd /c "... > file"`. After every item verify no changed file begins with bytes `FF FE` or `FE FF`; new text files are UTF-8 without BOM, LF endings.

**S-3 No invented APIs.** Before you call a function, assert a signature, use a config key, or pass a CLI flag — read its definition in the tree. If the source contradicts your assumption, the source wins and your NOTES records the correction.

**S-4 Coupled values move together.** Any file that pins a hash, version, or count of another file (`shell/BUILD-MANIFEST.txt`, `RELEASE-MANIFEST.json`, `tools/release/fixture_allowlist.json`, `tools/mutation/pane_input_bypass_mutations.js` PINNED_BASELINE, config-integrity test pins, `pytest.ini` contracts) must be updated in the SAME commit as the change that invalidated it, following that file's own documented re-pin protocol where one exists.

**S-5 Test-suite side effects.** The shell test suite REWRITES `shell/BUILD-MANIFEST.txt` and `evidence/hardening/h13..h16-*.txt` when it runs. After ANY shell-suite invocation: run `git status`; restore evidence-lane rewrites byte-exact from HEAD using `cmd /c "git show HEAD:<path> > <path>"` then hash-verify; only intended changes may remain.

**S-6 Test hygiene.** Set `PYTHONDONTWRITEBYTECODE=1` or use `python -B` for every Python run. Delete any `__pycache__`/`.pyc`/`.pytest_cache` you create before committing.

**S-7 Minimal diff.** Change nothing outside the item's named files except what S-4 forces; list every forced coupling in NOTES.

**S-8 Per-item discipline.** fail-before evidence captured → minimal fix → pass-after evidence captured → affected suite run with MEASURED counts (never expected) → changed-file list → `git status` clean-except-intended → ONE commit `<PHASE> <item>: <summary>` → bundle folder `release-planning\bundles\CONVERGE01\<item>\` with `NOTES.md`.

**S-9 Park, never improvise.** If pass criteria cannot be checked mechanically, if you hit an unexpected tracked-byte difference, or if the correct action would exceed this directive's scope — PARK with a dossier naming the blocker, and continue the queue. Never repair outside scope.

**S-10 Prohibitions.** No billed API calls. No provider/LLM calls. No GPU compute. No model downloads or model loads. No launching operator-owned services. No gate promotion. No reviewer PASS.

**S-11 Network grant (narrow).** Network is permitted ONLY to `registry.npmjs.org`, `pypi.org`, `files.pythonhosted.org`, and the Electron dist host used by the pinned `electron` package — and ONLY for lock-driven installs into worktree environments. Use `npm ci`, NEVER `npm install`. Install Python packages only from a module's committed lock (`--require-hashes` where the lock carries hashes; otherwise the lock's pinned `==` versions, recording that hashes were absent). NEVER install from a URL, git ref, or local path not named in a committed lock. Any other host is out of bounds: park the affected item.

**S-12 A red suite is not an authorization.** A suite is satisfied when (a) every failure traces to a named pre-existing baseline or a parked cause, and (b) NO failure is attributable to work done in this loop. You do NOT make a red suite green by importing source from outside the candidate, by editing test assertions, or by touching a HELD item. Those PARK and go to the reviewer.

**S-13 Held items stay held.** These are the operator's alone and are NOT yours to advance, revert, or build upon: H-5 / OD-20 canonical-lineage ruling; H-6 composed-system proof; MT-09 signature; G26 attended debug; all live-acceptance and display legs; OP-2 implementation (dossier only); OP-6 Distillery UI; true clean-room install.

**S-14 No assertion edits without authority.** Never modify a test assertion to accommodate observed behavior. A test failing because implementation changed is EVIDENCE — park it with a dossier naming both sides. The only permitted assertion edits are those a NEW feature you build in this loop requires; name and justify each in NOTES.

---

## §2 · BOOT

Verify all of the following; any mismatch → write `release-planning\bundles\CONVERGE01\BOOT-FAILURE.md` naming the mismatch and STOP.

1. `release-planning\OPERATOR-INSTRUCTIONS.log` contains ENTRIES 001 through 008.
2. SHA-256 of `release-planning\SWS-REM-DIR-20260828-CANDIDATE-R2.md` = `9061c2e8a40bcff4258f77692ff0e23ffebde98eaa638446a22a7aa914b3691d`.
3. SHA-256 of `release-planning\LOOP-PROTOCOL-20260828.md` = `acae8fb8b9b4d673f28ae5b6d4b74fbfed0c86e0e70bfa1f86638237219bae44`.
4. In `release-worktree`: `git rev-parse HEAD` = `ad8c4c7` (full: begins `ad8c4c7`), branch `main`, `git status --porcelain` EMPTY. **The mount can be slow — always let `git status` finish; empty output from a killed command is NOT a clean tree.**
5. These seven commits exist, in this order after `a945d99`: `1f0a59c` (C0 N-14b UI adoption), `65bd58f` (C0 N-16 runtime evidence), `e2e8e55` (C0 suite reconcile), `e03811e` (C1 N-13b rebase), `1397a14` (C1 OP-3 terminals), `e55cde9` (C1 OP-4 Token Center embed), `ad8c4c7` (C1 OP-1 shortcut installer).

Then write `release-planning\bundles\CONVERGE01\CHECKPOINT.json` (the existing file is STALE — overwrite it):
`{"phase":"V","head":"ad8c4c7","completed":[{"phase":"C0","commits":["1f0a59c","65bd58f","e2e8e55"]},{"phase":"C1-partial","commits":["e03811e","1397a14","e55cde9","ad8c4c7"],"remaining":["OP-2"]}]}`
Keep it current at every phase boundary for the rest of the run.

---

## §3 · PHASE V — VERIFY THE PRIOR BUILDER'S WORK (do this before building anything)

You are independently re-deriving another seat's claims. Do not trust its reports; check bytes and re-run suites. Evidence → `bundles\CONVERGE01\V\`. **Findings here are recorded, not repaired** — if something fails verification, write it up and continue; repairs are the operator's call unless S-12/S-13 permit them.

**V-1 Encoding sweep.** For every file changed in `a945d99..ad8c4c7`: confirm none begins `FF FE`/`FE FF` and none contains NUL bytes. Record the file count.

**V-2 UI adoption (`1f0a59c`).** Confirm `shell/static/app.css`, `shell/static/workspace-bg.svg`, `shell/static/workspace-bg-v2.png` are committed. Confirm ZERO external references (`http://`, `https://`, `//cdn`, protocol-relative) in the CSS/SVG and that `app.css` references the background as a local path. Confirm the shell's CSP and security-header emitting code is byte-identical to `a945d99` (diff those code paths explicitly). Record the PNG's SHA-256 and byte size.

**V-3 N-16 runtime evidence (`65bd58f`).** Confirm `.runtime/` is in `.gitignore`; confirm no `evidence/startup-tests/tokencenter-*.json` files remain tracked or untracked; read the startup-test writer in `shell/src/` and confirm its target is now a gitignored runtime path. **Behavioral proof:** exercise the startup-test write path (hermetically — do NOT launch a real module service; drive the writer directly or via its unit test), then confirm `git status` is still empty afterward.

**V-4 N-13b rebase (`e03811e`).** Confirm exactly one adapter — `shell/modules/llamacpp.json` — still contains `D:/Product Software` or `C:/Users/Sslaw`, and that it is recorded as an intentional optional skip. Confirm the other six adapters point into the worktree. Confirm `.pre-rebase` backups exist. Re-run `tools/release/rebase_adapters.py --dry-run` and confirm it is now a NO-OP (idempotent).

**V-5 OP-3 terminal cap (`1397a14`).** Read the cap definition and confirm the visible-pane limit is 8. Re-run the SOW desktop suite (`node --test test/*.test.js` from `modules/sow/apps/desktop`) and the terminal suite; record MEASURED counts. Prior seat claimed 1104 desktop / 225 terminal — record whether yours match.

**V-6 OP-4 Token Center embed (`e55cde9`).** Confirm the shell CSP gained `frame-src` scoped to `http://127.0.0.1:8765` and NOTHING wider; confirm Token Center's framing policy allows ancestor `http://127.0.0.1:5180` ONLY and that no other M-1 protection was weakened (diff its header code against `a945d99`). Re-run the Token Center suite; record MEASURED counts (prior claim: 32).

**V-7 OP-1 shortcut installer (`ad8c4c7`).** Confirm `tools/release/install_shortcut.ps1` and its test exist and that `shell/static/sovereign.ico` is a local asset. Re-run its test with `-TargetDir` INSIDE a worktree temp lane. **Do NOT run the installer against the real Desktop or Start Menu.** Confirm no shortcut was created outside the worktree.

**V-8 Manifests untouched.** Confirm `RELEASE-MANIFEST.json` and `shell/BUILD-MANIFEST.txt` are unchanged across all seven commits (`git log --name-only a945d99..ad8c4c7 -- <those two paths>` must be empty). This is required — they are regenerated from final bytes at the very end.

**V-9 The flagged commit `e2e8e55` — verify, do not act.** This commit imported `modules/sow/control_plane/canonical_registry.py` and `modules/sow/adapters/local/llamacpp.py` into the candidate. Confirm their SHA-256 values are exactly `88c2362ad731d2e433beabc65329ab75f44349a70e2493671903bcc5b6df4161` and `b2484b887ca4087070ffbdbb8abc86b88fe58d63c5c2aa4ae5d15009ce418b29` respectively, and that these are byte-identical to the copies inside the frozen RESET baseline ZIP (`SOVEREIGN_RESET_BASELINE_20260827T193540Z.zip`, sha256 `00ae9eefc7dada37943bb08a305020c73e2c029a6398858e8bba27078c349a7c`) — read them from the ZIP without extracting it anywhere permanent. These files are the subject of the operator's HELD OD-20 ruling (S-13): **you neither extend nor revert this.** Also enumerate every test assertion `e2e8e55` changed (seven shell test files) into `bundles\CONVERGE01\V\assertion-inventory.md` — one row per changed assertion, old value, new value, and the file. This inventory goes to the reviewer under OD-23. Record; do not modify.

**V-10 Shell suite baseline.** Run the full shell suite (`py -3.12 -B -m unittest discover -s shell/tests`), apply S-5 restoration immediately afterward, and record MEASURED counts. Prior seat claimed 165/165 after its reconciliation. Whatever you measure is the baseline you carry forward under S-12.

Write `bundles\CONVERGE01\V\VERIFICATION-REPORT.md`: one row per check, VERIFIED / DISCREPANCY / UNVERIFIABLE with the bytes or output behind each. Checkpoint, then continue — **do not stop even if you find discrepancies.**

---

## §4 · PHASE C1-FINAL — OP-2 frontier-provider design dossier (BUNDLE ONLY, zero product bytes)

Write `bundles\CONVERGE01\C1-OP-2\FRONTIER-PROVIDER-DOSSIER.md`. Read every file you cite (S-3). Cover:
1. **Current state** — where SOVEREIGN's model list actually comes from (`modules/sovereign/SYSTEM_MANIFEST.json` is the declared authority), its loopback-only Ollama client, and precisely how adding hosted frontier providers (ChatGPT / Claude / Grok) breaks that trust boundary.
2. **Candidate mechanism** — reusing SOW's existing provider-CLI adapters (`modules/sow/adapters/frontier/claude_code.py`, `codex.py`, `grok_build.py`) as subprocess providers mapped into SOVEREIGN roles, versus a direct-API client. Trade-offs of each: credential handling, failure modes, evidence capture, offline behavior, testability.
3. **Operator decisions required before any code** — spend authority (OD-5 currently DENY; the feature is inert without a new grant), credential custody (cite the packaging-gate credential classes in `tools/release/package_boundary_gate.py` that must never see a real key), which SOVEREIGN roles may use hosted models versus must remain local, and telemetry routing (Token Center's collector matrix already covers these providers).
4. **Implementation plan** sized in discrete commits for a future authorized batch.
NO product bytes change. Checkpoint.

---

## §5 · PHASE C2 — environment provisioning

1. **Disk preflight:** require ≥12 GB free on the worktree volume. Short → PARK all of C2 with the measured figure and continue to C3 (C3 does not need environments). Record free space either way.
2. **Locks are read-only inputs.** Read each module's lock/requirements FIRST. A missing, partial, or self-inconsistent lock → PARK that module's provisioning with a dossier. NEVER regenerate or edit a lock. No tracked lockfile byte may change in this phase.
3. Provision into gitignored environment directories, lock-driven per S-11: SOVEREIGN `.venv` (from `WORKSPACE-RESOLVED-LOCK`), Debate `.venv`, SOW Python env (this supplies `jsonschema` and unblocks the full SOW pytest suite), Distillery `.venv`, Token Center (stdlib — verify only, no install), node dependencies via `npm ci` against committed package-locks. For each: capture installer output, a `pip freeze` / `npm ls` snapshot, and a version-vs-lock diff. ANY resolution differing from the lock → PARK with dossier; never silently accept.
4. **Supply-chain disposition:** run `npm audit` / `pip-audit` (or whatever equivalent those tools actually support — S-3) per environment. Critical/high findings: fix only if the lock permits; otherwise PARK as a NAMED accepted risk destined for the release notes. Re-run the repository secrets scan. Confirm every environment directory is gitignored — extending `.gitignore` is the ONLY tracked change C2 may commit.
5. Per-module import/self-check that needs only its own venv (no services, no models). Checkpoint.

---

## §6 · PHASE C3 — release identity

1. **`LICENSE`** at worktree root and in each module directory, exact text: `Copyright (c) 2026 Samuel Lawson / Dark Science Division. All rights reserved.` followed by: `Interim license record; canonical license text to be supplied by the operator before external distribution.`
2. **`THIRD-PARTY-NOTICES.md`** at root — enumerate bundled open-source dependencies and their licenses, built from the C2 lock/audit data. (An all-rights-reserved product that ships OSS dependencies must carry these notices.)
3. **`VERSION.json`** at root: `{"system":"sovereign-workspace","version":"1.0.0-rc.1","source_commit":"<filled at C6>","modules":{ ...per-module versions from RELEASE-MANIFEST... }}`. It is `rc.1` because ratification has not occurred — the plain `1.0.0` identity is minted by the operator at promotion, never by a builder.
4. **`SBOM.json`** — CycloneDX-style, or a documented equivalent you construct, built offline from the locks + C2 snapshots + a native-binary inventory carrying SHA-256 for every Electron / node-pty / `.exe` / `.node` / `.pyd` / `.dll` in the tree.
5. **Per-module release archives:** `git archive` each module path at HEAD → `release-artifacts/<module>-<version>-src.zip` plus a `.sha256` sidecar, plus an annotated tag `rel/<module>/<version>` in the worktree repo. For Debate, confirm its pin matches the existing release identity `d03ba417914e1465898f5144cc5735afb92f7f6da5e846e0a968fe48c531d265` and note it — do not re-cut its upstream artifact.
6. **Rollback of record:** do NOT manufacture one. Name the frozen `SOVEREIGN_RESET_BASELINE_20260827T193540Z.zip` (`00ae9eef…`) as the previous-baseline rollback artifact in the release notes.
7. Commit (LICENSE, notices, VERSION.json, SBOM, tags). `release-artifacts/` is gitignored and hashed into the manifest at C6. Checkpoint.

---

## §7 · PHASE C4 — install path + self-host proxy test

Build under `tools/release/`: `build_release.ps1` (produces the artifact set from a clean `git archive`), `install.ps1 -Dest <dir> [-TargetDir <shortcut dir>]`, `verify_install.ps1 -Dest <dir>`, `uninstall.ps1 -Dest <dir>`.

`install.ps1` writes an **install manifest** listing EVERY path it creates, including any shortcut written outside `-Dest`. `uninstall.ps1` removes exactly the manifest's paths and proves the manifest was fully consumed — the honest claim is "removes what it recorded," never "touched nothing outside Dest."

**Self-host proxy test** into `D:\producttion software 2\install test area\` (note the spaces; start empty), with `-TargetDir` pointing INSIDE that area so no shortcut escapes: build → install → verify → uninstall, every step captured. The installed shell must boot and serve `/` from the installed location (loopback GET, then clean stop — hermetic self-check, not a live-acceptance leg). **State the limitation explicitly in NOTES:** this host already has Python, node, and Ollama installed, so the test proves install/verify/uninstall MECHANICS and layout, and does NOT prove clean-room independence; a fresh-VM clean-room install is a residual reviewer/operator item. Clean up the test area after capturing evidence. Checkpoint.

---

## §8 · PHASE C5 — deterministic program at final bytes

Run this ONLY after the last code/tooling commit. Record MEASURED counts with a determinism label per suite; S-12 governs what "satisfied" means.

**Deterministic-hermetic (must run):** SOW full pytest (now provisioned — record measured results against the `2382/0/1` host-coupled contract in `pytest.ini`), SOW desktop `node --test`, SOW terminal suite, shell suite (S-5 restoration after), Token Center suite, all `tools/release/test_*.py`, and every gate: package boundary against a clean `git archive` extraction, provenance cross-hash, release-manifest check, governance BOM, model consistency, innerHTML sink audit.

**Model/service-dependent (must NOT run):** any Debate, SOVEREIGN, or Distillery leg requiring Ollama models or a live service PARKS as an environment limitation with that exact cause. Do not load a model. Do not claim coverage you did not run.

Zero unexplained warnings or orphan processes, or park with a dossier. Any failure attributable to work done in THIS loop is fixed within scope and C5 restarts from the top. Pre-existing baseline failures are recorded, not fixed (S-12). Checkpoint.

---

## §9 · PHASE C6 — seal (manifests last, nothing after)

1. Regenerate the three module provenance records and `RELEASE-MANIFEST.json` against the FINAL tree, now also enumerating LICENSE, THIRD-PARTY-NOTICES, VERSION.json, SBOM.json, the release archives, and the adopted UI assets. Prior records superseded per the existing convention; previous files preserved.
2. Fill `VERSION.json`'s `source_commit` with the final commit.
3. Re-cut the candidate ZIPs from `git archive` of the final commit into `release-artifacts/` with `.sha256` sidecars; cold-extract each to a temp area and hash-verify.
4. Build `release-planning\REVIEW-PACKAGE-CONVERGE01.zip` (+ `.sha256`): all patches since `a945d99`, every CONVERGE01 bundle except files >5 MB, and the custody documents. Note excluded large files as reproducible from the commit.
5. Write `bundles\CONVERGE01\RELEASE-NOTES-DRAFT.md`: versions, changes since baseline, the named rollback artifact, and EVERY accepted limitation and park BY NAME — including: the OD-20 lineage ruling outstanding, the OD-23 assertion inventory, the pre-existing shell-suite baseline, supply-chain accepted risks from C2, no-spend limitations (Grok registry leg, G28), Distillery status-only plus deferred live testing plus deferred UI (OP-6), H-6, MT-09, G26, live-acceptance legs, OP-2 awaiting rulings, upstream Distillery W-4 tagging (outside write boundary), canonical license text pending, and true clean-room install pending.
6. Write `bundles\CONVERGE01\CONVERGE-REPORT.md`: full commit chain from `a945d99`, per-phase results, all measured totals, the §10 checklist filled in, and the RESIDUAL QUEUE.
7. Final commit. Append a CONVERGE-01 section to `bundles\LOOP-RUN-REPORT.md` without rewriting anything above it.

---

## §10 · CONVERGENCE CHECKLIST — the exit gate

Every row must read DONE-CANDIDATE, PARKED(reason) or DEFERRED(operator). No blanks.

`[ ] V verification report  [ ] C1 OP-2 dossier  [ ] C2 disk preflight  [ ] C2 provisioning (all modules)  [ ] C2 supply-chain disposition  [ ] C3 LICENSE + third-party notices  [ ] C3 VERSION + SBOM  [ ] C3 archives + tags  [ ] C4 install tooling  [ ] C4 self-host proxy test  [ ] C5 deterministic suites  [ ] C5 gates  [ ] C6 manifests regenerated  [ ] C6 ZIPs re-cut + cold-verified  [ ] C6 review package  [ ] C6 release-notes draft  [ ] C6 converge report`

The RESIDUAL QUEUE must contain ONLY reviewer verdicts, operator physical acts and rulings, and operator-deferred items. Include these verbatim if still unresolved: `OD-20 ruling on the e2e8e55 lineage import`, `OD-23 assertion-inversion adjudication`, `pre-existing shell-suite baseline disposition`, `19 CANDIDATE gates + Gate-5/5b reviewer backlog`. If any checklist row is blank, or any residual item is something a builder could have done hermetically, **you are not converged — loop back and build it.**

End every report with exactly: `BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`

Execute §2 BOOT now. Then V → C1-final → C2 → C3 → C4 → C5 → C6, to convergence. Stop only when §10 has no blank row.

---END PASTE---

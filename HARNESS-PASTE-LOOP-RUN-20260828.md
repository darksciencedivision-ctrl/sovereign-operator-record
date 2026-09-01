# PASTE FOR FRESH DEEPSEEK HARNESS SESSION — EXECUTE THE LOOP
## SWS-REM-DIR-20260828 · authorized 2026-08-28T01:36:53Z · zero placeholders

Copy everything between the BEGIN/END markers into the fresh session.

---BEGIN PASTE---

You are the BUILDER seat, `qwen3.8-max (deepseek harness)`, executing an authorized autonomous remediation loop under directive SWS-REM-DIR-20260828 R2 and its Annex A loop protocol. You have local disk access and are expected to use it. You do not stop between work items. You do not ask the operator anything. You halt only on the six exception classes defined below, and most of those park-and-continue rather than halt.

## 0. BOOT — verify before anything else (read-only)

Custody root: `D:\producttion software 2\release-planning\`. Archive root: `D:\producttion software 2\`.

Verify each of the following with `Get-FileHash -Algorithm SHA256`. Any mismatch → write `BOOT-FAILURE.md` to custody stating expected vs measured, and halt (E-2 discipline). No mismatch → proceed without commentary.

| File | Expected SHA-256 |
|---|---|
| custody\SWS-REM-DIR-20260828-CANDIDATE-R2.md | 9061c2e8a40bcff4258f77692ff0e23ffebde98eaa638446a22a7aa914b3691d |
| custody\LOOP-PROTOCOL-20260828.md | acae8fb8b9b4d673f28ae5b6d4b74fbfed0c86e0e70bfa1f86638237219bae44 |
| archive\SOVEREIGN_SYSTEM_BASELINE_20260827.zip | 75e4be075dcb5629c3175850f5aa3f342c7c693f2967c3620d6cd4914ac27138 |
| archive\SOVEREIGN_RESET_BASELINE_20260827T193540Z.zip | 00ae9eefc7dada37943bb08a305020c73e2c029a6398858e8bba27078c349a7c |
| archive\SOVEREIGN_RAW_SOURCE_BASELINE_20260827.zip | 8594fdc266fee721c9ef69d5595d5bab5402d2ce00de5ee65720994d4efafc9c |

Then read, from custody: `OPERATOR-INSTRUCTIONS.log` (confirm ENTRY 001 contains the expanded token dated 2026-08-28T01:36:53Z — that is your authorization; its SHA-256 at recording was bf1bc1d338f429fc8993692fce9944f8e42bd7e0ff2946af253a659e7b3c4b45), `SWS-REM-DIR-20260828-CANDIDATE-R2.md` (your governing directive — its text overrides this paste on any conflict), `LOOP-PROTOCOL-20260828.md` (loop rules + the operator's accepted decision-block defaults), `PUNCH-LIST-V2.1-20260828.md` (the work queue authority), `ADJUDICATION-RECORD-R2-20260828.md` and `ADJUDICATION-RECORD-20260828.md` (byte-verified findings context).

## 1. Your authorization (recorded; do not re-request it)

Scope: PHASE 0 + BATCH 1 + BATCH 2 (H-5 implementation HELD for OD-20 — dossier only) + BATCH 3, hermetic legs, decision-block defaults accepted with no overrides. Seats: builder = you; reviewer = chatgpt seat (not present in this session — therefore you mark every completed item CANDIDATE, never PASS); validator = claude (cowork); operator = sam, involved only at T-2/T-3 and real exceptions.

## 2. Write boundaries — absolute

You MAY write to: `D:\producttion software 2\release-worktree\` (create it — this is your workspace), `D:\producttion software 2\release-planning\bundles\` (create it — your output channel), and new `.sha256` sidecar files placed BESIDE the SYSTEM and RAW archives (Phase-0 deliverable). You MAY NOT write to, rename, or delete: any frozen ZIP or PDF, any existing custody file, anything under `D:\Product Software\`, or anywhere else on the disk. Violating this is E-2 — the one unforgivable class.

## 3. Exceptions — the only halt/park reasons (Annex A §2)

E-1 money/billing/GPU → halt leg, report. E-2 irreversible act or write outside boundaries → never do it; if a task seems to require it, park with dossier. E-3 spec contradiction that changes what the work IS → park with dossier, continue queue. E-4 pass criteria not mechanically checkable → park. E-5 security regression caused by your change → revert your change, park. E-6 physical-operator dependency (service launches, MT-09, G26, OD-20 ruling, anything needing attended presence or network/credentials you lack) → park for T-2. Parking never stops the loop. Nothing else stops the loop. "Uncertain whether the operator would want this" is resolved by the decision-block defaults in Annex A §4 — apply them.

## 4. THE QUEUE — run in order, no stops between items

**PHASE 0 (do first):**
P0-1 Generate `SOVEREIGN_SYSTEM_BASELINE_20260827.zip.sha256` and `SOVEREIGN_RAW_SOURCE_BASELINE_20260827.zip.sha256` beside the archives (format: `<hash>  <filename>`, matching the existing RESET sidecar).
P0-2 Build the worktree: extract the SYSTEM ZIP's `Production Workspace` content into `D:\producttion software 2\release-worktree\`, `git init`, write a `.gitignore` excluding environments, models, logs, databases, caches, evidence scratch, node_modules, venvs, build output; initial commit. Record the FULL construction recipe per directive §2 (every command, tool versions, .gitignore bytes, resulting commit SHA, UTC) as `bundles\PHASE0\WORKTREE-RECIPE.md`. Your commit is DERIVED identity; the ZIP hash is authority.
P0-3 Re-verify inside the worktree the [B] findings you will fix (C-1a/b/c paths, H-3 both files, H-4 import line, C-3 provenance states) — record as `bundles\PHASE0\PREFLIGHT-VERIFICATION.md`. Unexpected differences → E-3 park, continue.

**BATCH 1:**
B1-1 **C-1 packaging gate**: remove from the worktree (worktree only — the frozen ZIP preserves the originals) `modules/sovereign/runtime/` runtime state (sovereign.db + evidence/quick session tree), `modules/sovereign/ui/ui_shell/.env.local`, `modules/sow/apps/desktop/docs/loop/logs/phase17c_iter90_main.log`. Then implement `tools/release/package_boundary_gate.py`: allow-list-oriented scan that FAILS on `*.db`, `*.sqlite*`, `.env*`, runtime/session/log/cache classes, credential patterns — with a declared-fixture allow-list that is value-based. Tests prove it rejects each class and passes the cleaned tree. Frozen `evidence/cp01|cpm1` captures are historical evidence: gate must treat the `evidence/` lane as quarantined-historical, not scan-fail it.
B1-2 **H-3/M-1(minimal)**: fix the Token Center refresh contract. Implement a real per-process CSRF token: server generates it at start, exposes it to the same-origin UI, compares on POST (decision-block M-1 Option A); update `static/app.js` to send it. Fail-before test (bare POST → 403, UI fetch pattern → 403), pass-after (UI pattern → 200, cross-origin/absent token still 403). Keep the change minimal; the full M-1 header/origin/content-type hardening lands in B2.
B1-3 **H-4**: add `FAILED` to the import in `shell/src/server.py`; regression test: a runner whose `start()` raises → state FAILED with `PROCESS_START_FAILED`, no NameError, no orphan.
Each B1 item: fail-before evidence, minimal diff, pass-after evidence, affected local suite if runnable (provision from locks if possible offline; if provisioning needs network → run what runs, park the rest E-6), changed-file list, git commit per item.

**BATCH 2:**
B2-1 **C-2**: rewrite `modules/sovereign/INSTALL-PROVENANCE.json` with the correct source hash `150e518e6d0b15b524ff61cda8fe8eb29aec6bbfb30196f9931908d12ff7ec51`; preserve the wrong record as `INSTALL-PROVENANCE.previous.json`; add a mechanical check rejecting cross-module hashes.
B2-2 **C-3**: generate current INSTALL-PROVENANCE.json for debate, sow, distillery against the candidate's actual contents; keep legacy supersession files untouched under their existing names; author `RELEASE-MANIFEST.json` enumerating per module: current + historical provenance paths, source identity, locks, artifact hashes. Validation logic reads the manifest, never globs.
B2-3 **M-1 full**: exact-origin parse, content-type + body-size in the mutation guard, standard security headers on all Token Center responses, HTTP/CSRF/lifecycle test suite.
B2-4 **M-3**: classify all 17 renderer.js innerHTML sites (static/escaped/hazard); fix any hazard via esc(); record the audit table.
B2-5 **M-5**: strip BOMs from the six governance JSONs (SYSTEM_MANIFEST.json, constitution/*.json, clu_runtime_policy.json, synthesis_contract.json) — verify each still loads via its consumers; normalize the evidence-envelope convention prospectively (document, don't rewrite history).
B2-6 **M-6**: reconcile README_PRODUCTION.md and model_hierarchy.json TO SYSTEM_MANIFEST.json; verify the `qwen3.8:27b` tag against any local Ollama manifest evidence in the tree — if unresolvable, flag in the bundle, do not guess a replacement (E-3 park for that one value).
B2-7 **OD-20 dossier (analysis only — NO implementation)**: full RESET-vs-SYSTEM SOW lineage diff (you have both ZIPs): every RESET-only, SYSTEM-only, changed file; callers/dependencies of `control_plane/canonical_registry.py` and `adapters/local/llamacpp.py`; a disposition recommendation per file. Output `bundles\BATCH2\OD-20-LINEAGE-DOSSIER.md`. H-5 code changes are HELD.
B2-8 **M-9**: add Host validation + CSP + nosniff + no-referrer to `modules/distillery/serve.py`; tests.

**BATCH 3:**
B3-1 **H-1**: reorder SOW session-manager to admission-before-spawn (or inert two-stage start); keep kill-on-deny as backstop; regression tests for deny-before-create, admit-then-create, no orphan on deny.
B3-2 **H-2**: Electron upgrade unit. Requires network for packages — if unavailable: prepare the complete upgrade plan, pinned target version, node-pty ABI plan, and the full regression band spec; park execution E-6. If available: execute, minimal diff, run the band.
B3-3 **M-4**: replace the full-timeout `WaitForSingleObject` job wait with bounded `pids()` polling; Windows-runnable test if possible, else spec + park.
B3-4 **M-7**: implement the receipts reader that fails on undeclared `ok:false`; machine-tag the deliberate-falsification receipts.
B3-5 **OD-18 hardening**: create `tests/security/` with adversarial tests matching the threat model's claimed mitigations; update THREAT_MODEL.md status column truthfully.
B3-6 **L-2** correct pytest.ini suite contract, stated host-coupled. **L-3** regenerate README/module-matrix docs from RELEASE-MANIFEST.json (fixes the Debate "v1.2 P1" label). **L-6** strip BOMs from the 15 active .py files (prospective policy per OD-21). **L-1** add metacharacter validation at .cmd-shim argv construction.

## 5. Output discipline

Per item: `bundles\BATCH<N>\<ITEM-ID>\` containing the diff/changed-file list, fail-before + pass-after evidence, test output, notes, status = CANDIDATE or PARKED(<E-class>, dossier). Per batch: `bundles\BATCH<N>\BATCH-REPORT.md`. At queue end: `bundles\LOOP-RUN-REPORT.md` — items completed CANDIDATE, items parked with reasons, T-2 queue (everything awaiting operator physical acts + OD-20), measured test counts (measured only, never expected), final worktree commit SHA, and every file you wrote. Commit per item; tag nothing.

Rules that never bend: no reviewer PASS from you; frozen archives and existing custody files untouched; claims you can't verify from bytes stay [U]; findings marked CLOSED in Punch List v2.1 §5 stay closed; end every report with:
`BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`

Begin with §0 BOOT now. Run the queue to exhaustion. Do not stop between items. Do not ask the operator anything — park and continue.

---END PASTE---

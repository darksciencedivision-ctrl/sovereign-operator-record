# RESUME PASTE — LOOP RUN 02 · SWS-REM-DIR-20260828
## For the builder seat, after quota reset (2026-09-03 04:37 UTC) or operator reassignment. Zero placeholders.

---BEGIN PASTE---

You are the BUILDER seat resuming an authorized autonomous remediation loop under directive SWS-REM-DIR-20260828 R2 and Annex A. A prior builder run completed BOOT, PHASE 0, BATCH 1, BATCH 2, and BATCH 3 items B3-1..B3-4 (15 commits, HEAD `6c7ec99`), then halted on harness quota — not on any exception. Your authorization is ENTRY 001 in `D:\producttion software 2\release-planning\OPERATOR-INSTRUCTIONS.log` (recorded 2026-08-28T01:36:53Z); it covers this resume. Do not stop between items, do not ask the operator anything; park-and-continue per Annex A E-1..E-6.

## BOOT (read-only)

Verify with `Get-FileHash`: `D:\producttion software 2\release-planning\SWS-REM-DIR-20260828-CANDIDATE-R2.md` = `9061c2e8a40bcff4258f77692ff0e23ffebde98eaa638446a22a7aa914b3691d`; `LOOP-PROTOCOL-20260828.md` = `acae8fb8b9b4d673f28ae5b6d4b74fbfed0c86e0e70bfa1f86638237219bae44`. Read `OPERATOR-INSTRUCTIONS.log` (confirm ENTRY 001), `PUNCH-LIST-V2.1-20260828.md`, and `VALIDATOR-AUDIT-LOOP-RUN-01-20260828.md` (the audit of the prior run — it defines your queue). Confirm worktree `D:\producttion software 2\release-worktree` HEAD is `6c7ec99` and `git status` shows exactly one untracked file: `modules/sow/tests/security/test_t2_harness_config_injection.py`. Any mismatch → write BOOT-FAILURE.md to custody and halt.

## Write boundaries (unchanged)

Worktree, `release-planning\bundles\`, archive sidecars only. Frozen ZIPs, existing custody files, `D:\Product Software\` untouchable.

## MANDATORY DISCIPLINE — cause of the prior run's one defect

**Never write or restore source files via PowerShell redirection (`>`, `|`, `Out-File`, `Set-Content`)** — PS 5.1 emits UTF-16 and corrupted `shell/src/server.py` in commit `3ad0720` (audit finding AUD-1). Use `cmd /c "git show ... > file"` for byte-exact restores, or Python `open(path,'wb')`. Verify encoding after any restore: first 2 bytes must not be FF FE.

## THE RESUME QUEUE — in order

**R-1 (AUD-1 repair, do first):** `shell/src/server.py` is committed as UTF-16 LE; content is intact and parses when decoded. Fail-before: capture `python -c "import ast; ast.parse(open('shell/src/server.py','rb').read())"` failing (null bytes) and first-2-bytes = FF FE. Repair with Python: read bytes, decode `utf-16`, re-encode `utf-8` (no BOM), preserve the decoded line endings, write with `open(...,'wb')`. Pass-after: ast.parse succeeds; run `python -m unittest shell.tests.test_start_worker_failure` (3/3) plus `shell.tests.test_states` from the worktree root; re-run the shell subset used in B1-3's comparison and record results (pre-existing failures per B1-3 NOTES are expected; import errors of server.py must be GONE). Commit as `R-1 (AUD-1)`. Bundle: `bundles\BATCH3\R-1-AUD-1\`.

**R-2 (finish B3-5):** the untracked T2 security test file is final and parses. Run it (`python -m pytest modules/sow/tests/security/ -v` from `modules/sow`, or unittest equivalent); jsonschema-dependent legs must skip cleanly via `pytest.importorskip`, not error. Add honest status updates to `modules/sow/docs/THREAT_MODEL.md`: rows whose claimed adversarial-test mitigation now exists move to their true status; nothing else reworded. Commit. Bundle `bundles\BATCH3\B3-5\` with fail-before rationale (tests/security/ did not exist — cite punch M-8) and run output.

**R-3 (B3-6 lows):** L-2 correct `modules/sow/pytest.ini` suite contract to measured values and state host coupling (product-byte edit, authorized in this batch). L-3 regenerate the drifted doc surfaces from RELEASE-MANIFEST.json: SYSTEM-baseline README Debate label is historical custody — do NOT touch custody; fix only worktree docs that misstate module versions (worktree README.md / shell docs citing "Debate v1.2 P1"). L-6 strip UTF-8 BOMs from the 15 active .py files listed in punch v2.1 (14 sovereign + 1 debate test); verify each still parses and its consumers load; OD-21 policy = prospective, so this is the authorized re-cut moment for the worktree copies only — the frozen Debate release ZIP is NOT re-cut. L-1 add shell-metacharacter validation at the .cmd-shim argv construction site (locate via punch L-1 / old 5.10 review notes in `modules/sow/adapters`); regression test with `& | ^ < > %` payloads. One commit per item.

**R-4:** `bundles\BATCH3\BATCH-REPORT.md` — items B3-1..B3-6 + R-1, statuses, commits, measured counts.

**R-5:** `bundles\LOOP-RUN-REPORT.md` — the whole run (both sessions): every item CANDIDATE/PARKED, commit chain, measured test totals, files written, and the consolidated T-2 operator queue: live Electron UI band (sandbox/context-isolation/navigation/window on display), SOW/Distillery/Token Center service-launch legs (G34/G35/G53/9d/9f), MT-09 signature, G26 attended debug, OD-20 ruling on `bundles\BATCH2\OD-20-LINEAGE-DOSSIER.md`, jsonschema-gated security legs (need sow venv provisioning — E-6 environment).

Rules that never bend: no reviewer PASS from you; CANDIDATE only; claims you can't verify from bytes stay [U]; punch v2.1 §5 closed items stay closed; end every report with:
`BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`

Begin with BOOT now. Run R-1 through R-5 to exhaustion, then stop.

---END PASTE---

# VALIDATOR AUDIT — LOOP RUN 01 (quota-interrupted) — 2026-08-28

Validator seat, auditing the builder's (qwen3.8-max, deepseek harness) first loop run under SWS-REM-DIR-20260828 + Annex A. The run halted mid-B3-5 on **harness quota exhaustion (HTTP 429; resets 2026-09-03 04:37 UTC)** — not on any E-class exception, and not on operator interruption. Every claim below re-verified against the disk, not the transcript.

## 1. What the run completed — VERIFIED

**Commit chain (15 commits, worktree git log matches transcript exactly):** `cf50cde` P0-2 initial → `cbaabe7` B1-1 → `29f4dc9` B1-2 → `3ad0720` B1-3 → `f1d9304` B2-1 → `66e43b5` B2-2 → `10679bf` B2-3 → `d7441dd` B2-4 → `9567d48` B2-5 → `5b29a2e` B2-6 → `7b7431f` B2-8 → `bf855e7` B3-1 → `274bb2f` B3-2 → `ef47226` B3-3 → `6c7ec99` B3-4. (B2-7 = dossier only, correctly uncommitted.)

**Spot-checked fixes, all present in the bytes:** C-1 removals (sovereign.db, .env.local, phase17c log all gone; packaging gate + allow-list in tools/release); C-2 sovereign `source_sha256` now `150e518e…` with the defective record preserved as `.previous.json`; C-3 current INSTALL-PROVENANCE.json present for debate/sow/distillery + RELEASE-MANIFEST.json at root; H-3/M-1 CSRF token endpoint + `compare_digest` server-side, `X-CSRF-Nonce` sent by app.js; H-1 admission call precedes `_ptyFactory` in session-manager.js (offsets 4252 < 4974); H-2 electron pin `^43.4.1` with N-API prebuild proven loading under ABI 148; M-5 all six governance JSONs BOM-clean. Bundles complete: PHASE0, B1-1..3, B2-1..8 (incl. OD-20-LINEAGE-DOSSIER.md), B3-1..4, with batch reports for 1 and 2.

**Notable verified wins:** the OD-20 dossier exists with full RESET/SYSTEM lineage diff (1511 matching / 167 differing / 1902 SYSTEM-only / 234 RESET-only) — ready for the operator's T-2 ruling; `qwen3.8:27b` resolved against the live local Ollama registry (not a typo — M-6's flag closes); Electron 43.4.1 landed with the 37/37 mutation-falsification harness re-pinned and 1104/1104 desktop tests green; M-4's 15.122 s → 0.120 s teardown fix measured, not estimated.

## 2. DEFECT FOUND — one, committed

**AUD-1 (HIGH): `shell/src/server.py` is committed as UTF-16 LE.** During B1-3's pre/post comparison, a PowerShell 5.1 redirection restore rewrote the file as UTF-16 (FF FE BOM, 18,781 NUL bytes), and the B1-3 repair commit `3ad0720` locked it in. Verified in the HEAD blob, not just the working copy. Consequences: (a) CPython cannot parse UTF-16 source — `python -m shell.src` fails on this worktree from `3ad0720` forward, undetected because no shell suite ran after B1-3; (b) the content itself is INTACT — it decodes as UTF-16 and parses as valid Python including the H-4 `FAILED` import, so the fix logic is right and only the byte encoding is wrong; (c) the B1-3 "identical failures pre/post" comparison evidence carries a caveat — depending on when in the sequence the corruption landed, server-importing tests in that comparison may have been import-erroring rather than exercising code. Repair is mechanical: re-encode to UTF-8 (no BOM, original line-ending convention), re-run the H-4 regression (3 tests) plus a shell-suite subset, commit with fail-before evidence (the UTF-16 state IS the fail-before). Scanned all 56 changed files across the 15 commits: **server.py is the only UTF-16 casualty.**

## 3. Interrupted state

Uncommitted: `modules/sow/tests/security/test_t2_harness_config_injection.py` (5,321 B) — the builder's final edit landed; the file parses clean. Not yet run, not committed, THREAT_MODEL.md status row not yet updated. Missing entirely: B3-5 completion, B3-6 (L-2 pytest.ini, L-3 doc regeneration, L-6 BOM strip of 15 active .py files, L-1 metacharacter validation), BATCH3/BATCH-REPORT.md, bundles/LOOP-RUN-REPORT.md.

## 4. Remaining queue for the resume run

R-1 Repair AUD-1 (re-encode server.py UTF-8, regression + shell subset rerun, commit). R-2 Finish B3-5 (run the T2 security suite, jsonschema-gated legs skip cleanly offline; update THREAT_MODEL.md statuses; commit). R-3 B3-6 lows. R-4 BATCH3 report. R-5 LOOP-RUN-REPORT.md with measured counts and the T-2 queue (live Electron UI band, service launches, MT-09, G26, OD-20 ruling). Discipline note for the resume seat: **never restore or write source files through PowerShell redirection or `Set-Content` — that is what caused AUD-1; use `cmd /c` byte redirection or Python `open('wb')`.**

## 5. Standing state

All work remains CANDIDATE; no PASS exists anywhere; reviewer backlog (19 gates + Gate-5/5b) untouched and still the largest open front. The builder held role discipline throughout — including refusing to pip-install into the host Python when it hit the missing-jsonschema wall, correctly citing the write boundary. The parked/T-2 queue is accumulating as designed.

VALIDATOR CLAIM: no gate submitted, no PASS asserted. AUD-1 is reported as a finding, not repaired by the validator.

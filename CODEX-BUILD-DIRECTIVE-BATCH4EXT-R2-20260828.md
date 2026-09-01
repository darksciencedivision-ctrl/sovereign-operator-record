# CODEX WORK DIRECTIVE — BATCH 4-EXTENDED · REVISION R2 (surgical)
## SWS-REM-DIR-20260828 · builder: codex · ENTRY 004 · supersedes the R1 paste (kept as history)

**Operator:** paste between the markers into Codex, working directory `D:\producttion software 2\`, full file access. ENTRY 004 already recorded — nothing to fill.

---BEGIN PASTE---

You are the BUILDER seat `codex (local harness)`, executing Batch 4-Extended under SWS-REM-DIR-20260828 R2 + Annex A, authorized by OPERATOR-INSTRUCTIONS.log ENTRY 004. You audited this tree as the ENTRY-003 auditor; your findings are now your work orders. You build; you never grade your own work; you never write reviewer PASS; everything is CANDIDATE. No questions to the operator; park-and-continue per E-1..E-6.

## §0 BOOT — all five checks pass or you write BOOT-FAILURE.md to custody and stop

1. `OPERATOR-INSTRUCTIONS.log` contains ENTRIES 001–004; ENTRY 004 grants exactly: `P-D2 P-D3 P-D4 P-D5 P-D6 N-11 N-14 N-15 N-13`, `OD-22: ANNOTATE`, builder=codex.
2. SHA-256: `SWS-REM-DIR-20260828-CANDIDATE-R2.md` = `9061c2e8a40bcff4258f77692ff0e23ffebde98eaa638446a22a7aa914b3691d`; `LOOP-PROTOCOL-20260828.md` = `acae8fb8b9b4d673f28ae5b6d4b74fbfed0c86e0e70bfa1f86638237219bae44`.
3. Worktree `D:\producttion software 2\release-worktree`: `git rev-parse HEAD` = `5a529462d4d626c4d201be00f4de84245913a3e9`, branch main; porcelain shows nothing tracked-modified (ignored files permitted).
4. Read in full: `ADJUDICATION-RECORD-R3-20260828.md`, `PUNCH-LIST-V3-20260828.md`, `UI-VISUAL-BASELINE-SPEC-20260828.md`, `design\UI-TARGET-20260828.png`.
5. Confirm `audit-codex\` exists; it is now FROZEN — you never write there again.

## §1 STANDING RULES — violations are batch failures, not style points

R-1 **Write set**: `release-worktree\` and `release-planning\bundles\` only. Frozen ZIPs, sidecars, custody documents, `audit-codex\`, `D:\Product Software\`: read-only forever.
R-2 **Encoding**: never write/restore source via PowerShell redirection or `Set-Content`. Python `open(...,'w',encoding='utf-8')` / `'wb'`, or `cmd /c` redirection. After every item: no changed file begins `FF FE`/`FE FF`; new text files are UTF-8 no BOM, LF.
R-3 **No invented APIs**: before calling or asserting any function/signature/config key, read its definition in the tree. If source and your assumption disagree, the source wins and the bundle records the correction.
R-4 **Coupled values move together**: any file that pins a hash/version/count of another file (BUILD-MANIFEST, config-integrity pins, RELEASE-MANIFEST, mutation-harness PINNED_BASELINE, pytest contracts) is updated in the same commit as the change that invalidated it, following that file's own documented re-pin protocol where one exists.
R-5 **Suite side-effects**: the shell test suite REWRITES `shell/BUILD-MANIFEST.txt` and `evidence/hardening/h13..h16-*.txt` when run (proven in runs 01 and audit). After ANY shell-suite invocation: `git status`; evidence-lane rewrites are restored byte-exact from HEAD (`cmd /c "git show HEAD:<path> > <path>"`, then hash-verify); only intended changes may remain staged.
R-6 **Test hygiene**: `PYTHONDONTWRITEBYTECODE=1` (or `python -B`) for every Python run; any bytecode/caches you create are deleted before the item's commit.
R-7 **Minimal diff**: nothing outside the item's named files except what R-4 forces; every forced coupling is listed in NOTES.md.
R-8 **Per item**: fail-before captured → fix → pass-after captured → measured suite counts → changed-file list → `git status` clean-except-intended → one commit `B4E <id>: <summary>` → `bundles\BATCH4\<id>\NOTES.md`.
R-9 **Park, don't improvise**: pass criteria you cannot check mechanically, or an unexpected tracked-byte difference → park with dossier, continue queue. Never repair outside scope.
R-10 **No network, no billed calls, no GPU, no service launches, no model loads.** Every item below is static/hermetic.

## §2 QUEUE — strict order; 9 is last; nothing follows 9

### 1 · P-D2 (HIGH) — process_tree direct-exec import
Target: `modules/sow/adapters/frontier/process_tree.py` line 15 (`from adapters.cmd_shim import assert_cmd_shim_argv_safe`). `modules/sow/adapters/cmd_shim.py` is NOT modified.
Fail-before: from `modules\sow`, run your audit probe (a clean `run_managed_process` of a trivial command via the direct-execution job-member path); capture `ModuleNotFoundError: No module named 'adapters'`, returncode 1, empty stdout.
Fix (this shape, nothing broader):
```python
try:
    from adapters.cmd_shim import assert_cmd_shim_argv_safe
except ImportError:  # direct execution: package root not on sys.path
    import importlib.util as _ilu; from pathlib import Path as _P
    _spec = _ilu.spec_from_file_location("cmd_shim", _P(__file__).resolve().parents[1] / "cmd_shim.py")
    _mod = _ilu.module_from_spec(_spec); _spec.loader.exec_module(_mod)
    assert_cmd_shim_argv_safe = _mod.assert_cmd_shim_argv_safe
```
Pass-after, all captured: (a) clean managed run → returncode 0, non-empty stdout, `pids()` empty after; (b) M-4 timing probe (`bundles\BATCH3\B3-3\probe_m4_timing.py`) genuine success sub-second; (c) `test_cmd_shim_metacharacters` 17/17 on host Python AND `py -3.12`; (d) one hostile argv (`C:\x\%COMSPEC%^&whoami`) refused THROUGH the managed path with the exact metacharacter error; (e) NEW integration test `modules/sow/tests/integration/test_managed_clean_success.py` — spawns a trivial clean command via `run_managed_process`, asserts rc 0 + output + no survivors + guard still raises on hostile argv; runs green on both Pythons. Commit.

### 2 · N-15 — shell pre-flight npm detection
Read `shell/src/probe.py` first (R-3); locate the npm probe that yields `[WinError 2]` (it execs `npm` as an .exe; npm is a `.cmd` shim — PATHEXT resolution missing).
Fail-before: invoke the existing probe function on this host; capture the WinError result verbatim.
Fix, minimal: resolve via `shutil.which("npm")` (covers PATHEXT). If the probe's contract needs a version string: exec the RESOLVED path; if it ends `.cmd`/`.bat`, argv = `["cmd.exe","/d","/c",resolved,"--version"]`, validated by `assert_cmd_shim_argv_safe` (import per the item-1 robust pattern), `shell=False`, timeout ≤10 s, stdout captured. No other probe touched.
Pass-after: probe reports npm + version on this host; NEW regression test (temp dir on PATH containing a fake `npm.cmd` emitting a version → detected; hostile-charactered resolved path → refused); every existing `shell/tests/*` file that exercises preflight/probe runs green (measured; apply R-5 restoration after). Commit.

### 3 · N-11 — Debate default seat VRAM fit
Facts: live-measured `phi4:14b` (12 GB) on the 8 GB card → 51/49 CPU/GPU split, ~5 GB idle residency. Target default: `qwen3:8b` (installed, fully resident).
Procedure: `rg -n "phi4" modules/debate/` → enumerate EVERY hit in the bundle, classified: default-seat config (change), coupled pin/test (re-pin per its own protocol), historical doc/evidence (untouched). Change only default-seat config sites; if `modules/debate/tests/test_v1_2_1_config_integrity.py` pins config hashes, follow its documented re-pin steps in the same commit (R-4) and say so.
Pass-after: `rg "phi4" modules/debate/` shows only the untouched historical class; Debate config/integrity test file(s) green (measured, `py -3.12`); NO model loaded (verification of actual load = T-2 leg, QUEUED-T2 in NOTES). Bundle carries the release-notes line: "default seats sized for full residency on 8 GB cards; final ruling folds into OD-9." Commit.

### 4 · P-D4 — shell BUILD-MANIFEST refresh
Read the regeneration mechanism first: `rg -n "BUILD-MANIFEST" shell/tests/` — the suite rebuilds it (run 01 observed 62→63 entries). Regenerate via that mechanism (preferred) or, if it is inseparable from a full suite run, run the minimal test that rebuilds it and apply R-5 to everything else it touches.
Pass-after: script in the bundle verifying EVERY manifest line against measured hashes — zero mismatches (the `src/server.py` line must now read `54d391c591ca16371d8d89de69ddede0be89e2cbe38a5c2640d09bc1829e9154`); any other stale line found is corrected and listed. Commit.

### 5 · P-D5 — v1.2 P1 corrections (OD-22 = ANNOTATE)
(a) `BUILD-DIRECTIVE-SWS-UI-001.md` line 79: replace the row's Debate cells with: `Debate Table v1.2.1-hardening` | `Debate_Table_v1.2.1_Hardening_20260823_143520.zip` | `sha256 d03ba417914e1465898f5144cc5735afb92f7f6da5e846e0a968fe48c531d265 — VERIFIED`.
(b) Prepend to `docs/ADR-003.md` AND `docs/DISCOVERY.md`, as the first line, exactly:
`> Historical record of the v1.2-P1 era (annotated 2026-08-28 per OD-22): Debate is v1.2.1-hardening as of RELEASE-MANIFEST.json; original text preserved below.`
Nothing else in either file changes (diff = 1 added line each).
Pass-after: `rg -n -i "v1\.2\s+P1" --glob "*.md" --glob "*.txt"` from worktree root → hits ONLY in the two annotated files and quarantined evidence/dev lanes. Commit.

### 6 · P-D6 — hygiene + gate policy
Delete (untracked only): every `__pycache__/`, `*.pyc`, `.pytest_cache/`, and test-created `*.sqlite`/`*.db` outside quarantined evidence lanes. KEEP `node_modules` (desktop suite dependency; gitignored; absent from archives by construction). Then, captured: gate vs clean archive — `git archive HEAD` to a temp dir → `package_boundary_gate.py` → exit 0, 0 violations; gate vs live tree → captured as-is with the node_modules-dominated count. NEW file `tools/release/README-GATE-POLICY.md` (≤15 lines): release verdict binds to the clean archive of the release commit; live-tree scans are operational hygiene. Commit (README only; deletions are untracked).

### 7 · N-13 — adapter path rebase (config-driven)
Step 1 — inventory (bundle artifact `adapter-path-inventory.json`): for each of the seven `shell/modules/*.json`, every field whose value contains `D:/Product Software` or `C:/Users/Sslaw`, with JSON-pointer paths. The rebase mapping is built FROM this inventory — no blind string replace.
Step 2 — `shell/config/install.json` (NEW): `{"modules_root": "D:\\producttion software 2\\release-worktree", "python_312": "<output of: py -3.12 -c \"import sys;print(sys.executable)\">"}` (measure it, don't guess).
Step 3 — `tools/release/rebase_adapters.py` (NEW): reads install.json; rewrites exactly the inventoried fields by mapping old workspace-root prefix → `modules_root` and the hard-coded python.exe → `python_312`; backs up each adapter as `<name>.json.pre-rebase` (supersession convention, preserved); idempotent (second run = no-op, verified); `--dry-run` prints the full field diff; refuses (exit 2, no writes) any target path that does not exist on disk.
Step 4 — execute against this worktree. Post-conditions, all captured: `rg "Product Software|Users/Sslaw" shell/modules/*.json` → zero hits (backups excluded); dry-run-after = no-op; NEW test `tools/release/test_rebase_adapters.py` covering mapping, idempotency, refusal, backup creation — green.
NOTES must state: paths are now candidate-correct; module ENVIRONMENTS (venvs) are not provisioned by this item (E-3/T-2 prerequisite before worktree modules can start). Commit.

### 8 · N-14 — UI visual baseline
Governed by `UI-VISUAL-BASELINE-SPEC-20260828.md` + `design\UI-TARGET-20260828.png`. Step 1 (mandatory, before any edit): static delta audit — read `shell/` templates/static CSS/JS, compare against the spec's layout and tokens section by section; write `bundles\BATCH4\N-14\delta-table.md` (spec element | present in tree | delta). The capture is from shell build 2026-08-21 — the theme LIKELY EXISTS in this lineage; expect the delta to be small or empty. If empty: item closes as PARITY-VERIFIED, no code, evidence only. If non-empty: implement ONLY the listed deltas under the spec's hard constraints — assets local/self only (background image as local static file, hash recorded), CSP + security headers byte-identical (diff the header-emitting code paths to prove it), presentation-only, contrast preserved. Pass-after: shell suite green (measured, R-5 applied); served-asset inventory with zero external references; before/after template/CSS diff. Screenshot parity = QUEUED-T2. Commit (or evidence-only close).

### 9 · P-D3 (LAST — no byte changes after this item)
a. Regenerate provenance for debate/sow/distillery via `tools/release/generate_module_provenance.py` against the FINAL tree; prior records superseded per convention, previous files preserved.
b. Regenerate `RELEASE-MANIFEST.json`: every embedded digest measured fresh — including sow `package-lock.json` (pre-batch actual `6a79bbcc…`; measure again now) and all files this batch touched.
c. Run ALL gates with these invocations (adjust only if a tool's own --help contradicts; R-3), each captured:
`python tools\release\package_boundary_gate.py` (vs clean `git archive HEAD` extraction) · `python tools\release\provenance_cross_hash_check.py --registry tools\release\module_source_registry.json` · `python tools\release\release_manifest_check.py` · `python tools\release\check_governance_bom.py` · `python tools\release\check_model_consistency.py` (SOVEREIGN-scoped; N-11 touched Debate, not SOVEREIGN — must still pass unmodified; if it unexpectedly covers Debate, update its registry inputs minimally, R-4) · `python tools\release\innerhtml_sink_audit.py --file modules\sow\apps\desktop\renderer\renderer.js --ledger tools\release\innerhtml_audit.json` · the full release-tool unittest aggregate (every `tools/release/test_*.py`).
d. **The aggregate and every gate must be green.** Any failure: fix within batch scope, regenerate a+b again, re-run c. Manifests describe final bytes, always.
e. Append to the batch report verbatim: "Any batch that changes module bytes ends with provenance/manifest regeneration and a full gate re-run; a manifest generated before the last byte change is stale by definition." Commit.

## §3 OUTPUT

`bundles\BATCH4\<id>\` per item (fail-before, diff stat, pass-after, measured counts, NOTES.md incl. R-4 couplings and any parked sub-leg). `bundles\BATCH4\BATCH-REPORT.md`: item table (id | commit | CANDIDATE/PARKED/PARITY-VERIFIED | measured counts), commit-chain tail from `5a52946`, T-2 additions (N-14 screenshot parity, N-13 venv provisioning, N-11 live-load check). Append a `BATCH 4-E` section to `bundles\LOOP-RUN-REPORT.md`; rewrite nothing above it. Final state: clean porcelain, no FF FE in any changed file, no reviewer PASS anywhere. End every report with:
`BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`

Execute §0 now, then the queue in order to exhaustion. Stop after item 9.

---END PASTE---

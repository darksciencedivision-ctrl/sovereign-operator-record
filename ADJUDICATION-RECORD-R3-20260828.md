# CROSS-SEAT ADJUDICATION — ROUND 3 — CODEX AUDIT — 2026-08-28

Validator seat, adjudicating the Codex auditor's report (ENTRY 003 seat) on loop runs 01+02. Every Codex discrepancy bearing on current state was independently re-derived from the disk before acceptance. Verdicts here supersede conflicting statements in earlier records; nothing is rewritten.

## 1. Codex audit quality — ACCEPTED AS SOUND

Integrity program (§1) fully corroborates the custody chain: all seven reference hashes, all three sidecars, the 21-commit chain, the append-only log through ENTRY 003, and an independent Phase-0 re-derivation that reproduced `cf50cde`'s exact tree hash (`c8ae569d…`) from the frozen ZIP — the strongest reproducibility proof this program has. Its falsification legs worked as designed: planted foreign hash rejected, planted undeclared `ok:false` receipt rejected, independent hostile payload `C:\audit\%COMSPEC%^&whoami` refused with exact metacharacter identification. Boundary conduct: one disclosed transient side-effect (the shell suite's self-writing evidence files, restored byte-exact with hashes recorded, clean final status) — acceptable with disclosure; this is the same suite behavior run 01 hit, now documented twice. The review-package patch verification (byte-identical after normalizing only the git-version signature across 2.34.1→2.46) is accepted.

## 2. VALIDATOR ERRATUM — my run-02 audit characterization is superseded

VALIDATOR-AUDIT-LOOP-RUN-02 stated "clean — zero defects found." Codex's per-claim verification of that audit found the *measurements* exact and the *characterization* wrong, and my own re-derivation today confirms it. What my audit actually established was that the R-1..R-5 queue items did what they claimed; it did not hunt for regressions introduced BY those items or for staleness cascades across items. Two of each existed. The corrected characterization: **run 02 completed its queue faithfully; the final HEAD is not internally release-consistent.** The prior audit file stands as history; this section supersedes its verdict line.

## 3. Discrepancy adjudication — all six re-derived

| ID | Codex verdict | Validator re-derivation | Adjudicated status |
|---|---|---|---|
| D-1 | HIGH, historical, repaired | consistent with R1/R2 records | ACCEPTED — closed history |
| D-2 | HIGH, current | **REPRODUCED**: `process_tree.py:15` `from adapters.cmd_shim import …` fails `ModuleNotFoundError: No module named 'adapters'` when the file is executed directly (the Windows job-member path); reproduces even on Linux. The 0.142 s "fast" M-4 timing at HEAD is a fast *failure*. The 17 L-1 tests cover rejection, not clean success — a test-scope hole | **ACCEPTED — OPEN HIGH.** New punch item P-D2 |
| D-3 | HIGH, current | **REPRODUCED**: RELEASE-MANIFEST pins lock `573eba33…`, actual lock is `6a79bbcc…` (post-Electron); the three provenance records' embedded source digests describe the mid-loop tree, not HEAD (L-6 BOM strips and B3 additions changed module contents after B2-2 generated them) | **ACCEPTED — OPEN HIGH.** Root cause is a process gap, not a bad record: manifest/provenance generation ran mid-queue and was never re-run after the last byte change — exactly the "no stale evidence for later bytes" rule the directive states for tests, unapplied to manifests. New punch item P-D3: regeneration becomes a mandatory terminal step of any batch that changes module bytes |
| D-4 | MEDIUM, current | **REPRODUCED**: BUILD-MANIFEST.txt pins `6eeb30e3…` (the corrupted pre-R-1 blob); actual server.py is `54d391c5…` | **ACCEPTED — OPEN MEDIUM.** P-D4. Note the irony worth preserving: the build manifest currently authenticates the UTF-16 corruption |
| D-5 | LOW, current | **REPRODUCED**: three `v1.2 P1` claims remain — BUILD-DIRECTIVE-SWS-UI-001.md:79 (a live operator-statement table row), docs/ADR-003.md:9 and docs/DISCOVERY.md:70 | **ACCEPTED WITH A SPLIT.** The loop report's absence claim is false as stated — DISCREPANCY stands. But ADR-003 and DISCOVERY are dated decision/discovery records of the v1.2-P1 era; §8 evidence discipline argues for annotating (a superseded-by note), not rewriting them. The BUILD-DIRECTIVE row is live and should be corrected. Split goes to the reviewer/operator as OD-22 |
| D-6 | MEDIUM, operational | consistent: committed HEAD archive passes the gate 0-violation; live tree fails on 1,255 ignored files (1,001 node_modules from the authorized B3-2 npm install, 177 bytecode, rest caches) — nothing committed | **ACCEPTED — OPEN MEDIUM, operational.** P-D6: a pre-packaging cleanup step (or gate-on-archive-only policy statement). The 62/64 release-tool aggregate is fully explained by D-3 + D-6; no third defect hides in it |

Gap-hunt items adjudicated: repository-wide BOM inventory (198 tracked; 13 outside evidence/dev classification incl. the UTF-16 `WORKSPACE-RESOLVED-LOCK.txt`) — folded into OD-21's prospective policy, not new work now; secrets scan — all fixture-valued, consistent with three prior scans; the single pre-existing NUL in an evidence-lane inspector copy — historical, no action.

## 4. Updated program state

Open engineering set after three audit layers: **P-D2 (HIGH), P-D3 (HIGH), P-D4 (MED), P-D6 (MED), P-D5/OD-22 (LOW, split disposition)** — plus the standing holds (H-5/OD-20, H-6, T-2 physical queue) and the untouched reviewer backlog (19 gates + Gate-5/5b). Everything else from Punch List v2.1 remains CANDIDATE with three independent seats now agreeing on the per-item evidence.

Proposed **BATCH 4** (requires an operator scope grant per Annex A): P-D2 fix (import made path-robust or deferred into the guarded call site; regression test = clean `run_managed_process` succeeds AND hostile argv still refused — closing the test-scope hole Codex identified), P-D4 build-manifest refresh, P-D5 BUILD-DIRECTIVE row correction + historical-doc annotation per OD-22 ruling, P-D6 cleanup, and **P-D3 last** — regenerate provenance records + RELEASE-MANIFEST against the final post-fix tree, then re-run all release gates, so the manifests describe the bytes that exist when the batch ends. Grant line for the operator, verbatim:

```
OPEN SWS-REM-DIR-20260828 BATCH-4: P-D2 P-D3 P-D4 P-D5 P-D6 / OD-22: <annotate|rewrite> historical docs / UTC: <ts>
```

## 5. What three audit layers now demonstrate

Builder → validator → independent auditor each caught what the previous layer missed: run 01's corruption (validator), the validator's overbroad clean bill (auditor). Each layer's *measurements* have survived every re-derivation; only *characterizations* have needed correction. That is the intended failure mode of this architecture — errors surface at the next seat instead of compounding — and it is the strongest argument the eventual ratification report can make.

VALIDATOR CLAIM: no gate submitted, no PASS asserted, no scope opened. Batch 4 awaits the operator's grant line.

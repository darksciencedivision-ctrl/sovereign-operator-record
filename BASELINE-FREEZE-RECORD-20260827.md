# BASELINE FREEZE RECORD — 2026-08-27

Status: RECORD OF FACT (measured, not asserted)
Compiled: 2026-08-27, from `D:\producttion software 2` on desktop-03ptabh
Method: SHA-256 computed on-device with `sha256sum`; ZIP internals read with Python `zipfile` without extraction. No file in the folder was modified.

## Frozen input packages

All three packages are preserved as immutable inputs. **No candidate is designated by this record** — candidate designation is an operator decision (see OD-1 in the execution plan).

| File | SHA-256 | Size (bytes) | Sidecar |
|---|---|---|---|
| SOVEREIGN_RESET_BASELINE_20260827T193540Z.zip | `00ae9eefc7dada37943bb08a305020c73e2c029a6398858e8bba27078c349a7c` | 15,759,737 | PRESENT — MATCHES |
| SOVEREIGN_RESET_BASELINE_20260827T193540Z - Copy.zip | `00ae9eefc7dada37943bb08a305020c73e2c029a6398858e8bba27078c349a7c` | 15,759,737 | (byte-identical to above) |
| SOVEREIGN_SYSTEM_BASELINE_20260827.zip | `75e4be075dcb5629c3175850f5aa3f342c7c693f2967c3620d6cd4914ac27138` | 29,047,584 | **ABSENT** |
| SOVEREIGN_RAW_SOURCE_BASELINE_20260827.zip | `8594fdc266fee721c9ef69d5595d5bab5402d2ce00de5ee65720994d4efafc9c` | 3,960,712 | **ABSENT** |

Also present: `SOVEREIGN_BASELINE_REPORT_20260827.pdf` (19,799 B), `SOVEREIGN_RESET_BASELINE_REPORT_20260827.pdf` (13,654 B), `SOVEREIGN_RESET_BASELINE_20260827T193540Z.zip.sha256` (112 B).

## Package identities (read from inside the archives)

**RESET baseline** (created 2026-08-27T19:35:40Z): source-first snapshot of `D:\Product Software\Production Workspace`. 4,141 entries under one wrapper directory. Integrated tree retained **byte-faithful with Debate v1.2** installed; Debate v1.2.1-hardening supplied only under `LATEST_MODULE_SOURCES/`. Self-describes as "not a ratified production release"; carries `SOURCE_MANIFEST.sha256`, `VERIFY-BASELINE.ps1`, `SOURCE_SELECTION.json`, package report PDF.

**CONSOLIDATED pair** (created 2026-08-27T23:05:06Z, ~3.5 h later): SYSTEM (3,587 entries) + RAW_SOURCE (974 entries) cut from `D:\SOVEREIGN_BASELINE_20260827`, selection rule "newest upstream per module (operator decision, 2026-08-27)". Integrates **Debate v1.2.1-hardening as the installed module** (release ZIP sha256 `d03ba417…`, manifest 51/51 verified, director-accepted 2026-08-23), flagged `advanced_past_integration_test: true`. SOVEREIGN SPA TypeScript source recovered from the build-verify tree and added 2026-08-27 (proof: `dist/assets/index-DU3oGUhs.js` byte-identical, sha256 `e0ec572e…`).

## Defects confirmed from the frozen bytes (independent re-check, not carried forward)

1. `Production Workspace/evidence/GATE-LEDGER.json` (RESET): 8 gates PASS/reviewer, 1 PASS/operator, 1 STOP/reviewer, **19 CANDIDATE with `evaluated_by: null`** (7a, 7b, 8a–8j, 9c–9h, 9j — no 9i entry exists). Matches punch-list item 1.1.
2. `Production Workspace/modules/sovereign/INSTALL-PROVENANCE.json` (RESET): `source_sha256: 620e8459c74fb5fe9d2dfe0c4693cea02b8346292804d2d2a01fb71cb615be96`, which `SOURCE_SELECTION.json` records as the hash of `SOVEREIGN_DISTILLERY_ENTERPRISE_20260821T011825Z_5ff6f56e.zip`; the SOVEREIGN enterprise ZIP it names hashes to `150e518e…`. **Wrong-module provenance hash confirmed.** Matches punch item 3.5 / directive Phase 2.
3. `SOURCE_SELECTION.json` archive validation: `Debate_Table_v1.2_Phase1_Production_20260811_201116 - Copy.zip` computes `29b364b0…` against sidecar expectation `be6cfe8c…` — **sidecar mismatch confirmed**; both Distillery ZIP sidecars match.

## Immutability declaration

From this record forward, planning treats all three ZIPs and their two report PDFs as read-only inputs. Any remediation work occurs in a separate operator-designated worktree on an extracted copy. Missing sidecars for the consolidated pair are a Phase-0 action (generate and record), not a reason to modify the ZIPs.

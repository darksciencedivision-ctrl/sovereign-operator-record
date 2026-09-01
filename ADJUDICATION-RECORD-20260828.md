# CROSS-SEAT ADJUDICATION RECORD — 2026-08-28

Role: validator seat. Inputs adjudicated: the Grok builder report (Phase-0 freeze + deep review) and the ChatGPT deep review (findings N-1…N-10, staleness reclassification), both against the frozen `SOVEREIGN_SYSTEM_BASELINE_20260827.zip` (`75e4be07…ac27138`). Every verdict below was re-derived from the ZIP bytes on the operator's own disk today — not accepted from either report. Where a claim could not be checked from the frozen bytes, it is marked UNVERIFIABLE-HERE, not endorsed.

## A. Claims CONFIRMED against the bytes

| Claim (source) | Byte evidence | Verdict |
|---|---|---|
| N-1 runtime state packaged (ChatGPT) | `modules/sovereign/runtime/sovereign.db` + `runtime/evidence/quick/session_9798…/quick-20260825…/{prompt.txt, raw_generation.json, accepted.txt, evidence.json}` present in the ACTIVE module tree | **CONFIRMED — CRITICAL.** Correction below (A′) narrows one detail. |
| `.env.local` packaged (both) | `modules/sovereign/ui/ui_shell/.env.local`, content exactly `VITE_SOVEREIGN_BACKEND_URL=http://127.0.0.1:5175` | CONFIRMED — no secret, boundary violation stands |
| N-4 shell FAILED NameError (ChatGPT) | `shell/src/server.py` imports `ModuleRunner, EXTERNAL, READY, DEGRADED, STARTING` — **no FAILED** — and line 329 executes `runner._set(FAILED, …)` in the `except` path | **CONFIRMED — real bug.** A failing `runner.start()` raises a secondary `NameError` instead of recording PROCESS_START_FAILED |
| N-3 Token Center refresh 403 (ChatGPT) | `piggybank.py:823–831` rejects empty `X-CSRF-Nonce`; `static/app.js:264` sends `fetch("/api/refresh", {method:"POST"})` with no header; string `X-CSRF` absent from app.js entirely | **CONFIRMED — the UI's own Refresh cannot succeed** |
| N-5 nonce not validated (ChatGPT) | The only nonce logic in piggybank.py is get + non-emptiness; no generation, no expected-value comparison anywhere in the file | CONFIRMED |
| 5.1 spawn-before-admit still OPEN (both) | `session-manager.js`: `_ptyFactory(` at line 69, `.admit(` at line 103 | CONFIRMED OPEN — HIGH |
| 5.2 Electron 31 still OPEN (both) | `apps/desktop/package.json` devDependencies `electron: ^31.0.0` | CONFIRMED OPEN — HIGH |
| 5.3 STALE/closed (both) | `main.js`: `will-navigate` at 2550/2554, `setWindowOpenHandler` at 2553 | CONFIRMED CLOSED in these bytes — remove from open list |
| No current INSTALL-PROVENANCE for debate/sow/distillery (Grok) | SYSTEM active modules carry only `debate/INSTALL-PROVENANCE.previous.json`, `sow/INSTALL-PROVENANCE.previous.json`, `distillery/INSTALL-PROVENANCE.json.previous`; current files exist only for sovereign (wrong hash) and tokencenter | CONFIRMED — and note the **inconsistent supersession naming** (`.previous.json` vs `.json.previous`), which will defeat naive glob checks |
| N-2 SOW lineage divergence (ChatGPT) | `control_plane/canonical_registry.py` exists ONLY under `evidence/cpm1/before-b/…` capture, not in active `modules/sow`; no SOW llamacpp adapter anywhere in SYSTEM (only `shell/modules/llamacpp.json`) | CONFIRMED — canonical SOW source authority unresolved |
| Zero LICENSE/COPYING files (both) | 0 matches across the entire SYSTEM archive | CONFIRMED |
| Distillery still 1.1.0rc3 (both) | `pyproject.toml` `version = "1.1.0rc3"`; `docs/VERSION_MATRIX.json` contains 1.1.0rc3 | CONFIRMED — W-4 coupling intact, unexecuted |
| Gate ledger state (all three seats) | SYSTEM `evidence/GATE-LEDGER.json`: 29 rows — 8 PASS/reviewer, 1 STOP/reviewer, 1 PASS/operator, 19 CANDIDATE/`evaluated_by: null`; 9a/9b/9i absent | CONFIRMED — identical to RESET's ledger |
| N-9 Distillery serve.py headers (ChatGPT) | No CSP, no nosniff, no Host validation in `modules/distillery/serve.py` | CONFIRMED — LOW/MED given GET-only loopback |
| README drift (Grok) | SYSTEM `README_BASELINE.md` still labels `debate/ Debate Table v1.2 P1` after the v1.2.1 swap | CONFIRMED |
| 5.7 threat-model disclosure (ChatGPT "closed via disclosure") | `modules/sow/docs/THREAT_MODEL.md` now carries implementation-status markers including not-implemented; **but `modules/sow/tests/security/` still does not exist (0 entries)** | **SPLIT VERDICT:** the honesty half is repaired; the adversarial-test gap remains an open operator disposition, not a closure |

## A′. Corrections to confirmed claims

1. ChatGPT reported `sovereign.db-shm` / `sovereign.db-wal` packaged. **Not present** — only `sovereign.db`. The finding stands on the .db + prompt/generation evidence; the sidecar-file detail is an overclaim.
2. ChatGPT located SOW mutable state (`.approvals/`, `.recovery/`, `%SystemDrive%` caches) in "the Consolidated SOW package." Byte check: all `.approvals`/`.recovery`/cache hits live under **frozen evidence captures** (`evidence/cp01/before/…`, `evidence/cpm1/before-*/…`) — not the active module tree. The one active-tree leak is `modules/sow/apps/desktop/docs/loop/logs/phase17c_iter90_main.log`. This matters for remediation: evidence captures are deliberately historical and should be quarantined-as-evidence, not "cleaned," while the active-tree log is a genuine packaging defect. Grok's placement ("cache .db under evidence/") was the accurate one.
3. BOM counts: 37 `.py` files in the SYSTEM archive open with a UTF-8 BOM. Most sit in evidence captures, but two locations are consequential: `dev/v1.2.1-hardening/release/staging/Debate_Table_v1.2.1_Hardening_20260823_143520/tests/test_v1_2_1_config_integrity.py` — **inside the director-accepted Debate release artifact** — and its worktree twin. Grok's "one Debate test" is confirmed and is more significant than either report noted: the flagship release object is not encoding-clean.
4. 5.4 renderer sinks: the two filed sinks (rail :456, minimized :504) are repaired — the W-36 (R-54) comment documents the fix and interpolations route through `esc()` (e.g. line 507). **The filed finding is CLOSED.** However renderer.js contains 17 `innerHTML` sites total; several are static or esc()-wrapped, but no systematic audit of all 17 exists. Grok's "other innerHTML sites remain" and ChatGPT's "closed" are both right at different scopes — new item M-3 captures the residual.

## B. Claims UNVERIFIABLE from the frozen bytes (not endorsed, not rejected)

- Grok's git pin `d746010b…` and read-only copies under `/home/workdir/artifacts/immutable/` — exist only on Grok's ephemeral Linux sandbox. See D-2.
- ChatGPT's 5.6 closure ("receipt-reader tests and gate machinery now exist"): active SOW carries `selfcheck/receipt-path.js` and the receipts corpus, but I found no reader that fails a build on an undeclared `ok:false`. Status: **REVERIFY, not closed.**
- Both seats' full-file syntax sweeps (1,055/0 Python, 630/0 JS), the SOW ~850/105/17/14 lineage diff, the 190 fixture-shaped secret hits, RESET manifest 3291/0 — plausible, consistent with my spot checks, accepted as [U] pending reviewer rerun.
- All live-tree, Windows-host, and runtime claims (both seats correctly declined to fabricate these).

## C. Where the seats disagree — resolutions

| Disagreement | Resolution from bytes |
|---|---|
| 5.4: Grok "partially stale" vs ChatGPT "closed" | Filed finding CLOSED; residual sink audit is a NEW item (M-3), not a reopening |
| 5.7: ChatGPT "closed via disclosure" vs punch list "write tests or retract" | Disclosure half done; `tests/security/` absent. Operator must either accept disclosure as the closure or order the tests (OD-18) |
| 5.6: ChatGPT closed vs Grok silent | REVERIFY — evidence of machinery exists, the failing-check behavior is unproven |
| Reset entry counts (Grok 3,292 files vs 4,141 entries) | Both right: 4,141 ZIP entries include directories; ~3,29x are files. No contradiction |
| Candidate choice: both recommend SYSTEM | Consistent, and consistent with my C-1 analysis — but it remains **OD-1**, an operator designation, and pasting Grok's token WOULD resolve OD-1 implicitly (see D-1) |

## D. Defects in Grok's proposed Phase-0 authorization token

The draft token is close to right and should not be pasted as-is:

1. **D-1 — it silently resolves OD-1.** The token names `75e4be…` as the immutable ZIP while Grok separately asks "say explicitly whether the writable tree is SYSTEM or RESET." Pasting it designates SYSTEM. That is a fine decision to make — but it should be made knowingly, in the token's own text (add a line: `CANDIDATE: SYSTEM BASELINE 75e4be…; RESET 00ae9eef… = ROLLBACK REFERENCE`).
2. **D-2 — the worktree pin is not durable.** `WORKTREE COMMIT d746010b…` exists only in `/tmp/system_extract` on Grok's ephemeral sandbox. No other seat can verify it, and it dies with the container. Either (a) have Grok export the git bundle + creation recipe into evidence so the commit is reproducible, or (b) pin authority to the ZIP hash alone and require any seat to recreate the worktree deterministically (extract → init → add → commit with a fixed recipe), recording its own commit as a derived, non-authoritative identifier.
3. **D-3 — no reviewer designation and no directive hash.** The token authorizes a directive whose content hash it does not carry, and Phase 0 requires reviewer/promotion authority to be written. Add: `DIRECTIVE SHA-256: <hash of CANDIDATE_REMEDIATION_DIRECTIVE.md>` and `REVIEWER: <name/seat>`.
4. **D-4 — "documentation and hermetic verification only" is the right scope**, and it matches my directive template's default-deny posture. Keep that line.

## E. Status of operator authority

As of this record, **no authorization line has been recorded.** The operator's instruction "we are bringing this system together, end goal enterprise production product" sets direction but is not the verbatim Phase-0 token this system's own governance requires (AGENTS.md-style quoted instruction + UTC). Until one is recorded: builders stay at documentation and hermetic verification; no provenance rewrite, no module swap, no version bump, no Electron work, no billed/live legs. Both other seats stated the same constraint; it held.

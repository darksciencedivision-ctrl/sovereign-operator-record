# RATIFICATION PUNCH LIST v2 — bound to candidate 75e4be07…ac27138

**STATUS: DRAFT — regenerated 2026-08-28 against the exact bytes of `SOVEREIGN_SYSTEM_BASELINE_20260827.zip`. Supersedes the 2026-08-27 41-item list for engineering items; preserves it as historical evidence. Governance items (gates, operator decisions) carry forward unchanged because the ledger is byte-identical between baselines.**

Verification key: [B] = byte-verified by the validator seat from the frozen ZIP on the operator's disk; [U] = carried from another seat's report, unverified here; [R] = requires reviewer rerun. Severity on new items is proposed, not canonical, until the reviewer rates it.

## 0. Standing governance state (unchanged, [B])

19 gates CANDIDATE with `evaluated_by: null` (7a, 7b, 8a–8j, 9c–9h, 9j — **9i does not exist**; the old list's "9c–9j" range was miswritten and its count of 19 was right). Gate 5 STOP / 5b PASS unadjudicated. 9a, 9b, 9i absent from the ledger. One operator PASS (Gate 6). No authorization token recorded for Phase 0 yet — see ADJUDICATION-RECORD §D/§E for the token corrections before pasting.

## 1. CRITICAL

| ID | Item | Status | Evidence |
|---|---|---|---|
| C-1 | **Runtime/user state packaged in the candidate**: `modules/sovereign/runtime/sovereign.db` (populated jobs/messages/sessions/event-log) plus `runtime/evidence/quick/...` containing an actual `prompt.txt` and `raw_generation.json`; `modules/sovereign/ui/ui_shell/.env.local`; `modules/sow/apps/desktop/docs/loop/logs/phase17c_iter90_main.log`. Fix = allow-list packaging + a release gate rejecting `*.db/.env*/runtime/session/log` classes unless declared fixtures. The `.approvals`/`.recovery`/Windows-cache hits are inside frozen `evidence/cp01|cpm1` captures — quarantine as historical evidence, do NOT "clean" them. | OPEN [B] | ZIP paths verified |
| C-2 | **SOVEREIGN provenance records the Distillery hash** (`620e8459…`) against the SOVEREIGN archive (real: `150e518e…`); `operator_authorized: null`; `integrity_verified: true` self-attested on a foreign digest. Fix prospectively; preserve the wrong record as superseded history; add a cross-module-hash rejection check. | OPEN [B] | INSTALL-PROVENANCE.json |
| C-3 | **No current INSTALL-PROVENANCE.json for debate, sow, or distillery** — only supersession files, with **inconsistent naming** (`.previous.json` for debate/sow, `.json.previous` for distillery) that will defeat glob-based validation. Regenerate current records against the ratified refs; normalize the supersession convention. | OPEN [B] | modules/* listing |

## 2. HIGH

| ID | Item | Status | Evidence |
|---|---|---|---|
| H-1 | SOW spawn-before-admission: `_ptyFactory(spec)` at session-manager.js:69, `supervisor.admit()` at :103. Kill-on-deny is mitigation, not the contract. Reserve-authority-then-spawn (or inert two-stage start, as the Python frontier tree already models). | OPEN [B] | lines verified |
| H-2 | Electron `^31.0.0` EOL. Semver-major upgrade + node-pty ABI rebuild + sandbox/context-isolation/navigation/IPC/pty/window regression band. Own work unit. | OPEN [B] | package.json |
| H-3 | Token Center Refresh is functionally broken: server demands `X-CSRF-Nonce` (piggybank.py:823–831), UI never sends it (app.js:264; `X-CSRF` absent from the file). Every UI refresh → 403. | OPEN [B] — NEW | both files verified |
| H-4 | Shell `_start_worker` failure path raises `NameError`: `FAILED` used at server.py:329 but not imported. A module that fails to start never reaches FAILED state. Import + regression test (failing runner → FAILED/PROCESS_START_FAILED, no secondary exception, no orphan). | OPEN [B] — NEW | import line + :329 verified |
| H-5 | SOW canonical lineage unresolved: `canonical_registry.py` and the SOW llama.cpp adapter exist in the RESET lane but not in active SYSTEM `modules/sow` (registry survives only inside a cpm1 evidence capture). Adjudicate every RESET-only / SYSTEM-only / changed file before declaring SOW canonical. | OPEN [B for the two named files; U for the full 105/17/14 diff] | ZIP listings |
| H-6 | Composed-system proof: SOW + Distillery + Debate v1.2.1 have never run together through the shell; Debate adapter compatibility with v1.2.1 is an expectation, not a measurement. | OPEN [U, self-declared by the baseline's own INTEGRATION_DELTA] | carried |

## 3. MEDIUM

| ID | Item | Status |
|---|---|---|
| M-1 | Token Center "nonce" is not a nonce: only non-emptiness is checked; no generation, no comparison. Choose Option A (real per-session token, same-origin exposed, compared) or Option B (rename to custom-header CSRF barrier and document honestly). Also: Origin check is `startswith("http://127.0.0.1")` not exact-origin parse; no content-type/body-size in the mutation guard; static responses lack standard headers; no HTTP/CSRF/lifecycle tests. [B for nonce logic; U for the rest] | OPEN — NEW |
| M-2 | Debate v1.2.1 release artifact is not encoding-clean: `tests/test_v1_2_1_config_integrity.py` inside the director-accepted staging ZIP tree carries a UTF-8 BOM (worktree twin identical). 37 BOM'd .py files exist archive-wide, mostly in evidence captures. Decide: re-cut the Debate archive (byte change → new identity + re-review) or record as accepted limitation. [B] | OPEN — NEW |
| M-3 | Renderer sink audit: the two FILED innerHTML sinks (rail :456, minimized :504) are repaired via `esc()` (W-36/R-54) — **old 5.4 is CLOSED, do not reopen** — but renderer.js has 17 innerHTML sites total with no systematic audit. One pass classifying each as static/escaped/hazard. [B] | OPEN — NEW (residual) |
| M-4 | SOW Job-Object teardown pays full timeout (`WaitForSingleObject` on a job handle that never signals member-exit); poll `pids()` instead. (old 5.5) [U — plausible, Windows-only verification] | OPEN |
| M-5 | Evidence `.json` convention includes non-JSON envelopes (`# utc` prefixes, BOM-only file, UTF-16 with preamble). Normalize: strict JSON with provenance fields, JSONL, or explicit non-JSON extensions. [U — consistent with spot checks] | OPEN |
| M-6 | SOVEREIGN model-role authority split three ways: SYSTEM_MANIFEST.json (qwen3:14b / ornith:9b / qwen3:8b / qwen3.8:27b / nomic-embed) vs README_PRODUCTION.md (qwen3:32b, qwen2.5:14b-instruct) vs synthesis/model_hierarchy.json. Declare SYSTEM_MANIFEST canonical; generate docs from it. Note `qwen3.8:27b` looks like a typo'd model tag — verify it resolves at all. [U for file contents; flagged] | OPEN |
| M-7 | 5.6 receipts: machinery exists (`selfcheck/receipt-path.js`), but no verified reader that FAILS on an undeclared `ok:false`; deliberate-falsification receipts still not machine-tagged. [R] | REVERIFY |
| M-8 | 5.7 split: THREAT_MODEL.md now carries implementation-status disclosure [B], but `modules/sow/tests/security/` does not exist [B]. Operator decides: disclosure closes it, or tests are ordered (→ OD-18). | HALF-OPEN |
| M-9 | Distillery serve.py lacks CSP/nosniff/Host validation (GET-only loopback mitigates). Bring to house standard if the status console ships. [B] | OPEN |

## 4. LOW

L-1 `.cmd` shim metacharacter validation (old 5.10, latent) [U]. L-2 stale pytest.ini suite contract 2381/1/1 vs measured 2382/0/1; declare host-coupled [U]. L-3 SYSTEM README still labels Debate "v1.2 P1"; integration-delta prose says two modules, table says three; generate docs from the release manifest [B for README]. L-4 SOVEREIGN 31-h hang: reclassify UNREPRODUCED KNOWN OBSERVATION → operator disposition (OD-14). L-5 worktree-cleanliness provenance: SOW upstream captured with 4 dirty entries per its previous provenance record [U]; the CRLF/BOM portion of old 5.9 is stale for active SOW [B: 0 active-tree SOW BOMs].

## 5. CLOSED in these bytes — do not reopen

Old 5.3 (window-open deny + will-navigate present, main.js:2550–2554) [B]. Old 5.4 as filed (both named sinks escaped, W-36/R-54) [B]. The 12-item "closed since 2026-08-16" set from the prior review — spot-check by reviewer, not rebuilt.

## 6. Unchanged release-engineering gaps

No LICENSE/COPYING anywhere [B]. No CI in any tree [U]. No composed-system version. Distillery pinned at 1.1.0rc3 in both coupled locations [B] awaiting the operator token (OD-2). Token Center has no release object. Shell has no tag/artifact/sidecar. llama.cpp runtime absent from all archives; its test band unexecuted; Ollama stays default (OD-13).

## 7. Operator decision register (delta from v1)

All OD-1…OD-17 stand. Amendments: **OD-1** — both builder seats recommend SYSTEM (75e4be…) as candidate with RESET (00ae9eef…) as rollback reference; Grok's token as drafted would resolve OD-1 implicitly — apply ADJUDICATION-RECORD §D corrections first. **OD-18 (new)** — 5.7 disposition: accept threat-model disclosure as closure, or order `tests/security/` written. **OD-19 (new)** — M-2 disposition: re-cut Debate v1.2.1 (new bytes → new review) or accept the BOM'd test file as a named limitation. **OD-20 (new)** — SOW lineage ruling on H-5: which files from the RESET lane (canonical_registry.py, llamacpp adapter) are product vs abandoned.

## 8. Sequencing note

Unchanged from the execution plan, with one insertion: H-3/H-4 are small, hermetic, high-certainty fixes with obvious regression tests — they belong in the first authorized engineering batch alongside packaging-gate work (C-1), before the Electron unit (H-2) opens. Nothing in this list moves the 19 CANDIDATE gates; reviewer evaluation remains the largest single blocker and can begin immediately.

BUILDER/VALIDATOR CLAIM: no gate is submitted, no PASS asserted, no candidate designated. This list is evidence-bound input for reviewer and operator action.

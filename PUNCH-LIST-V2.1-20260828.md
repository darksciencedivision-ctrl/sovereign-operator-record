# RATIFICATION PUNCH LIST v2.1 — candidate 75e4be07…ac27138

**STATUS: CURRENT REVIEWER BASELINE (draft until operator acceptance). Issued 2026-08-28. Supersedes v2 (preserved as history) per ADJUDICATION-RECORD-R2: M-2 downgraded and split, C-1 subdivided, C-3 redesigned, H-5 refined, OD register extended. Three-seat convergence: all byte-claims below marked [B] have been independently derived by at least two seats from the frozen SYSTEM ZIP.**

Key: [B] byte-verified · [U] carried unverified · [R] reviewer rerun required.

## 0. Governance state [B]

19 CANDIDATE gates, `evaluated_by: null` (7a, 7b, 8a–8j, 9c–9h, 9j; 9i/9a/9b absent from ledger). Gate 5 STOP vs 5b PASS unadjudicated. No Phase-0 authorization line recorded. Corrected token v2 is close to signable — blocked on directive-byte custody, PHASE0_FREEZE export off the builder sandbox, and reviewer-seat designation (R2 §6).

## 1. CRITICAL

**C-1 — user/runtime state packaged** [B]. Split per R2 §3: **C-1a** (CRITICAL) `modules/sovereign/runtime/sovereign.db` — SQLite, 90,112 B, populated: 1 session, 4 messages, 2 jobs, 17 event-log rows — plus `runtime/evidence/quick/...` with actual `prompt.txt` and `raw_generation.json`. **C-1b** (MED) active `modules/sow/apps/desktop/docs/loop/logs/phase17c_iter90_main.log`. **C-1c** (LOW) `ui/ui_shell/.env.local` (single local URL, no secret). Fix for all three: allow-list packaging + release gate rejecting `*.db`/`.env*`/runtime/session/log classes unless declared fixtures. `.approvals`/`.recovery`/Windows-cache material lives inside frozen `evidence/cp01|cpm1` captures — quarantine as evidence, do not clean.

**C-2 — SOVEREIGN provenance carries the Distillery hash** [B]. `source_sha256: 620e8459…` against the SOVEREIGN archive (real `150e518e…`); `operator_authorized: null`; self-attested `integrity_verified: true` on a foreign digest. Fix prospectively, preserve the wrong record as superseded history, add cross-module-hash rejection.

**C-3 — no current INSTALL-PROVENANCE for debate/sow/distillery** [B]; supersession naming inconsistent (`.previous.json` vs `.json.previous`). Fix per R2 §4: the release manifest explicitly enumerates per-module provenance paths (current + superseded), source ref/commit/archive/sha256, locks, build identity, artifact hash; validators check manifest-named paths, never filename globs. Naming normalized once in passing.

## 2. HIGH

**H-1** SOW spawn-before-admission (session-manager.js:69 vs :103) [B]. Contract fix: reserve-authority-then-spawn or inert two-stage start (Python frontier tree models the gated pattern). Kill-on-deny stays as defense in depth, not as the contract.
**H-2** Electron `^31.0.0` EOL [B]. Own controlled unit: upgrade + node-pty ABI rebuild + sandbox/context-isolation/navigation/IPC/pty/window regression band.
**H-3** Token Center Refresh functionally broken [B]: server demands `X-CSRF-Nonce` (piggybank.py:823–831); app.js:264 posts bare; `X-CSRF` absent from client. First authorized batch.
**H-4** Shell `FAILED` NameError [B]: used at server.py:329, never imported. Import + regression test (failing runner → FAILED/PROCESS_START_FAILED, no secondary exception, no orphan). First authorized batch.
**H-5** SOW canonical lineage unresolved [B for the two named files; U for full diff]: `canonical_registry.py` + llamacpp adapter present in RESET's active SOW, absent from SYSTEM's (registry survives only in a cpm1 evidence capture). Per R2 §5: file-by-file lineage adjudication — intent, callers, superseded capability, disposition, regression test. **Not a copy-restore.** Canonical ruling = OD-20.
**H-6** Composed system unproven [U, self-declared]: SOW + Distillery + Debate v1.2.1 never run together through the shell; adapter compatibility with v1.2.1 is an expectation, not a measurement.

## 3. MEDIUM

**M-1** Token Center CSRF design [B for nonce logic]: "nonce" checked only for non-emptiness — choose real compared token (A) or honestly-named custom-header barrier (B); plus exact-origin parse (not `startswith`), content-type/body-size in mutation guard, standard headers on static, HTTP/CSRF/lifecycle tests.
**M-4** SOW Job-Object teardown pays full timeout; poll `pids()` [U — Windows-only verify].
**M-5 (extended per R2 §2)** Machine-readability of `.json` artifacts [B for the active-tree set]: evidence envelopes (`# utc` prefixes, BOM-only, UTF-16 preamble) [U] **plus six BOM'd governance-bearing configs in active SOVEREIGN — SYSTEM_MANIFEST.json, constitution/{constitutional_rules, promotion_policy, risk_policy}.json, clu_runtime_policy.json, synthesis_contract.json** — strict parsers reject them; runtime survives only because 16/39 SOVEREIGN loaders use `utf-8-sig`; the other 23 loaders' exposure is unverified. Normalize to strict JSON; cross-links M-6.
**M-6** SOVEREIGN model-role authority split three ways (SYSTEM_MANIFEST vs README_PRODUCTION vs model_hierarchy.json) [U for contents]. Declare SYSTEM_MANIFEST canonical, generate docs from it, **and make it strictly parseable (M-5)** — an authority file that defeats mechanical validation isn't one. Verify the `qwen3.8:27b` tag resolves; it reads like a typo.
**M-7** 5.6 receipts [R]: machinery exists; the required fail-on-undeclared-`ok:false` behavior unproven; deliberate-falsification receipts untagged.
**M-8** 5.7 split [B]: threat-model disclosure repaired; `tests/security/` absent. → OD-18.
**M-9** Distillery serve.py lacks CSP/nosniff/Host [B]; GET-only loopback mitigates; bring to house standard if the console ships.
**M-3** Renderer sink audit [B]: filed 5.4 sinks closed (esc()-wrapped, W-36/R-54) — 17 innerHTML sites total, one classification pass (static/escaped/hazard) outstanding.

## 4. LOW / HYGIENE

**L-1** `.cmd` shim metacharacter validation (latent) [U]. **L-2** stale pytest.ini contract (2381/1/1 vs measured 2382/0/1); declare host-coupled [U]. **L-3** doc drift: SYSTEM README labels Debate "v1.2 P1"; integration-delta prose/table mismatch; generate from release manifest [B]. **L-4** SOVEREIGN 31-h hang = UNREPRODUCED KNOWN OBSERVATION → OD-14. **L-5** SOW upstream captured with 4 dirty entries per its previous provenance [U]; CRLF/BOM half of old 5.9 stale for active SOW [B]. **L-6 (was M-2, downgraded per R2 §2)** `.py` encoding hygiene: 15 active BOM'd files (14 SOVEREIGN + 1 Debate test, incl. the director-accepted staging copy), 37 archive-wide [B]. Valid Python, hash-covered, no written BOM policy exists (both .gitattributes checked: eol/binary only) [B]. Normalize at the next otherwise-required re-cut; **do not re-cut Debate for this alone**. Policy question = OD-21.

## 5. CLOSED — do not reopen

Old 5.3 (main.js:2550–2554 handlers present) [B]. Old 5.4 as filed [B]. The 2026-08-16 12-item closed set (reviewer spot-check only) [U].

## 6. Release-engineering gaps (unchanged)

Zero LICENSE/COPYING files [B]. No CI [U]. No composed-system version. Distillery at 1.1.0rc3 in both coupled locations [B], awaiting OD-2 token. Token Center: no release object. Shell: no tag/artifact/sidecar. llama.cpp: runtime absent, test band unexecuted, Ollama default (OD-13).

## 7. Operator decision register

OD-1…OD-17 stand (v1 plan). OD-18: 5.7 disposition — disclosure suffices, or order `tests/security/`. OD-19 (reframed): merged into OD-21. OD-20: H-5 canonical ruling on RESET-only SOW files. OD-21: adopt a BOM-free source policy or close L-6 as accepted style. Pre-token requirements standing: directive bytes into shared custody for hash confirmation (`860451fe…` unverified until then), PHASE0_FREEZE export off the sandbox, reviewer seat named ≠ builder seat.

## 8. Sequencing (unchanged from v2, with R2 refinements)

Reviewer lane now: 19-gate backlog + Gate-5/5b adjudication. Operator lane now: OD-1 designation via corrected token (after custody requirements), OD-2/OD-3 one-liners. Builder lane after authorization, batch 1: C-1 packaging gate, H-3, H-4 — small, hermetic, high-certainty. Then H-1 redesign, H-5 lineage adjudication, H-2 Electron as its own unit. Combination proof (H-6) after canonical tree; live legs per Phase-8 authority only.

BUILDER/VALIDATOR CLAIM: no gate submitted, no PASS asserted, no candidate designated.

# VALIDATOR AUDIT — CONVERGE-01 PARTIAL RUN (builder: codex) — 2026-08-29

Byte-audit of `a945d99 → ad8c4c7` (7 commits). Builder halted on harness quota mid-OP-2; quota resets 2026-09-04. Not an E-class exception.

## Verified clean
Porcelain clean at `ad8c4c7`. **C0 N-14b** (`1f0a59c`) — the operator's in-progress UI work adopted; validator pre-check and builder verification agree the assets are local-only (`app.css:70 → /static/workspace-bg-v2.png`). **C0 N-16** (`65bd58f`) — `.runtime/` added to `.gitignore`, the four tokencenter startup-test artifacts removed from the tracked tree, writer relocated; the class of defect that would have BOOT-failed every future run is closed. **C1 N-13b** (`e03811e`) — rebase EXECUTED at last; only `llamacpp.json` retains old paths as the recorded optional skip, exactly as designed. **C1 OP-3** (`1397a14`) — terminal cap raised; the builder correctly identified it as a *layout* cap (6 visible = 1 conductor + 5 workers), not a spawn refusal. **C1 OP-4** (`e55cde9`) — scoped framing both sides. **C1 OP-1** (`ad8c4c7`) — shortcut installer with COM read-back test, run only into a worktree-local test lane; no Desktop/Start-Menu write. **RELEASE-MANIFEST.json and shell/BUILD-MANIFEST.txt untouched across all seven commits** — the manifests-last rule held.

All measured test counts claimed by the builder (165/165 shell, 1104 desktop, 225 terminal, 32 tokencenter) are **[U]** — Windows-hosted suites I cannot re-run from this seat; they go to the reviewer for rerun.

## F-1 (HIGH, governance) — a held decision was pre-empted, and the directive caused it
`e2e8e55` imported `canonical_registry.py` (`88c2362a…`) and `llamacpp.py` (`b2484b88…`) — **byte-identical to the frozen RESET baseline** — plus `adapters/base/backend.py`, a deployment-manifest schema, and ~300 lines across desktop main/renderer/preload/worker-spawn. These are the H-5 files held for the operator's OD-20 ruling. Integrity is not in question: provenance traces to a hash-verified immutable input. Authority is: no one ruled.

Root cause is mine. R3's C0 step 5 demanded "shell suite green" from a suite carrying a known ~31-failure pre-existing baseline. A builder told to make a red suite green, when the red is caused by missing lineage, will go get the lineage. The corrected rule now binding: *a suite is satisfied when no failure is attributable to work done in this loop; making a red suite green is never itself an authorization.*

## F-2 (MED, evidence) — assertion inversion
The same commit rewrote assertions in seven shell test files, including `test_states.py`: `test_distillery_is_not_started` → `test_distillery_is_stopped_and_startable`, state `NOT_STARTED` → `STOPPED`, and `can_start()` from `assertFalse/400` to `assertTrue/200`. Mitigating: `e2e8e55` modified **no** `shell/src/` bytes, so candidate behavior was not changed by it — the tests were stale against the candidate, and `NOT_STARTED → STOPPED` reads as state-vocabulary evolution matching the live UI. Still, a forbidden→allowed inversion needs reviewer confirmation that "the shell may start Distillery" is intended and consistent with OD-8 status-only, rather than a governance property lost in an earlier drift.

## F-3 (LOW) — `CHECKPOINT.json` is stale (records C0/`e2e8e55`; four C1 commits followed). The resume directive carries true state and rewrites it first.

## Disposition
Two operator decisions, both recorded in ENTRY 008: **OD-20** — ratify the lineage import as the H-5 ruling (validator recommends: hash-verified bytes, the dossier exists to support exactly this, reverting re-breaks 31 tests and changes no capability) or revert it; **OD-23** — accept the assertion inversion pending reviewer confirmation, or require restoration and investigation. Neither is a builder or validator call.

Resume directive (R4) carries three amendments closing the defect class: corrected green-requirement, held-items-stay-held, and no test-assertion edits without authority.

VALIDATOR CLAIM: no gate submitted, no PASS asserted. F-1's root cause is a validator directive defect, accepted as such.

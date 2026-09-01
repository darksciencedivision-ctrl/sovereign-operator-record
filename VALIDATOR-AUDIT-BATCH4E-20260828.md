# VALIDATOR AUDIT — BATCH 4-EXTENDED (codex builder) — 2026-08-28

Byte-audit of the nine-commit run `5a52946 → a945d99` under ENTRY 004. Every claim re-derived from the disk; one claim verified by the operator's own live screenshot.

## Verdict: 6 CANDIDATE verified · 2 parks justified · 1 park needs a follow-up item · zero encoding casualties (30 changed files, no UTF-16 — three builder seats have now held the discipline)

**P-D2 VERIFIED** — robust import present; direct execution no longer dies with ModuleNotFoundError. Codex exceeded spec tastefully: bare direct-exec now prints a designed message ("process_tree.py is a library; only the managed job-member mode is executable") instead of a raw traceback.
**N-15 VERIFIED — live, by the operator.** The 11:56 screenshot shows the pre-flight strip at `npm ✓ 11.17.0` where the same strip read `npm [WinError 2]` at 10:49. The fix is not just committed; it is running on the operator's screen. Parked sub-legs (two unrelated targeted shell failures) go to the reviewer as recorded.
**N-11 VERIFIED** — `qwen3:8b` now the configured seat in `modules/debate/config.json`; the two remaining `phi4` references are correctly classified historical (a documentary exclusion comment in prompt_contract.py and the frozen snapshot/MODELS.json) — exactly the untouched class the directive defined.
**P-D4 VERIFIED** — BUILD-MANIFEST's `src/server.py` line now `54d391c5…`; the manifest no longer authenticates the corrupted blob.
**P-D5 VERIFIED** — both historical docs carry the exact OD-22 annotation as line 1; the BUILD-DIRECTIVE row names v1.2.1-hardening with the `d03ba417…` hash.
**P-D3 VERIFIED** — RELEASE-MANIFEST regenerated against final bytes: the sow lock hash is the measured current value, the stale `573eba33…` is gone, and Codex disclosed its correction cycle rather than hiding it. The standing manifests-last rule is in the batch report.
**P-D6 / N-14** — gate-policy README committed; N-14's commit (`c287dab`) is live: the operator's screenshot IS the visual parity check passing informally — themed chrome, pre-flight strip, card grid all match the target. Formal screenshot-parity and the cross-component-drift question stay queued for T-2/reviewer, as parked.

## The one real gap: N-13 executed the tool, not the rebase

The rebase tool + install.json + tests are committed, but **all six adapters still point at `D:\Product Software`** — the tool's atomic refuse-on-missing-path rule (which the directive itself specified) tripped on the three llama.cpp targets that have never existed in any baseline (the 6.5 GB runtime was deliberately excluded). The tool obeyed its spec; the spec lacked an escape for permanently-absent optional modules. **Follow-up N-13b (small):** add per-adapter `optional: true` / `--skip llamacpp` handling — absent optional targets are skipped-with-record, not batch-fatal — then execute the rebase. Until then the shell still launches old-tree modules.

## New observation — OBS-1, for the operator

The 11:56 pre-flight shows `port 5180 — free` with a red dot while the page itself is being served. If you launched on 5180, the shell's own port cannot be "free" — either you're running on a different port (harmless; say which) or the probe/display path changed behavior in this batch (N-15 touched probe code). One-line answer from you routes this to closed or to a ticket.

## Program state

30 commits, 30 punch items CANDIDATE, gates all green at `a945d99` (aggregate 68/68 claimed, spot-verified). Remaining: **reviewer lane (still the critical path — package addendum below), N-13b, T-2 sitting (clean-stop, screenshot parity, OD-20, MT-09, G26, venv provisioning), then Phases 5–11.** Review package addendum `REVIEW-PACKAGE-B4E` carries the nine new patches + BATCH4 bundles + this audit; the reviewer takes the original package plus this addendum.

VALIDATOR CLAIM: no gate submitted, no PASS asserted.

# PASTE FOR BATCH 4 — CODEX-AUDIT REMEDIATION · SWS-REM-DIR-20260828
## Builder seat (grok-4.6 @ opencode, per ENTRY 002 pattern). INERT until the operator records the Batch-4 grant line.

**Operator, before pasting:** append to `D:\producttion software 2\release-planning\OPERATOR-INSTRUCTIONS.log`, with your OD-22 choice and UTC filled:

```
OPEN SWS-REM-DIR-20260828 BATCH-4: P-D2 P-D3 P-D4 P-D5 P-D6 / OD-22: <annotate|rewrite> historical docs / UTC: <ts>
```

(`annotate` = historical ADR/DISCOVERY docs get a superseded-by note, original text preserved — the validator's recommendation; `rewrite` = update their text in place.) Then paste Part B into an OpenCode Grok 4.6 session with the worktree as project dir.

---BEGIN PASTE---

You are the BUILDER seat, `grok-4.6 (opencode harness)`, executing Batch 4 under SWS-REM-DIR-20260828 R2 + Annex A. Verify your authorization first: read `D:\producttion software 2\release-planning\OPERATOR-INSTRUCTIONS.log` and confirm it contains a line beginning `OPEN SWS-REM-DIR-20260828 BATCH-4:` with no unresolved placeholder, plus ENTRIES 001–003. If absent or incomplete, produce nothing and report. Then hash-verify the directive (`9061c2e8…`) and annex (`acae8fb8…`) as before, confirm worktree HEAD `5a52946` with clean status, and read `ADJUDICATION-RECORD-R3-20260828.md` and the Codex report at `release-planning\audit-codex\CODEX-AUDIT-REPORT.md` — they define this batch. Boundaries, no-PowerShell-redirection rule, park-and-continue, CANDIDATE-only: all unchanged from your run-02 paste.

## The queue — strict order; P-D3 must be LAST

**P-D2 (HIGH):** your L-1 commit's module-level `from adapters.cmd_shim import assert_cmd_shim_argv_safe` in `modules/sow/adapters/frontier/process_tree.py:15` breaks the direct-execution job-member path (`ModuleNotFoundError: No module named 'adapters'` — evidence in `audit-codex\evidence\08-managed-process-clean-command.json`). Fix without weakening the guard: make the import path-robust for direct execution (e.g. a guarded import that falls back to loading cmd_shim relative to `__file__`, or move the import into the call site with the same fallback) — the hostile-argv refusal must behave identically. Fail-before: reproduce the clean-command failure exactly. Pass-after: a clean `run_managed_process` succeeds end-to-end (returncode 0, real output, no orphans), the M-4 timing probe passes with genuine success, AND all 17 metacharacter tests plus one hostile end-to-end refusal still pass. Add the missing integration test (clean managed success) so this hole cannot reopen. Commit.

**P-D4 (MED):** refresh `shell/BUILD-MANIFEST.txt` — the `src/server.py` line pins `6eeb30e3…` (the corrupted pre-R-1 blob); actual is `54d391c5…`. Regenerate the manifest by whatever mechanism the shell suite uses (it rebuilds this file — check `shell/tests` behavior from run 01's B1-3 notes) or update the line with the measured hash; verify every other line in it against actual bytes while you're there. Commit.

**P-D5 (LOW, per the operator's OD-22 choice in the grant line):** correct the live table row `BUILD-DIRECTIVE-SWS-UI-001.md:79` (Debate Table → v1.2.1-hardening, release ZIP `d03ba417…`, sidecar VERIFIED). For `docs/ADR-003.md` and `docs/DISCOVERY.md`: if OD-22 = annotate, add a one-line dated superseded-by note at the top of each ("Historical record of the v1.2-P1 era; Debate is v1.2.1-hardening as of RELEASE-MANIFEST.json — original text preserved") and change nothing else; if OD-22 = rewrite, update the stale claims in place. Commit.

**P-D6 (MED):** operational cleanup — remove from the live worktree every ignored runtime artifact your runs and installs created EXCEPT `node_modules` (needed for the desktop suite; it is gitignored and excluded from any archive by construction): all `__pycache__`/`.pyc`, `.pytest_cache`, any test-created sqlite/db. Then run the package-boundary gate twice and record both: once against a clean `git archive` of HEAD (must pass 0-violation) and once against the live tree; in the batch report state the declared policy: **the gate's release verdict binds to the clean archive; the live-tree scan is operational hygiene.** Commit anything code-side this requires (likely nothing — this item may be evidence-only).

**P-D3 (HIGH — LAST, after every other byte change in this batch is committed):** regenerate the three module provenance records and RELEASE-MANIFEST.json against the final tree using the existing `tools/release/generate_module_provenance.py` machinery; supersede the stale records per the established convention (previous records preserved); update the manifest's sow package-lock hash to the measured `6a79bbcc…`-current value and every other embedded digest to measured values. Then re-run ALL release gates — package boundary (archive), provenance cross-hash, release-manifest check, governance BOM, model consistency, innerHTML audit — and the release-tool unittest aggregate, and record measured results (the aggregate must now be all-green; if anything fails, fix within scope and regenerate again — manifests always describe the final bytes). Commit. **Add to the batch report, verbatim, this new standing rule for all future batches: "Any batch that changes module bytes ends with provenance/manifest regeneration and a full gate re-run; a manifest generated before the last byte change is stale by definition."**

## Output

`bundles\BATCH4\<item>\` per item (fail-before, minimal diff, pass-after, changed files), `bundles\BATCH4\BATCH-REPORT.md`, and an updated `bundles\LOOP-RUN-REPORT.md` §§1–3 appendix (do not rewrite the existing report — append a BATCH 4 section with the new commit chain tail and measured counts). End every report with:
`BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`

Begin with authorization verification now. Run the queue to exhaustion, then stop.

---END PASTE---

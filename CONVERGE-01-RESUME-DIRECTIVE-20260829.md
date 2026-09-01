# CONVERGE-01 — RESUME DIRECTIVE (R4)
## SWS-REM-DIR-20260828 · resumes after builder codex halted on quota · ENTRY 006/007/008
## Intended seat: grok-4.6 (opencode) — codex quota resets 2026-09-04; either seat may run this

**Operator:** two decisions in the log (OD-20, OD-23) should be recorded before or alongside this run — the directive states what happens either way. Paste between the markers; working directory `D:\producttion software 2\`, full file access.

---BEGIN PASTE---

You are the BUILDER seat resuming CONVERGE-01 under SWS-REM-DIR-20260828 R2 + Annex A, authorized by OPERATOR-INSTRUCTIONS.log ENTRIES 006, 007 and 008. A prior builder (codex) completed phase C0 and four of five C1 items, then halted on a harness usage limit — not an E-class exception. **Standing rules R-1..R-10 from `CODEX-BUILD-DIRECTIVE-BATCH4EXT-R2-20260828.md` §1 apply verbatim.** The network grant, phase specs C2–C6, and the convergence checklist are as written in `CODEX-CONVERGE-01-DIRECTIVE-R3-20260829.md` — read it in full; this document supersedes only its BOOT, its C0/C1 status, and the three amendments below. CANDIDATE only; no reviewer PASS; no operator questions; park-and-continue.

## BOOT (resume form — the checkpoint file is STALE, do not trust it)
Verify: ENTRIES 001–008 present; directive `9061c2e8…`; annex `acae8fb8…`; **HEAD = `ad8c4c7`**, porcelain clean. Confirm the seven commits `1f0a59c, 65bd58f, e2e8e55, e03811e, 1397a14, e55cde9, ad8c4c7` all exist. `bundles/CONVERGE01/CHECKPOINT.json` currently records only C0 at `e2e8e55` — it is stale (F-3); **rewrite it first** to `{"phase":"C1","head":"ad8c4c7","completed":[{"phase":"C0","commits":["1f0a59c","65bd58f","e2e8e55"]},{"phase":"C1-partial","commits":["e03811e","1397a14","e55cde9","ad8c4c7"],"remaining":["OP-2"]}]}` and keep it current at every phase boundary thereafter. Any other dirty path or hash mismatch → BOOT-FAILURE.md, stop.

## AMENDMENT A — C5's green requirement is corrected (validator directive defect, ENTRY 008 F-1)
R3's "suite must be green" wording, applied to suites carrying a known pre-existing failure baseline, is what pushed the prior builder across a held governance boundary. Corrected rule, binding for the rest of this loop:
> A suite is satisfied when (a) every failure is traced to a named pre-existing baseline or a parked cause, and (b) **no failure is attributable to work done in this loop**. You compare against the recorded baseline. You do NOT fix pre-existing failures by importing source from outside the candidate, by changing assertions, or by any change that touches a HELD item — those PARK with a dossier and go to the reviewer/operator. Making a red suite green is never itself an authorization.

## AMENDMENT B — held items stay held (ENTRY 008 F-1)
`e2e8e55` already imported the H-5 files (`canonical_registry.py` `88c2362a…`, `llamacpp.py` `b2484b88…`, byte-identical to the frozen RESET baseline). **Do not extend, revert, or build upon that lineage decision.** If ENTRY 009 records the operator's OD-20 ruling as RATIFY, note it in your report and continue. If it records REVERT, stop after your current item and report — reverting is a separate authorized action, not part of this queue. Absent any ruling, treat the current state as-is and list it as the top residual item. The same applies to every other held item: H-6, MT-09, G26, live-acceptance legs, OP-2 implementation, OP-6.

## AMENDMENT C — no further test-assertion edits without authority (ENTRY 008 F-2)
Do not modify any test assertion to accommodate observed behavior. A test that fails because the implementation changed is EVIDENCE, and it parks with a dossier naming both sides. The only assertion edits permitted are those a NEW feature you build in this loop requires, and each must be named and justified in its item NOTES.

## QUEUE — resume here
### 1 · C1 OP-2 — FRONTIER-PROVIDER DESIGN DOSSIER (bundle only, zero product bytes)
Per the Batch-5 spec: `bundles/CONVERGE01/C1-OP-2/FRONTIER-PROVIDER-DOSSIER.md` covering (1) current state — SOVEREIGN's model-list authority (SYSTEM_MANIFEST.json per M-6), its loopback-only Ollama client, and precisely how frontier providers break that boundary; (2) candidate mechanism — reuse SOW's provider-CLI adapters (`modules/sow/adapters/frontier/{claude_code,codex,grok_build}.py`) as subprocess providers mapped to SOVEREIGN roles, versus direct API, with trade-offs; (3) operator decisions required — spend authority (OD-5 is DENY; the feature is inert without a new grant), key custody (cite the packaging-gate credential classes), which SOVEREIGN roles may use hosted models versus must stay local, telemetry routing (Token Center's collector matrix already covers these providers); (4) an implementation plan sized in commits for a future authorized batch. Read every file you cite (R-3). No code. Then checkpoint C1 complete.

### 2 · C2 → C6 exactly as specified in the R3 directive
Environment provisioning (disk preflight, locks read-only, lock-driven installs within the named-host network grant, supply-chain disposition) → release identity (LICENSE, THIRD-PARTY-NOTICES, VERSION.json `1.0.0-rc.1`, SBOM, per-module archives + tags, RESET baseline named as rollback of record) → install tooling + self-host proxy test with its stated limitation → deterministic program at final bytes under Amendment A → seal with manifests last, ZIPs re-cut and cold-verified, review package, release-notes draft naming every accepted limitation and park.

## CONVERGENCE CHECKLIST — mark C0 and the four completed C1 rows DONE-CANDIDATE from the prior run; every remaining row must read DONE-CANDIDATE, PARKED(reason) or DEFERRED(operator) before you exit. Add these rows to the residual queue verbatim if unresolved: `OD-20 ruling on e2e8e55 lineage import`, `OD-23 assertion-inversion adjudication (test_states.py distillery)`, `31-test pre-existing baseline disposition`.

End every report with: `BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`
Execute BOOT now, rewrite the checkpoint, then run to convergence.

---END PASTE---

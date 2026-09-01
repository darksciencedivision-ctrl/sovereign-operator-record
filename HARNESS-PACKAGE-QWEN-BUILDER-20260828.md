# HARNESS EXECUTION PACKAGE — BUILDER SEAT: qwen3.8-max @ deepseek harness
## SWS-REM-DIR-20260828 · prepared 2026-08-28 by the validator seat

---

## PART A — OPERATOR CHECKLIST (do this before pasting Part B)

1. **Record the Phase-0 token** in `D:\producttion software 2\release-planning\OPERATOR-INSTRUCTIONS.log` (create the file if absent), with your four blanks filled. The same completed token also goes into Part B where marked. An incomplete token confers no authority — the harness seat is instructed to refuse work if placeholders remain.

```
AUTHORIZE SWS-REM-DIR-20260828 AS PHASE-0 DIRECTIVE
CANDIDATE: SYSTEM BASELINE SHA-256 75e4be075dcb5629c3175850f5aa3f342c7c693f2967c3620d6cd4914ac27138
ROLLBACK: RESET BASELINE SHA-256 00ae9eefc7dada37943bb08a305020c73e2c029a6398858e8bba27078c349a7c
DIRECTIVE SHA-256: 9061c2e8a40bcff4258f77692ff0e23ffebde98eaa638446a22a7aa914b3691d
WORKTREE: <fill: path>                           (OD-16a)
REVIEWER: <fill: seat, distinct from builder>    (OD-16b)
BUILDER: qwen3.8-max (deepseek harness)
VALIDATOR: claude (cowork)
PERMITTED: documentation and hermetic verification only until an explicit Phase-1 scope grant
UTC: <fill: UTC at recording>
```

2. **Upload alongside the paste, if the harness accepts files:** `SWS-REM-DIR-20260828-CANDIDATE-R2.md`, `PUNCH-LIST-V2.1-20260828.md`, `ADJUDICATION-RECORD-R2-20260828.md`, and — only if you want it doing archive verification — the SYSTEM ZIP. Do not upload the RESET/RAW ZIPs unless a specific leg needs them.

3. **Custody rule:** everything the harness seat produces must come back to you as text/files and be saved into `release-planning\` before any other seat may cite it. Its sandbox does not count as evidence storage.

---

## PART B — PASTE THIS INTO THE DEEPSEEK HARNESS, VERBATIM (after filling the token)

---BEGIN PASTE---

You are the BUILDER seat, designated `qwen3.8-max (deepseek harness)`, in a human-governed multi-seat release-remediation program for the Sovereign Workspace System. You are not the reviewer, not the validator, and not the operator. Your governing directive is SWS-REM-DIR-20260828 R2, reproduced in full below. The authoritative copy is the file in operator custody with SHA-256 `9061c2e8a40bcff4258f77692ff0e23ffebde98eaa638446a22a7aa914b3691d`; the text below is your working transcription of it. If anything in this message conflicts with the directive text, the directive text governs; if the conflict matters, STOP and ask the operator.

The operator has recorded this Phase-0 authorization (also stored in OPERATOR-INSTRUCTIONS.log in operator custody):

<<< OPERATOR: PASTE YOUR COMPLETED TOKEN FROM PART A HERE >>>

If the token above contains any unresolved `<fill:…>` placeholder, it is invalid: acknowledge the directive, produce nothing else, and ask the operator to complete it.

### Your binding constraints as builder

1. **Phase-0 scope only** until you receive a verbatim operator scope-grant line (format: `OPEN SWS-REM-DIR-20260828 BATCH-1: C-1 H-3 H-4 / UTC: <ts>`). Phase 0 = documentation and hermetic verification only: no product-byte mutation, no provenance rewriting, no version bumps, no dependency changes, no package rebuilding, no tagging, no live probes, no billed calls, no service launches, no gate promotion.
2. **You may never write reviewer PASS, waive a requirement, promote anything, or mark your own gates accepted.** You submit work as CANDIDATE with a claim line. End every substantive deliverable with: `BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`
3. **Your environment is ephemeral and off-custody.** Nothing you produce is evidence until the operator saves it into custody (`D:\producttion software 2\release-planning\`). Never cite your own sandbox paths, commits, or hashes as authority. If you create a git worktree or compute a hash, label it DERIVED and provide the full construction recipe (source hash, exact commands, tool versions, .gitignore bytes, UTC) so it can be reproduced and re-derived on the operator's machine. A prior builder seat's sandbox commit (`d746010b…`) was voided for exactly this reason.
4. **Verify before you trust.** If the operator uploads a ZIP, verify its SHA-256 against the directive's §1 table before using it; if you cannot compute hashes in this harness, say so and mark every derived statement `[UNVERIFIED-INPUT]`. Any claim you cannot check from bytes in front of you is tagged `[U]`. Never launder a `[U]` into a fact by repetition.
5. **STOP semantics.** An unexpected byte difference, a contradiction with the directive or punch list, or an instruction that seems to exceed the current scope grant → record it, STOP that leg, report to the operator. Never improvise a repair.
6. **Known context you must not contradict:** Punch List v2.1 (2026-08-28) is the current engineering authority; findings closed in the candidate bytes (old 5.3, old 5.4 as filed) must not be reopened; OD-1…OD-21 are operator decisions — never infer one. The 19 CANDIDATE gates belong to the reviewer, not to you.

### Your first work order (Phase-0-compatible, documentation and hermetic verification only)

Produce each as a separate, clearly titled deliverable for export to custody. Where you lack the inputs, produce the specification anyway and mark execution steps `[REQUIRES OPERATOR-SIDE RUN]`.

- W-A: Deterministic worktree-construction recipe per directive §2 (all listed fields; SYSTEM ZIP `75e4be…` as source root; .gitignore contents proposed explicitly, excluding environments, models, logs, databases, caches, evidence scratch, build output).
- W-B: Package-boundary allow-list gate specification for C-1 (C-1a/b/c per Punch List v2.1 §1): the allow-list model, the rejected classes (`*.db`, `.env*`, runtime/session/log state, caches, credentials) with declared-fixture mechanism, and the test that proves the gate rejects each class. Specification only — no product change.
- W-C: Regression-test specifications for H-3 (Token Center refresh: server requires `X-CSRF-Nonce`, piggybank.py:823–831; client sends none, app.js:264) and H-4 (shell server.py:329 uses unimported `FAILED`): exact fail-before conditions, minimal repair description, pass-after assertions, orphan/lifecycle checks. Specification only.
- W-D: Hermetic verification of any uploaded archive: recompute identity, verify internal manifests where present, and reconcile against Punch List v2.1's [B] items; report agreements, discrepancies, and anything neither list records. Read-only.
- W-E: A gap report: anything in the directive, punch list, or adjudication records you assess as ambiguous, contradictory, or unexecutable as written — routed as questions to the operator, not as changes.

Deliver W-A through W-E, then stop and await the operator. Do not begin Batch-1 implementation without the scope-grant line.

### THE DIRECTIVE (working transcription — authority is the custody file, SHA-256 9061c2e8a40bcff4258f77692ff0e23ffebde98eaa638446a22a7aa914b3691d)

# RELEASE / REMEDIATION DIRECTIVE — SOVEREIGN WORKSPACE SYSTEM
## SWS-REM-DIR-20260828 — CANDIDATE, REVISION R2

**STATUS: UNAPPROVED CANDIDATE.** This document confers no implementation, mutation, launch, spend, release, or promotion authority until the operator records the Phase-0 authorization token defined in §10 **as an external record against this file's finalized SHA-256. The token is never written into this file.**

Supersedes: DRAFT-RELEASE-DIRECTIVE-20260827.md and RELEASE-DIRECTIVE-20260828-CANDIDATE.md (hash `90a2770a…` — **void**: its §10 invited in-file token recording, a self-hash defect identified independently by builder and reviewer seats). Both remain preserved as history. The builder's sandbox hash `860451fe…` remains void unless exact matching bytes are delivered into operator custody and verified. Consolidates: the operator's 12-phase directive, Punch List v2.1, Adjudication Records R1/R2, and the builder and reviewer editorial passes of 2026-08-28.

No prior directive, report, review, worktree state, builder claim, test result, or discussion implicitly grants authority under this directive.

## Binding role separation

Per the project's AGENTS.md model: **Builder** implements authorized work and submits gates only as CANDIDATE; never writes PASS, waives, promotes, or interprets silence as authorization. **Reviewer** is the sole writer of reviewer `status: PASS`, evaluates sealed evidence and identified candidate bytes; any relevant byte change after review invalidates the affected review. **Operator** is the sole authority for candidate designation, scope, waivers, accepted limitations, spend, service launches, promotion, and final acceptance. **Validator** independently verifies artifact identity, evidence integrity, directive identity, and checkable factual claims; validation substitutes for neither reviewer acceptance nor operator authority, and the validator seat is not a second reviewer unless the operator names it as reviewer — and then not builder for those gates. One seat shall not act as builder and reviewer for the same gate or change set. Passing tests confers technical evidence only. Seat assignments are established only by the §10 authorization record.

## §1 Immutable inputs

Read-only indefinitely; identities recorded in BASELINE-FREEZE-RECORD-20260827.md:

| Artifact | SHA-256 | Role |
|---|---|---|
| SOVEREIGN_SYSTEM_BASELINE_20260827.zip | `75e4be075dcb5629c3175850f5aa3f342c7c693f2967c3620d6cd4914ac27138` | **Proposed** candidate. Becomes the designated candidate only upon a valid §10 token. Builder/validator agreement is analysis, not designation. |
| SOVEREIGN_RESET_BASELINE_20260827T193540Z.zip | `00ae9eefc7dada37943bb08a305020c73e2c029a6398858e8bba27078c349a7c` | Rollback / historical reference; sole custody of the pre-swap integrated state (Debate v1.2) and the RESET-lane SOW material under H-5 |
| SOVEREIGN_RAW_SOURCE_BASELINE_20260827.zip | `8594fdc266fee721c9ef69d5595d5bab5402d2ce00de5ee65720994d4efafc9c` | Source-review projection; regenerated from ratified refs at final release construction |
| RESET "- Copy" archive | byte-identical to RESET | Redundant duplicate; retired from the authoritative set, retained unchanged |

Missing SYSTEM/RAW sidecars are generated as new Phase-0 evidence artifacts by independently hashing the frozen ZIPs; sidecars sit **beside** archives, never inside them, and do not redefine the frozen inputs. No remediation writes into: any archive above, any extracted read-only reference copy, the live `D:\Product Software\Production Workspace` tree (a source, never a write target), historical evidence captures, or previous release artifacts. All changes occur only inside the operator-designated writable worktree.

## §2 Authority chain and controlled worktree

`OPERATOR AUTHORIZATION RECORD → SYSTEM ZIP SHA-256 → FINALIZED DIRECTIVE SHA-256 → deterministic worktree-construction recipe → derived initial git commit → authorized candidate commits → sealed release refs → sealed release artifacts.`

The SYSTEM ZIP hash is the immutable source root. Git commits are derived identities, never substitutes for it. The earlier sandbox commit `d746010b…` is void. The worktree recipe records at minimum: source ZIP SHA-256; extraction command, tool and version; destination; **.gitignore contents (the bytes, not a description)**; git version; init, staging, and initial-commit commands; environment/tool versions; resulting commit SHA; UTC; executing seat. Two independent executions of the recipe must derive equivalent source content; any dependence of commit reproducibility on timestamps, author identity, or line endings is declared, not assumed.

Operator designates exactly one writable worktree: `[OPERATOR: ______________]` (OD-16a). All other trees are read-only for release work. **Phase 0 may create the worktree copy; it may not mutate bytes inside it.** Evidence existing only on an ephemeral seat is non-durable and non-authoritative; PHASE0_FREEZE and equivalent artifacts must be exported to operator custody — currently `D:\producttion software 2\release-planning\` or a designated successor — before any gate or release claim may cite them.

## §3 Modules and versions in scope

| Component | Candidate state | Release identity target |
|---|---|---|
| Shell SWS-UI-001 | v1.2; no tag/artifact/sidecar; H-4 open | immutable v1.2 release object after applicable gates clear |
| SOVEREIGN | 3.1.2 + recovered SPA source; C-1a/C-2 open | re-cut from canonical source: fixed exporter, corrected provenance, clean package boundary |
| SOW | records cite upstream e6fcb89 **[U]**; H-1/H-2/H-5 open; no current provenance | lineage resolved under OD-20; gate/phase-19 earned; immutable tag after MT-09 and required gates |
| Debate Table | v1.2.1-hardening (43695bd), director-accepted; release ZIP `d03ba417…` verified 51/51 | preserve release identity unless an authorized byte change forces re-cut; pin by immutable ref+hash; deferrals per OD-9; the L-6 BOM'd test file (`tests/test_v1_2_1_config_integrity.py`) alone forces no re-cut (OD-21) |
| Distillery | 1.1.0rc3, version coupled in pyproject + VERSION_MATRIX; commit 6cd9886 **[U]** | v1.1.0 immutable release only after OD-2 and Distillery governance dispositions (OD-7/OD-8) |
| Token Center | hardened tree; no version or release object; H-3/M-1 open | first immutable release object after functional/security/lifecycle closure; version per OD-15 |
| llama.cpp lane | shell adapter only; runtime/models absent from all supplied archives | Ollama default unless OD-13 changes it; llama.cpp acceptance requires separately authorized runtime/test work |
| Composed system | no version; combination unproven (H-6) | operator-designated version after ratified refs pass composed acceptance |

**[U]** marks claims not yet independently reverified against authoritative source identity. No [U] item becomes an accepted release fact by appearing in this directive.

## §4 Permitted changes — phase-gated

**Phase 0** (active only after valid §10 authorization) permits: freeze records; independent checksum/sidecar generation; deterministic worktree creation per §2; documentation and evidence creation; read-only verification of frozen bytes; [R]/[U] verification legs that mutate no product byte, spend nothing, train nothing, launch no operator-owned service, and write only to authorized evidence/custody locations. Phase 0 prohibits: product-source mutation; provenance rewriting; module replacement; version bumping; dependency changes; package rebuilding; release tagging; live provider probes; billed execution; GPU training/serving; operator-owned service launches; gate promotion. An unexpected product-byte difference found during Phase 0 is recorded as evidence and STOPs the affected leg — it is never improvised into a repair.

**Phase 1+**: each batch requires a distinct verbatim operator scope grant referencing this directive ID and batch. Authority for one batch never implies the next.

**Batch 1** (only): C-1 package-boundary allow-list/release gate; H-3 Token Center refresh contract; H-4 shell FAILED import defect. Every repair: fail-before reproduction (or mechanical equivalent), minimum necessary change, pass-after regression test, affected local suite, recorded changed-file set, no unrelated cleanup.

**Batch 2** (only, when opened): C-2/C-3 provenance repair with manifest-enumerated provenance paths; prospective supersession-naming normalization; **H-5 lineage adjudication — may not begin until OD-20 is recorded**; M-1; M-3; M-5 including the six BOM'd governance JSONs named in v2.1; M-6 authority consolidation. Historical provenance records are never renamed or rewritten to normalize them: legacy filenames stay as historical bytes; new records follow one convention; RELEASE-MANIFEST.json enumerates current and historical paths; validation never depends on glob inference.

**Batch 3** (only, when opened): H-1 admission-before-spawn; H-2 Electron upgrade as its own unit — node-pty ABI rebuild plus sandbox, context-isolation, navigation, IPC, terminal, process, recovery, lifecycle regression bands; never combined with unrelated Electron/UI changes; M-4; remaining authorized M/L items per v2.1.

Thereafter: supply-chain closure, SBOM, vulnerability disposition, license closure (zero LICENSE files exist today), clean-room install, full deterministic program, live acceptance under §6 authority, gate/evidence reconstruction, immutable release construction, independent ratification — per RATIFICATION-EXECUTION-PLAN Phases 5–11, no architectural redesign.

**Permanently prohibited without formal amendment**: modifying immutable inputs; functional scope expansion; feature additions unrelated to an authorized defect; deleting or rewriting historical evidence; unapproved module substitution; silent source-lineage merging; builder-authored acceptance; implicit waiver; **treating the issuance or discussion of this directive as closure of any punch-list item**. Default rule: what is not explicitly permitted is prohibited until the operator clarifies.

## §5 Required tests and evidence

Measured results only — never assumed or historical expected counts. As applicable: complete module suites at exact candidate/tagged commits; shell suite against final ratified refs; explicit Debate v1.2.1 shell compatibility; a regression test for every repaired defect with fail-before/pass-after evidence where reproducible; Host/Origin/CSRF/content-type/length/schema tests; security-header tests; path containment; secrets/redaction with value-based fixture allow-list; Token Center cold-start/existing-process/foreign-process/clean-stop lifecycle; orphan detection; Windows path-with-spaces install; clean environments (no inherited .venv, node_modules, runtime state, services); build/install/verify/uninstall/rollback; final package-boundary scan; final source/provenance/manifest verification. The stale pytest.ini expected count is corrected **only within an authorized batch scope** (it is a product-byte edit) and must state host coupling truthfully. The complete deterministic suite re-runs after the last product/dependency/configuration byte change; no stale result may be cited for bytes created after that test. Every final test record binds: directive ID, candidate commit, source-manifest hash, module/version identity, test command, environment identity, measured result, UTC, executing seat. Unexplained warnings, PowerShell errors, resource leaks, or orphan processes bar unconditional acceptance of the affected band.

## §6 Spend and live authority

**DEFAULT DENY.** Without explicit operator authority, agent seats initiate no billed provider calls, no sessions with possible billing, no GPU training or serving, no Distillery compute, no operator-owned service launches, no production-account mutations. Openings: OD-2 (exact text `AUTHORIZE SD-RBR-v1.0 W-4 W-5 RELEASE`); OD-4 (attended launches for G34/G35/G53/9d/9f); OD-5 (bounded Grok-registry/G28 spend or accepted limitation); OD-8 (Distillery compute). Every grant records: exact target, spend/compute bound, allowed provider/runtime, allowed test, expiry or completion condition, UTC. Provider-free startup is first proven unable to create billed sessions; `SOW_CONDUCTOR_AUTOLAUNCH=0` enforced in configuration and immediately pre-spawn. No authorization is reusable outside its recorded scope.

## §7 Open operator decisions

OD-1 candidate designation — resolved only by a valid §10 token. OD-2 Distillery token. OD-3 MT-09 signature. OD-4 service launches. OD-5 provider spend. OD-6 SOW CLAUDE.md. OD-7 Distillery governance states — silence is not a disposition. OD-8 Distillery role. OD-9 Debate deferrals. OD-10 Token Center data. OD-11 SOVEREIGN model drift. OD-12 LATEST_MODULE_SOURCES. OD-13 Ollama default (stands unless changed). OD-14 31-h hang. OD-15 composed-system + Token Center versioning. **OD-16 — one ID, three acts: (a) worktree path, (b) reviewer designation, (c) G26 attended debug; the §10 token fills (a) and (b) only.** OD-17 GitHub debts. OD-18 5.7 disposition. **OD-19 — retired, superseded by OD-21; retained in the ledger so no one hunts a missing slot.** OD-20 H-5 lineage ruling — prerequisite to Batch-2 H-5 work. OD-21 BOM/encoding policy: release-blocking conformance or prospective hygiene. No agent seat infers an OD from discussion, likely intent, prior architecture, or convenience.

## §8 Evidence discipline

From Phase 0 onward, evidence is content-addressed, candidate-bound, gate-local where applicable, immutable once sealed, attributed (producer + UTC), and independent of mutable live files. Historical evidence is preserved exactly; oddly named legacy supersession files are not renamed in place — normalization is prospective, and the manifest (never glob behavior) determines current vs. historical provenance, source identity, artifact identity, and release relationships. C-2's foreign hash and the mismatched Debate v1.2 archive stay preserved, explicitly classified as historical defective records. Deliberate-falsification receipts are machine-tagged so undeclared `ok:false` is mechanically distinguishable. No gate cites an unsealed mutable worktree file as final evidence. The authoritative punch list is the newest operator-custodied version bound to the current candidate — v2.1 at issuance; regeneration happens against identified candidate bytes, never memory or prose.

## §9 Ratification definition — all nine simultaneously

(1) Every applicable gate reviewer-evaluated PASS or operator-accepted limitation naming the unresolved condition. (2) Every module pinned by immutable annotated tag/ref + full 40-hex commit SHA + artifact SHA-256; branch-only pins prohibited. (3) Every suite green at its release ref, measured counts recorded, reproducible from the recorded ref. (4) Composed system passes the shell/integration suite with all modules at ratified refs, dependencies reprovisioned from approved locks. (5) Final baselines rebuilt from ratified refs, cold-extraction verified, manifested, hashed. (6) No NOT_RUN, STOP, or undeclared ok:false survives undispositioned. (7) No open HIGH/CRITICAL without written operator acceptance; every accepted MEDIUM disclosed in release notes. (8) Final operator acceptance recorded verbatim with UTC, identifying the exact sealed artifacts. (9) A successor can reproduce the release from recorded refs, artifacts, and procedures without undocumented knowledge. Anything less is a candidate.

## §10 Phase-0 authorization record — external, never in-file

**Hashing rule.** The directive SHA-256 used for authorization is the SHA-256 of this finalized immutable file. The authorization token is recorded as a **separate external record** and is never inserted into these bytes — that separation is what prevents self-referential hash invalidation. Sequence: this file is completed → saved immutably in operator custody → hashed by the validator on the operator's disk → validator states the exact hash → operator records the token externally. Any subsequent edit to this file creates a new byte identity requiring a new hash and a new authorization record.

**Token location.** The token is recorded in `D:\producttion software 2\release-planning\OPERATOR-INSTRUCTIONS.log` (operator custody; created at first use). When the §2 worktree exists, the log is mirrored into the worktree's `evidence/OPERATOR-INSTRUCTIONS.log`; the custody copy remains the original. (The worktree cannot host the original: it does not exist until Phase 0 runs, and Phase 0 runs only after the token.)

**Required token** — verbatim, no unresolved placeholders (incomplete tokens confer no authority):

```
AUTHORIZE SWS-REM-DIR-20260828 AS PHASE-0 DIRECTIVE
CANDIDATE: SYSTEM BASELINE SHA-256 75e4be075dcb5629c3175850f5aa3f342c7c693f2967c3620d6cd4914ac27138
ROLLBACK: RESET BASELINE SHA-256 00ae9eefc7dada37943bb08a305020c73e2c029a6398858e8bba27078c349a7c
DIRECTIVE SHA-256: <validator-confirmed hash of this finalized file>
WORKTREE: <operator-designated path>            (OD-16a)
REVIEWER: <seat, distinct from builder>          (OD-16b)
BUILDER: <seat>
VALIDATOR: <seat>
PERMITTED: documentation and hermetic verification only until an explicit Phase-1 scope grant
UTC: <operator UTC>
```

Phase-0 authorization confers only §4 Phase-0 permissions. Later authority arrives as discrete scope grants, e.g. `OPEN SWS-REM-DIR-20260828 BATCH-1: C-1 H-3 H-4 / UTC: <ts>`; a grant covers only its listed items and expires when they are submitted as candidate evidence or the operator revokes it.

VALIDATOR CLAIM: this candidate directive asserts no gate PASS, performs no promotion, closes no punch-list item, and designates no candidate unless and until the operator records a valid external §10 token against this file's finalized hash.

### END OF DIRECTIVE

Acknowledge your seat, restate your Phase-0 constraints in one paragraph, then produce W-A through W-E. Stop after W-E.

---END PASTE---

# RATIFICATION EXECUTION PLAN — SOVEREIGN WORKSPACE SYSTEM

**STATUS: DRAFT — UNAPPROVED.** Prepared 2026-08-27 by the validator/analyst role. Companion documents: `BASELINE-FREEZE-RECORD-20260827.md` (measured facts), `DRAFT-RELEASE-DIRECTIVE-20260827.md` (Phase-0 authority template, unsigned).

Inputs: the operator's 12-phase release directive ("the Directive", Phases 0–11) and the 2026-08-27 Ratification Punch List ("the Punch List", Phases 1–6 + Done definition). The two documents use colliding phase numbers; this plan uses the Directive's numbering as the spine and maps Punch List items into it. Where this plan states a fact, its tag means: **[V]** verified today against the frozen ZIP bytes; **[U]** asserted by the Punch List against the live trees, not independently verifiable from the frozen ZIPs (live-tree access was declined for this planning pass — re-verify at execution).

---

## §1 Verification basis

Checked today, from the frozen packages only:

- [V] RESET baseline ZIP matches its sidecar (`00ae9eef…`); "- Copy" is byte-identical.
- [V] Consolidated pair (SYSTEM `75e4be07…`, RAW `8594fdc2…`) has **no sidecars** — a Phase-0 action.
- [V] GATE-LEDGER.json: 19 CANDIDATE gates with `evaluated_by: null`; 1 reviewer STOP; 8 reviewer PASS; 1 operator PASS. No 9i entry exists (also no 9a/9b) — consistent with "reconstruct or retire" (Directive Phase 9).
- [V] SOVEREIGN INSTALL-PROVENANCE carries the Distillery enterprise ZIP hash (`620e8459…`) instead of the SOVEREIGN archive's (`150e518e…`).
- [V] Debate v1.2 production ZIP fails its sidecar (`29b364b0…` vs expected `be6cfe8c…`) per SOURCE_SELECTION archive validation.
- [V] The two baselines embody **different integration decisions**: RESET keeps Debate v1.2 installed (v1.2.1 source-only); CONSOLIDATED installs v1.2.1-hardening with `advanced_past_integration_test: true` (combination unproven).
- [V] SOVEREIGN SPA source was recovered from the build-verify tree with a byte-identity proof (`index-DU3oGUhs.js`, `e0ec572e…`) because the release ZIP ships only the built bundle — confirming the exporter defect (Punch 3.4).
- [U] All live-tree claims: SOW e6fcb89 / Distillery 6cd9886 HEADs, `git tag -l` empty, 4 uncommitted SOW entries, CRLF/BOM counts, pytest.ini staleness, session-manager line numbers, absence of tests/security and .github, Distillery governance states, receipt ok:false counts, GitHub account debts.

The Punch List's "closed since 2026-08-16" set (R-01, N-01/A-5, A-1…A-9, R-03, R-12, N-02, C3) is likewise [U]. Per its own instruction these are not re-opened here — but the reviewer's Phase-1 pass should spot-check a sample rather than accept the closure list on faith.

## §2 Open operator decisions (nothing below Phase 0 proceeds until OD-1; the rest gate the phases named)

| ID | Decision | Gates | Source |
|---|---|---|---|
| OD-1 | **Designate the candidate ZIP.** RESET (sidecar-verified, Debate v1.2 installed) vs CONSOLIDATED pair (newer, v1.2.1 installed, combination untested, no sidecars). Note: choosing CONSOLIDATED largely pre-satisfies Directive Phase 1's "integrate v1.2.1" but inherits an unproven combination; choosing RESET keeps a proven-but-superseded integration and makes v1.2.1 integration new work. | Phase 0 | Freeze record |
| OD-2 | Distillery release token, verbatim: `AUTHORIZE SD-RBR-v1.0 W-4 W-5 RELEASE` | W-4/W-5 (tag v1.1.0, pin) | Punch 2.1 |
| OD-3 | Sign SOW freeze manifest MT-09 (signature, not delegable) | gate/phase-19, MT-10 | Punch 2.2 |
| OD-4 | Launch operator-owned services (SOW Electron shell, Distillery :5184, Token Center :8765) for the five NOT_RUN(SERVICE_LAUNCH_IS_OPERATOR_OWNED) legs: G34, G35, G53, 9d, 9f | NOT_RUN dispositions | Punch 2.3 |
| OD-5 | Provider spend ruling: Grok registry leg + G28 — bounded spend or accepted limitation | Phase 8; NOT_RUN dispositions | Punch 2.4 |
| OD-6 | SOW CLAUDE.md correction (resets Phase-0 signature scope) | OD-3 | Punch 2.5 |
| OD-7 | Distillery governance disposition: D-9 fail-closed, HG-3 hardware-blocked, G2–G6 not executed, F-26…F-30 deferred — resolve or scope out in release notes; silence prohibited | Distillery release | Punch 2.6 |
| OD-8 | Distillery role: status-only vs authorized training runtime (+ compute authorization if training) | Directive Phase 4 | Directive |
| OD-9 | Debate deferral set: four Phase-7 heuristics, four Phase-8 perf items, enterprise-scale soak — in or out | Debate release notes | Punch 2.8 |
| OD-10 | Token Center data: migrate, archive, or discard existing data; canonical installation selection | Directive Phase 3 | Directive |
| OD-11 | SOVEREIGN model-assignment drift: reconcile changed assignments to an operator decision | Directive Phase 2/4 | Directive |
| OD-12 | LATEST_MODULE_SOURCES: release component or generated output | Directive Phase 1 | Directive |
| OD-13 | llama.cpp: keep Ollama as production default (default stands unless changed) | Directive Phase 4 | Directive |
| OD-14 | SOVEREIGN 31-H unreproduced hang: blocks ratification or ships as known issue | Phase 7/10 | Punch 5.11 |
| OD-15 | System version assignment + Token Center first version | Phase 10 | Punch 6.2, 3.6 |
| OD-16 | Worktree path + reviewer designation + G26 attended debug session (CONDUCTOR_LAUNCH_PROCESS_DEATH — the one NOT_RUN caused by a defect) | Phase 0; Phase 4 | Directive; Punch 2.7 |
| OD-17 | GitHub account debts (RSA rotation, cached-commit GC, bio, pins) — blocks public release only | Punch 6.4 track | Punch 2.9 |

## §3 Contradiction and gap register

- **C-1 Dual candidates** [V]: two baselines, 3.5 h apart, different Debate integration states. Resolved only by OD-1. Both self-describe as non-ratified.
- **C-2 Directive vs RESET on Debate** [V]: Directive Phase 1 orders v1.2.1 integrated into the canonical tree; RESET deliberately preserved v1.2 installed. Not a defect in either — but the plan must not treat RESET as already Phase-1-compliant.
- **C-3 Phase-number collision**: Punch "Phase 1–6" ≠ Directive "Phase 0–11". Mapping fixed in §4; all future references should cite Directive phases.
- **C-4 Gate-count nuance** [V]: Punch 1.1 says "9c–9j" (19 gates); the ledger has no 9i at all. Evaluating 19 and reconstructing-or-retiring 9a/9b/9i are separate work items — do not let the reviewer's 19-gate pass silently satisfy Directive Phase 9's missing-entry item.
- **C-5 Punch severities carried, not re-scored** (its own admission): Phase-5 severity labels are the prior reviewer's. The 5.1 admit-after-spawn race is labeled HIGH and is an architecture-order defect; re-scoring belongs to the reviewer, not the builder.
- **G-1 No CI anywhere** [U]: every recorded suite result is one human on one Windows host. Directive Phase 6's clean-room proof partially compensates; a minimal CI (even local, scripted, deterministic) is the durable fix (Punch 6.1).
- **G-2 No licence files** (at least Distillery) [U]: blocks external distribution; model licences/redistribution terms also undocumented (Directive Phase 5).
- **G-3 Token Center has no release identity of any kind** [U/V-consistent]: newest, best-hardened, zero packaging.
- **G-4 Consolidated pair lacks sidecars** [V]: generate + record in Phase 0 regardless of OD-1.
- **G-5 The Punch List itself is unversioned prose in chat**: it should be committed into the worktree and hash-recorded so "the punch list" is a fixed referent.

## §4 Unified phase plan (Directive spine ← Punch List mapping)

Owners: **B** builder, **R** reviewer, **O** operator. Every phase ends by freezing its evidence content-addressed and gate-local (Directive Phase 9 discipline applies from the start, not retroactively).

**Phase 0 — Freeze authority and candidate.** O signs directive (all §2 blanks), OD-1 designated; B generates missing sidecars (G-4), initializes version control from the designated ZIP's verified contents, adds .gitignore, records starting commit + ZIP hash + manifest hash + directive hash + UTC; all other trees read-only. ← Punch: none (pre-punch). *Exit: one immutable input, one controlled worktree, written authority.*

**Phase 1 — One canonical tree.** B declares canonical locations for shell, five modules, llama.cpp adapter; integrates Debate v1.2.1 (already integrated if OD-1 = CONSOLIDATED — then the work is *proving* it: adapter repoint + shell compatibility suite, Punch 4.3); v1.2 preserved as historical artifact; OD-12 resolved; module identity + system module matrix generated. ← Punch 4.3, part of 4.1. *Exit: every executable component maps to one canonical source dir and one declared version.*

**Phase 2 — Provenance repair.** B fixes SOVEREIGN INSTALL-PROVENANCE (C-4 hash swap [V]); OD-11 reconciled; automated wrong-module-hash check added; altered Debate v1.2 ZIP quarantined; new v1.2.1 archive + manifest + sidecar cut from canonical source; extraction-wrapper defect fixed; all INSTALL-PROVENANCE files validated; RELEASE-MANIFEST.json generated. ← Punch 3.5, 4.4. *Exit: every installed byte traceable.*

**Phase 3 — Token Center.** O: OD-10. B: canonical install, launcher placement, loopback + header/CSRF enforcement, full lifecycle tests (cold start, existing-process recognition, foreign-process rejection, stop/restart/port-release/no-orphans), log-privacy check. ← Punch 3.6 (first release object), 2.3 (service launch legs). *Exit: one canonical Token Center with lifecycle evidence.*

**Phase 4 — Module/integration defects.** B repairs, each with fail-before/pass-after regression test: Punch 5.1 (admit-before-spawn race — HIGH), 5.2 (Electron 31 EOL upgrade, own unit, node-pty ABI rebuild + re-run sandbox/context-isolation/navigation/IPC/node-pty/window tests), 5.3 (two zero-cost hardening handlers — do first, independent of 5.2), 5.4 (innerHTML escapes), 5.5 (Job-Object wait → pid polling), 5.6 (receipt reader failing on undeclared ok:false), 5.7 (tests/security or retract threat-model claim + status column), 5.8 (pytest.ini contract), 5.9 (CRLF/BOM check + 4 uncommitted entries), 5.10 (metacharacter validation); plus Directive Phase-4 module lists: shell detection/hermetic tests/headers/redaction/absolute-exe/path rebase/themes; SOW canonical_registry, truthful provider discovery, Conductor spawn (G26 — OD-16 attended), autolaunch=0, no-billed-session proof; Debate v1.2.1 regression + readiness + Ollama-absent + WS lifecycle; SOVEREIGN self-test from clean artifact, dependency closure, preflight failures, real reasoning job; Distillery per OD-7/OD-8; llama.cpp manifest reconciliation, single eviction authority, VRAM/concurrency rules, unexecuted test band under explicit launch authority (OD-13 default stands). *Exit: every known defect has a regression test that failed before and passes after.*

**Phase 5 — Supply chain.** B: SBOM (Python/Node/Electron/native/models), vulnerability scan + disposition of every critical/high, deprecated JSON Schema resolver replaced, lockfile verification, native-binary hashes, secrets scan with value-based fixture allow-list (Punch 6.5), no private data packaged, model licences or external provisioning manifest (G-2). *Exit: reproducible closure, no undeclared binaries, no private data.*

**Phase 6 — Clean-room install.** B: one build / one install / one verify / one uninstall command; empty-destination, spaces-in-path, no-preexisting-state tests; absolute paths resolved at install; layout + manifest verification; documented-launcher start; no undeclared host dependencies. Reuses Punch 4.1 (reprovision from locks, Electron/node-pty vendor-hash verification). *Exit: an independent clean machine can run the whole lifecycle from artifacts + docs alone.*

**Phase 7 — Deterministic test program.** B: all suites (Punch 4.2 counts re-measured, never expected), all security/manifest/provenance bands, zero ResourceWarnings/stray errors/orphans, full re-run after last code change, results bound to candidate commit + manifest hash. OD-14 dispositioned. *Exit: all deterministic tests green against final bytes.*

**Phase 8 — Live acceptance.** O authority + spend caps (OD-4, OD-5) mandatory. Clean host, authorized launches, per-module live verification, one cross-module workflow session, screenshots, provider-disabled proof, capped live probes if authorized, crash containment, watcher exercised, rollback to previous immutable baseline demonstrated, clean shutdown. *Exit: the installed candidate demonstrates every required workflow and rollback.*

**Phase 9 — Evidence/gate system.** B rebuilds oracle predicates as behavioral assertions; recomputes goals; reconciles oracle/log/reports/ledger; fixes line-count and changed-file accounting; content-addressed gate-local evidence; reconstructs or retires 9a/9b/9i (C-4); historical records superseded, never rewritten. **R clears the backlog: 19 CANDIDATE gates individually (Punch 1.1), adjudicates the Gate-5 STOP vs 5b supersession (Punch 1.2), dispositions every NOT_RUN with O (Punch 1.3).** Builders never self-accept. *Exit: every claim independently reproducible from frozen evidence.* — Note: R's backlog work can and should start in parallel from Phase 1; it is placed here only because ledger reconciliation completes here.

**Phase 10 — Release set.** B produces: source ZIP, installer/runtime ZIP, model package or provisioning manifest, per-module raw-source archives (release objects per Punch 3.1–3.8: Distillery W-4/W-5 after OD-2; SOW phase-19 tag after OD-3/OD-6; SOVEREIGN exporter fixed + re-cut, provenance corrected; Token Center first object; Debate pin-format confirmed; shell SWS-UI-001 v1.2 object), RELEASE-MANIFEST.json, sidecars, signatures, SBOM + disposition, evidence bundle, docs, release notes (naming every accepted MEDIUM and every OD-scoped-out item), PDF report, rollback package — all naming the same release version (OD-15), commit, build ID, manifest root. Baseline ZIPs re-cut **from tags, not working trees**, cold-extraction verified (Punch 4.5). *Exit: complete immutable artifact set with one identity.*

**Phase 11 — Ratification.** R receives sealed artifacts only: verifies signatures/hashes, clean install, reruns deterministic + authorized live suites, confirms evidence matches tested bytes, issues gate-by-gate verdict. Blockers → fix → new commit → full re-review. O reviews, records verbatim acceptance with UTC in `evidence/OPERATOR-INSTRUCTIONS.log`, signs manifest, authorizes promotion. Only then: PRODUCTION label, publish to baseline location, archive former candidate, republish GitHub from ratified tags (Punch 6.4, after OD-17). *Exit: the nine-condition "Definition of ratified" holds simultaneously.*

## §5 Critical path and parallelism

The longest dependency chain is **OD-1 → Phase 0 → Phase 1 (combination proof) → Phase 4 (Electron upgrade + retests) → Phase 6/7 → Phase 8 → Phase 10 → Phase 11**, because Electron 43.x is semver-major with an ABI rebuild and everything downstream tests against the upgraded bytes.

Parallel from day one, independent of OD-1: reviewer backlog (19 gates + STOP adjudication — the Punch List is right that nothing routes around this); operator one-liners OD-2/OD-3; Punch 5.3's two zero-cost Electron handlers; sidecar generation (G-4); SBOM inventory; licence work.

Deliberately late: re-cutting baselines (only from tags), GitHub republication (only from ratified tags), any PRODUCTION labeling.

## §6 What this plan does not claim

No gate status is asserted or altered by this plan. No candidate is designated. No punch item is closed. All [U] items require re-verification against the live trees at execution time under the signed directive. This document is a builder/analyst work product submitted for operator amendment and approval.

# RELEASE / REMEDIATION DIRECTIVE — SOVEREIGN WORKSPACE SYSTEM
## SWS-REM-DIR-20260828 — CANDIDATE

**STATUS: UNAPPROVED CANDIDATE. This document confers no authority until the operator records the authorization token in §10 with this file's SHA-256, a named reviewer, and a UTC timestamp. It supersedes DRAFT-RELEASE-DIRECTIVE-20260827.md (preserved as history) and consolidates: the operator's 12-phase directive, Ratification Punch List v2.1 (2026-08-28), Adjudication Records R1 and R2, and the builder's corrected token structure.**

Role separation (binding, per the project's AGENTS.md model): the **builder** implements and submits gates as CANDIDATE and never self-evaluates; the **reviewer** is the sole writer of `status: PASS` and evaluates sealed artifacts, re-reviewing after any byte change; the **operator** is the sole authority for scope, candidate designation, waivers, spend, promotion, and final acceptance, recorded as verbatim quoted instructions with UTC. Passing tests confers no authority. Current seat assignments are recorded in §10; one seat may not hold builder and reviewer roles for the same gates.

---

## §1 Immutable inputs

All read-only, preserved indefinitely, hashes recorded in BASELINE-FREEZE-RECORD-20260827.md:

| Artifact | SHA-256 | Role |
|---|---|---|
| SOVEREIGN_SYSTEM_BASELINE_20260827.zip | `75e4be075dcb5629c3175850f5aa3f342c7c693f2967c3620d6cd4914ac27138` | **CANDIDATE** (designated by the §10 token; both builder seats and the validator's analysis support this designation) |
| SOVEREIGN_RESET_BASELINE_20260827T193540Z.zip | `00ae9eefc7dada37943bb08a305020c73e2c029a6398858e8bba27078c349a7c` | ROLLBACK / historical reference; sole custody of the pre-swap integrated state (Debate v1.2) and of the RESET-lane SOW files under H-5 adjudication |
| SOVEREIGN_RAW_SOURCE_BASELINE_20260827.zip | `8594fdc266fee721c9ef69d5595d5bab5402d2ce00de5ee65720994d4efafc9c` | Source-review projection; regenerated from tags at ratification |
| "- Copy" of the RESET ZIP | identical | Redundant duplicate; retired from the authoritative set, retained on disk |

Missing sidecars for SYSTEM and RAW are generated and recorded as a Phase-0 action. No remediation ever writes into any archive above or into the live `D:\Product Software\Production Workspace` tree.

## §2 Authority chain and worktree

`OPERATOR TOKEN → SYSTEM ZIP SHA-256 → this directive's SHA-256 → deterministic worktree construction recipe → derived initial git commit → candidate commits → sealed release refs/artifacts.` The ZIP hash is the root authority; any git commit is derived, non-authoritative until its construction recipe (extract → init → .gitignore → add → commit, with tool versions) is recorded in evidence so any seat can reproduce it. The builder's earlier sandbox commit `d746010b…` is void. The operator designates exactly one writable worktree path: `[OPERATOR: ____________]`. Everything else is read-only for release work. All PHASE0_FREEZE artifacts produced on ephemeral seats must be exported to operator custody (`D:\producttion software 2\release-planning\` or successor) before they are cited by any gate.

## §3 Modules and versions in scope

| Component | State in candidate [verified] | Release identity target |
|---|---|---|
| Shell SWS-UI-001 | v1.2, no tag/artifact/sidecar; H-4 defect open | v1.2 release object after gate backlog clears |
| SOVEREIGN | 3.1.2 + 44 recovered SPA files; C-1a/C-2 open | re-cut with fixed exporter, corrected provenance, clean packaging |
| SOW | upstream e6fcb89 [U]; H-1/H-2/H-5 open; no current provenance | gate/phase-19 tag after MT-09 (OD-3) and H-5 lineage ruling (OD-20) |
| Debate Table | v1.2.1-hardening (43695bd), director-accepted, release ZIP `d03ba417…` verified 51/51 | confirm pin format; deferral set per OD-9; no re-cut for L-6 alone (OD-21) |
| Distillery | 1.1.0rc3 @ 6cd9886 [U for commit]; version coupled in pyproject + VERSION_MATRIX | v1.1.0 tag after OD-2 token; role per OD-8 |
| Token Center | hardened tree, no version, H-3/M-1 open | first release object; version per OD-15 |
| llama.cpp lane | shell adapter only; runtime/models absent from all archives | Ollama remains default (OD-13); test band under explicit launch authority |
| Composed system | no version, combination unproven (H-6) | version assigned per OD-15; proven in Phase-4-equivalent combination testing |

## §4 Permitted changes — phase-gated

**Phase 0 (in force upon §10 token): documentation and hermetic verification only.** Permitted: freeze records, sidecar generation, worktree creation per §2, punch-list/evidence documentation, read-only verification of any frozen bytes, [R]/[U] verification legs that execute nothing billed and touch no product file. Prohibited: every product-byte mutation, provenance rewrite, version bump, dependency change, live or billed leg.

**Phase 1+ (each requires its own operator scope line):**
Batch 1 — C-1 packaging allow-list gate; H-3 Token Center contract; H-4 shell FAILED import — each with fail-before/pass-after regression tests.
Batch 2 — C-2/C-3 provenance repair (manifest-enumerated per R2 §4; supersede, never rewrite); H-5 lineage adjudication executing OD-20; M-1, M-3, M-5 (incl. the six BOM'd governance JSONs), M-6 authority consolidation.
Batch 3 — H-1 admission-before-spawn redesign; H-2 Electron upgrade as its own unit with full regression band; M-4; remaining M/L items per v2.1.
Then: supply-chain closure (SBOM, scans, licenses — currently zero LICENSE files exist), clean-room install path, deterministic program, live acceptance under §6 authority, evidence/gate rebuild, release set, independent ratification — per the RATIFICATION-EXECUTION-PLAN Phases 5–11, unchanged.

Prohibited without directive amendment, always: writing into immutable inputs; functional scope changes; feature additions; deleting historical evidence; builder-marked acceptance; treating this directive's issuance as closing any punch item.

## §5 Required tests

Per-module full suites at tagged commits with **measured counts only** (stale pytest.ini contract corrected in passing, declared host-coupled); shell compatibility suite against Debate v1.2.1 (H-6 — expectation ≠ measurement); regression test accompanying every defect repair, failing before and passing after; security band (CSRF/Host/Origin/content-type/length/schema, headers, redaction, path containment, secrets scan with value-based fixture allow-list); Token Center full lifecycle; clean-room build/install/verify/uninstall incl. path-with-spaces; zero unexplained warnings/orphans; full re-run after the final byte change; all results bound to candidate commit + manifest hash.

## §6 Spend and live authority

**DEFAULT DENY.** No billed provider calls, no GPU training/serving, no operator-owned service launches by agent seats. Openings require verbatim operator lines: OD-2 (Distillery token, exact text `AUTHORIZE SD-RBR-v1.0 W-4 W-5 RELEASE`), OD-4 (attended service launches for G34/G35/G53/9d/9f), OD-5 (bounded Grok-registry/G28 spend or accepted limitation), OD-8 (Distillery compute). Provider-free startup must be proven unable to create billed sessions before any live leg; `SOW_CONDUCTOR_AUTOLAUNCH=0` enforced at configuration and immediately pre-spawn.

## §7 Open operator decisions

OD-1 candidate designation — **resolved by the §10 token itself, explicitly**. OD-2 Distillery token. OD-3 MT-09 signature. OD-4 service launches. OD-5 provider spend. OD-6 SOW CLAUDE.md correction. OD-7 Distillery governance states (silence prohibited). OD-8 Distillery role. OD-9 Debate deferral set. OD-10 Token Center data. OD-11 SOVEREIGN model-assignment drift. OD-12 LATEST_MODULE_SOURCES disposition. OD-13 Ollama default (stands unless changed). OD-14 31-h hang disposition. OD-15 system + Token Center versioning. OD-16 worktree path, reviewer designation, G26 attended debug. OD-17 GitHub debts (public release only). OD-18 5.7: disclosure suffices or tests/security ordered. OD-20 H-5 canonical ruling. OD-21 BOM policy (subsumes OD-19). None is decided by any agent seat.

## §8 Evidence discipline

Content-addressed, gate-local evidence from Phase 0 onward; superseded records preserved (`*.previous.json` normalized to one convention, enumerated by the release manifest, never globbed); historical wrong records (C-2's foreign hash, the mismatched Debate v1.2 ZIP) quarantined and cited as history, never edited; no evidence cites mutable live files; deliberate-falsification receipts machine-tagged (M-7); the punch list current at any moment is the latest version in operator custody (v2.1 at issuance), and regeneration happens against candidate bytes only.

## §9 Ratification definition (unchanged, binding)

The nine simultaneous conditions of the 2026-08-27 punch list's "Definition of ratified" — every gate reviewer-evaluated or operator-accepted-as-limitation; every module pinned tag + 40-hex sha + artifact sha256; suites green at tags with measured counts, reproducible from fresh clones; composed system passing at ratified refs from lock-reprovisioned trees; baselines re-cut from tags and cold-verified; no undispositioned NOT_RUN/STOP/ok:false; zero open HIGH/CRITICAL without written acceptance, accepted MEDIUMs named in release notes; operator acceptance quoted verbatim with UTC in evidence/OPERATOR-INSTRUCTIONS.log; successor-rebuildable without questions. Anything less is a candidate.

## §10 Authorization token

The operator authorizes Phase 0 by recording the following, verbatim, with blanks filled (the DIRECTIVE SHA-256 below must be the hash of THIS file as delivered to operator custody — the validator recomputes and confirms it before signing; the builder's earlier sandbox hash `860451fe…` is void unless its file is delivered and matches):

```
AUTHORIZE SWS-REM-DIR-20260828 AS PHASE-0 DIRECTIVE
CANDIDATE: SYSTEM BASELINE SHA-256 75e4be075dcb5629c3175850f5aa3f342c7c693f2967c3620d6cd4914ac27138
ROLLBACK: RESET BASELINE SHA-256 00ae9eefc7dada37943bb08a305020c73e2c029a6398858e8bba27078c349a7c
DIRECTIVE SHA-256: <validator-confirmed hash of this file>
WORKTREE: <operator-designated path>
REVIEWER: <seat, distinct from builder>
BUILDER: <seat>
VALIDATOR: <seat>
PERMITTED: documentation and hermetic verification only until a Phase-1 scope line
UTC: <operator UTC>
```

Subsequent phase openings use one-line scope grants referencing this directive ID and the batch (e.g. `OPEN SWS-REM-DIR-20260828 BATCH-1: C-1 H-3 H-4`).

VALIDATOR CLAIM: this candidate directive asserts no gate status, designates no candidate by itself, and closes no punch item.

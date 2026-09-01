# DRAFT RELEASE / REMEDIATION DIRECTIVE — SOVEREIGN WORKSPACE SYSTEM

**STATUS: DRAFT — UNAPPROVED. NO AUTHORITY UNTIL SIGNED BY THE OPERATOR.**
Prepared 2026-08-27 by the validator/analyst role as input to Phase 0. Per AGENTS.md role separation: the builder implements, the reviewer alone writes PASS, the operator alone authorizes scope, waives, promotes, and accepts. This document confers none of those authorities; it is the template the operator amends and signs.

Fields marked `[OPERATOR]` require an explicit operator entry before this directive is in force.

## 1. Exact modules and versions in scope

| Component | Version at candidate | Release identity target |
|---|---|---|
| Shell/UI (SWS-UI-001) | v1.2, no tag/artifact/sidecar | v1.2 release object after gate backlog clears |
| SOVEREIGN | 3.1.2 (enterprise release 2026-08-13) | re-cut with fixed exporter + corrected provenance |
| SOW (Multi-Model Terminal) | live tree e6fcb89 [unverified from frozen bytes] | gate/phase-19 tag after MT-09 signature |
| Debate Table | v1.2.1-hardening (43695bd), director-accepted | confirm pin format matches other modules |
| Distillery | 1.1.0rc3 pending W-4 → 1.1.0 | v1.1.0 annotated tag after release token |
| Token Center | **no version exists** | first release object; version `[OPERATOR]` assigns or delegates numbering |
| llama.cpp adapter/runtime | manifest/implementation reconciliation pending | Ollama remains production default unless operator decides otherwise |
| System (composed) | **no system version exists** | `[OPERATOR]` assigns (punch 6.2) |

## 2. Authorized source locations

- Immutable inputs (read-only): the three ZIPs and reports in `D:\producttion software 2`, hashes per BASELINE-FREEZE-RECORD-20260827.md.
- Canonical candidate source: the ZIP the operator designates under OD-1 (execution plan §2).
- Writable release worktree: `[OPERATOR]` designates exactly one path. All other product trees, archives, and the live `D:\Product Software\Production Workspace` become read-only for release work.
- Historical archives (altered Debate v1.2 ZIP, superseded provenance records): quarantined, retained, never edited.

## 3. Permitted changes

Only these change classes are authorized in the release worktree; anything else requires a directive amendment:

1. Defect repairs enumerated in the execution plan Phase 4 / punch Phase 5, each with a regression test that fails before and passes after.
2. Provenance and manifest corrections (SOVEREIGN INSTALL-PROVENANCE, Debate re-archive, RELEASE-MANIFEST generation) — superseding, never rewriting, historical records.
3. Release-engineering additions: tags, sidecars, SBOM, CI configuration, version identity, .gitignore, install/verify/uninstall tooling.
4. Evidence and gate-system rebuilds per directive Phase 9 (content-addressed, gate-local, no self-marked acceptance).
5. Dependency upgrades explicitly listed: Electron 31 → supported line (with post-upgrade re-tests), deprecated JSON Schema resolver replacement. No other dependency moves without amendment.

Prohibited without amendment: writing into any immutable input or protected tree; changing module functional scope; adding features; deleting historical evidence; builder-marked gate acceptance.

## 4. Required tests

- Per-module full suites at the tagged commit, **measured counts recorded, never expected counts** (punch 4.2 lists last-measured figures; they must be re-measured, and pytest.ini's stale suite contract corrected — punch 5.8).
- Shell compatibility suite against Debate v1.2.1 (the adapter targets the v1.2 contract; compatibility is an expectation until the suite says otherwise — punch 4.3).
- Clean-room build/install/verify/uninstall on an empty destination, including a path-with-spaces account (directive Phase 6).
- Security band: CSRF/Host/Origin/content-type/length/schema on all state-changing routes, security headers, redaction, path containment, secrets scan, Token Center lifecycle tests (directive Phases 3–5).
- Live acceptance per directive Phase 8, only under §5 authority below.
- Zero unexplained warnings, orphan processes, or stale evidence; results bound to candidate commit + source-manifest hash.

## 5. Provider / GPU spending authority

**DEFAULT: DENY.** No billed provider calls, no GPU training or serving.
- `[OPERATOR]` Grok registry leg + G28 live workers: authorize bounded spend of `[amount/limits]`, or accept both as release limitations.
- `[OPERATOR]` Distillery hardware gate (HG-3 BLOCKED_HARDWARE_CAPACITY): status-only component, or authorized training runtime with explicit compute authorization.
- Provider-free startup paths must be demonstrated unable to create billed sessions before any live testing (SOW_CONDUCTOR_AUTOLAUNCH=0 enforced at config and immediately pre-spawn).

## 6. Reviewer and promotion authority

- Builder: implements, submits gates as CANDIDATE, may never self-evaluate. (Standing BUILDER CLAIM: no gate submitted for evaluation is asserted PASS by the builder.)
- Reviewer: `[OPERATOR names the independent reviewer]` — sole authority to write `status: PASS`; evaluates sealed artifacts, not the development tree; re-reviews after any byte change.
- Operator: sole authority to designate the candidate (OD-1), issue tokens (e.g. `AUTHORIZE SD-RBR-v1.0 W-4 W-5 RELEASE`), sign freeze manifests (MT-09), disposition NOT_RUN/STOP legs, accept limitations, sign the release manifest, and promote. Final acceptance is recorded as a live-session instruction quoted verbatim with UTC into `evidence/OPERATOR-INSTRUCTIONS.log`.

## 7. Promotion definition

Artifacts may be labeled PRODUCTION only when all nine conditions of the punch list's "Definition of ratified" hold simultaneously, the reviewer recommends promotion, and the operator signs the release manifest. Anything less remains a candidate.

---

Operator signature block (unsigned):

```
DIRECTIVE ID: ______________  ACCEPTED/AMENDED: ______________
CANDIDATE (OD-1): __________________________________________
WORKTREE PATH: _____________________________________________
REVIEWER: __________________________________________________
SPEND AUTHORITY: ___________________________________________
UTC: __________________  OPERATOR: _________________________
```

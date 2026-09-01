# EPC-01 — ENTERPRISE PRODUCTION COMPLETION DIRECTIVE
## Autonomous single-seat completion loop over Punch List V7 · 2026-08-30

**Basis seal.** `bb5edf8a522502d1d2329d3904b3bf72fbd5f140` (RATIFY-01)
**Queue of record.** `PUNCH-LIST-V7-20260830.md` — sha256 `66f8c2ff95e466460462e0c05d0271f3f8d38307a41a714cdb75e0c1682a9c02`, 24,750 bytes, recorded ENTRY 024
**Protocol lineage.** Amends and replaces `LOOP-PROTOCOL-20260828.md` (Annex A to SWS-REM-DIR-20260828 R2) for this programme only. Annex A's role separation, exception discipline and custody cadence are retained; its seat roster, decision block and queue are superseded.

---

## §1 · Goal

Bring the Sovereign Workspace to **enterprise production level as a single-operator product**, by executing Punch List V7 to queue exhaustion without operator interrupts.

"Enterprise production level" is fixed by operator ruling at T-1 and means: a product an enterprise can deploy to individual operator workstations with a clean install, a supported upgrade path, an auditable supply chain, real observability, complete operator documentation, and a stated support and versioning policy. It explicitly **does not** mean multi-tenant deployment. P4-1 (authentication), P4-2 (RBAC), P4-3 (security audit logging) and P4-8 (multi-user) are **scoped out by ruling and must be written into the release notes as named limitations — never omitted, never silently absent.**

The loop's second-order goal, standing above any individual item: **act in the best interest of the system.** Where an item as written would make the system worse, the loop records why, parks the item, and proceeds. Completing the list is not the goal; the goal is the product.

---

## §2 · Seats, and an honest statement of their limits

| Role | Seat | Function |
|---|---|---|
| Builder | claude (cowork), builder pass | Implements. **Never writes PASS.** |
| Reviewer | claude (cowork), reviewer pass | Evaluates each item against criteria written *before* implementation. Writes CANDIDATE-ACCEPTED-BY-REVIEWER or FAIL. |
| Validator | claude (cowork), validator pass | Re-derives every claim from bytes. Audits bundles. Writes the record. |
| Operator | sam | Owns promotion. Not involved until T-2. |

**Declared limitation, recorded because it is real.** All three seats are one model in one session. This is **weaker independence than the multi-seat harness** used for CONVERGE-01 and LOCAL-01, where builder, reviewer and validator were different models. A single seat can enforce role *discipline* — separate passes, criteria frozen before implementation, verdicts derived only from artifacts — but it cannot manufacture genuine independence of judgment. Three controls compensate, and none of them fully closes the gap:

1. **Criteria-before-code.** No item may begin implementation until its pass criteria are written, mechanically checkable, and recorded. The reviewer pass evaluates against the recorded criteria only.
2. **Artifact-only review.** The reviewer pass may read the changed files, the test output and the evidence bundle. It may not rely on the builder's stated reasoning.
3. **Byte re-derivation.** The validator pass re-computes every number it publishes — hashes, counts, exit codes — rather than quoting the builder or reviewer.

Any gate verdict this loop produces carries this limitation on its face. **The loop may move a gate from CANDIDATE to reviewer-evaluated. It may not write PASS, and it does not promote.**

---

## §3 · Authority and exceptions

### 3.1 Granted (operator ruling, T-1, 2026-08-30)

- Full authority to implement, test, refactor and document anything inside `D:\producttion software 2\release-worktree` and to write bundles to `D:\producttion software 2\release-planning\`.
- **Authority to overwrite `release-artifacts/`** with clean rebuilds. Build output only; regenerable from any commit.
- Authority to create scratch destinations, virtual environments and package installs **scoped inside a scratch directory or the worktree**. Never system-wide.
- Authority to commit and re-seal on the working branch.
- Authority to run every test suite, release gate, mutation harness and static analysis in the tree.
- Authority to start and stop the four HTTP services on loopback for testing, and to stop them again.

### 3.2 Withheld (not granted at T-1 — these PARK, they do not stop)

| Withheld | Consequence in the loop |
|---|---|
| Provisioning a VM or container | **V-1 executes in reduced form** — see §6 V-1. A true bare-metal clean room is parked for T-2. |
| Launching GUI applications unattended | **V-2 and V-3 park as E-5.** Queued for T-2, never attempted. |
| Rewriting git history | **OD-37 stays open by design.** The loop re-seals; it does not rewrite. Rewriting would invalidate every commit SHA, the seal, and every hash in the gate ledger — a governance loss far larger than the exposure it closes, and the distribution is already clean of it. |
| System-wide package mutation | All installs are scratch- or worktree-scoped. |

### 3.3 Exceptions — the ONLY reasons this loop may halt

Retained from Annex A §2, amended for this programme:

- **E-1 Money.** Any action with possible billing, provider session, or GPU compute not covered by a recorded grant.
- **E-2 Irreversibility.** Any write outside the worktree, the scratch tree and the custody folder; any deletion of an immutable input; anything in §3.2.
- **E-3 Spec contradiction.** The directive, punch list and bytes disagree in a way that changes what the work IS, not how hard it is. Record, park, continue; halt only if it poisons the whole queue.
- **E-4 Security regression.** A change makes a verified security property worse. **Loopback-only enforcement is a verified security property** — any change that weakens it is E-4 by definition.
- **E-5 Physical-operator dependency.** Requires the operator's attended presence, signature, or screen. Queued for T-2, never an ad-hoc stop.
- **E-6 Queue exhausted.**

**Nothing else is a reason to stop.** "I am uncertain whether the operator would like this" is specifically not a reason to stop: §4 is the operator's stated will; apply it.

---

## §4 · Decision block — every default resolved

The operator granted full authorization and required that the loop not stop to ask. Therefore **no entry in this block may read "cannot default."** Each line below is the ruling the loop applies.

| ID | Ruling applied by the loop |
|---|---|
| **D-1 Scope** | Enterprise-grade **single-operator**. P4-1/2/3/8 scoped out and written into release notes as named limitations. |
| **D-2 Licence** | **Proprietary, all rights reserved, commercial.** The loop writes complete licence text, generates the third-party NOTICE from the SBOM, resolves the 173 undeclared components, and takes a written position on the 14 MPL-2.0 components. The licence file is marked `PENDING COUNSEL SIGN-OFF`. **The loop drafts; it never self-approves a licence.** |
| **D-3 Versioning** | Adopt a single **product version** for the composed workspace, currently `1.0.0-rc.1`. Per-module versions remain independent and are documented in a versioning policy (P4-10) rather than forced into alignment. Forcing six modules to one number would destroy real provenance. |
| **D-4 State location** | Runtime state moves to `%LOCALAPPDATA%\SovereignWorkspace\` with the install root read-only after install. This is the root fix for P0-5, P1-1 and P4-4 and is implemented once, in Batch 4. |
| **D-5 Upgrade model** | Side-by-side install with state migration, not in-place overwrite. `install.ps1` keeps its refuse-non-empty guard; a new `upgrade.ps1` handles version-to-version transition and state carry-forward. |
| **D-6 Observability** | Adopt stdlib `logging` with a JSON formatter, levels, rotation, and a single configurable destination under the state root. `print()` is permitted only in CLI tools whose output *is* the interface. |
| **D-7 Telemetry** | **None.** No crash reporting, no phone-home, no opt-in framework. A sovereign local product does not emit. P4-7 closes as a written architectural decision, not an implementation. |
| **D-8 Install authority** | `tools/release/install.ps1` is the single authoritative install path. The README manual sequence and `shell/tools/install_sow.py` are reconciled to it or removed. |
| **D-9 Vendored-history tests** | The two structurally unsatisfiable tests (P2-10) are **skipped with a recorded reason naming the absent upstream commit**, not deleted and not silently xfailed. |
| **D-10 Reproducibility** | Pin `git archive` entry times so artifacts are content-addressed. If pinning proves infeasible, the release notes state plainly that per-module hashes change per seal regardless of content. |
| **D-11 Boundary gate** | `package_boundary_gate` is repaired to scan the *archive*, not the working tree, and to honour the fixture allowlist. A gate that always fails is worse than no gate. |
| **D-12 Schema freeze** | The thirteenth schema is **admitted** — `deployment-manifest.schema.json` is legitimate. The freeze count, the amendment record and the published integrity hash are updated to twelve→thirteen with attribution to commit `e2e8e55`. |
| **D-13 Gate evaluation** | The reviewer pass evaluates all 19 CANDIDATE gates in one continuous sweep, one consolidated verdict table. Verdicts are `ACCEPTED-BY-REVIEWER`, `REJECTED` or `NOT-EVALUABLE-WITHOUT-OPERATOR`. **No PASS is written.** |
| **D-14 Ceiling** | The ratified 8B nameplate class (OD-36, ENTRY 021) stands. Any change that would evict `granite4.2:8b` or `qwen3:8b` from the live slate is E-3. |
| **D-15 Docs cut** | `docs/` internal build artifacts (58 of 85) are export-ignored, not deleted. ADRs, DECISIONS, DISCOVERY, RELEASE-ASSURANCE and the new operator documentation ship. |
| **D-16 Parked items** | Parked items accumulate with a dossier each and do not stop the loop. A dossier states: what was attempted, the exact blocking condition, and what the operator would need to do. |

---

## §5 · The loop

```
for each item in the ordered queue:

  ┌─ CRITERIA PASS ────────────────────────────────────────────────┐
  │ write mechanically checkable pass criteria BEFORE any edit     │
  │ record them in the batch bundle; they are frozen from here     │
  └────────────────────────────────────────────────────────────────┘
                              ↓
  ┌─ BUILDER PASS ─────────────────────────────────────────────────┐
  │ capture fail-before evidence (the failing command + output)    │
  │ make the MINIMAL change that satisfies the criteria            │
  │ capture pass-after evidence                                    │
  │ run the affected suite; record counts and exit code            │
  │ NEVER write PASS                                               │
  └────────────────────────────────────────────────────────────────┘
                              ↓
  ┌─ REVIEWER PASS ────────────────────────────────────────────────┐
  │ read ONLY: changed files, test output, evidence bundle         │
  │ evaluate against the FROZEN criteria                           │
  │ verdict: CANDIDATE-ACCEPTED-BY-REVIEWER | FAIL                 │
  └────────────────────────────────────────────────────────────────┘
                              ↓
     FAIL → builder fixes within the same authorized scope
            re-review · MAX 3 CYCLES
            3 fails, or criteria not mechanically evaluable
            → PARK with dossier, take next item
                              ↓
  ┌─ VALIDATOR PASS (per batch, not per item) ─────────────────────┐
  │ re-derive every published number from bytes                    │
  │ confirm zero tracked modifications outside the item's scope    │
  │ seal the batch bundle to release-planning\                     │
  └────────────────────────────────────────────────────────────────┘
```

**No seat stops the loop between items.** Parked items do not stop the loop.

### 5.1 Regression discipline

Every defect item ships with a test that **fails before the fix and passes after**. An item whose fix cannot be expressed as a failing test parks under E-3 rather than being accepted on assertion. This is the rule that makes single-seat review meaningful: the test, not the reviewer's judgment, carries the verdict.

### 5.2 Scope discipline

The builder pass touches only files named in the item's criteria. A change outside that set is either a dependency the criteria failed to name — in which case the criteria are amended, the amendment recorded, and the reviewer pass re-run from the amended criteria — or scope creep, which is reverted.

---

## §6 · The queue

Batches run in order. Items inside a batch may run in any order unless a dependency is named. **Dependencies drive the order, not severity** — which is why P0-5 sits in Batch 4 rather than Batch 1: it cannot be fixed before D-4 relocates state.

**Already closed before the loop starts.** **P3-8** (stale zero-byte `.git/index.lock`) was removed during the systems analysis and `git fsck` reported clean. It carries no queue row. It is named here so that its absence from the batches reads as closed rather than overlooked.

### PHASE 0 — Freeze
| # | Item | Pass criteria |
|---|---|---|
| 0.1 | Record T-1 authorization | Operator's authorization recorded verbatim with UTC in `OPERATOR-INSTRUCTIONS.log`; this directive hashed and recorded |
| 0.2 | Re-verify basis | `git rev-parse HEAD` = `bb5edf8a…`; `git status --porcelain --untracked-files=no` empty; V7 hash re-computes to `66f8c2ff…` |
| 0.3 | Baseline capture | All seven suites run and counts recorded as the before-state; any deviation from Appendix A of V7 is investigated before work starts |

### BATCH 1 — P0 clearance (the items that stop distribution)
| Item | Pass criteria |
|---|---|
| **P0-3** | `release-artifacts/` rebuilt at HEAD; all 8 sidecars verify; `unzip -Z1` over source and install zips returns **0** for `^evidence/`, **0** for `^dev/`, **0** for the N-40 config path, and **254** for `modules/sow/docs/evidence/`; `release-build-manifest.json` names the current seal |
| **P0-2** | `modules/sow/requirements.txt` exists with a pinned transitive closure including `jsonschema`; `install.ps1` provisions a SOW environment from it; `main.js` invokes that interpreter, not bare `py`; **a new test imports `control_plane.ipc.run_gateway` in an environment without ambient `jsonschema` and passes** |
| **P0-1** | `LICENSE` contains complete commercial licence text with no "interim" or "to be supplied" language; all five module licences consistent; `NOTICE` generated from the SBOM listing every third-party component and its licence; a written position on the 14 MPL-2.0 components; file header marks `PENDING COUNSEL SIGN-OFF` |
| **P0-4** | `README.md` contains zero absolute paths outside the install root; the install section is the D-8 authoritative path; a literal-string test asserts no `D:\Product Software`, `D:\producttion software 2`, `Sov 1` or `Debate table` occurrence in README |

### BATCH 2 — Fast defects (single pass, all mechanically testable)
| Item | Pass criteria |
|---|---|
| **P1-4** | `pytest --collect-only` at the repo root exits 0 with no `INTERNALERROR`; same in an extracted archive; a regression test asserts collection succeeds from the root |
| **P1-5** | The shell suite runs from the repo root and from `shell/`; both green except environment-only failures, which are skipped with reason |
| **P1-7** | `test_no_tracked_text_file_carries_a_utf8_BOM` green; `main.js` and `preload.js` byte-identical apart from the removed BOM (diff proves it) |
| **P2-11** | Terminal-suite pin updated to the measured count; test green; the pin's docstring states that a rise requires a deliberate update |
| **P3-7** | `sovereign-ui` carries a real version consistent with D-3; a test asserts no shipped `package.json` has version `0.0.0` |
| **P2-8** | `THREAT_MODEL.md` carries the registered-absence marker; `test_threat_model_honesty` green; the marker's presence is itself asserted |
| **P1-2** | Per D-12: freeze count updated to 13, amendment record created naming `e2e8e55`, integrity hashes republished; **all 6 cascading failures green**; `freeze_manifest_check` exits 0 |

### BATCH 3 — Test integrity
| Item | Pass criteria |
|---|---|
| **P1-3** | The three `/ready` tests patch the seam `/ready` actually calls; each fails if `installed_model_records` is broken (proven by deliberate mutation); ready / degraded / unavailable are each asserted distinctly |
| **P2-10** | Both tests skip with a reason naming the absent upstream commit; `pytest -rs` output shows the reason; no test silently xfails |
| **P3-1** | The debate suite writes temp state only under `tmp_path`; after a full run `git status --porcelain` shows zero new untracked entries under `modules/debate/`; a guard test equivalent to SOW's `test_runtime_writes_are_gitignored` exists for debate |
| **P3-9** | Runtime caches gitignored at every level they occur; `git status` clean after a full suite run |
| **V-5** | With P1-4 fixed: one whole-product `pytest` invocation from the root completes and reports a single consolidated count. **This number has never existed before; it becomes the product's baseline.** |

### BATCH 4 — Lifecycle triad (D-4, D-5) — the largest single piece of work
| Item | Pass criteria |
|---|---|
| **P4-4** | All runtime writes resolve under `%LOCALAPPDATA%\SovereignWorkspace\`; a test asserts no module writes inside the install root at runtime; the four services start and answer healthy with state relocated |
| **P0-5** | `uninstall.ps1` succeeds on an installation that has been used (proven: install → generate runtime state → uninstall → exit 0); offers `-KeepData` and `-PurgeData`; `-KeepData` leaves the state root intact; neither branch deletes unrecorded files silently |
| **P1-1** | `upgrade.ps1` exists; proven by installing version A, generating state, upgrading to version B, and asserting state carried forward and services healthy |
| **P4-5** | Documented state inventory; `backup.ps1` / `restore.ps1` producing and consuming a single verifiable archive; round-trip proven by test |

### BATCH 5 — Supply chain and provenance
| Item | Pass criteria |
|---|---|
| **P2-1 / P2-2 / P2-3** | SBOM regenerated **from lock files, not from `.venv`**; zero component paths resolve into any `.venv`; Python coverage complete for all five modules (no `parked` markers); `source-commit` equals the seal at generation time; generation wired into `build_release.ps1` so it cannot drift again |
| **P2-4** | Every path in `RELEASE-MANIFEST.json` resolves in the archive, or is explicitly declared as an out-of-archive reference with a stated reason; all declared sha256 verify; `release_manifest_check` exits 0 with zero dangling |
| **P2-5** | Every SBOM component carries a licence declaration or is listed in a written exceptions register with a reason; `NOTICE` complete; MPL-2.0 position recorded |
| **P2-6** | `npm audit` reports 0 vulnerabilities at any severity in both node roots; the audit is wired into the release gates |
| **P2-7** | Per D-10: two consecutive builds of an unchanged module produce identical sha256, or the limitation is documented in release notes with a measurement proving it |
| **P2-9** | Per D-11: `package_boundary_gate` scans the archive, honours the allowlist, and exits 0 on a clean archive and non-zero on a deliberately planted violation (**both directions proven**) |
| **P1-8** | The MCP registration pins this repository's root; a test asserts no shipped config names a path outside the install root |

### BATCH 6 — Hygiene
| Item | Pass criteria |
|---|---|
| **P3-2** | Zero files in the built archive contain the developer username, `Product Software`, `producttion software 2`, `Sov 1` or `Debate table`; a release gate asserts this and fails the build if violated |
| **P3-3** | Per D-15: internal build artifacts export-ignored; archive `docs/` contains only operator-facing documents; count asserted by test |
| **P3-4 / P3-5** | Shipped configs carry template placeholders, not absolute paths; `rebase_adapters` resolves them; **proven by V-1** rather than asserted |
| **P3-6** | `llamacpp.json` declares a state class truthfully — a third state is added to the schema, or the adapter is marked not-installed. `state_class: runnable` against a nonexistent path is not permitted to ship |

### BATCH 7 — Enterprise-grade single-operator (D-1, D-6, D-7)
| Item | Pass criteria |
|---|---|
| **P4-6** | Structured `logging` with levels, JSON formatter and rotation, writing under the state root; `print()` remaining only in CLI tools whose output is the interface; a test asserts no library module calls bare `print()` |
| **P4-7** | Per D-7: closed as a written architectural decision recording that the product deliberately emits nothing. No implementation. |
| **P1-9** | Per D-8: exactly one authoritative install path exists. The README manual sequence and `shell/tools/install_sow.py` are either reconciled into `install.ps1` or removed; whichever survives is named as authoritative in `docs/INSTALL.md`; a test asserts no second install procedure is documented anywhere in the shipped tree |
| **P4-9** | `docs/INSTALL.md`, `docs/OPERATIONS.md`, `docs/TROUBLESHOOTING.md`, `docs/SECURITY.md` and a workspace-level `THREAT_MODEL.md` exist; each is executable in the sense that every command in it was run and its output recorded |
| **P4-10** | `docs/SUPPORT-POLICY.md` states platform support, version scheme (D-3), deprecation policy and compatibility guarantee |
| **P4-1/2/3/8** | **Closed as documented limitations.** Release notes state plainly: no authentication, no RBAC, no security audit log, single-operator only, loopback-only by design. A test asserts these statements are present — the disclosure cannot silently disappear the way P2-8's marker did. |

### BATCH 8 — Continuous integration
| Item | Pass criteria |
|---|---|
| **P1-6** | A Windows CI lane definition exists that runs every suite, every release gate, the boundary gate, `npm audit`, and the V-1 clean-room; it is proven by local execution of the same script the lane invokes, so the lane is not merely declared |

### BATCH 9 — Gate evaluation (D-13)
| Item | Pass criteria |
|---|---|
| **G-1** | All 19 CANDIDATE gates evaluated in one continuous reviewer sweep; one consolidated verdict table with per-gate evidence citations; each verdict is `ACCEPTED-BY-REVIEWER`, `REJECTED` or `NOT-EVALUABLE-WITHOUT-OPERATOR`; **the §2 independence limitation is stated on the table itself**; ledger updated with `evaluated_by` populated; **no PASS written** |

### BATCH 10 — Verification
| Item | Pass criteria |
|---|---|
| **V-1** | **Reduced clean-room, executed:** install to a fresh empty destination from the built install zip; a Python 3.12 venv with no ambient `jsonschema`; run `install.ps1` end to end; assert extraction, hash verification, venv provisioning, `npm ci`, adapter rebasing, and install-manifest emission all succeed; then start the services from the *installed* copy and probe all four health endpoints. **Bare-metal clean room parked for T-2** — this proves the install path, not a virgin OS. |
| **V-4** | All four mutation harnesses run; results recorded; any surviving mutant is a finding, not a pass |
| **V-2 / V-3** | **PARKED — E-5.** GUI launch not authorized at T-1. Dossiers written, queued for T-2. |

### CLOSE
| # | Item | Pass criteria |
|---|---|---|
| C.1 | Full re-run | Every suite green or skipped-with-reason; consolidated count recorded |
| C.2 | Re-cut | All 8 artifacts rebuilt at the final seal; sidecars verify; exposure assertions from P0-3 re-run and green |
| C.3 | Re-seal | Commit; new seal recorded |
| C.4 | Final bundle | `EPC-01-FINAL-REPORT` with: item-by-item verdicts, parked dossiers, the gate verdict table, before/after test baselines, and an explicit statement of what remains |
| C.5 | Record | ENTRY in `OPERATOR-INSTRUCTIONS.log`; punch list V8 emitted covering only what remains |

---

## §7 · Custody

One consolidated bundle per batch, written to `D:\producttion software 2\release-planning\bundles\EPC01\`, containing: frozen criteria, changed-file set with hashes, fail-before and pass-after evidence, suite counts and exit codes, the reviewer verdict table, and any parked dossiers. The validator pass audits each bundle against the bytes on arrival.

The loop does **not** emit one interrupt per item, per batch, or at all. It reports at C.4.

---

## §8 · Exit criteria

The loop is complete when **all** of:

1. Every Batch 1 through Batch 10 item is `CANDIDATE-ACCEPTED-BY-REVIEWER` or `PARKED` with a dossier.
2. Every parked item has a dossier naming the exact blocking condition and what the operator must do.
3. A whole-product test run exists and its count is recorded (V-5).
4. All 8 release artifacts are rebuilt at the final seal and pass the P0-3 exposure assertions.
5. The 19 gate verdicts are recorded with `evaluated_by` populated and the independence limitation stated.
6. `EPC-01-FINAL-REPORT` and `PUNCH-LIST-V8` are written and hashed.
7. The worktree has zero tracked modifications outside committed work.

**The loop does not write PASS. The loop does not promote. Ratification remains the operator's, at T-3.**

---

## §9 · Standing constraints

- **Never weaken loopback-only binding.** E-4 by definition.
- **Never write a PASS.** Builder never; reviewer writes CANDIDATE-ACCEPTED-BY-REVIEWER; validator writes measurements.
- **Never delete an immutable input.** Evidence, ledgers and prior bundles are append-only.
- **Never claim a measurement not taken.** "Not measured" is always an available and preferred answer.
- **Never let a disclosure become silent.** Every scoped-out limitation is asserted by a test, because P2-8 proved a documented disclosure can vanish without one.
- **Report faithfully.** If an item fails, say so with the output. If a step was skipped, say that.

---

**VALIDATOR CLAIM.** This directive asserts no gate status, closes no punch item, and takes effect only when named in a recorded operator authorization. It supersedes `LOOP-PROTOCOL-20260828.md` for this programme only.

Authored by: validator seat claude (cowork), 2026-08-30

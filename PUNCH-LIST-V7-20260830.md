# PUNCH LIST V7 — what stands between this and a production enterprise product

**Seal:** `bb5edf8a522502d1d2329d3904b3bf72fbd5f140` (RATIFY-01)
**Date:** 2026-08-30 · **Supersedes:** PUNCH-LIST-V6-20260830.md (`872979ea…`)
**Basis:** every item below was measured this session on the target platform
(Windows 11, Python 3.12.10, Node 24.16.0) or read from bytes at the seal.
Items carried from V6 are re-verified, not copied.

---

## 0. Executive position

The system is **materially healthier than the standing record implied**. 4,726
automated tests pass on the target platform, all four HTTP services start and
answer healthy, the operator's 8B ceiling is enforced in a running system, 45 of
45 resolvable manifest hashes verify, no live credential ships, and all 598
shipped Python files parse clean.

What blocks an *enterprise* release is not code quality. It is four things:

1. **One legal blocker** — the licence is an unfinished placeholder.
2. **Two delivery blockers** — a runtime dependency nothing installs, and
   shipping artifacts that predate the fix they were supposed to carry.
3. **A lifecycle gap** — the product can be installed but not upgraded, and
   cannot be uninstalled once used.
4. **Nineteen gates nobody has reviewed** — governance, not engineering.

Counts: **44 items**. P0 ×5, P1 ×9, P2 ×11, P3 ×9, P4 ×10.

---

## P0 — blocks any external distribution

### P0-1 · The licence is a placeholder
**Evidence.** `LICENSE`, line 2, verbatim: *"Interim license record; canonical
license text to be supplied by the operator before external distribution."* All
five module `LICENSE` files carry the same interim header.
**Impact.** No enterprise customer's legal review passes an "all rights reserved"
placeholder that names itself provisional. This blocks distribution outright,
independently of any technical readiness.
**Fix.** Operator supplies canonical licence text; propagate to root and five
modules; reconcile with the third-party obligations in P2-5.
**Effort.** Legal decision + 1 hour to apply.

### P0-2 · The product does not run on a clean machine — `jsonschema` is never installed
**Evidence — chain executed, not inferred.**
- `modules/sow/apps/desktop/main.js:444` spawns `py -3.12 -m control_plane.ipc.run_gateway` — the **system** interpreter, no virtualenv.
- Importing that module with `jsonschema` blocked at `sys.meta_path` returns **`IMPORT FAILS without jsonschema`**.
- `jsonschema` is imported by **12 SOW runtime modules** (`adapters/roster.py`, `control_plane/gates/{criteria,engine}.py`, `control_plane/ipc/envelope.py`, `control_plane/orchestration/session_approvals.py`, `control_plane/recovery/succession.py`, `debate_service/service.py`, `node_runtime/context/loaders.py`, `node_runtime/gate/local_gate.py`, `node_runtime/supervisor/provider_node_registration.py`, `persistence/store.py`, `tools/live/emit_provider_node_registration.py`) plus 8 test modules. V6 said "at least eight"; the measured figure is 12.
- It is declared **only** in `requirements-dev.txt`, unpinned, as `jsonschema>=3.2`. SOW has no runtime requirements file.
- `tools/release/install.ps1` provisions Python for SOVEREIGN (3.12, from `WORKSPACE-RESOLVED-LOCK.txt`) and Debate (3.14, from `requirements.lock.txt`), and runs `npm ci` for two node roots. **It never provisions anything for SOW.**

**Impact.** On a clean Windows host with Python 3.12 and no `jsonschema`, the SOW
IPC gateway dies on import the first time the desktop app spawns it. Highest
severity item in this list.
**Fix.** Create `modules/sow/requirements.txt` with a pinned transitive closure;
add a SOW venv (or a pinned system install) to `install.ps1`; change `main.js` to
invoke that interpreter rather than bare `py`.
**Effort.** Half a day, plus a clean-VM proof (V-1).

### P0-3 · The shipping artifacts are stale and still carry the exposure OD-35 closed
**Evidence.** OD-35 changed what `git archive` emits. It did not change the eight
files in `release-artifacts/`, which were cut at the previous seal at 01:15 and
are what a recipient actually receives. Measured in those files:

| artifact | entries | `evidence/` | `dev/` | N-40 config |
|---|---|---|---|---|
| `sovereign-workspace-1.0.0-rc.1-source.zip` | 4206 | 2041 | 580 | **present** |
| `sovereign-workspace-1.0.0-rc.1-install.zip` | 4207 | 2041 | 580 | **present** |

The producer was re-run at the new seal into a separate directory
(non-destructive; `release-artifacts/` untouched). It cuts clean: source
29,803,530 → 10,409,052 bytes, install 29,971,904 → 10,482,906, root `evidence/`
0, `dev/` 0, N-40 absent, the 254 nested SOW receipts preserved.
**Impact.** Until `release-artifacts/` is replaced, the N-40 populated
authorization switch and the full development record are on disk in the files
that ship.
**Fix.** Replace the eight artifacts and their sidecars from the new build.
**Effort.** Minutes. Verified replacements already built.

### P0-4 · The operator-facing README instructs installing from the developer's directory
**Evidence.** `README.md:15`: *"SOVEREIGN and Debate Table come from the packaged
archives in `D:\Product Software\`"*, followed by two `Expand-Archive` commands
against that path and a named ZIP ending `- Copy.zip`.
**Impact.** The primary install document points at a directory that exists on one
machine. A recipient following the README fails at step 1.
**Fix.** Rewrite the README install section to the supported `install.ps1` path.
**Effort.** 1–2 hours (see also P1-9, divergent install paths).

### P0-5 · Uninstall refuses to run on any installation that has been used
**Evidence.** `tools/release/uninstall.ps1:39-46` builds `$actualInstall` from
`Get-ChildItem -Recurse` over the entire install root and compares it to the
install manifest with `SetEquals`. Any file created after install — a log, a
published document, a queue entry — is "extra" and the script throws
*"Refusing uninstall because exact installed path set changed"*. Runtime state is
declared to live **inside** the install root: `${root}/runtime`,
`${root}/published`, `${root}/library/queues`, `${root}/logs`.
**Impact.** The product cannot be cleanly removed once used. The intent — refuse
to delete unexpected files — is sound; the lifecycle is unhandled. If the check
were relaxed instead, line 71's `Remove-Item -Recurse -Force $destRoot` would
delete all operator data with no export and no prompt. Both branches are wrong.
**Fix.** Move runtime state out of the install root (P4-4); teach uninstall to
distinguish install-manifest paths from runtime paths; add an explicit
`-KeepData` / `-PurgeData` choice.
**Effort.** 1–2 days, coupled to P4-4.

---

## P1 — blocks production use

### P1-1 · No upgrade path exists
`install.ps1`, `uninstall.ps1`, `verify_install.ps1` and `install_shortcut.ps1`
ship. **Zero upgrade or update scripts exist.** `install.ps1` refuses a non-empty
destination, so the only route from 1.0.0-rc.1 to any later build is uninstall
(blocked by P0-5) then reinstall (destroying state, P4-4). *Effort: 2–3 days.*

### P1-2 · Frozen schema set drifted — 13 files where 12 are asserted
`deployment-manifest.schema.json` was added in commit `e2e8e55` (2026-08-29, the
day before the seal) without updating the assertion or recording an amendment.
**Six of SOW's twelve test failures plus the freeze-check exit-1 all cascade from
this single addition.** The published integrity hash also drifted (`338D9822…`
where `B1A16B22…` was expected). The drift machinery worked; nobody reconciled
what it found. *Effort: 2–4 hours.*

### P1-3 · The debate readiness contract has no test coverage
Three `/ready` tests monkeypatch `app.installed_models`, but `/ready` was
refactored to call `installed_model_records()` (`modules/debate/app.py:2087`).
The patch no longer intercepts, so the tests reach real ollama and fail with
`ConnectError`/`"unavailable"` instead of the asserted `RuntimeError`/
`"degraded"`. **The endpoint is correct** — live it returns `200 "ready"` with
both seats installed. The tests are dead, and the ready/degraded/unavailable
distinction is currently unguarded. *Effort: 2 hours.*

### P1-4 · Repo-root `pytest` crashes, and the crash reaches the recipient
`modules/sow/conftest.py:26` calls `item.path.resolve().relative_to(ROOT)` on
every collected item; anything outside `modules/sow` raises `ValueError` and
pytest aborts with `INTERNALERROR`. Reproduced in the worktree **and in the
extracted archive**, where a customer running `pytest` at the root gets a crash
rather than a result. One `is_relative_to()` guard away from working.
*Effort: 15 minutes.*

### P1-5 · There is no way to run the shell suite as shipped
`shell/tests/__init__.py:12` does `from shell.tests._fswatch import FsWatch`, an
absolute import requiring the archive root on `sys.path` — so the suite must run
from the root, which is exactly where P1-4 crashes. The two defects compose.
With P1-4 fixed the suite passes 189/190 (the one failure being `fatal: not a
git repository`, an archive artifact). *Effort: folded into P1-4.*

### P1-6 · No continuous integration of any kind
Zero workflow files (`.github/workflows`, GitLab, Azure). Every result in this
document was produced by hand. The release tooling is Windows-only by
construction, so a lane needs Windows runners. Without CI, P1-2-class drift
recurs silently. *Effort: 3–5 days for a meaningful lane.*

### P1-7 · Two core Electron files carry a UTF-8 BOM
`apps/desktop/main.js` and `apps/desktop/preload.js`, written by PowerShell 5.1
`Set-Content -Encoding UTF8`. A BOM is content: it changes the blob and every
hash taken over it. Caught by `test_text_integrity`. *Effort: 15 minutes.*

### P1-8 · A shipped MCP registration pins a foreign development path
The codex registration pins its working directory to
`D:/multi model terminal app/sovereign-orchestration-workspace` — the original
upstream tree, not this repository. Caught by
`test_mcp_registration_provenance`. *Effort: 1 hour.*

### P1-9 · Three divergent install paths are documented and implemented
`README.md` describes a five-step manual install; `tools/release/install.ps1`
implements a different automated one from a pre-built ZIP; `shell/tools/
install_sow.py` is a third mechanism for SOW specifically. No document states
which is authoritative. *Effort: 1 day to consolidate and document.*

---

## P2 — integrity, provenance and supply chain

### P2-1 · The SBOM is generated from the developer's virtual environments
164 of its 168 path-like strings resolve into `modules/debate/.venv` and
`modules/sovereign/.venv`, six of them naming CPython **3.14** binaries
(`_yaml.cp314-win_amd64.pyd`, `pip3.14.exe`, …). Composition: 208 npm, 96 PyPI,
173 unpurled file entries. *Effort: 1–2 days to regenerate from locks.*

### P2-2 · The SBOM declares its Python coverage incomplete, in its own metadata
`c2-python-audit: unavailable-not-in-lock`, `sow-python-lock: partial-parked`,
`distillery-python-lock: missing-parked`. The disclosure is honest, and the
consequence is that **the SBOM cannot be used to vulnerability-scan a
mostly-Python product** — the first thing an enterprise buyer will do with it.
*Effort: folded into P2-1.*

### P2-3 · The SBOM declares a source commit two commits behind the seal
`sovereign:source-commit = b39d4721…`, an ancestor of the seal, now two behind.
One of those two commits is this programme's own. *Effort: minutes, once P2-1 is
regenerated as part of the build.*

### P2-4 · `RELEASE-MANIFEST.json` has four dangling paths, one of them its own hash authority
53 path-like strings, 49 resolve, 4 dangle:
`release-artifacts/release-build-manifest.json` (declared
`release_archive_hash_authority`, gitignored and by construction never shipped),
`bundles/PHASE0/WORKTREE-RECIPE.md`, and two `modules/distillery/dist/*` wheels.
**Of 54 declared sha256 pairs, 45 resolve and all 45 verify with zero
mismatches** — the hashing is sound; the path set is not. *Effort: 2 hours.*

### P2-5 · Third-party licence coverage is incomplete
**173 of 477 SBOM components carry no licence declaration**; 10 are `UNKNOWN`;
14 are MPL-2.0 (file-level copyleft, needs a redistribution position). No
NOTICE/attribution file ships. *Effort: 2–3 days.*

### P2-6 · One HIGH-severity npm advisory
`nanoid < 3.3.18` (GHSA-2v37-7h3g-55p8, CVSS 5.9) in the SOVEREIGN UI, transitive
via the Vite/Vitest build chain, `fixAvailable: true`. Not a runtime dependency —
but it is a **HIGH** in every scanner an enterprise buyer runs. The SOW Electron
tree audits clean at **0 vulnerabilities**; Electron is at `^43.4.1`.
*Effort: 30 minutes.*

### P2-7 · Release artifacts are not reproducible across seals
The six per-module zips are **byte-identical in content** across the two seals
(verified by extraction and `diff -r`) but carry different sha256, because
`git archive` stamps entries with the commit time. A consumer diffing artifact
hashes between releases sees all eight change when only two did — so
"hash-verified" is not a change-detection signal here. *Effort: 1 day
(`--mtime` pinning or a deterministic packer).*

### P2-8 · `THREAT_MODEL.md` lost a required disclosure marker
`test_threat_model_honesty` requires the registered-absence marker
`ADVERSARIAL TESTS are **NOT IMPLEMENTED**`. It is gone. The test exists
precisely to stop an unimplemented protection from quietly reading as
implemented. *Effort: 30 minutes.*

### P2-9 · `package_boundary_gate` fails
Exit 1: 23,834 files scanned, 20,093 violations, 9 credential hits, 2,114
quarantined-historical, 0 allowlisted. The credential hits are **all declared
fixtures** (`sk-synthetic000000000000000000`, declared at
`tools/../export_enterprise.py:69` as `DECLARED_FIXTURE_SECRETS`) and the bulk of
violations are runtime caches in the working tree — but a release gate that
always fails teaches everyone to ignore it. *Effort: 1 day to make it meaningful.*

### P2-10 · Two shipped tests are structurally unsatisfiable
`distillery::test_regression_locus_is_ancestor_of_head` and SOW's
`test_u326_before_after_import_hygiene` both `git show` commits (`e80a5b1a…`,
`b529314`) belonging to the modules' **upstream** histories, absent from this
66-commit consolidated repository. They can never pass as shipped and are not
defects in the code they test. *Effort: 2 hours to skip-with-reason or re-point.*

### P2-11 · The terminal-suite coverage pin is stale
225 tests reported against a pin of 222 — a rise, i.e. new coverage that should
update the pin. *Effort: 15 minutes.*

---

## P3 — hygiene and residue

| ID | Item | Evidence | Effort |
|---|---|---|---|
| P3-1 | Debate suite litters the source tree | Running it created **16 untracked `.r*` directories** under `modules/debate/tests/`, not cleaned up and not gitignored. SOW has a guard for this class (`test_runtime_writes_are_gitignored.py`); debate has none. | 2 h |
| P3-2 | Developer identifiers still in the shipped archive | **35 files** carry the username, **56** the `Product Software` tree, **25** a `Sov 1` path. All in shipped product, not the development record. OD-35 removed 68 of 103 and 199 of 255 — it did not reach zero. | 1–2 d |
| P3-3 | `docs/` ships the workshop | **58 of 85** markdown files are internal build directives, review pastes, resume pastes and stop reports (`CP-M1-G26-DECISION-PASTE.md` and kin). Same exposure class OD-35 addressed in `evidence/`, in a directory OD-35 did not touch. ADRs, DISCOVERY and DECISIONS legitimately ship, so the cut is not mechanical. | 1 d |
| P3-4 | `shell/config/install.json` ships developer paths as seed values | `modules_root: D:\producttion software 2\release-worktree`, `python_312: C:\Users\Sslaw\…\python.exe`. `install.ps1:91-99` and `rebase_adapters.py` do rewrite these — so it is a leak, not a functional break. | 2 h |
| P3-5 | Six module registry files hardcode this machine's paths | `shell/modules/{debate,distillery,sovereign,sow,tokencenter}.json` `root` values; distillery and tokencenter additionally hardcode the absolute path to this machine's `python.exe` rather than templating it. Rewritten at install — unproven until V-1. | 2 h |
| P3-6 | `llamacpp.json` declares `state_class: runnable` against a placeholder | Its own description calls the root *"a NEUTRAL PLACEHOLDER, not a real path"*; the path exists on no machine. The schema offers no third state for "present but not installed" (S-19). | 4 h |
| P3-7 | `sovereign-ui` ships as version `0.0.0-private` | A shipped component with a null version is a provenance and SBOM smell. | 15 m |
| P3-8 | Stale `.git/index.lock` observed | Zero-byte, dated 2026-08-30T01:43, from a git write that died during LOCAL-01. Removed this session; `fsck` clean. Recorded because it indicates an interrupted tooling path. | done |
| P3-9 | Runtime caches tracked in the working tree | `.pytest_cache` at three levels and `modules/sow/.runtime` predate this session and drive most of P2-9's violation count. | 1 h |

---

## P4 — enterprise category gaps

These are not defects. **The product is architected as a single-operator local
workstation tool, and it is a good one.** "Enterprise" implies requirements it
does not currently address at all. Each needs a product decision before it needs
engineering.

| ID | Gap | Current state |
|---|---|---|
| P4-1 | **Authentication** | None. Services are unauthenticated. |
| P4-2 | **Authorization / RBAC** | None. No user or role concept exists. |
| P4-3 | **Audit logging** | None in the security sense. The gate ledger records build events, not operator actions. |
| P4-4 | **State location** | Runtime state lives *inside the install root* (`runtime/`, `published/`, `library/queues/`, `logs/`) rather than `%LOCALAPPDATA%`/`%PROGRAMDATA%`. Root cause of P0-5 and P1-1. |
| P4-5 | **Backup / restore / export** | No mechanism. No documented state inventory. |
| P4-6 | **Observability** | **4 files use `logging`; 93 use bare `print()`.** No log levels, no structured output, no destination configuration, no rotation. |
| P4-7 | **Crash reporting / telemetry** | None, and no opt-in framework to add one defensibly. |
| P4-8 | **Multi-user / concurrency** | Single-operator by construction. Loopback-only binding is *enforced* (`server.py:191-193` refuses non-loopback) — a genuine strength for the current design and a hard stop for shared deployment. |
| P4-9 | **Operator documentation** | No RUNBOOK, OPERATIONS, ADMIN, INSTALL, TROUBLESHOOTING or SECURITY document. `THREAT_MODEL.md` exists only inside `modules/sow`. README is 97 lines and starts from the wrong path (P0-4). |
| P4-10 | **Platform, support and versioning policy** | Windows-only by construction. No stated support model, SLA, deprecation policy or compatibility guarantee. Six unrelated version numbers ship (`1.0.0-rc.1`, `3.1.2`, `1.1.0rc3`, `0.1.0`, `0.0.0-private`, `v1.2.1-hardening`) with no document explaining the scheme. |

---

## G — governance

### G-1 · Nineteen of twenty-nine gates have never been evaluated
The ledger holds 29 rows: **9 PASS** (all historical, gates 0–6), **19 CANDIDATE
with `evaluated_by: null`**, **1 STOP** (gate 5, adjudicated *correct*). No
candidate gate has been evaluated by anyone, all programme. Gate activity spans
2026-08-21 to 2026-08-27; gates 9a and 9b do not exist.

**No amount of engineering moves this number.** It requires a reviewer and
calendar time, and it is the true gate on the word "ratified". Now disclosed to
recipients in `docs/RELEASE-ASSURANCE.md`.

---

## V — verification still owed

| ID | Item | Why it matters |
|---|---|---|
| V-1 | **Clean-room install on a bare VM** | P0-2 was proven by import-blocking, not by installing to a bare host. P3-4/P3-5 path rewriting is unproven. This single test would validate or refute six items. |
| V-2 | End-to-end SOVEREIGN cycle through the UI | Services answer healthy; no work has been driven through them. |
| V-3 | Launch the Electron desktop app | 1,104 unit tests pass; the app itself was never started. |
| V-4 | Run the mutation harnesses | `test:falsify`, `:authority`, `:orchestration`, `:panewrite` exist and were not run. |
| V-5 | Re-run every suite after P1-4 | The repo-root crash means no whole-product test result has ever existed. |

---

## Appendix A — measured test baseline (first run on the target platform)

| Suite | Pass | Fail | Notes |
|---|---|---|---|
| SOW pytest | 2,959 | 12 | 10 m 14 s |
| SOW Electron (`node --test`) | 1,104 | 0 | 1 m 43 s |
| shell pytest | 189 | 1 | sole failure is `not a git repository` |
| distillery pytest | 240 (+431 subtests) | 1 | |
| debate pytest | 176 | 5 | |
| tokencenter pytest | 32 | 0 | |
| SOVEREIGN UI vitest | 26 | 0 | `tsc -b --noEmit` clean |
| **Total** | **4,726** | **19** | |

Every suite was also run against the extracted archive, which showed 18/18/6
failures for SOW/debate/distillery against 12/5/1 in the worktree. **The
difference is not defects** — those suites shell out to `git`, and an extracted
archive is not a repository. Both runs are recorded so the distinction is
auditable.

## Appendix B — live service baseline

| Service | Endpoint | Result |
|---|---|---|
| SOVEREIGN | `:5175/v1/health` | 200 · `status: ok`, `product_version: 3.1.2`, `configured_models_ready: true`, `missing_configured_models: []`, `loopback_only: true`, `same_origin: true`, all 5 routes up |
| Debate | `:8700/` · `/ready` | 200 · `status: "ready"`, ollama reachable, both seats installed |
| Distillery | `:5184/health` | 200 · `{"ok":true,"status":"idle","compute":false}` |
| TokenCenter | `:8765/healthz` | 200 · `{"ok":true,"collector_error":null}` |

SOVEREIGN's live slate is `qwen3:8b`, `deepseek-r1:8b`, `dolphin3:8b`,
`granite4.2:8b` — the ratified 8B nameplate class, enforced in a running system.
`granite4.2:8b` at 8.8B true parameters is admitted **only** because OD-36 was
ratified; under a strict 8.0e9 arithmetic bound this slate would not stand. The
ratification is load-bearing, not ceremonial.

## Appendix C — what is verifiably sound

Recorded so the list above is read in proportion.

- 4,726 tests pass on the target platform; 598 shipped Python files all parse clean.
- All four services start and answer healthy; the model ceiling holds live.
- 45 of 45 resolvable `RELEASE-MANIFEST` hashes verify, zero mismatches.
- Three of four release gates PASS (`release_manifest_check`, `check_model_consistency`, `check_governance_bom`).
- **No live credential ships.** Every credential-pattern hit in the archive is a declared detector fixture.
- The installer is well engineered: zip-slip guarded, artifact hash-verified against its sidecar, refuses a filesystem-root destination, refuses a non-empty destination, refuses to overwrite existing shortcuts, and emits a per-file sha256 install manifest.
- The release producer refuses to build with tracked modifications, cuts all eight artifacts explicitly from a named commit, and rejects strangers in its output directory.
- Loopback-only binding is enforced in code, not merely configured.
- `tiktoken` is correctly optional-guarded; `torch`/`bitsandbytes` are confined to a standalone characterization CLI no product path calls — **E-2's closure stands**.
- The drift-detection machinery (schema freeze, BOM check, threat-model honesty, coverage pins) **works**; P1-2, P1-7, P2-8 and P2-11 are all cases of it correctly reporting drift nobody reconciled.

---

## Recommended sequence

1. **P0-3** — replace the stale artifacts. Minutes; the exposure is live now.
2. **P0-2 + V-1** — pin and provision SOW's runtime dependency, then prove it on a bare VM. This is the difference between "works here" and "is a product".
3. **P0-1** — the licence decision. Legal, not engineering, and nothing ships without it.
4. **P1-4, P1-7, P2-11, P3-7** — the fifteen-minute items. Clear them in one pass.
5. **P1-2** — reconcile the schema addition; six failures fall out together.
6. **P0-4 + P1-9 + P4-9** — one documentation pass covering install, runbook and troubleshooting.
7. **P0-5 + P1-1 + P4-4** — the lifecycle triad. Move state out of the install root and the other two become tractable.
8. **P1-6** — CI, to stop P1-2-class drift recurring.
9. **G-1** — start gate review in parallel with all of the above. It is the long pole and nothing else shortens it.

**Estimate.** P0 and P1 are **3–5 focused engineering days**. Verification (V-1
through V-5) is **about a week** on top. P4 is a product-scope decision, not a
schedule. **G-1 is unbounded and is the real gate.**

The honest summary: **weeks from shippable, not months — and the code is not what
is holding it.**

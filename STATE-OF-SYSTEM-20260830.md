# STATE OF SYSTEM — SOVEREIGN WORKSPACE

**Seal of record:** `0b6e3fe7346e7eaa581ce7fc02cf36f46e58bc75` (LOCAL-01)
**Version:** 1.0.0-rc.1 · **Compiled:** 2026-08-30 · **Seat:** validator (claude, cowork)
**Supersedes:** the state sections of PUNCH-LIST-V5-20260830.md

## Method, so you can discount this correctly

Every number below was re-derived from bytes in this session. Repository
measurements were taken in `release-worktree` at the seal. Distribution
measurements were taken from a clean `git archive 0b6e3fe7 | tar -x` export
(3,508 files), because that — not the working tree — is what a recipient gets.

Three things I could **not** measure from this seat, named so nothing here is
mistaken for proven:

1. **No live run.** The Ollama daemon is not reachable from the validator VM.
   Everything about model selection below is verified in code and tests, not on
   screen.
2. **No suite run on the target platform.** This seat is Linux/Python 3.10; the
   product is Windows/Python 3.12. Where I ran tests I say so and I say what the
   failures actually were.
3. **No clean-room install.** Still never performed, by anyone, all programme.

---

# 1 · WHAT EXISTS

A Windows-local shell (`SWS-UI-001`) supervising six module adapters, backed by
3,577 tracked files and 2,999 test functions across 287 test files.

| module | adapter state | open action | what it is |
|---|---|---|---|
| **SOVEREIGN** | runnable | browser :5175 | Orchestration engine; five model roles under `SYSTEM_MANIFEST.json` |
| **SOW** | runnable | `focus_window` | Multi-Model Terminal — Electron, node-pty, xterm; Conductor + worker panes |
| **Debate Table** | runnable | browser :8700 | Multi-model debate service |
| **Distillery** | runnable | browser :5184/console | Grounded distillery, stdlib `ThreadingHTTPServer` |
| **Token Center** | runnable | browser :8765 | Token accounting |
| **llama.cpp** | runnable | none | Declared optional; installer skips it |

**Python test coverage by module** (test files / total Python files):

| module | python files | python test files |
|---|---:|---:|
| SOW | 333 | 169 |
| Distillery | 76 | 29 |
| Debate | 34 | 22 |
| Token Center | 4 | 3 |
| **SOVEREIGN** | **74** | **0** |

**SOVEREIGN has seventy-four Python files and no Python tests at all.** Its only
automated tests are five TypeScript/TSX UI tests under `ui/ui_shell/tests/`. The
orchestration engine itself — `system_manifest.py`, `publication_gate.py`,
`praxis/memory_gate.py`, the model-role machinery and the `/v1/health` server
whose probe you watched fail — has zero test coverage in this repository.

That is the sharpest coverage finding in the system. SOVEREIGN is the module you
reported failing to start, the module whose role assignments carry the ceiling
ruling, and the module carrying the contradicting manifest below — and nothing
tests it.

---

# 2 · WHAT THE ARCHIVE ACTUALLY CONTAINS

This is the single most consequential measurement in the report.

| directory | files | size |
|---|---:|---:|
| `modules/` | 1,385 | 23 MB |
| `evidence/` | **1,545** | **74 MB** |
| `dev/` | 357 | 4 MB |
| `docs/` | 95 | 2 MB |
| `shell/` | 76 | 3 MB |
| `tools/` | 28 | 1 MB |
| **total** | **3,508** | **103 MB** |

**1,902 of 3,508 files (54%) and roughly 78 of 103 MB (76%) of the shipped
archive are the development record**, not the product. `evidence/` alone is
larger than every module combined.

That is not a tidiness complaint. It is the root of three separate findings:

* the populated frontier authorization file (N-40) lives in `evidence/cpm1/`;
* 103 files carrying the developer username `Sslaw` and 255 carrying
  `Product Software` are overwhelmingly under `evidence/`;
* a customer receiving this archive receives the complete internal history of
  the project, including every failed run, every stop report, and every path on
  the developer's machine.

**One decision — does development history ship? — resolves most of N-40, most of
S-16/N-18, and removes three quarters of the archive's bytes.**

---

# 3 · WHAT IS PROVEN

Measured at the seal, from a clean export, this session:

| gate | result |
|---|---|
| `provenance_cross_hash_check` | **PASS** — all five modules OK |
| `check_governance_bom` | PASS |
| `check_model_consistency` | PASS *(but see N-43 — it does not read the file that contradicts it)* |
| `release_manifest_check` | PASS (repository scope, which is its designed scope) |
| `package_boundary_gate` | PASS on a clean tree *(see N-44)* |
| `innerhtml_sink_audit` | PASS — 39 templates, 112 interpolations, all classified |

**`provenance_cross_hash_check` is the headline.** It has been red since
CONVERGE-01, it was the release blocker, and it is now green — measured, not
reported.

Also verified in bytes: the 8B ceiling refuses `phi4:14b` (14.7B),
`deepseek-r1:70b` (70.6B), `ornith:9b`, embedding models and Ollama Cloud
pointers, each with a stated reason, pinned by test. The two picker defects you
reported personally — phi4:14b authorized to spawn, deepseek-r1:70b offered then
refused after the click — are closed at the classifier. No model was pulled. The
`live_operation.json` interlock holds on the production path.

---

# 4 · WHAT IS CLAIMED BUT NOT MEASURED

Named so it is not mistaken for cleared.

* **The 2,999-test suite has never been verified green by anyone but the seat
  that ran it.** There is no CI. There is no recorded suite artifact inside the
  seal.
* **`modules/sow/conftest.py:26` makes a whole-repository pytest run impossible.**
  It computes `item.path.resolve().relative_to(ROOT)` against `modules/sow`, so
  any single invocation that collects tests both inside and outside SOW dies with
  an INTERNALERROR at collection. Measured directly. Suites must be run per-area.
* **The release producer has never been run.** No `release-build/` directory and
  no `release-build-manifest.json` exists at custody root, so F-7's rewritten
  8-item cut plan is unexercised.
* **The release tooling is Windows-only by construction.** `rebase_adapters.py`
  builds paths with backslashes; its five tests fail on POSIX for that reason
  alone. A CI lane for this product needs Windows runners.
* LV-1…LV-8 live verification, the S-15 tamper re-exercise, the N-39 `FF FE`
  census, and a clean-room install all remain untaken.

**What I actually ran here:** `tools/release/` on Linux/3.10 gave 81 passed,
6 failed, 1 file uncollectible. All 7 non-passes are environment artifacts —
Windows path separators and a Python 3.12-only f-string. **None is a defect.**
I report them only so the numbers are not later mistaken for a red suite.

---

# 5 · WHAT IS DEFECTIVE

## 5.1 The interlock (N-40 · HIGH · your desk alone)

`evidence/cpm1/before-b/modules/sow/config/live_operation.json` is **tracked and
ships**. It reads `live_operation_authorized: true`, scope
`openai_codex_cli, claude_code, grok_build`, Conductor bound to `gpt-5.6-sol`.

Its sibling copy of the interlock computes `_DEFAULT_PATH` from `__file__`, so it
resolves to that file. I loaded it from the archive export: it returns
**AUTHORIZED**.

Stated precisely, without inflation: **your production interlock is intact.**
`modules/sow/config/live_operation.json` is absent and DENIED-by-absence holds.
I found no path by which the production tree loads the evidence copy. But the
guarantee written in the file and repeated across your governance documents —
*"a fresh clone stays DENIED-by-absence"* — **is false of this repository.**

Attribution: inherited from `cf50cde`, the P0-2 baseline import. Not the
builder's doing. It has been in every seal this programme has produced, and I
missed it in three prior audits.

## 5.2 The ceiling ruling (N-41 · your desk)

`model_ceiling.py` sets the limit at 9.0e9 — "the 8B nameplate class" — where
ENTRY 017 says *"nothing above eight billion parameters."* The tension is real
and the builder documented it openly: `qwen3:8b`, the model you named to prove
the system with, truly carries 8.2B.

**Correction to my own first reading.** I initially recorded the module's
"changes no behaviour today" line as false. It is not. The two readings it
compares are *nameplate ≤ 8B* and *true < 9.0B* — two implementations of the
same nameplate class, which do select an identical set. The builder also left a
test whose docstring says that tightening to a strict 8.0e9 must fail and send
the question back to ENTRY 017 rather than silently drop your model. That is a
deliberate tripwire, not concealment, and it is good practice.

The finding is narrower and it stands: **the ceiling as built admits models above
8.0e9 true parameters, and that reading is load-bearing.**
`SYSTEM_MANIFEST.json` now assigns:

| role | model | true params | under your literal words |
|---|---|---|---|
| PRIMARY_REASONER | `qwen3:8b` | 8.2B | refused |
| ADVERSARIAL_CHALLENGER | `granite4.2:8b` | **8.8B** | refused |
| CRITIC | `dolphin3:8b` | 8.0B | admitted |
| SYNTHESIZER | `deepseek-r1:8b` | 8.2B | refused |

**Four of five sealed roles fail a strict arithmetic reading, and SOVEREIGN
would not start under one.** The builder chose the reading that keeps your
system running, documented it, and flagged it. What is missing is only your
ratification: a boundary you ruled on was interpreted, and S-9 says park rather
than decide. One line either way.

## 5.3 The contradicting role table (N-42/N-43 · MED)

Commit `f0fe9dd` is subject-lined *"bring **every** model assignment under the
operator's 8B ceiling."* It changed `SYSTEM_MANIFEST.json` and left
`modules/sovereign/PACKAGE_MANIFEST.txt` declaring, under its own header
`CURRENT DEFAULT MODEL-ROLE ASSIGNMENTS`: `qwen3:14b`, `qwen3:32b`,
`qwen2.5:14b-instruct`. Three models the ceiling refuses. S-4 violation.

Not a runtime misroute — no code parses that file for roles. But it is
hash-enumerated at `RELEASE-MANIFEST.json:98`, its hash is the
`source_content_manifest_sha256` in `modules/sovereign/INSTALL-PROVENANCE.json`,
and `docs/DISCOVERY.md:29` records its table as `FACT`.

And `check_model_consistency` — the gate named for exactly this — **never reads
that file.** It verifies README, `model_hierarchy.json` and `cross_channel`
against `SYSTEM_MANIFEST.json` and stops. It passes while a shipped,
hash-enumerated document contradicts it. This is the S-15 pattern in a second
location: a checker whose name implies coverage it does not have.

## 5.4 The optional adapter that cannot run (new · MED)

`shell/modules/llamacpp.json` declares `state_class: runnable` with an argv
pointing at `C:/sovereign-workspace/optional-runtimes/llama.cpp/current/llama-server.exe`
— a path its own description calls *"a NEUTRAL PLACEHOLDER, not a real location"*
that exists on no machine.

The schema offers only two classes: `runnable` and `not_started`. **There is no
"present but not installed" class.** So the honest state you asked for after
N-22 — *present and unavailable* — is not currently expressible. This is S-19
("no enabled control that does nothing") reappearing as a schema limitation
rather than a UI bug.

---

# 6 · CORRECTIONS TO THE STANDING RECORD

Two long-parked exception items are **materially smaller than their labels**, and
one is **sharper**. Correcting them is the point of this seat.

**E-2 — "Distillery has no Python lock" — is not a defect.** Distillery imports
**zero** third-party packages. It serves :5184 on
`http.server.ThreadingHTTPServer`, pure standard library. An empty dependency set
is the accurate declaration, not a missing one. **E-2 should be closed, not
worked.**

**E-1 — "SOW has only unpinned ranges" — is half-solved and half-mis-stated.**

* The **Electron** side is reproducible. `package.json` carries `^` ranges, but a
  `package-lock.json` v3 resolves all 19 packages to exact versions with
  integrity hashes, and `install.ps1:126` uses **`npm ci`**, which installs
  strictly from the lock. The ranges are cosmetic under `npm ci`. Residual risk
  is network dependence on `registry.npmjs.org`, not version drift.
* The **Python** side has a real, specific defect that the old wording obscured.
  SOW's only dependency file is `requirements-dev.txt`:
  `jsonschema>=3.2` and `pytest>=7`. There is no runtime requirements file — yet
  **`jsonschema` is imported by at least eight runtime modules**, including
  `control_plane/ipc/envelope.py`, `debate_service/service.py` and
  `node_runtime/context/loaders.py`. A production install that installs runtime
  dependencies only gets no `jsonschema` and SOW's control plane fails at import.

**E-1 restated: a runtime dependency is declared in a dev-only file, unpinned,
with no lock.** That is narrower and far more fixable than "no reproducible
provisioning," and it does not need a ruling about where locks come from — it
needs a `requirements.txt` and a pin.

**S-16 in executing configuration is handled.** The three config files carrying
`Sslaw` — `shell/config/install.json`, `shell/modules/distillery.json`,
`shell/modules/tokencenter.json` — are all rewritten at install
(`install.ps1:97-99`, `rebase_adapters.py:38-39`, proven by
`test_rebase_adapters.py`). The residual 103/255-file population is historical
evidence and documentation, which ships but does not execute — a disclosure
problem, resolved by the §2 decision.

---

# 7 · THE HONEST DISTANCE TO ENTERPRISE PRODUCTION

**What this system is today:** a well-instrumented, heavily-governed
controlled-development candidate with a genuine hash-rooted authority chain, a
real gate suite, ~3,000 tests, and a release blocker that just went green. The
engineering discipline in this repository is above what most shipped products
carry.

**What it is not:** a product anyone has installed on a machine other than this
one, run end-to-end, or reviewed.

Four things separate those two states, and only one of them is code:

1. **Nothing has ever been reviewed.** The ledger holds 29 gate rows: 9 PASS (all
   historical, gates 0–6), **19 CANDIDATE with `evaluated_by: null`**, and Gate 5
   at **STOP**. Not one candidate gate has been evaluated by anyone, all
   programme. Four review packages sit ready and unopened. This is the longest
   pole in the project and it is not close.
2. **Nothing has ever been installed clean.** Every install proof is a same-host
   proxy on a machine that already has Python, Node and Ollama. E-3 stands
   untouched.
3. **Nothing runs unattended.** No CI exists, the suite cannot be invoked in one
   command, and the release producer has never been run.
4. **The archive ships the workshop with the product** (§2).

None of that is a reason to unwind the seal. All of it is why the seal is not
yet reviewable, and why "enterprise production" is a programme rather than a
punch list of defects.

---

# 8 · WHAT I GOT WRONG THIS SESSION

Recorded because the pattern matters more than the individual error.

I reported `release_manifest_check` failing with 12 problems and traced a
regression to CLOSEOUT-01. **Withdrawn.** `.gitattributes:37` sets
`*.pre-CONVERGE01 export-ignore`, which removes those paths from `git archive`
only; they stay tracked, and the checker is repository-scoped by construction.
I had invoked a gate outside the scope it was written for and read the result as
a defect — the same error class as my earlier `package_boundary_gate`
retraction.

I also nearly reported an undeclared `pywin32` dependency in SOW. The win32
imports came from inside `node_modules`; my exclusion filter was applied to the
wrong stream. Caught before it reached you, and named here so the method error
is visible rather than buried.

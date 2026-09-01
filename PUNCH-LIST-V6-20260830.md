# PUNCH LIST V6 — SOVEREIGN WORKSPACE → ENTERPRISE PRODUCTION

**Seal of record:** `0b6e3fe7346e7eaa581ce7fc02cf36f46e58bc75` (LOCAL-01) · version 1.0.0-rc.1
**Compiled:** 2026-08-30 · **Seat:** validator (claude, cowork)
**Supersedes:** PUNCH-LIST-V5-20260830.md `b232ae4793ae70015ab0d3bb0a2f340d26eafabdc76e5e7e8c92ab58663a650b`
**Companion:** STATE-OF-SYSTEM-20260830.md `4f713a79fef491656bd264894948aafff1fbb80c722a1f9ddabb55cae902bc5e`

Ordered by what unblocks the most. Every item carries an owner seat and the
evidence that closes it. Nothing here closes on a green test alone (S-17).

---

# 0 · WHAT CHANGED SINCE V5

**Closed and verified, not merely reported:**

| was | now |
|---|---|
| **OD-27** — provenance cross-hash red since CONVERGE-01 | **CLOSED.** PASS, all five modules, re-derived from a clean export |
| **N-25** — no local models in the SOW picker | **CLOSED at the classifier.** Whole library enumerated through one ceiling authority |
| **N-24** — picker offered `phi4:14b` and authorized it to spawn | **CLOSED.** Refused with a stated reason, pinned by test |
| deepseek-r1:70b offered, then refused after the click | **CLOSED.** Refused before it is offered |
| **N-22** — llamacpp absent from the shell | **half-closed.** Present again, but see **P-9** |
| **N-23** — Open button did nothing | **CLOSED.** `focus_window` + `POST /api/open` |
| **N-27** — SOVEREIGN readiness probe failed | **CLOSED.** `probe.py` timeout was `min(5, poll_ms/1000)` = 0.5 s against a 1.25 s health endpoint |
| **N-16 class** — runtime writes in the tracked tree | **CLOSED** for receipts; 7 more instances found and parked as N-38 |
| **E-2** — "Distillery has no Python lock" | **NOT A DEFECT — close it.** Distillery imports zero third-party packages |

**Newly raised:** N-40 · N-41 · N-42 · N-43 · N-44 · N-45 · N-46, and the
llamacpp state class. **Withdrawn by me:** the CLOSEOUT-01 manifest regression
(method error — see the state report §8).

---

# 1 · YOUR DESK — nothing below moves without these

| # | ruling, in plain terms | unblocks | my recommendation |
|---|---|---|---|
| **OD-35** | **Does the development record ship?** `evidence/` and `dev/` are 1,902 of 3,508 files and 76% of the archive's bytes. Ship them, or cut them at `export-ignore`? | N-40, most of S-16/N-18, archive size, and what a customer sees | **CUT THEM.** One `.gitattributes` change removes the populated authorization file, 103 username files, 255 personal-path files, and 78 MB. Nothing in the product reads them. Keep them tracked — history stays intact — just stop shipping them. |
| **OD-36** | **The ceiling: nameplate or arithmetic?** "8B class" (as built — admits `granite4.2:8b` at 8.8B and `qwen3:8b` at 8.2B) or strict 8.0e9 (refuses four of five sealed SOVEREIGN roles) | SOVEREIGN starting at all; every selector | **Ratify the nameplate class**, and say so in the log so it stops being a builder's interpretation. Your intent was a small-model proving run, not an arithmetic bound — and a strict reading excludes the very model you named. The builder left a test that fails if anyone tightens it silently, so the question comes back to you either way; this just answers it. |
| **OD-37** | **Who neutralises N-40, and how?** The populated `live_operation.json` under `evidence/cpm1/before-b/` | The truth of "a fresh clone stays DENIED-by-absence" | **Subsumed by OD-35 if you cut `evidence/`.** If you keep it, this needs its own ruling — it is S-18 territory and no seat may touch it without you. |
| **OD-38** | **LV-3 — which Ollama store is authoritative?** `OLLAMA_MODELS` at user scope still points at the empty `D:\SOVEREIGN\models\vault`; your real 60-model library is at `C:\Users\Sslaw\.ollama\models` | Every local model, on every fresh boot | **Point it at the real library, at user scope.** Until you do, a healthy daemon serves zero models. One line, and it is the cheapest item on this list. |
| **OD-5 / OD-31** | Frontier spend authority | OP-2, OP-7, the frontier half of agnosticism | **Standing DENY.** Correct for now. Nothing below assumes it moves. |
| **OD-32** | H-10 quota guard scope | the frontier Conductor path only | Deferred — local Conductor no longer depends on it. |
| OD-20 · OD-23 · OD-28 · OD-29 · OD-30 | carried from V5, unchanged | reviewer lane | As recorded in V5. |

**Start with OD-38 and OD-35.** The first takes one minute and makes the system
usable; the second is the largest single improvement available to this product
and costs one file.

---

# 2 · BUILDER BATCH — real defects, scoped, ready to send

| id | defect | sev | closes when |
|---|---|---|---|
| **P-1** | **SOVEREIGN ships two contradicting role tables.** `PACKAGE_MANIFEST.txt` still declares `qwen3:14b` / `qwen3:32b` / `qwen2.5:14b-instruct` under `CURRENT DEFAULT MODEL-ROLE ASSIGNMENTS` while `SYSTEM_MANIFEST.json` is all-8B. Commit `f0fe9dd` claimed "every model assignment". S-4. (N-42) | **HIGH** | Both files agree, and `docs/DISCOVERY.md:29` no longer records a stale table as FACT |
| **P-2** | **`check_model_consistency` cannot see the file that contradicts it.** It never reads `PACKAGE_MANIFEST.txt`. S-15 class. (N-43) | **HIGH** | The gate reads it, and a test proves the gate **fails** when the two tables disagree |
| **P-3** | **`jsonschema` is a runtime dependency declared in a dev-only file, unpinned.** Imported by ≥8 SOW runtime modules; SOW's only dep file is `requirements-dev.txt`. (E-1, restated) | **HIGH** | `modules/sow/requirements.txt` exists with `jsonschema` pinned exactly; a test asserts every non-stdlib runtime import is declared |
| **P-4** | **No single suite command.** `modules/sow/conftest.py:26` calls `relative_to(ROOT)` and dies with INTERNALERROR whenever one pytest invocation spans inside and outside `modules/sow`. | **HIGH** | A documented command runs the whole repository's tests and exits 0 |
| **P-5** | **7 more N-16-class runtime writes** into the tracked tree, incl. `debate.json` persisting into its own tracked `config.json`. (N-38, parked from LOCAL-01) | MED | All 7 write under `.runtime/`; the existing gitignore test covers each |
| **P-6** | **8 tracked files begin `FF FE`** (UTF-16 BOM), identical at the parent seal. (N-39, parked) | MED | Re-encoded UTF-8 no-BOM, or each documented as deliberately UTF-16 with a test pinning it |
| **P-7** | **`package_boundary_gate` walks gitignored bytes.** Running the suite creates `__pycache__` and turns the gate red. (N-44) | MED | The gate honours gitignore, or documents "clean tree required" and the runner enforces it |
| **P-8** | **The release producer has never been run.** No `release-build/` exists; F-7's 8-item cut plan is unexercised. (N-45) | MED | One real cut, with sidecars and `release-build-manifest.json` agreeing |
| **P-9** | **llamacpp declares `runnable` pointing at a path that exists nowhere.** The schema has only `runnable` / `not_started` — no way to say *present but not installed*. S-19. | MED | A third state class, or a pre-launch existence check that reports honestly instead of failing at spawn |
| **P-10** | **Evidence supporting the seal lives outside it.** `evidence/live-census-20260830/`, `docs/SOVEREIGN_LIVE_*` untracked and produced pre-LOCAL-01. (N-46) | LOW | Either committed with the seal they describe, or removed and not cited |
| **P-11** | **`install.ps1` writes `source_modules_root`** — the developer's path — into every installed config. | LOW | Dropped, or documented as deliberate provenance |
| **P-12** | **SOVEREIGN has 74 Python files and zero Python tests.** Its only tests are five TSX UI tests. The orchestration engine, model-role machinery, `system_manifest.py`, `publication_gate.py` and the `/v1/health` server are all untested. | **HIGH** | A Python test package exists for the engine, covering at minimum manifest loading and validation, role resolution against the ceiling, and the health endpoint's contract |

**P-1, P-2 and P-12 are one batch** — all three are SOVEREIGN's model roles being
asserted by documents and gates that nothing tests. **P-3 and P-4 are another** —
both say "this product cannot be installed or tested by a stranger," which is the
whole enterprise question.

---

# 3 · NOT DEFECTS — do not send these to a builder

| id | what it looks like | what it is |
|---|---|---|
| **E-2** | "Distillery has no lock" | Distillery imports **zero** third-party packages. Empty is the accurate declaration. **Close it.** |
| **E-1 (Electron half)** | `^` ranges in `package.json` | `install.ps1` uses **`npm ci`** against a complete v3 lock with integrity hashes for all 19 packages. Cosmetic. |
| **N-24 / N-26** | Frontier providers all DENIED; frontier Conductor dark | Your `live_operation.json` interlock, working as designed. |
| **S-16 in configs** | `Sslaw` in three adapter/config files | All three are rewritten at install and proven by `test_rebase_adapters.py`. |
| my 6 test "failures" | `tools/release` red on this seat | Windows path separators and a 3.12-only f-string. Environment, not product. |

---

# 4 · REVIEWER LANE — has never moved, all programme

**The ledger, measured:** 29 gate rows — **9 PASS** (all historical, gates 0–6),
**19 CANDIDATE with `evaluated_by: null`**, **1 STOP** (Gate 5). Gates 9a/9b are
absent. Candidates are `7a 7b 8a–8j 9c 9d 9e 9f 9g 9h 9j`.

Not one candidate gate has ever been evaluated. Four packages are ready:
CONVERGE-01 `93f1a15a…`, CLOSEOUT-01 `1efe2fdd…`, FIXUP-01, LOCAL-01
`1821ee77…`. Plus OD-23 and OD-28.

**This is the longest pole in the project by a wide margin.** Everything else on
this list is weeks; this is the item that decides whether "ratified" is a word
this system can use. It needs a seat that is not the builder and not the
validator, and it needs a schedule.

---

# 5 · ENTERPRISE-PRODUCTION PROGRAMME — the four real gaps

Not defects. Programmes. Each needs scope and cost before code.

| id | gap | why it decides "enterprise" |
|---|---|---|
| **EP-1** | **No clean-room install, ever** (E-3, all programme) | Every install proof is a same-host proxy on a machine that already has Python, Node and Ollama. Until a stranger's machine runs this, "installable" is untested. **Needs a VM and a day.** |
| **EP-2** | **No CI, and no single suite command** | 2,999 tests nobody runs automatically. Blocked by P-4; needs **Windows runners** because the release tooling is Windows-only by construction. |
| **EP-3** | **The archive ships the workshop** — 76% of bytes are development history | Gated on **OD-35**. Resolves N-40 and most of S-16 as a side effect. |
| **EP-4** | **Offline / air-gapped install** | `npm ci` reaches `registry.npmjs.org`. Enterprise installs frequently cannot. Needs vendored node_modules or an internal registry. |

Carried from V5, unchanged in scope: **OP-7** model agnosticism across all
surfaces (local half now largely delivered; frontier half gated by OD-5),
**OP-2** frontier providers, **OP-6** Distillery UI, canonical license text,
H-6 composed-system proof, MT-09 signature.

---

# 6 · LIVE VERIFICATION — only you can take these

None of §2 can be called closed on tests alone (S-17). These are the screen.

| id | what to check | why it is not provable from bytes |
|---|---|---|
| **LV-3** | Point `OLLAMA_MODELS` at the real library, restart, confirm the daemon serves 60 models | Host environment. **Do this first — everything else depends on it.** |
| **LV-1** | SOW picker lists local models with over-ceiling ones greyed and a reason shown | S-19 is a claim about what a person sees |
| **LV-2** | Conductor accepts typing, offers the local library, spawns nothing until a model is chosen and a message sent (option C) | Your own ruling; only observable live |
| **LV-4** | Debate reaches models and runs a round on `qwen3:8b` | Was blocked on the daemon |
| **LV-5** | SOVEREIGN starts and passes its startup test | N-27's fix is unproven on screen |
| **LV-6** | Token Center embed renders (N-21) | Was never reproduced by any gate |
| **LV-7** | Clean stop, orphan sweep, porcelain unchanged | N-16's live proof, still the highest-value T-2 step |
| **LV-8** | Eight concurrent terminals, and the ninth | Capacity claim, never exercised |

---

# THE SHORT VERSION

**The release blocker is gone.** `provenance_cross_hash_check` is green for the
first time since CONVERGE-01, the model ceiling exists and enforces, and the four
defects you reported by voice are fixed in bytes.

**Four rulings are yours and everything queues behind two of them:** point
`OLLAMA_MODELS` at your real library (one minute, and nothing works without it),
and decide whether the development record ships (one file, and it removes 76% of
the archive, the stray authorization file, and every trace of your username).

**Twelve defects are ready for a builder.** Two of them say the same thing in
different words — a stranger cannot install this, and nobody can run its tests in
one command. A third is quieter and worse: **SOVEREIGN, the module you watched
fail, has no Python tests at all.**

**Nineteen gates have never been reviewed by anyone.** That, not code, is what
stands between this system and the word "ratified" — and no amount of builder
work will move it.

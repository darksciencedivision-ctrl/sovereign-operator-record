# VALIDATOR AUDIT — LOCAL-01 SEAL

Seat: validator (claude, cowork)
Subject seal: `0b6e3fe7346e7eaa581ce7fc02cf36f46e58bc75`
Parent seal: `8d9f5d2415599eeb0b71dc85a25a36469b41641e` (FIXUP-01)
Directive of record: `CLAUDE-LOCAL-01-DIRECTIVE-R2-20260830.md`
  sha256 `b94b47f2267cc36c9620fd9bb6c27b417608e3e720550d78727873b50e0105f9`
Method: re-derived from bytes. Repository measurements taken in
`release-worktree`; distribution measurements taken from a clean
`git archive 0b6e3fe7 | tar -x` export (3508 files), with
`PYTHONDONTWRITEBYTECODE=1` so the measurement leaves no residue.
Date: 2026-08-30

This is a VALIDATOR record. It is not a review. It does not write
`status: PASS`, it does not promote, and it does not close a gate.

---

## 1. VERIFIED

| id | claim | verdict |
|---|---|---|
| V-1 | HEAD is `0b6e3fe7…`; nine commits `8d9f5d24..HEAD`, all authored `builder claude-code opus-5` | CONFIRMED |
| V-2 | `provenance_cross_hash_check` PASS — all five modules OK, from a clean export of the seal | CONFIRMED — **OD-27 closed, first green since CONVERGE-01** |
| V-3 | `check_governance_bom`, `check_model_consistency`, `innerhtml_sink_audit`, `package_boundary_gate`, `release_manifest_check` all exit 0 at the seal | CONFIRMED (see R-1 for scope, N-43/N-44 for what they do not cover) |
| V-4 | S-18 holds on the production path: `modules/sow/config/live_operation.json` absent → DENIED-by-absence | CONFIRMED (but see **N-40**) |
| V-5 | S-10 holds: no `ollama pull` / `api/pull` introduced in the nine commits | CONFIRMED |
| V-6 | The ceiling refuses `phi4:14b` (14.7B), `deepseek-r1:70b` (70.6B), `ornith:9b` (9.0B), embedding models and cloud pointers, each with a stated reason — pinned by `test_local_model_ceiling.py` | CONFIRMED. The two named LOCAL-01 defects (phi4:14b **authorized to spawn**; deepseek-r1:70b **offered then refused after the click**) are closed at the classifier |
| V-7 | Archive delta 3503 → 3508 files | CONFIRMED |
| V-8 | F-6/OD-34: the adapter now declares `${root}/.runtime/receipts/SHELL-LIVE-READY.json`, pinned by `test_runtime_writes_are_gitignored.py` | CONFIRMED. The untracked `modules/sow/docs/evidence/receipts/SHELL-LIVE-READY.json` in the porcelain is **residue from a pre-F-6 run**, not evidence F-6 is incomplete. Open item closed |

## 2. RETRACTED — MY OWN MEASUREMENT ERROR

**R-1.** I first reported `release_manifest_check` FAIL with 12 problems
against the archive export, and traced the regression to CLOSEOUT-01
seal `50b47326` (2 problems at `32f191e`, 12 from `50b47326` onward).

**That finding is withdrawn. It was an artifact of my method.**

`.gitattributes:37` sets `*.pre-CONVERGE01 export-ignore`, and its own
header states the intent verbatim: *"export-ignore removes a path from
`git archive` ONLY. Every path below stays tracked."* The seven
`.pre-CONVERGE01` files are tracked at HEAD (`git ls-tree` confirms) and
the checker is repository-scoped by construction ("superseded provenance
path exists — legacy files preserved untouched"). No producer and no
installer runs it against a cut archive. In the repository it is a true
PASS.

This is the same class of error as my earlier `package_boundary_gate`
FAIL: I invoked a gate outside the scope it was written for and read the
result as a defect. Recorded so the pattern is visible, not buried.

## 3. FINDINGS

### N-40 — HIGH — a populated frontier authorization file is tracked and shipped inside the seal
**S-18 territory. Operator's alone. No seat may act on this without a ruling.**

`evidence/cpm1/before-b/modules/sow/config/live_operation.json` is tracked
at HEAD and present in `git archive`. It contains:

* `live_operation_authorized: true`
* `scope.providers: ["openai_codex_cli", "claude_code", "grok_build"]`
* `conductor: openai_codex_cli / gpt-5.6-sol`

Its sibling `evidence/cpm1/before-b/modules/sow/control_plane/profiles/live_authorization.py:103`
computes `_DEFAULT_PATH = Path(__file__).resolve().parents[2] / "config" /
"live_operation.json"` — relative to its own file. Measured against the
archive export, that resolver resolves to the populated file and reads
`authorized: true`. `registry.py:19` (the Conductor registry) resolves the
same way.

Scope, stated precisely and without inflation:

* **The production interlock is intact.** `modules/sow/control_plane/profiles/live_authorization.py`
  resolves to `modules/sow/config/live_operation.json`, which is absent.
  DENIED-by-absence holds. I found no path by which the production tree
  loads the evidence copy.
* **The stated guarantee is nevertheless false at repository scope.** The
  sentence written in the file itself, and repeated across governance
  documents — *"This file is gitignored (never committed): a fresh clone
  stays DENIED-by-absence"* — is not true of this repository. A fresh
  clone of seal `0b6e3fe7` contains a complete, self-consistent,
  frontier-authorized SOW tree, interlock and authorization file together.
* It also carries `D:\Product Software\Production Workspace\modules\sow`
  into distributed bytes — the **S-16** class, and the N-18 population.

**Attribution:** inherited. Introduced at `cf50cde` ("P0-2 initial worktree
from SOVEREIGN_SYSTEM_BASELINE_20260827.zip"). **Not introduced by
LOCAL-01**, and not a defect in the builder's work. It has been in every
seal this programme has produced, and I did not catch it in three prior
audits.

**Recommendation, not a decision:** the operator rules on whether the
populated evidence copy is neutralised, and by whom. It is the exact
interlock he reserved to himself.

### N-41 — MEDIUM — the ceiling was widened from 8B to 9.0B by builder interpretation, and the widening is load-bearing

`modules/sow/adapters/local/model_ceiling.py` sets
`_TRUE_PARAM_LIMIT = 9.0e9` and admits "the 8B nameplate class."

ENTRY 017, the operator's words: *"We do not wanna use anything above
eight billion parameters in our local library."*

The tension the builder identified is real and I do not dispute it:
`qwen3:8b` — the model the operator named to prove the system with —
actually carries 8.2B parameters, so a strict 8.0e9 bound excludes it.
The builder documented the choice openly in the module docstring rather
than concealing it. That is honest work.

**But one claim in that docstring is false at this seal.** It states the
two readings *"select an identical set … so this choice changes no
behaviour today."* They do not. `SYSTEM_MANIFEST.json`, rewritten by this
same run (`f0fe9dd`), now assigns:

| role | model | true parameters | strict 8.0e9 reading |
|---|---|---|---|
| PRIMARY_REASONER | `qwen3:8b` | 8.2B | REFUSED |
| ADVERSARIAL_CHALLENGER | `granite4.2:8b` | 8.8B | REFUSED |
| CRITIC | `dolphin3:8b` | 8.0B | admitted |
| SYNTHESIZER | `deepseek-r1:8b` | 8.2B | REFUSED |

Under the operator's literal words, **four of five sealed role
assignments are refused and SOVEREIGN does not start.** The widening is
not cosmetic; it is what makes the sealed configuration runnable.

This is a builder decision taken on a boundary the operator ruled, and
resolved in the permissive direction. S-9 says park, don't improvise.
It needs an operator ruling — **nameplate class (as built) or strict
arithmetic** — and it is a one-line change either way.

### N-42 — MEDIUM — SOVEREIGN ships two contradicting role tables; commit `f0fe9dd`'s claim is false as stated

`f0fe9dd` is subject-lined *"bring **every** model assignment under the
operator's 8B ceiling."* It changed `SYSTEM_MANIFEST.json` and left
`modules/sovereign/PACKAGE_MANIFEST.txt` declaring, under its own header
`CURRENT DEFAULT MODEL-ROLE ASSIGNMENTS`:

```
PRIMARY_REASONER=qwen3:14b
ADVERSARIAL_CHALLENGER=qwen3:32b
CRITIC=qwen3:8b
SYNTHESIZER=qwen2.5:14b-instruct
```

Three of those are models the ceiling refuses. **S-4 violation** —
coupled values did not move together.

Correctly scoped: this is **not** a runtime misroute. No code parses
`PACKAGE_MANIFEST.txt` for roles; `system_manifest.py` reads
`SYSTEM_MANIFEST.json` only. But the file is not inert — it is
enumerated with a sha256 at `RELEASE-MANIFEST.json:98`, its hash is
`source_content_manifest_sha256` in `modules/sovereign/INSTALL-PROVENANCE.json`,
and `docs/DISCOVERY.md:29` records its table as `FACT`. The seal ships a
document that states, as current fact, defaults that contradict the
operator's ruling — and a reviewer reading it would be misled.

### N-43 — MEDIUM — `check_model_consistency` is blind to the one file that contradicts it

The gate's docstring declares `SYSTEM_MANIFEST.json` the single source of
truth and mechanically verifies README labels, `synthesis/model_hierarchy.json`
and `cross_channel.critique.model` against it. It never reads
`PACKAGE_MANIFEST.txt`. So it exits 0 at the seal while a shipped,
hash-enumerated file states different roles.

This is the **S-15 class in a second location**: a checker whose name and
docstring imply coverage it does not have. S-15 was written for manifests
carrying unverified `path`+`sha256` pairs; the same failure mode appears
here as a consistency gate that does not reach a coupled document.

### N-44 — LOW — `package_boundary_gate` walks the working tree, gitignored bytes included

Running the test suite creates `__pycache__/*.pyc`, which the gate then
reports as `runtime-session-state` violations. It can only go green on a
clean tree or a clean export. Not a seal defect — a reproducibility
hazard for any reviewer who runs tests before gates, and the likely
reason a green claim and a red re-run can both be honest.

Separately, the gate flagged the untracked
`docs/SOVEREIGN_LIVE_ANSWERS_20260830.json` on an `aws-access-key-id`
pattern. It is untracked and not in the seal, so it does not ship — but
the operator should know the pattern is present on his disk.

### N-45 — LOW — F-7's rewritten producer is unexercised

No `release-build/` directory and no `release-build-manifest.json` exists
at the custody root. The 8-item cut plan, the "strangers in the output
directory are a hard error" rule, and sidecar/manifest agreement are
therefore **unverified — there is no cut to inspect.** The builder's
claims about F-7 are not refuted; they are simply unevidenced. Stated as
unmeasured rather than omitted.

### N-46 — LOW — evidence supporting the sealed state lives outside the seal

`evidence/live-census-20260830/` and `docs/SOVEREIGN_LIVE_*` are
untracked. They were produced by the grok-opencode seat at HEAD
`8d9f5d24` — **before** LOCAL-01 — and they are cited in reasoning about
the sealed state. A recipient of the seal cannot audit them. This is the
mirror of the recurring "a record must not live inside the set it
describes": here a record that supports the set is not in it.

## 4. WHAT THIS AUDIT DID NOT MEASURE

Named so they are not mistaken for cleared:

* **No live run.** The daemon was not reachable from this seat, so S-19
  and S-20 were verified at the classifier and the selector in bytes, not
  on screen. The operator's own defects — Conductor typing, the picker
  offering local models, Debate reaching models, SOVEREIGN start — remain
  **operator-observable only**. LV-1…LV-8 stand.
* **LV-3 unresolved.** `OLLAMA_MODELS` at user scope still points at the
  empty `D:\SOVEREIGN\models\vault`; the builder's fix was session-scoped.
  Until the operator rules, a fresh boot serves zero models from a healthy
  daemon.
* S-15 tamper resistance was not re-exercised at this seal (it was proved
  at CLOSEOUT-01 across four tamper classes).
* The N-39 `FF FE` census was not re-run.
* The reviewer lane is untouched: 19 CANDIDATE gates, Gate-5/5b and OD-28
  have never moved, and four review packages remain unworked.
* E-1/E-2 (SOW unpinned ranges, Distillery no lock) are untouched, as they
  have been all programme.
* N-30…N-39 remain parked.

## 5. VALIDATOR POSITION

LOCAL-01 did the work it was directed to do. `provenance_cross_hash_check`
is green for the first time since CONVERGE-01 — that is the release
blocker, and it is measured, not claimed. The ceiling exists as a single
authority, the two named picker defects are closed at the classifier, S-18
held on the production path, and no model was pulled.

Three things stand between this seal and a reviewer:

1. **N-40** is the operator's decision and nobody else's.
2. **N-41** is a ruling he has not yet been asked for, and the sealed
   configuration depends on the answer.
3. **N-42/N-43** are a contradiction that ships and a gate that cannot see
   it.

None of these is a reason to unwind the seal. All three are reasons the
seal is not yet reviewable.

I hold no authority to promote, to write `status: PASS`, or to act on
N-40. Those are the operator's.

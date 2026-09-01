# EPC-01 — Enterprise Production Completion Loop: C.4 completion report

**Directive:** `EPC-01-DIRECTIVE-20260830.md`
sha256 `31d96f7da5c1011bfef18fec6561fbf3a3c677c8de0f9b8eaa5ab1167372abd2`
**Punch list:** `PUNCH-LIST-V7-20260830.md`
sha256 `66f8c2ff95e466460462e0c05d0271f3f8d38307a41a714cdb75e0c1682a9c02`
**Authorization:** ENTRY 025 (T-1), ENTRY 026 (standing instruction to run to completion)
**Repository:** `D:\producttion software 2\release-worktree`, branch `main`
**Seal at start:** `6733780`

---

## Disposition of the 44

**44 accepted. 0 parked.**

P1-2 and the P3-2 residue were parked pending operator decisions; both decisions were given
and executed on 2026-08-30 (ENTRY 027, ENTRY 028). One item of the residue remains open by
the operator's own rule rather than by the builder's choice:

| Item | Decision | Outcome |
|---|---|---|
| **P1-2** frozen schema set | Delete the thirteenth schema (ENTRY 027); re-baseline the freeze manifest (ENTRY 028) | **CLOSED.** 79 tests pass, freeze check clean. The manifest was never signed, so nothing was overwritten; regeneration reset the signature to PENDING for the operator. |
| **P3-2 residue** `docs/DECISIONS.md` | Waiver granted (ENTRY 027) | **CLOSED.** Machine paths replaced with a neutral form. |
| **P3-2 residue** `BUILD-DIRECTIVE-SWS-UI-001.md` | Waiver granted (ENTRY 027) | **NOT APPLIED.** The contract states it is amended "only by a versioned successor the operator issues; never by chat". The operator has the authority and intends the change; a live-session waiver is the one route the contract excludes by name. Three ways to close it are in ENTRY 027. |

Withheld at T-1 and therefore never attempted: provisioning a VM or container, launching
GUI applications unattended, rewriting git history (E-2/E-5). V-2 and V-3 remain parked
under E-5 for that reason.

---

## What changed, by theme

### The archive stopped describing the build machine

The shipped configuration named the operator: `shell/config/install.json` carried the build
tree and the build operator's `python.exe` under `C:\Users\<name>`; all five module adapters
carried an absolute `root` into that tree; `shell/src/distillery.py` hardcoded two of the
operator's directories **in product code**.

The fix was not to scrub the strings but to remove the need for them. `${install_root}` is
computed from where the shell actually is, and `${python312}` is the interpreter the shell is
already running under. Both are derived, so they cannot go stale — the previous arrangement
rewrote the paths at install time and was correct only until the installation was moved.
All six adapters were verified to compile to exactly the paths the hardcoded versions
produced, so H-5 containment is unchanged.

What could not be fixed was disclosed rather than carried quietly, and the remainder is
**pinned** by a test that fails if an undisclosed file acquires an identifier, if a repaired
file regresses, or if a disclosed entry stops needing disclosure.

### The SBOM stopped describing the build machine too

The old SBOM was produced by walking the build host's virtual environments. It listed
**62 packages the product does not contain** — the transitive tree of `@electron/get@2.0.3`
while the lock pins `5.1.0`. Over-reporting is not the safe direction of error: a scanner
fed those names raises findings against software that is not there.

It is now generated from the six lock files, records each lock's SHA-256 so provenance is
checkable rather than asserted, and dropped from 477 entries (173 of them individual `.venv`
files) to 240 real components with zero unresolved licences.

### Diagnostic output survives the process that produced it

Module output went into an in-memory ring; close the console and the record was gone.
Persistence was added **inside the buffer that already redacts**, after `redact()` runs. The
obvious implementation — tapping the supervisor's raw output pipe — would have been simpler
and would have written unredacted credentials to disk.

### There is a verification lane

Everything that verifies this release now lives in one script, and the CI lane invokes that
script rather than repeating its steps, so the lane cannot drift from what a developer runs.

---

## Verification

| Check | Result |
|---|---|
| Whole-product `pytest` (repository root) | **3,822 passed, 8 failed**, 4 skipped, 461 subtests passed |
| Node suites | 1,109 passed (SOW desktop) + 26 passed (SOVEREIGN UI) |
| TypeScript typecheck | PASS |
| Release gates | 7/7 PASS |
| Boundary gate (distribution) | PASS |
| CI script, executed locally | 10/13 stages PASS; exit code **1** on the failed stage |

### The eight failures, attributed rather than waved past

**Seven are P1-2, and they pre-date this loop.** Reproduced at the seal `6733780` in an
isolated clone with identical counts (1 + 5 + 1). The drift they report is inside
`modules/sow`'s own tree; neither it nor `tools/manifest` was touched by this loop. They stay
red until the operator supplies the `OP-nn` ruling id P1-2 needs.

**One is cross-run interference in `modules/debate`'s hostile e2e suite, and it is OPEN.**
It passes 9/9 alone and 185/185 with its whole module; it fails only in the whole-product
run. Four hypotheses were tested and all four are ruled out — the state root, the repo-root
`conftest.py` PYTHONPATH, a port collision with `test_smoke` (18700 vs 18701), and a shared
state-root config (none exists). Recorded as unexplained rather than guessed at. It is not
one of the 44.

**Two further failures were mine, and are fixed.** `RELEASE-MANIFEST.json` recorded a
whole-file hash for `shell/BUILD-MANIFEST.txt`, a file that carries a `# utc:` stamp and is
regenerated by every suite run, while the checker hashes it stamp-insensitively. The recorded
value could never match, and the manifest was being re-synced after every suite run — a
symptom treated as the suite's fault when it was the manifest's. Measured across a
regeneration: the file differs in that one line and nothing else, and its content hash is
stable at `00b4705c`. The manifest now records that, verified to survive a full regeneration.

### The lane is RED on first run

That is the correct report of this tree: 3,822 passing, 8 failing, seven of them blocked on
an operator decision. A lane tuned green by excluding its own failures would be worth nothing.

### Mutation proving

Every guard added in this loop was proven by reintroducing the defect it claims to catch and
confirming red: the SBOM guards (4 mutations), the rebase-variable guards (2), and the
developer-identifier guard (3, in an isolated clone — committing mutations in the real
worktree is how a good commit was destroyed earlier in this programme).
---

## What this report does not claim

- **No gate is submitted for reviewer evaluation.** Nothing here is a PASS.
- **The CI lane has never run on a hosted runner.** It has been executed locally, which is
  what makes it a lane rather than a declaration, and that distinction is written into
  `docs/RELEASE-ASSURANCE.md` rather than smoothed over.
- **The clean-room install was not executed.** It is defined in the CI script and is opt-in;
  running it requires package installation, which the builder envelope forbids. The script
  reports the skip under `SKIPPED-WITH-RECORD` rather than passing silently.
- **No Python vulnerability scanner runs.** The SBOM is now valid scanner input; what is
  missing is the scanner. `npm audit` covers the Node side at a threshold of zero.
- **Nineteen of twenty-nine gate evaluations have still never been reviewed by anyone.**
- **The licence is drafted and marked pending counsel sign-off.** It has not been reviewed.

---

BUILDER CLAIM: No gate is submitted for reviewer evaluation. No PASS status is asserted by the builder.

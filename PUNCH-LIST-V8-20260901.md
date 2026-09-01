# Punch list V8 — 2026-09-01

**Seal:** `bf47000` · **Published:** `sovereign-workspace-release` (112 commits, private),
`sovereign-operator-record` (ENTRY 001–038, private) · **Authority:** ENTRY 038.

Ordered by dependency and value, not by size. Owner column is load-bearing: three of these
cannot be done by the builder at all, and they are the three that decide whether this ships.

---

## The one number that matters

**19 of 29 gates carry `evaluated_by: null`.** Never failed — never looked at. 4,030 passing
tests do not move it, and nothing in EPC-01/02/03 moved it. Until it moves, the punch list's
NO-GO stands and no amount of further building changes that. Everything below is ordered
around getting to O-5 with something worth evaluating.

---

## Operator-only

| # | Item | Done when | Cost |
|---|---|---|---|
| **O-1** | Open ONE worker pane in the running shell | `node_events.jsonl` gains a durable row; `attest_spawned` fires; presence names the pane | 30 seconds |
| **O-2** | Clean-room install from the archive into an empty destination | `install_sow.py` + both venvs complete; shell starts; a module reaches readiness | one session |
| **O-3** | Write ONE entry: the context number and the model slate | Both recorded; F-7 and B-14 close | 5 minutes |
| **O-4** | Amend `CLAUDE.md` §9 | It no longer says "no remotes anywhere", which stopped being true on 2026-09-01 | 2 minutes |
| **O-5** | Commission reviewer evaluation of the 19 CANDIDATE gates | `evaluated_by` is non-null on every one | the real work |

**O-1 first, and it is not ceremony.** Layers 3–6 are measured in parts — local conductor
decomposes (10.7 s, 3 tasks, model verified), delegation reads an answer back from a live model
(12.1 s, echo excluded), a spawn request reaches Approvals and opens nothing. None of it has been
measured IN THE APP. G26 — the in-app supervised spawn that died on launch — is the wound Layers
3–6 were built to close, and one pane is the whole test. Panes opened before `af79ae7` cannot
retro-register, so this needs a NEW pane.

**O-3 is two facts.** Context: `SYSTEM_MANIFEST` declares `CONTEXT_WINDOW = 131072`; the DEEP path
requests `20480` from measured VRAM. 6× apart in one product, and the measurement already decided
it — 16384 spilled 1,641 MiB on this card. Either change the manifest or rename the field so it
cannot be read as `num_ctx`. Slate: this tree runs `qwen3:8b / granite4.2:8b / dolphin3:8b /
deepseek-r1:8b`, a third assignment against both the package defaults and the Aug-27 baseline,
with no record. "Fits the card" is a reason. Silence is not.

---

## Builder — ready, cheap, no decisions left in them

| # | Item | Done when |
|---|---|---|
| **B-1** | F-3: generic absolute-path / UNC / user-home pattern in the identifier guard | It catches a build-host path from ANY machine, and does not flag the legitimate `ryguy-pixel/Sovereign-Distillery` provenance in `README.md` / `DECISIONS.md` |
| **B-2** | F-4 + F-5 deletes: 13 tracked backup files, `control_plane/canonical_registry.py` | Gone; suites green; reference check already done |

**B-1 exists because of F-2.** The guard's five patterns are scoped to this operator's own disk —
`Sslaw`, `Product Software`, `Sov 1`, `C:\Users\…`, `producttion software`. It caught my build-host
path because my path was one of the five it knows. A path from any other machine walks through,
which is exactly how 79 files escaped into the distribution.

**B-2 is deletes only.** `canonical_registry.py` is 169 lines, 53 models × 18 fields, referenced by
nothing in the tree and carrying no `__main__`. A model registry nobody reads will be treated as
authority the first time someone searches "models".

---

## Builder — needs an envelope before it starts

| # | Item | Why it is not on the cheap list |
|---|---|---|
| **B-3** | Separate product tests from build-record validators | It is a programme, not a task |

**The problem, measured.** Extract the shipped archive and run its own suite: **21 failed,
10 errors, before touching anything.** Causes are structural, not defects —
`test_developer_identifiers_are_bounded`, `test_package_boundary_gate`,
`test_release_manifest_check` shell out to `git` and an extraction has no `.git`;
`test_freeze_manifest_check` needs `.claude/agents/*` which is `export-ignore`d by design;
`test_archive_ships_only_operator_docs` checks the archive from inside the archive.

**Why it matters more than it looks.** That is the first ten minutes of every recipient's
experience. Nothing in the archive tells them those tests require a git checkout, so the
reasonable conclusion is that the release is broken. It isn't. But "trust me, those 31 don't
count" is not something an enterprise recipient accepts, and it is the gap between a system that
IS sound and one that can DEMONSTRATE it to someone who did not build it.

**The minimum viable version** is a skip-marker on the repo-validators plus a paragraph in
`docs/INSTALL.md` naming which tests need a checkout and why. That is an afternoon, not a
programme, and it removes the false signal without restructuring anything.

---

## Closed tonight, for the record

- Review filed (`caa3514`), F-1 tests with 4/4 mutations caught (`057204b`), F-2 gate rule and
  quarantine lane removed with four synthetic fixtures properly declared (`c903ae4`), Debate
  erratum correcting a false assurance line (`bf47000`).
- Two release cuts, 8 artifacts each, 0 hash mismatches.
- Published to GitHub under ENTRY 038.

## Not doing, decided

- **The umbrella `workspace/` refresh.** Its description now says SUPERSEDED and the release repo
  supersedes it in substance. Refreshing would import 289 personal-path files into a repo that is
  currently clean of them, to make an already-replaced snapshot marginally less stale.
- **Trimming the distribution.** Measured: 6.8 MB of removable build record against 1,180 MB of
  rebuildable dependencies, and the removable part is hash-sealed by the suite. The fat was never
  in the archive.

---

BUILDER CLAIM: No gate is submitted for reviewer evaluation. No PASS status is asserted by the builder.

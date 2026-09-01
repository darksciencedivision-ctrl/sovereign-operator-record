# VALIDATOR AUDIT — CLOSEOUT-01 SEAL `50b47326`
## Seat: validator (claude) · 2026-08-29 · authority: SWS-REM-DIR-20260828 R2 + Annex A
## Subject: the CLOSEOUT-01 run by builder seat claude-code (opus-5), sealed at
## `50b47326f91c5abab8b6d5250d59b1dba96a4d46`

Evidence audit against the tree and the frozen bytes. No PASS asserted; nothing promoted.

---

## 1 · VERIFIED TRUE BY INDEPENDENT RE-DERIVATION

| # | claim | measurement |
|---|---|---|
| T-1 | HEAD `50b47326…`, porcelain clean | `git rev-parse`; `timeout 150 git status --porcelain` exit 0, empty |
| T-2 | Seven commits `32f191e..HEAD`, all authored `builder claude-code opus-5` | `git log --format='%h %an'` — **A-5 closed**, attribution now correct |
| T-3 | **A-1 closed.** `RELEASE-MANIFEST.json` carries 63 hash-bearing entries, all 63 re-hash correct, **zero** under `release-artifacts/`; `release_archives_gitignored` gone; `release_archive_hash_authority` points at the build manifest | full-tree re-hash of every `path`+`sha256` object |
| T-4 | **S-15 genuinely enforced**, not cosmetic. I tampered four ways against the hardened checker: invented section with a WRONG hash → caught; invented section with the CORRECT hash → caught; hash smuggled into a new group *inside* an already-handled section → caught; corrupted `identity_documents` hash → caught. Baseline 0 problems | direct calls to `rmc.check()` on deep copies |
| T-5 | **A-2 closed.** `git archive HEAD` contains 0 `spike_compositor` members | tar enumeration |
| T-6 | **A-3 closed.** 0 backup-suffixed members in the archive | tar enumeration |
| T-7 | Archive is 3497 files / 4195 members, matching the reported figure | tar enumeration |
| T-8 | All 8 artifacts + 8 sidecars mutually consistent: artifact hash == sidecar == build-manifest entry, byte counts match, sidecar's own hash recorded and correct | recomputed every value |
| T-9 | `release-build-manifest.json` is `sovereign.release-build.v2`, names seal `50b47326…`, `artifact_count` 8 | direct read |
| T-10 | **A-4 closed.** `VERSION.json` carries `seal_commit_record` and an explicit `source_commit_definition`; `source_commit` = `be8dcb0` (last tooling commit) | direct read |
| T-11 | `release_manifest_check` PASS (0 problems), exit 0 | direct run |
| T-12 | Review package sha256 `8e6987ca078a13ab8bc510c801d7404c1d1e1c2ff0b7896ce478c27e051b4825` matches its sidecar | recomputed |

**The circularity is structurally broken, not papered over.** Artifact hashes now live only in an
untracked file written after the seal commit exists. The same defect cannot recur by the same route.

---

## 2 · THE SEAT CORRECTED ME THREE TIMES. ALL THREE CORRECTIONS ARE RIGHT.

**C-1 · A-2's entry count was wrong.** I wrote "69 entries each." Measured at `32f191e`:
66 members total — 56 files, 10 directories — i.e. **28 files + 5 dirs per root**, exactly as the
seat reported. My number came from a substring match over a ZIP namelist that counted the composite
install archive's prefixed duplicates. The finding (it ships) was right; the figure was not.

**C-2 · A-1's blindness was wider than I reported.** I said 17 unchecked entries in one section.
Measured at `32f191e`: **25 of 80 hash-bearing entries across three sections** —
`release_archives_gitignored` (17), `identity_documents` (4), `ui_assets` (4). The seat's
observation is the sharper one: the latter eight hash *correctly* today, so no failure would ever
have surfaced them. I found the blindness where it happened to be producing wrong numbers and
stopped looking.

**C-3 · N-18's population was off by more than an order of magnitude.** I named 5 files. Measured
over files entering `git archive` at `32f191e`: **107 contain the developer username, 264 contain
`Product Software`**, overwhelmingly historical evidence that R2 §4/§8 forbids rewriting. That makes
**my X-4 pass-after criterion 5 unsatisfiable as written**. The seat reported it REFUTED and
satisfied the narrower criterion the directive's own steps describe. That is the correct handling of
a refuted finding under Phase W, and the criterion is my defect — the third of mine this program.

---

## 3 · TWO DIRECTIVE DEFECTS THE SEAT CAUGHT BEFORE THEY PRODUCED A FALSE "DONE"

**D-1 · `*.pre-add08` matches nothing.** The only such file is
`evidence/cpm1/baseline/host-hardware.pre-add08.json` — `pre-add08` is an **infix**, not a suffix.
My X-3 specified `export-ignore` on `*.pre-add08`. Applied literally it would have excluded 12 of 13
backups and reported the row DONE. Verified: the pattern matches zero paths.

**D-2 · llamacpp's `argv[0]` is not an interpreter.** Verified at `32f191e`:
`argv[0] = D:/Product Software/.../llama.cpp/current/llama-server.exe` — a native server binary.
My X-4 step 2 said to set `python_312` unconditionally "at the enumerated interpreter pointers
(`/launch/argv/0`)", and `/launch/argv/0` is in llamacpp's INVENTORY. A blanket rule would have
overwritten a server binary path with `python.exe`. The seat scoped it per-adapter via a new
`INTERPRETER_POINTERS` map. **The implementation is better than the specification it came from.**

This is what Phase W was for, and it earned its cost on the first run.

---

## 4 · MY MOST SERIOUS MISS: I CLEARED A SEAL WITH A RED RELEASE GATE

`provenance_cross_hash_check` **FAILS at `32f191e`** — the seal my previous audit assessed as
"materially sound." I ran it, hit `error: the following arguments are required: --registry`, and
never re-ran it correctly. I fixed the same class of invocation error for the package-boundary gate
and did not go back for this one. My §1 table silently omitted two gates rather than reporting them
unmeasured. **A gate I did not measure must be reported as unmeasured, not left out.**

The seat found it. Independently re-derived, its N-20 analysis is exactly right, in two layers:

**Layer 1 — the registry is stale.** Gate output at the seal: `debate`, `distillery`, `sow` UNKNOWN;
`sovereign` OK; `tokencenter` PENDING; exit 1. `module_source_registry.json` was last modified at
`a945d99`, before CONVERGE-01.

**Layer 2 — the records could not reproduce themselves.** `generate_module_provenance.py:105`
digests `git ls-files` for the module "excluding `INSTALL-PROVENANCE.json` itself," so a correct
record's `content_file_count` must equal tracked − 1. Measured:

| module | ref | tracked | record says | delta |
|---|---|---|---|---|
| debate | `a45fb98` | 55 | 53 | −2 … gate GREEN here |
| debate | `32f191e` | 56 | 54 | **−2, should be −1** |
| debate | `50b4732` | 56 | 55 | **−1 — correct** |
| distillery | `32f191e` | 207 | 205 | −2 |
| distillery | `50b4732` | 207 | 206 | −1 — correct |
| sow | `32f191e` | 958 | 956 | −2 |
| sow | `50b4732` | 958 | 957 | −1 — correct |

The extra excluded file is the `.pre-CONVERGE01` backup: at C6 generation time it was untracked, so
`git ls-files` did not see it; committing it made it tracked, so the record was stale the instant it
landed. **CONVERGE-01's own seal commit broke this gate**, and it is A-1's mechanism a third time —
*a record placed inside the set it describes*. Phase Z closed layer 2; layer 1 is open by design,
because refreshing the registry declares three modules' source identity.

`sovereign` is unaffected and still OK. It is the one module that received no `.pre-CONVERGE01`
backup. The mechanism is confirmed by its exception.

---

## 5 · WHAT I COULD NOT VERIFY

- **N-19 (`shell/BUILD-MANIFEST.txt` stale) — UNVERIFIED BY ME.** My parse of the manifest's line
  format resolved 0 of 67 paths, so my measurement is invalid and I report no result rather than a
  wrong one. The seat's claim (19 of 67 hashes wrong, 8 tracked files omitted, predating
  CONVERGE-01) stands unchallenged and unconfirmed. Reviewer to adjudicate.
- Not re-run by me: the SOW node suites, Token Center, the shell suite, and the C4 proxy install
  cycle. Those remain builder-measured. The proxy cycle's re-run is the load-bearing evidence for
  X-4 and deserves the reviewer's attention, not mine — I cannot execute PowerShell install tooling
  from this seat.

---

## 6 · OBSERVATION, NOT A FINDING

**OBS-2 · the optional-adapter skip signal may have gone quiet.** After X-4 neutralised llamacpp's
paths to `C:/sovereign-workspace/optional-runtimes/...`, the install log shows **0**
`SKIPPED-WITH-RECORD` lines, where it previously showed one. The likely cause is benign: the new
placeholder matches no rebase prefix, so no change is proposed and there is nothing to skip. But
N-13b exists precisely to make an absent optional adapter *visible in the record*, and that record
now appears empty. I have not proven harm. Reviewer should confirm whether the optional-skip signal
is intentionally retired or accidentally silenced.

---

## 7 · ON THE SEAT'S DECLARED DEVIATION

X-4 step 3 parked; the seat kept steps 1, 2 and 4 where the directive said "PARK X-4 in full."
It disclosed this unprompted and offered the revert. **Validator recommendation: accept.** Step 2
is the audit's own stated precondition for ever attempting step 3 — the value-matched rewrite is
the trap that would silently produce an install pointing at a nonexistent interpreter — and
reverting `9110390` would restore that trap. The risk it introduces was retired the honest way: the
full C4 proxy cycle was re-run end to end. Step 3's park rests on a host fact I confirm: every
Python 3.12 on this machine lives under the user profile, so a username-free placeholder and a
working development worktree are not jointly satisfiable here.

That said, the deviation is real and S-9 says park rather than improvise. The seat was right to
raise it rather than keep it quietly; the operator, not the validator, closes it.

---

## 8 · ASSESSMENT

Every item the directive assigned is closed or parked with a named blocker. A-1, A-2, A-3, A-4 and
A-5 are all verifiably shut, and A-1's fix is structural. Two of my directive defects were caught
before they produced false completions, and three of my measurements were corrected against bytes.
No claim in the builder's report failed re-derivation.

**Convergence verdict: CONVERGED, with one release gate RED by operator design.**
`provenance_cross_hash_check` exits 1 and cannot be closed by a builder — refreshing the registry is
a declaration of source identity. The residual queue is reviewer verdicts and operator rulings only.

**This candidate must not be promoted while that gate is red.** It is not a blocker on the
CLOSEOUT-01 work; it is a blocker on the release.

---

## 9 · OPERATOR DECISIONS

| id | decision | recommendation |
|---|---|---|
| **OD-27 (new, blocking)** | Refresh `module_source_registry.json` to the seal's provenance identities for debate, distillery, sow | Ruling required. This declares three modules' source identity — a statement about what the product *is*, which no builder or validator may make. Layer 2 is fixed, so the identities are now reproducible from the commit they name; that is what makes the ruling safe to take. |
| **OD-28 (new)** | N-19 `shell/BUILD-MANIFEST.txt` staleness | Route to the reviewer first — I could not verify it, and it predates CONVERGE-01. |
| **OD-29 (new)** | Accept the X-4 partial, or revert `9110390` | ACCEPT, per §7. |
| **OD-30 (new)** | X-4 step 3 disposition | Three costed options are in the seat's dossier. Not urgent; the shipped values are overwritten at install. |
| **OD-20** | `e2e8e55` lineage import | **RATIFY**, unchanged — byte-identical to the rollback-of-record baseline. |
| **OD-23** | `test_states.py` assertion inversion | No recommendation; reviewer reads the inventory first. |

Unchanged and open: the SOW and Distillery dependency-lock defects — reproducible provisioning is
still not achieved for those two modules, and that remains the largest genuine barrier to an
enterprise production claim. Also open: the 19 CANDIDATE gates plus Gate-5/5b, the T-2 attended
session, the true clean-room VM install, and OBS-1.

Reviewer package: `release-planning/REVIEW-PACKAGE-CLOSEOUT01.zip`
sha256 `8e6987ca078a13ab8bc510c801d7404c1d1e1c2ff0b7896ce478c27e051b4825` (verified this audit).

VALIDATOR CLAIM: no gate submitted for reviewer evaluation; no PASS asserted; nothing promoted.

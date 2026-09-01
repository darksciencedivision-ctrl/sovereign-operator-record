# VALIDATOR AUDIT — CONVERGE-01 SEAL `32f191e`
## Seat: validator (claude) · 2026-08-29 · authority: SWS-REM-DIR-20260828 R2 + Annex A
## Subject: the CONVERGE-01 run reported by builder seat grok-4.6 (opencode), sealed at `32f191e988e4690f5d68c83d541ca8668445dd03`

This is an evidence audit against the tree and the frozen ZIP bytes. It asserts no PASS and
promotes nothing. Facts are separated from interpretation; every negative finding names the
command that produced it.

---

## 0 · AUDIT METHOD AND ITS LIMITS (read before the findings)

Verification ran from the Linux side of the device bridge against the Windows worktree.
Consequences the operator must weigh:

- **L-1** `tools/release` unit tests re-run here produced 4 failures in `test_rebase_adapters`
  plus 1 loader error for `test_generate_release_identity`. Inspection shows the failures are
  Windows path-separator artifacts (`.../candidate\modules\debate` treated as one filename on
  POSIX). **These are artifacts of my method, not defects.** The builder's Windows measurement
  (72 collected, 71 pass / 1 known manifest failure at C5) stands as authoritative.
- **L-2** Not re-run by me: SOW desktop (1104), SOW terminal (225), Token Center (32), shell
  (166), and the C4 PowerShell proxy install cycle. Those remain builder-measured, reviewer-
  unverified.
- **L-3** `git status --porcelain` was run under `timeout` with its **exit status captured**
  (exit 0, empty output). Per the LOOP-RUN-01 lesson, empty output from a killed command is not
  a clean tree; this one completed.

---

## 1 · CLAIMS INDEPENDENTLY RE-DERIVED AS TRUE

| # | claim | how verified |
|---|---|---|
| T-1 | HEAD `32f191e…`, porcelain clean | `git rev-parse HEAD`; `timeout 120 git status --porcelain` exit 0, empty |
| T-2 | 11 commits `a945d99..32f191e` in the reported order | `git log --oneline` |
| T-3 | 6 annotated tags; `rel/shell/1.0.0-rc.1` on `a45fb98` | `git tag -l` + `rev-parse` |
| T-4 | All 8 release ZIPs match their `.sha256` sidecars | recomputed sha256 for each |
| T-5 | `zip-recut.json` hashes equal the on-disk artifacts, all 8 | field-by-field comparison |
| T-6 | Review package `93f1a15ac5b9fc6551f8d21fa292b9630055c7ab0f45be52b45330b0c3a8a33b` | recomputed; matches claim and sidecar |
| T-7 | **V-9** — RESET baseline is `00ae9eef…`; `canonical_registry.py` (`88c2362a…`) and `llamacpp.py` (`b2484b88…`) are byte-identical to the ZIP copies in **both** internal locations (`Production Workspace/` and `LATEST_MODULE_SOURCES/`), read via `zipfile` without extraction | independent Python re-derivation |
| T-8 | **V-11** — both C2 parks are real: `modules/sow/requirements-dev.txt` holds only `jsonschema>=3.2` / `pytest>=7`; `modules/distillery/INSTALL-PROVENANCE.json` has `lockfiles: []` and only a `pyproject.toml` exists | direct read |
| T-9 | Package-boundary gate on a clean `git archive` extract of the seal commit: **PASS** — 3565 scanned, 0 violations, 0 credential hits, 2011 quarantined-historical, 5 allowlisted | `package_boundary_gate.py --root . --allowlist tools/release/fixture_allowlist.json` inside the extract |
| T-10 | `check_governance_bom` PASS; `check_model_consistency` PASS | direct run |
| T-11 | `release-artifacts/release-build-manifest.json` is correct: `source_commit` `32f191e…`, both composite hashes match measurement | recomputed |
| T-12 | No native binaries ship: 0 `node_modules` entries and 0 `.exe/.node/.dll` in either candidate ZIP | zipfile enumeration |

**Erratum against my own earlier reading.** My first archive-gate run reported FAIL with 8
credential hits. That was my invocation error — `--allowlist` defaults to `None`, so the
value-based fixture allow-list was never loaded. Corrected invocation gives PASS. The FAIL is
retracted.

---

## 2 · FINDINGS

### A-1 · RELEASE-MANIFEST release-artifact hashes are 100% stale, and the gate cannot see it — HIGH

**Evidence.** `RELEASE-MANIFEST.json` carries a top-level key `release_archives_gitignored`
with 17 entries. Re-hashing every path the manifest enumerates: **63 of 80 match, 17 mismatch —
and the 17 are exactly that key's entries.** Every tracked-tree hash is correct; every release-
artifact hash is wrong. Example: the manifest records the source ZIP as `e4b3e3da…` / 29,820,189
bytes; the file is `27de97cd…` / 29,856,463 bytes. Those manifest values are the **C4-era build
outputs** recorded in `C4/NOTES.md`, not the C6 recut.

**Evidence.** `release_manifest_check.py` walks only `modules[].{locks,module_manifests,artifacts}`
and top-level `batch_files`. It never reads `release_archives_gitignored`. It therefore reported
`PASS (0 problems)` — truthfully as measured, but the number attests nothing about the release
archives. The gate is blind to that section **by construction**.

**Interpretation.** This is a structural circularity, not builder carelessness: the manifest is a
tracked file, so it enters the archive; the archive's hash therefore cannot be inside the manifest
that produced it. "Manifests last" closes the tracked-byte loop and cannot close this one.
C6/NOTES.md's line "`release_manifest_check` problem_count 0" is accurate but, unqualified,
implies coverage that does not exist.

**Recommendation (operator/reviewer to rule).** Remove `release_archives_gitignored` from
`RELEASE-MANIFEST.json` and let `release-artifacts/release-build-manifest.json` — which is
untracked, already carries the seal commit, and is already correct — be the sole authority for
artifact hashes; have the manifest reference it by name only. Then extend `release_manifest_check`
to fail on any manifest key holding `path`+`sha256` pairs it does not know how to check, so a
future unseen section cannot pass silently.

### A-2 · The spike-Electron risk's own closure condition is false, yet it is filed as accepted — MEDIUM (contradiction)

**Evidence.** `C2/PARKED-DOSSIERS.md`, item `C2-RISK-SPIKE-ELECTRON-LOCK-HELD-NONPRODUCT`, states:
"exclusion from release artifacts must be verified by C6 manifests/archive contents **or the risk
remains release-blocking**."

**Evidence.** `spike_compositor` ships in **both** candidate ZIPs, from **two** tracked roots —
`modules/sow/tools/spike_compositor` and `evidence/cpm1/before-b/modules/sow/tools/spike_compositor`
— 69 entries each. Both `package-lock.json` copies pin `electron` `31.7.7` and `extract-zip ^2.0.1`,
the two high-severity findings. C6 performed no exclusion check.

**Mitigating evidence.** No vulnerable binary is distributed: 0 `node_modules` entries and 0
`.exe`/`.node`/`.dll` in either archive (T-12). What ships is a dependency *declaration* that would
install a vulnerable Electron only if someone ran `npm ci` in that directory.

**Contradiction.** `RELEASE-NOTES-DRAFT.md` lists "spike Electron/extract-zip nonproduct" under
*Accepted limitations* without recording that the dossier's exclusion precondition was not met.
Two builder documents disagree on the item's status.

**Interpretation.** A builder-hermetic remedy exists (`.gitattributes export-ignore` on both spike
paths, which removes them from `git archive` without touching product bytes). Because a
builder-hermetic action remains, the run does **not** strictly satisfy the convergence definition
in ENTRY 006, even though the residual queue is otherwise correct.

### A-3 · Thirteen backup-suffixed files ship inside the candidate archives — MEDIUM

**Evidence.** The source ZIP contains 13 backup-suffixed entries: 7 new `.pre-CONVERGE01` files
(including `SBOM.json.pre-CONVERGE01`, 9,832 lines, and `RELEASE-MANIFEST.json.pre-CONVERGE01`),
5 pre-existing `.pre-rebase` adapter backups, and `evidence/cpm1/baseline/host-hardware.pre-add08.json`.
The `.pre-CONVERGE01` set was created and committed by `32f191e` and is enumerated in the manifest
policy line, so it is deliberate, not stray.

**Interpretation.** The `.pre-rebase` backups were blessed by the N-13 spec. The `.pre-CONVERGE01`
set was not specified anywhere in the CONVERGE-01 directive. A distribution that contains two
contradicting SBOMs and two contradicting release manifests is a provenance hazard: a consumer or
scanner has no rule for which is authoritative. Preserving prior records is right; **shipping** them
is not.

**Recommendation.** Keep the backups as history, move them out of the archive (bundle them, or
`export-ignore` the `*.pre-CONVERGE01` pattern). Operator ruling on whether `.pre-rebase` stays.

### A-4 · VERSION.json `source_commit` cannot name the seal commit — MEDIUM · **defect of authorship is mine**

**Evidence.** `VERSION.json.source_commit` = `a45fb98…` (the C4 commit). The seal is `32f191e…`.
`release-build-manifest.json` records `32f191e…`. The two identity documents disagree.

**Evidence of honest disclosure.** `C6/NOTES.md`: "VERSION.json pins tooling head `a45fb98` rather
than the seal commit so identity/SBOM hashes stay coupled inside `32f191e`." `RELEASE-NOTES-DRAFT.md`
states both commits explicitly. **The builder did not misrepresent this.**

**Interpretation.** CONVERGE-01 R2 §C6 instructed "Fill VERSION.json's `source_commit`" with the
final commit. That instruction is **unsatisfiable**: writing the final commit hash into a tracked
file changes the tree and therefore changes the commit hash. This is the same self-reference defect
I introduced once before in the §10 directive-token clause. **I accept authorship.** The builder
parked it correctly rather than fabricating a value.

**Recommendation.** Make the convention explicit rather than leaving two documents to disagree:
`source_commit` = last product/tooling commit (by definition), plus a new field naming where the
seal commit is recorded, e.g. `"seal_commit_record": "release-artifacts/release-build-manifest.json"`.
Amend the directive so no future seat inherits an impossible instruction.

### A-5 · All 11 commits are misattributed to a seat that did not author them — LOW/MEDIUM (traceability)

**Evidence.** Every commit `a945d99..32f191e` carries author
`builder qwen3.8-max (SWS-REM-DIR-20260828) <builder.qwen@sws-rem-dir-20260828.local>`; the
worktree's `git config user.name/user.email` still hold those values from LOOP-RUN-01. The actual
seats were `codex (local harness)` (C0/C1) and `grok-4.6 (opencode)` (V/C4-FINISH/C5/C6), as the
bundle reports correctly state.

**Interpretation.** The program's authority chain is hash-rooted and the commit author field is part
of the commit hash. **History must not be rewritten** — doing so would invalidate every hash,
tag, archive and manifest downstream, at far greater cost than the defect. The correct remedy is an
external attribution record plus fixing `git config` before the next run.

**Recommendation.** Add a SEAT-ATTRIBUTION record to custody mapping each commit to its true seat;
set `git config user.name` per seat at BOOT in the next directive; add it to the BOOT checklist.

### A-6 · Evidence bundles are not where the directive says — LOW

**Evidence.** The directive specified `bundles\CONVERGE01\...` relative to the worktree. The
worktree has no `bundles/` directory; the evidence is complete and well-formed at
`release-planning/bundles/CONVERGE01/`.

**Interpretation.** The deviation is an improvement — it keeps builder evidence out of the release
tree, exactly the lesson N-16 taught. The directive text is what is wrong. Correct the directive,
not the location.

---

## 3 · ASSESSMENT OF THE RUN

Evidence quality is the highest of any run in this program. The builder disclosed against its own
interest repeatedly: V-13 recorded that the proxy cycle was still incomplete; C4 captured the
24,462-entry interrupted residue before clearing it; C5 named the single tools/release failure and
traced it to a pre-existing cause under S-12 rather than making a red suite green; C2 refused to
synthesize the missing SOW and Distillery locks and parked them with dossiers. Phase V's two
load-bearing checks (V-9, V-11) both re-derive true under independent measurement. No finding in
this audit is a false claim by the builder; A-1 is an undisclosed gap, A-2 an internal contradiction,
A-3 an unspecified action, A-4 my own defect, A-5 a stale configuration.

**Convergence verdict: NOT STRICTLY CONVERGED.** A-1, A-2 and A-3 each admit a builder-hermetic
remedy, so the residual queue is not yet reviewer-and-operator-only. The gap is narrow — one
manifest-structure change, two `export-ignore` decisions — and does not impugn the run.

---

## 4 · WHAT THE OPERATOR IS BEING ASKED TO DECIDE

| id | decision | validator recommendation |
|---|---|---|
| **OD-20** | Ratify or revert the `e2e8e55` lineage import | **RATIFY** — strengthened by T-7: both files are byte-identical to the rollback-of-record baseline in both of its internal locations. The import restored baseline bytes; it did not invent content. |
| **OD-23** | Accept or reject the assertion inversion in `test_states.py` | No recommendation until the reviewer seat reads `V/assertion-inventory.md`. An assertion inversion is exactly the change class S-14 exists to stop; it needs adjudication, not a validator preference. |
| **OD-24 (new)** | Where do release-artifact hashes live? | Move them out of `RELEASE-MANIFEST.json` to `release-build-manifest.json`; harden the checker against unknown hash-bearing keys (A-1). |
| **OD-25 (new)** | Does `spike_compositor` ship? | Exclude both paths via `export-ignore` and close the risk mechanically, rather than accepting a shipped vulnerable pin (A-2). |
| **OD-26 (new)** | Do `.pre-CONVERGE01` / `.pre-rebase` backups ship? | Exclude `.pre-CONVERGE01` from archives; keep them tracked as history. Operator ruling on `.pre-rebase` (A-3). |

Unchanged and still open: the SOW and Distillery dependency-lock defects (reproducible provisioning
is **not** achieved for those two modules — this remains the single largest genuine barrier to an
enterprise production claim), the 19 CANDIDATE gates plus Gate-5/5b reviewer backlog, the T-2
attended session, and OBS-1.

Reviewer package ready: `release-planning/REVIEW-PACKAGE-CONVERGE01.zip`
sha256 `93f1a15ac5b9fc6551f8d21fa292b9630055c7ab0f45be52b45330b0c3a8a33b` (verified this audit).

VALIDATOR CLAIM: no gate submitted for reviewer evaluation; no PASS asserted; nothing promoted.

---

## 5 · ADDENDUM — finding N-18, recorded after §1-§4 were written

### N-18 · Developer-machine identifiers ship inside the candidate archive — MEDIUM (hygiene / privacy)

**Evidence.** Tracked files that enter `git archive` contain the developer's Windows username in an
interpreter path: `shell/config/install.json` (`python_312`), `shell/modules/distillery.json` and
`shell/modules/tokencenter.json` (`/launch/argv/0`), and `tools/release/rebase_adapters.py:13`
(`OLD_PYTHON` constant). `shell/modules/llamacpp.json` additionally carries three
`D:/Product Software/Production Workspace/...` paths from the previous machine — it is the declared
optional-skip adapter, so the rebase leaves it alone by design. The five `.pre-rebase` backups carry
both the username and the old root; A-3 already removes those from the archive.

**Explicitly NOT a functional defect.** `tools/release/install.ps1:94` resolves the destination
interpreter dynamically —
`$python312 = (& $pyLauncher -3.12 -c 'import sys;print(sys.executable)').Trim()` — writes it into
the destination `install.json`, and line 132 runs `rebase_adapters.py` against the destination. The
adapter mapping table covers `/launch/argv/0` for distillery, llamacpp and tokencenter. **An install
on another machine therefore repoints these correctly.** My initial reading — that installs would
be broken off-host — was wrong and is retracted before it left this document. What ships is a stale
development value, not a broken product.

**Interpretation.** For an enterprise release, a personal username in distributed product and
tooling bytes is a defect on its own terms. It is also a latent trap: `rebase_adapters.py` rewrites
the interpreter only when the existing value equals its hardcoded `OLD_PYTHON` constant, so anyone
who "sanitizes" the config to a placeholder without changing that logic will produce an install that
silently keeps a non-existent interpreter. The fix must change the rewrite to unconditional at the
interpreter pointers **before** the committed value is neutralized.

**Disposition.** Carried into CLOSEOUT-01 as item X-4, scoped last and explicitly parkable, with a
mandatory re-run of the C4 proxy install cycle if it lands. Generalized as standing rule **S-16**.

### Consequence for the convergence verdict

Unchanged in direction, stronger in degree: four builder-hermetic items now remain (A-1, A-2, A-3,
N-18), so CONVERGE-01 is not strictly converged. None of the four is a false claim by the builder.


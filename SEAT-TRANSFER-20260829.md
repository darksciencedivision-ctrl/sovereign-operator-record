# SEAT TRANSFER — BUILDER (CLOSEOUT-01) → T-2 OBSERVER
## SWS-REM-DIR-20260828 R2 · 2026-08-29 · issued by validator seat claude (cowork)

The outgoing seat's context does not transfer. This document is what does.

## 1 · State of record

| item | value |
|---|---|
| Candidate seal | `50b47326f91c5abab8b6d5250d59b1dba96a4d46` |
| Last product/tooling commit | `be8dcb004b91c3d2ff2986f69a400b9317d36115` |
| Predecessor seal | `32f191e…` (CONVERGE-01) → `a945d99…` (Batch 4-E) |
| System version | `sovereign-workspace 1.0.0-rc.1` — **rc, not promoted** |
| Worktree | `D:\producttion software 2\release-worktree`, porcelain clean |
| Rollback of record | `SOVEREIGN_RESET_BASELINE_20260827T193540Z.zip` sha256 `00ae9eef…` |
| Governing directive | `SWS-REM-DIR-20260828-CANDIDATE-R2.md` sha256 `9061c2e8…` |
| Loop protocol | `LOOP-PROTOCOL-20260828.md` sha256 `acae8fb8…` |
| Latest audit | `VALIDATOR-AUDIT-CLOSEOUT01-SEAL-20260829.md` sha256 `8258ade3…` |
| Reviewer package | `REVIEW-PACKAGE-CLOSEOUT01.zip` sha256 `8e6987ca…` |

## 2 · Seats

| seat | who | status |
|---|---|---|
| Operator | sam | holds scope, promotion, service launches, all OD rulings |
| Builder | claude-code (opus-5) | **CLOSEOUT-01 complete, seat released** |
| T-2 Observer | claude-code (opus-5), attended | **incoming — this transfer** |
| Reviewer | chatgpt | backlog untouched: 19 CANDIDATE gates + Gate-5/5b |
| Validator | claude (cowork) | continuing |

Git identity in the worktree is currently `builder claude-code opus-5`. The T-2 seat writes no
commits, so it does not change it; the next builder seat sets its own at BOOT (standing practice
since A-5).

## 3 · What is closed and verified

A-1 manifest circularity — broken structurally; artifact hashes now live outside the archive.
A-2 spike compositor — 0 members in the archive. A-3 backup snapshots — 0 members.
A-4 release-identity convention — recorded in `VERSION.json` and the gate policy README.
A-5 commit attribution — correct. N-20 layer 2 — provenance records reproduce themselves again.
All verified independently against bytes by the validator, not accepted on report.

## 4 · What is open — carried forward whole

**Blocking promotion:**
- **OD-27** — refresh `module_source_registry.json` for debate/distillery/sow.
  `provenance_cross_hash_check` exits 1 until then. Operator ruling: it declares source identity.

**Operator rulings pending:** OD-20 (ratify the `e2e8e55` lineage import — validator recommends
RATIFY), OD-23 (assertion inversion — reviewer reads the inventory first), OD-28 (N-19
`shell/BUILD-MANIFEST.txt` staleness — validator could not verify, no result reported),
OD-29 (accept the X-4 partial — validator recommends ACCEPT), OD-30 (X-4 step 3 disposition).

**Genuine engineering defects, unremediated:** SOW has unpinned dependency ranges and Distillery
has no Python lock. Reproducible provisioning is not achieved for those two modules. This remains
the largest real barrier to an enterprise-production claim, and no loop can close it — S-11 forbids
synthesizing a lock. It needs an operator decision about where authoritative locks come from.

**Open questions this run can answer:** OBS-1 (port tile red while serving), OBS-2 (optional-adapter
skip signal now silent).

**Operator-deferred:** OP-2 frontier providers (awaiting spend/key/role rulings), OP-6 Distillery
UI, Lane R research.

**Never yet done:** the true clean-room install on a machine without Python, node and Ollama. Every
install proof so far is a same-host proxy and proves mechanics and layout only.

## 5 · Standing practice inherited by the incoming seat

- Empty output from a killed command is not a clean tree. Capture the exit status.
- `BOOT-FAILURE.md` is for hash mismatch only; any other obstruction is reported in-session.
- A boot halt gets no closing claim line — there is no work product to characterize.
- A record must not live inside the set it describes. Three findings deep now (A-1, N-19, N-20).
- The validator is an input, not an authority. It has been corrected by builder seats five times
  across this program. Re-derive rather than trust.

## 6 · What the T-2 seat is for

Everything above was measured hermetically — suites, gates, hashes, archives. **None of it proves
the product works when a person uses it.** T-2 is that proof, and it is the last category of
evidence this candidate has never had. Its highest-value outputs are the clean-stop orphan sweep,
the VRAM census, and an honest answer on whether the Conductor takes input.

Transfer issued by: validator seat claude (cowork), 2026-08-29.

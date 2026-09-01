# FIXUP-01 — FULL WORK DIRECTIVE
## Builder seat: `claude-code (opus-5)` · SWS-REM-DIR-20260828 R2 + Annex A · ENTRY 014
## Parent: CLOSEOUT-01 seal `50b47326f91c5abab8b6d5250d59b1dba96a4d46`
## Basis: PUNCH-LIST-V5-20260830.md and bundles/T2-20260829/VALIDATOR-LIVE-OBSERVATIONS-01.md

**Operator:** paste the one-line kickoff. Long run. Record this file's SHA-256 externally.

---BEGIN PASTE---

You are the BUILDER seat `claude-code (opus-5)`, executing FIXUP-01 under SWS-REM-DIR-20260828 R2
and Annex A, authorized by OPERATOR-INSTRUCTIONS.log ENTRY 014.

Run-to-convergence: no stops between items or phases; park-and-continue per E-1..E-6; exit only
when §7 has no blank row. CANDIDATE only. You never write `status: PASS`; you never promote.

## Why this directive exists — read before you plan

Five defects reached the operator's screen through fully green test suites. Every gate passed.
The Token Center frame never navigates, the llamacpp module vanished from the UI, and the only
enabled "Open" button is on the one module that cannot open anything. In each case the tests
asserted that the **configuration was correct** and nothing asserted that the **product does the
thing**. Two further reports turned out to be fail-closed interlocks working exactly as designed.

So this run has an unusual acceptance bar: **a fix is not done when its test passes. It is done
when the running product demonstrably behaves.** Where you cannot drive the UI yourself, you will
hand a precise verification request to the validator seat, which has a browser on this host.

---

## §1 · STANDING RULES (S-1..S-18) — binding

- **S-1 · Write set.** Only `D:\producttion software 2\release-worktree` and
  `D:\producttion software 2\release-planning\bundles\FIXUP01`. Never the frozen ZIPs, never the
  operator's Desktop or Start Menu.
- **S-2 · Encoding.** Never create or rewrite a tracked file with PowerShell redirection or
  `Set-Content` without `-Encoding utf8NoBOM`. PowerShell 5.1 emits UTF-16 and has corrupted a
  tracked source file in this program once. Assert no tracked file begins `FF FE` after each commit.
- **S-3 · No invented APIs.** Read the source before calling it. Never guess a flag or field.
- **S-4 · Coupled values move together**, in the same commit, each site listed in NOTES.
- **S-5 · Suite side effects are restored.** The shell suite rewrites tracked evidence and manifest
  paths. Restore each byte-exactly from HEAD, hash-verify, and prove porcelain empty afterward.
- **S-6 · Test hygiene.** No test deleted, skipped, xfailed, or loosened to make a suite pass.
- **S-7 · Minimal diff.** No drive-by refactors or reformatting.
- **S-8 · Per-item discipline.** One item, one commit, one bundle, fail-before and pass-after.
- **S-9 · Park, do not improvise.** Never substitute a different solution for a blocked one.
- **S-10 · Prohibitions.** No billed API calls, no provider calls, no model downloads, no Ollama
  model loads, no writes outside the write set, no rewriting of any existing commit.
- **S-11 · Locks are read-only inputs.** Never regenerate, edit or synthesize a lockfile.
- **S-12 · A red suite is not an authorization.** A suite is satisfied when every failure traces to
  a named pre-existing baseline or parked cause and none is attributable to this loop.
- **S-13 · Held items stay held.** `e2e8e55` (OD-20) and the `test_states.py` assertion inversion
  (OD-23) are open operator rulings. Do not advance, revert or build on either.
- **S-14 · No assertion edits without authority.**
- **S-15 · No silently-unchecked hashes.** No manifest may carry a `path`+`sha256` pair its checker
  does not verify, and a test must prove the checker fails when it is wrong.
- **S-16 · No developer-machine identifiers in distributed bytes** unless overwritten at install
  time and that overwrite is proven by test.
- **S-17 · A rendered control must be a performable action (NEW).** If the UI offers it, the product
  must do it. No item in this run is complete on a green test alone: each carries either a
  demonstration against the running product, or a written LIVE-VERIFICATION REQUEST naming exactly
  what must be observed and by whom. A passing test that asserts only configuration does not close
  an item.
- **S-18 · Fail-closed interlocks are operator territory (NEW).** You will encounter two:
  the deliberate absence of `modules/sow/config/live_operation.json` (enforcement-by-absence), and
  the `H-10` quota guard that forces `SOW_CONDUCTOR_AUTOLAUNCH=0` (`shell/src/adapter.py:344`).
  **You may not create, populate, bypass, weaken, stub, mock around, or disable either.** They are
  open operator rulings OD-31 and OD-32. If an item appears to require one, PARK the item and say
  which ruling would unblock it. Making the product appear to work by opening a guard the operator
  has not opened is the most serious error available to you in this run.

---

## §2 · BOOT

1. HEAD == `50b47326f91c5abab8b6d5250d59b1dba96a4d46`
2. `git status --porcelain` empty **with exit status 0 captured** — empty output from a killed or
   timed-out command is NOT a clean tree
3. `OPERATOR-INSTRUCTIONS.log` contains ENTRIES 001–014
4. Read `PUNCH-LIST-V5-20260830.md` and
   `bundles\T2-20260829\VALIDATOR-LIVE-OBSERVATIONS-01.md` in full
5. Set your seat identity before the first commit:
   `git config user.name "builder claude-code opus-5 (SWS-REM-DIR-20260828)"` and
   `git config user.email "builder.claude@sws-rem-dir-20260828.local"`. Rewrite no existing commit.

Hash mismatch → `bundles\FIXUP01\BOOT-FAILURE.md`, stop. Any other obstruction → report in-session,
do not emit BOOT-FAILURE.md. Checkpoint after every phase to `bundles\FIXUP01\CHECKPOINT.json`.

---

## §3 · PHASE D — DIAGNOSE. Reproduce everything before you change anything.

The validator's observations are an input, not an authority; it has been corrected by builder seats
five times in this program. Reproduce each from bytes and record VERIFIED / REFUTED / PARTIAL in
`bundles\FIXUP01\D\DIAGNOSIS.md` with the exact command and output. **A refuted finding is
reported, never fixed.**

- **D-1 (N-21).** Start the shell on a spare port. Confirm the Token Center iframe's
  `contentWindow.location` is `about:blank` while its `src` attribute is correct, both CSP headers
  are as specified, and Token Center serves 200 standalone. Then find the mechanism: read the
  template and the client script that construct the iframe and determine **why** the navigation
  never commits. Prime suspect to confirm or eliminate: `src` and `sandbox` applied in an order
  that re-initialises the element. State the cause; do not fix it yet.
- **D-2 (N-22).** Determine whether the shell *drops* llamacpp at load with an error or *filters*
  it deliberately. Read the module-discovery path in `shell/src/`. Capture the shell's stderr at
  startup. Establish whether `X-4`'s placeholder path is the trigger.
- **D-3 (N-23).** Find the enable/disable guard for the `data-action="open"` control and state, by
  line, why a module with `open.kind: none` reaches the UI enabled while modules with real URLs are
  disabled.
- **D-4 (N-27).** SOVEREIGN's readiness probe fails. `.venv/Scripts/python.exe`,
  `sovereign_product`, and the `/v1/health` route at `server.py:1906` are all confirmed present.
  Launch the module's argv by hand, capture stderr, and find the actual cause. This is the one item
  whose cause is genuinely unknown — do not theorise in the report, measure.
- **D-5 (N-25).** Establish the exact shape of the local-model gap: which code path builds the
  picker's provider list, whether local enumeration is absent entirely or present-but-gated, and
  whether offering local models requires anything from `live_operation.json`. **If local selection
  turns out to be gated behind that file, S-18 applies: park and say so.** Local model use costs
  nothing, so a design that gates it behind the spend switch is itself a finding — report it.
- **D-6.** Re-measure the CLOSEOUT-01 baseline: `git archive HEAD` file count, the archive-bound
  package-boundary gate invoked as
  `package_boundary_gate.py --root . --allowlist tools/release/fixture_allowlist.json`
  (the flag is required; its default is None and omitting it yields a false FAIL), and the
  `tools/release` unittest count. These are your before-numbers.

Checkpoint.

---

## §4 · PHASE F — FIX. Strict order. One commit each.

### F-1 · N-21 — make the Token Center frame render
Fix the cause D-1 established. The shell's `frame-src` and Token Center's `frame-ancestors` are
already correct and **must not be widened** — this is a rendering defect, not a policy defect
(S-18-adjacent: do not "fix" it by loosening CSP). Additionally implement the fallback the OP-4
spec required and that does not exist: when Token Center is not reachable, the panel shows a status
and a Start control, never a blank frame.
Also address **N-21b**: `sandbox="allow-scripts allow-same-origin"` is an inert sandbox by the
browser's own warning. Either drop the attribute deliberately with a recorded rationale, or set a
combination that means something. Record which and why.
Pass-after must include a DOM-level demonstration that the frame's `contentWindow.location` is the
Token Center URL and its document is non-empty — plus a LIVE-VERIFICATION REQUEST per S-17.

### F-2 · N-22 — llamacpp must be visible and honestly unavailable
An optional runtime that is not installed presents as **present and unavailable**, never absent.
The operator cannot act on a module they cannot see. Restore llamacpp to the module list with a
clear unavailable state naming the missing runtime path. Do **not** solve this by reverting X-4's
path neutralisation — the foreign developer paths must stay out of distributed bytes (S-16).
This also closes **OBS-2**: state where the optional-adapter skip is now visible to a human.

### F-3 · N-23 — Open must be honest, then capable
Two halves, both required unless the second parks.
1. **Honest:** a module with `open.kind: none` must not present an enabled Open control. Fix the
   guard D-3 identified so enablement follows the module's actual capability and readiness.
2. **Capable:** SOW is an Electron desktop app; the adapter schema permits only `browser` or `none`
   (`shell/src/adapter.py:199`), so there is no way to raise an existing native window. Add a third
   `open.kind` that focuses the launched window, wire it in the shell backend, set it in
   `sow.json`, and extend the adapter tests. If raising a window cannot be done from the shell's
   process model without violating the write set or the Job Object design, **PARK the second half
   with the exact blocker** and ship the first.

### F-4 · N-27 — SOVEREIGN readiness
Fix whatever D-4 measured, within scope. If the cause is environmental rather than a product
defect — a missing dependency in the venv, a host service, a port conflict — **do not paper over it
in the adapter**: report it as an environment finding and park. Lengthening the timeout to make a
red probe green is the S-12 error and is forbidden.

### F-5 · N-25 — local models in the picker
Scoped by D-5. The goal is the operator's stated requirement: the picker offers **local models and
frontier providers from one selector**. `adapters/roster.py` already enumerates local Ollama models;
the Electron picker has no local path at all.
Constraints: local enumeration must degrade honestly when Ollama is not running — an empty local
section that says so, never a silent absence (the N-22 lesson). No model is loaded by you (S-10).
Nothing about `live_operation.json` is touched (S-18).
**This is the largest item in the run.** If it cannot be completed within the write set without
crossing into node-registry work that has no operator ruling behind it, deliver the enumeration and
display layer, PARK the remainder with a named blocker, and write what a follow-on batch would do.

### F-6 · Regression guard for the whole class
The three defects above shipped behind green suites. Add tests that would have caught them:
a test that every module the shell renders is present in the UI's module list; a test that every
enabled action control maps to a capability the adapter declares; and a test that the Token Center
panel resolves to a non-empty document or an explicit fallback. Put them where the shell suite runs
them. **This item is the one that stops this happening again — do not treat it as optional.**

---

## §5 · PHASE G — deterministic program at final bytes

After the last code commit, measured counts only, never adjectives:
SOW desktop `node --test` (expect 1104) · SOW terminal (225) · Token Center (32) · shell (166, with
S-5 restoration hash-verified, porcelain empty after) · `tools/release/test_*.py` (84 at parent;
state the new total) · gates: archive-bound package boundary with `--allowlist`, provenance
cross-hash (`--registry tools/release/module_source_registry.json` — **expected RED**, it is
blocked on operator ruling OD-27 and is not yours to fix), release-manifest check, governance BOM,
model consistency, innerHTML sink audit (`--file` and `--ledger` are required arguments; read
`innerhtml_audit.json` for its own `scope_file`).
Archive delta stated and accounted item by item against D-6.
S-12 governs. Any failure caused by F-1..F-6 is fixed in scope and Phase G restarts.

Checkpoint.

---

## §6 · PHASE H — seal, in the acyclic order

1. No further code changes.
2. Regenerate provenance, release identity and `RELEASE-MANIFEST.json` over **tracked bytes only** —
   no release-artifact hashes anywhere in the manifest.
3. Commit. **This is the seal.**
4. Only now `git archive` the seal commit into all release artifacts and sidecars.
5. Write `release-artifacts\release-build-manifest.json` naming the seal commit and every artifact
   and sidecar with byte counts and hashes.
6. Cold-extract every archive and hash-verify.
7. Re-run `release_manifest_check` and the archive-bound boundary gate against the sealed bytes.
8. Build `release-planning\REVIEW-PACKAGE-FIXUP01.zip` + sidecar: patches `50b47326..HEAD`, all
   `bundles\FIXUP01` evidence under 5 MB, custody docs.
9. `bundles\FIXUP01\RELEASE-NOTES-DRAFT.md` carrying every limitation forward, with the state of
   each item in this run.
10. `bundles\FIXUP01\FIXUP-REPORT.md`: commit chain, per-phase results, measured totals, the §7
    checklist filled, a residual queue of reviewer/operator items only, and a consolidated
    **LIVE-VERIFICATION REQUEST** listing every behaviour the validator or operator must confirm on
    the running product, with the exact steps.
11. Append FIXUP-01 to `release-planning\bundles\LOOP-RUN-REPORT.md`.

---

## §7 · CONVERGENCE CHECKLIST

```
[ ] D diagnosis report, six checks, each VERIFIED/REFUTED/PARTIAL
[ ] F-1 Token Center frame renders
[ ] F-1 fallback panel when Token Center is down
[ ] F-1b sandbox attribute resolved with rationale
[ ] F-2 llamacpp visible and honestly unavailable
[ ] F-2 OBS-2 skip signal visible to a human
[ ] F-3 Open control honest (enablement follows capability)
[ ] F-3 Open control capable (third open.kind) or PARKED with blocker
[ ] F-4 SOVEREIGN readiness fixed or parked as environment finding
[ ] F-5 local models enumerated in the picker
[ ] F-5 honest degradation when Ollama is down
[ ] F-6 regression tests for all three classes
[ ] G suites measured
[ ] G gates measured (provenance cross-hash expected RED, OD-27)
[ ] G archive delta accounted
[ ] H manifests regenerated, tracked bytes only
[ ] H artifacts cut AFTER the seal commit
[ ] H build manifest covers all artifacts and sidecars
[ ] H cold-extract verification
[ ] H review package
[ ] H release-notes draft
[ ] H fixup report with consolidated LIVE-VERIFICATION REQUEST
```

No blank rows, and no residual item that is builder-hermetic, or you are not converged.

---

## §8 · EXCEPTION CLASSES — park and continue

E-1 money · E-2 irreversibility · E-3 spec contradiction · E-4 security regression · E-5 physical
dependency · E-6 queue exhausted. Every park gets a dossier under `bundles\FIXUP01\` naming the
exact blocker and what would unblock it. A park is a finding, not a failure.

---

## §9 · OUT OF SCOPE — do not touch

`live_operation.json` and the H-10 quota guard (S-18; OD-31 and OD-32) · the stale
`module_source_registry.json` and the red provenance gate (OD-27) · `e2e8e55` and the assertion
inversion (S-13, S-14) · `shell/BUILD-MANIFEST.txt` staleness (OD-28, reviewer first) · the SOW and
Distillery dependency-lock defects (S-11) · the 19 CANDIDATE gates and Gate-5/5b (reviewer) ·
OP-2, OP-6 and the OP-7 frontier programme (operator, unruled) · anything requiring Ollama to be
running or a model to load.

End every report with:
`BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`

Execute BOOT now. Run D → F → G → H to convergence, then stop.

---END PASTE---

# LOCAL-01 — FULL WORK DIRECTIVE
## Builder seat: `claude-code (opus-5)` · SWS-REM-DIR-20260828 R2 + Annex A · ENTRY 017
## Parent seal `8d9f5d2415599eeb0b71dc85a25a36469b41641e`
## Objective, in the operator's words: **"use the local models to get this entire system running... prove this system works."**

---BEGIN PASTE---

You are the BUILDER seat `claude-code (opus-5)`, executing LOCAL-01 under SWS-REM-DIR-20260828 R2
and Annex A, authorized by OPERATOR-INSTRUCTIONS.log ENTRY 017.

Run-to-convergence: park-and-continue per E-1..E-6; exit only when §7 has no blank row. CANDIDATE
only. You never write `status: PASS`; you never promote.

## The objective is different from every run before it

Prior loops hardened release machinery. **This one makes the product work.** The operator has never
had a session where he could select a model and talk to it. That is the deliverable. A green suite
that does not end in a working conversation with a local model is a failed run.

## The operator's two rulings, which set the whole posture

**LOCAL ONLY. NO FRONTIER.** The operator has declined to authorize live frontier operation
(OD-31 → DENY for now). `modules/sow/config/live_operation.json` **stays absent**. You do not
create it, populate it, stub it, mock it, or route around it. Frontier providers remain correctly
DENIED and their rows must keep saying so honestly. Local models need none of this — FIXUP-01
Phase D proved local enumeration works with that file absent.

**8B PARAMETER CEILING, HARD.** No model above 8 billion parameters may be used, selected,
defaulted to, or pulled. The operator's card is 8 GB and a 12 GB model previously straddled it at a
51/49 CPU/GPU split. If a larger model is already installed locally it must be excluded from
selection, not merely un-defaulted. You pull no models at all (S-10).

**CONDUCTOR: option C.** It must launch, accept typing, and spawn nothing until the operator picks
a model and sends something.

---

## §1 · STANDING RULES (S-1..S-19)

S-1 write set: `release-worktree` and `release-planning\bundles\LOCAL01` only.
S-2 encoding: never PowerShell redirection or `Set-Content` without `-Encoding utf8NoBOM`; assert no
tracked file begins `FF FE` after each commit.
S-3 no invented APIs — read the source first.
S-4 coupled values move together, in one commit, each site listed.
S-5 suite side effects restored byte-exactly and hash-verified; porcelain empty after. **Select
restore targets from an explicit closed list, never from `git diff --name-only`** — that mistake
reverted a builder's own work in FIXUP-01.
S-6 no test deleted, skipped, xfailed or loosened.
S-7 minimal diff. S-8 one item, one commit, one bundle, fail-before and pass-after.
S-9 park, do not improvise. S-10 no billed calls, no provider calls, **no model downloads or pulls**.
S-11 locks are read-only inputs.
S-12 a red suite is not an authorization; every failure traces to a named pre-existing or parked cause.
S-13 held items stay held. S-14 no assertion edits without authority.
S-15 no manifest may carry a `path`+`sha256` pair its checker does not verify.
S-16 no developer-machine identifiers in distributed bytes unless overwritten at install and proven.
S-17 **a rendered control must be a performable action** — no item closes on a green test alone.
S-18 **fail-closed interlocks are operator territory.** `live_operation.json` stays absent. See §4
F-3 for the one narrow, operator-ruled exception concerning H-10, and its exact boundary.
S-19 **NEW — honest degradation.** Every state a component can be in must be visible and explained
where the operator looks. No silent absence, no blank panel, no enabled control that does nothing,
no model offered that cannot run. If something cannot work, the UI says which thing and why.

---

## §2 · BOOT

1. HEAD == `8d9f5d2415599eeb0b71dc85a25a36469b41641e`
2. `git status --porcelain` — **expect one untracked file**,
   `modules/sow/docs/evidence/receipts/SHELL-LIVE-READY.json` (N-29, ruled OD-34, fixed in this run
   as F-6). Capture the exit status; empty output from a killed command is not a clean tree.
3. ENTRIES 001–017 present.
4. Read `PUNCH-LIST-V5-20260830.md`, `VALIDATOR-AUDIT-FIXUP01-SEAL-20260830.md`, and
   `bundles\FIXUP01\D\DIAGNOSIS.md` — that diagnosis refuted four validator findings and its method
   is the standard for this run.
5. Set seat identity before the first commit; rewrite no existing commit.

---

## §3 · PHASE D — measure the ground before changing it

Record VERIFIED / REFUTED / PARTIAL with commands and output in `bundles\LOCAL01\D\DIAGNOSIS.md`.

- **D-1 · The local library.** Start the Ollama daemon if it is not running (it is a local service,
  no spend, and the operator has authorized local operation). Enumerate every installed model with
  its parameter count and on-disk size. **Name every model over 8B.** Do not pull anything.
- **D-2 · The VRAM admission gate.** `worker_pane_spawn` refuses every local pane when the host's
  VRAM budget cannot be established (`control_plane/nodes/pane_picker.py`). Determine whether that
  budget establishes on this host with Ollama up, and if not, exactly why. This gate, not the
  frontier switch, is what decides whether local panes can run at all.
- **D-3 · Can the Conductor be backed by a local model?** This is the crux of the run.
  `main.js:2941` describes the Conductor as running "the real interactive `claude` session on
  launch" — a frontier CLI. Read `adapters/conductor/`, `apps/desktop/conductor/` and the
  conductor-admission path and determine whether a local Ollama model can serve as the Conductor's
  session, or whether the Conductor is frontier-only by construction. **Answer this before writing
  any code.** If it is frontier-only, say so plainly — that is a finding, and the operator's
  option C would then need a build, not a flag.
- **D-4 · Is H-10 redundant today?** The validator measured that with `live_operation.json` absent,
  the governed conductor spawn fails closed before anything is created. Confirm or refute: with
  `SOW_CONDUCTOR_AUTOLAUNCH` unset, does any billable session actually spawn? Measure it; do not
  reason about it.
- **D-5 · What runs a terminal today?** Trace what happens when the operator selects a local model
  in the picker and starts a pane — end to end, from selection to a live prompt.
- **D-6 · Baselines.** `git archive HEAD` file count; archive-bound boundary gate invoked as
  `package_boundary_gate.py --root <extract> --allowlist <extract>/tools/release/fixture_allowlist.json`;
  suite counts: SOW desktop 1104, terminal 225, Token Center 32, shell 188, tools/release 84.

---

## §4 · PHASE F — make it work

### F-1 · Local models selectable and runnable, with the 8B ceiling enforced
Whatever D-1/D-2/D-5 found. The picker must offer only models at or under 8B; anything larger is
excluded from selection with a stated reason (S-19), not silently hidden. If the VRAM budget cannot
establish, the picker says so in words the operator can act on. Selecting a local model and starting
a pane must reach a live prompt.

### F-2 · SOW terminals run local models
The operator opens a terminal, picks a local model, types, and gets a response. Demonstrate it.

### F-3 · Conductor — operator ruling, option C
**Scope of the H-10 exception, and its limit.** The operator has ruled that the Conductor may launch
and accept input. The quota protection that replaces H-10 is this: **the Conductor spawns no session
until the operator selects a model and sends a message**, and under LOCAL-01's posture it may back
that session with a **local model only**. `live_operation.json` remains absent, so the frontier path
stays refused by the live gate regardless — you rely on that refusal, you do not remove it.
If D-3 found the Conductor frontier-only by construction, **PARK F-3 with the exact blocker and the
cost of building a local-backed conductor**, and report it. Do not fake a conductor.

### F-4 · Debate Table on local models
The operator reported Debate opens but reaches no models. With Ollama up and the 8B ceiling, Debate
must run a real multi-model exchange using local seats. Confirm which model each seat loads.

### F-5 · SOVEREIGN on local models
SOVEREIGN now starts (FIXUP-01 F-4). Its model list must offer the local library under the same 8B
ceiling. Frontier entries stay honestly unavailable.

### F-6 · OD-34 — SOW receipts leave the tracked tree
`modules/sow/docs/evidence/receipts/` is git-tracked and declared in `sow.json` `runtime_writes`, so
using the product dirties the release candidate. Relocate runtime receipts to a gitignored path with
a boundary-gate assertion, exactly as N-16 did for the shell. Existing tracked receipts are history:
leave them, stop writing new ones there.

### F-7 · OD-33 — `build_release.ps1` cuts, it does not enumerate
It currently `git archive`s only the two composite archives, then records whatever `*.zip` it finds
in the output directory as authoritative — so six stale per-module archives nearly shipped under
correct-looking hashes. Cut all eight from the seal. A validator-authored defect; fix it properly.

### F-8 · OD-27 — refresh `module_source_registry.json`
Ruled by the operator's delegate. Refresh debate, distillery and sow to the seal's provenance
identities, and fill tokencenter's PENDING entry from its own record. `provenance_cross_hash_check`
must go green. It has been red since CONVERGE-01 and is the last gate blocking release.

---

## §5 · PHASE G — measure at final bytes

All suites with measured counts; all six gates, **including provenance cross-hash, which must now be
GREEN** (F-8). Archive delta accounted item by item. S-5 restoration hash-verified from a closed
list. S-12 governs.

**And the acceptance that matters (S-17):** a captured, end-to-end demonstration — Ollama up, a
local model at or under 8B selected in the picker, a terminal reaching a live prompt, a real
exchange, and a clean stop with no orphaned processes. Screenshots or transcripts. If any leg cannot
be driven from your seat, write a precise LIVE-VERIFICATION REQUEST naming what the operator or
validator must observe.

---

## §6 · PHASE H — seal, acyclic order

No code changes → regenerate provenance, identity and `RELEASE-MANIFEST.json` over tracked bytes
only → commit (this is the seal) → **then** cut all eight artifacts from that commit → write the
untracked `release-build-manifest.json` → cold-extract and verify → re-run both gates →
`REVIEW-PACKAGE-LOCAL01.zip` + sidecar → release-notes draft → `LOCAL-REPORT.md` with the §7
checklist and a consolidated LIVE-VERIFICATION REQUEST → append to `LOOP-RUN-REPORT.md`.

---

## §7 · CONVERGENCE CHECKLIST

```
[ ] D-1 local library enumerated, every model over 8B named
[ ] D-2 VRAM admission gate: establishes, or exact reason
[ ] D-3 can the Conductor be backed by a local model — ANSWERED
[ ] D-4 H-10 redundancy measured, not reasoned
[ ] D-5 selection-to-prompt path traced
[ ] F-1 local models selectable, 8B ceiling enforced with stated reasons
[ ] F-2 SOW terminal reaches a live local prompt
[ ] F-3 Conductor launches and accepts typing (or PARKED with cost)
[ ] F-4 Debate runs a real local multi-model exchange
[ ] F-5 SOVEREIGN offers the local library under the ceiling
[ ] F-6 SOW receipts out of the tracked tree
[ ] F-7 build_release cuts all eight
[ ] F-8 provenance cross-hash GREEN
[ ] G suites measured
[ ] G all six gates measured, provenance GREEN
[ ] G end-to-end local demonstration captured
[ ] H seal, artifacts after the seal, cold-extract verified
[ ] H review package, notes, report with LIVE-VERIFICATION REQUEST
```

---

## §8 · EXCEPTION CLASSES
E-1 money · E-2 irreversibility · E-3 spec contradiction · E-4 security regression · E-5 physical
dependency · E-6 queue exhausted. Every park gets a dossier naming the blocker and what unblocks it.

## §9 · OUT OF SCOPE
`live_operation.json` and all frontier providers (operator ruled DENY) · any model above 8B · model
pulls of any size · `e2e8e55` and the assertion inversion (S-13, S-14) · N-19 BUILD-MANIFEST
staleness (OD-28, reviewer) · the SOW and Distillery dependency locks (S-11) · the 19 CANDIDATE
gates and Gate-5/5b (reviewer) · promotion to 1.0 (operator alone).

End every report with:
`BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`

Execute BOOT now. Run D → F → G → H to convergence, then stop.

---END PASTE---

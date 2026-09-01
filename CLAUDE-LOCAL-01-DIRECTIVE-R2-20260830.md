# LOCAL-01 — FULL WORK DIRECTIVE · REVISION R2
## Builder seat: `claude-code (opus-5)` · SWS-REM-DIR-20260828 R2 + Annex A · ENTRY 017 + 018
## Parent seal `8d9f5d2415599eeb0b71dc85a25a36469b41641e`
## Supersedes CLAUDE-LOCAL-01-DIRECTIVE-20260830.md (R1), which treated Conductor agnosticism as an open question. The operator has ruled it is not.
## Objective, in the operator's words: **"use the local models to get this entire system running... prove this system works."**

---BEGIN PASTE---

You are the BUILDER seat `claude-code (opus-5)`, executing LOCAL-01 R2 under SWS-REM-DIR-20260828 R2
and Annex A, authorized by OPERATOR-INSTRUCTIONS.log ENTRIES 017 and 018.

Run-to-convergence: park-and-continue per E-1..E-6; exit only when §7 has no blank row. CANDIDATE
only. You never write `status: PASS`; you never promote.

## The objective is different from every run before it

Prior loops hardened release machinery. **This one makes the product work.** The operator has never
had a session where he could select a model and talk to it. That is the deliverable. A green suite
that does not end in a working conversation with a local model is a failed run.

## Three operator rulings that set the whole posture

**1 · LOCAL ONLY, NO FRONTIER (OD-31 → DENY for now).**
`modules/sow/config/live_operation.json` **stays absent**. You do not create, populate, stub, mock,
or route around it. Frontier providers remain correctly DENIED and their rows must keep saying so
honestly. Local models need none of it — FIXUP-01 Phase D proved local enumeration works with that
file absent. This is not a deferral; it is a decision to prove the system on local models first.

**2 · 8B PARAMETER CEILING, HARD.**
No model above 8 billion parameters may be used, selected, defaulted to, or pulled. The operator's
card is 8 GB and a 12 GB model previously straddled it at a 51/49 CPU/GPU split. A larger model
already installed must be **excluded from selection with a stated reason**, not silently hidden. You
pull no models at all, of any size (S-10).

**3 · THE CONDUCTOR IS AGNOSTIC. THIS IS DESIGN INTENT, NOT A PREFERENCE.**
Operator, verbatim: *"the conductor seat is agnostic. It needs to have the local library anyways.
It's not frontier only. That would defeat the whole purpose of the system."*

R1 of this directive asked whether the Conductor *could* be backed by a local model and allowed a
park if it turned out to be frontier-only by construction. **That framing is withdrawn.** If the
Conductor is frontier-only today, that is a **defect to be corrected in this run**, not a finding to
report and set aside. The measurement in D-3 exists to scope the work, never to decide whether the
work happens.

Conductor behaviour, ruled (option C): it **launches**, it **accepts the operator's typing**, it
offers **the local library** under the 8B ceiling, and it **spawns nothing** until the operator
selects a model and sends something.

---

## §1 · STANDING RULES (S-1..S-20)

S-1 write set: `release-worktree` and `release-planning\bundles\LOCAL01` only.
S-2 encoding: never PowerShell redirection or `Set-Content` without `-Encoding utf8NoBOM`; assert no
tracked file begins `FF FE` after each commit.
S-3 no invented APIs — read the source before calling it.
S-4 coupled values move together, in one commit, each site listed in NOTES.
S-5 suite side effects restored byte-exactly and hash-verified; porcelain empty after. **Select
restore targets from an explicit closed list, never from `git diff --name-only`** — that mistake
reverted a builder's own work in FIXUP-01.
S-6 no test deleted, skipped, xfailed or loosened. S-7 minimal diff.
S-8 one item, one commit, one bundle, fail-before and pass-after.
S-9 park, do not improvise. S-10 no billed calls, no provider calls, **no model downloads or pulls**.
S-11 locks are read-only inputs. S-12 a red suite is not an authorization.
S-13 held items stay held. S-14 no assertion edits without authority.
S-15 no manifest may carry a `path`+`sha256` pair its checker does not verify.
S-16 no developer-machine identifiers in distributed bytes unless overwritten at install and proven.
S-17 **a rendered control must be a performable action** — no item closes on a green test alone.
S-18 **fail-closed interlocks are operator territory.** `live_operation.json` stays absent. The
narrow, operator-ruled H-10 exception is in F-3 and nowhere else.
S-19 **honest degradation.** Every state a component can be in must be visible and explained where
the operator looks. No silent absence, no blank panel, no enabled control that does nothing, no
model offered that cannot run. If something cannot work, the UI names which thing and why.
S-20 **MODEL AGNOSTICISM (NEW — the operator's design principle).** Any component that selects or
runs a model offers **the whole library**: every local model within the ceiling, and every frontier
provider the operator has authorized — today, none. A component that can only reach one class of
model is defective, not limited. Where frontier is unauthorized, its entries are shown and honestly
marked unavailable (S-19); they are never removed, and local is never gated behind a frontier
switch. This rule outlives this run and applies to every future component.

---

## §2 · BOOT

1. HEAD == `8d9f5d2415599eeb0b71dc85a25a36469b41641e`
2. `git status --porcelain` — **expect one untracked file**,
   `modules/sow/docs/evidence/receipts/SHELL-LIVE-READY.json` (N-29, ruled OD-34, fixed here as
   F-6). Capture the exit status; empty output from a killed command is not a clean tree.
3. ENTRIES 001–018 present.
4. Read `PUNCH-LIST-V5-20260830.md`, `VALIDATOR-AUDIT-FIXUP01-SEAL-20260830.md`, and
   `bundles\FIXUP01\D\DIAGNOSIS.md` — that diagnosis refuted four validator findings by measurement
   and its method is the standard for this run.
5. Set seat identity before the first commit; rewrite no existing commit.

---

## §3 · PHASE D — measure the ground. Scope the work; do not decide whether it happens.

Record VERIFIED / REFUTED / PARTIAL with commands and output in `bundles\LOCAL01\D\DIAGNOSIS.md`.

- **D-1 · The local library.** Start the Ollama daemon if it is not running — local service, no
  spend, operator-authorized. Enumerate every installed model with parameter count and on-disk size.
  **Name every model over 8B.** Pull nothing.
- **D-2 · The VRAM admission gate.** `worker_pane_spawn` refuses every local pane when the host's
  VRAM budget cannot be established (`control_plane/nodes/pane_picker.py`). Determine whether that
  budget establishes with Ollama up, and if not, exactly why. **This gate, not the frontier switch,
  is what decides whether local panes run at all.**
- **D-3 · The Conductor's model path — scoping only.** `main.js:2941` describes pane 1 as running
  "the real interactive `claude` session on launch." Read `adapters/conductor/`,
  `apps/desktop/conductor/`, the conductor-admission path and the conductor selection feed, and
  report **precisely what stands between the Conductor and a local Ollama session**: which module
  hardcodes the provider, whether the admission path admits `ollama_local`, whether the selection
  feed can carry a local model, and what a local-backed conductor session needs that does not exist.
  **This is a scoping measurement. Per ruling 3, the answer never authorizes skipping F-3.**
- **D-4 · Is H-10 redundant today?** With `live_operation.json` absent the governed conductor spawn
  passes through the live gate and should fail closed before anything is created. Confirm or refute
  by measurement: with `SOW_CONDUCTOR_AUTOLAUNCH` unset, does any billable session actually spawn?
- **D-5 · Selection to prompt.** Trace end to end what happens when a local model is selected in the
  picker and a pane is started.
- **D-6 · Baselines.** `git archive HEAD` file count; archive-bound boundary gate invoked as
  `package_boundary_gate.py --root <extract> --allowlist <extract>/tools/release/fixture_allowlist.json`;
  suites: SOW desktop 1104, terminal 225, Token Center 32, shell 188, tools/release 84.

---

## §4 · PHASE F — make it work. Strict order; F-3 depends on F-1.

### F-1 · Local models selectable and runnable, 8B ceiling enforced
Scoped by D-1/D-2/D-5. The picker offers only models at or under 8B; anything larger is excluded
**with a stated reason** (S-19). If the VRAM budget cannot establish, the picker says so in words
the operator can act on. Selecting a local model and starting a pane reaches a live prompt.
**This is the foundation the Conductor stands on — it lands first for that reason.**

### F-2 · SOW terminals run local models
The operator opens a terminal, picks a local model, types, and gets a response. Demonstrate it.

### F-3 · THE CONDUCTOR IS AGNOSTIC — the centre of this run
Ruled by the operator (ENTRY 018). Required, not optional.

1. **The Conductor offers the local library**, under the same 8B ceiling and through the same
   enumeration F-1 established. Whatever D-3 identified as hardcoding the provider is corrected.
2. **It launches and accepts typing** (option C), and **spawns nothing** until a model is selected
   and a message sent. That deferred spawn is the quota protection replacing H-10; the H-10
   exception extends this far and no further.
3. **Frontier entries remain visible and honestly unavailable** (S-19, S-20). You do not remove
   them, and you do not touch `live_operation.json` — the live gate keeps refusing frontier on its
   own, and you rely on that refusal rather than removing it.
4. **Acceptance:** the operator selects a local model in the Conductor, types, and receives a
   response from that model. Demonstrate it, or write the LIVE-VERIFICATION REQUEST that lets the
   validator demonstrate it.

**On parking.** F-3 may be parked **only** for a blocker outside the write set or a physical
dependency (E-5) — never because the Conductor is "frontier-only by construction." That is the
defect being corrected. If the work proves larger than one commit, land it in several, ordered so
each is coherent, and report the size honestly rather than narrowing the goal.

### F-4 · Debate Table on local models
Debate opens but reaches no models. With Ollama up and the ceiling enforced, Debate runs a real
multi-model exchange on local seats. Confirm which model each seat loads. S-20 applies: frontier
seats stay visible and honestly unavailable.

### F-5 · SOVEREIGN on local models
SOVEREIGN starts since FIXUP-01 F-4. Its model list offers the local library under the same ceiling,
sourced from the same enumeration rather than a second one. S-20 applies.

### F-6 · OD-34 — SOW receipts leave the tracked tree
`modules/sow/docs/evidence/receipts/` is git-tracked and declared in `sow.json` `runtime_writes`, so
using the product dirties the release candidate. Relocate runtime receipts to a gitignored path with
a boundary-gate assertion, exactly as N-16 did for the shell. Existing tracked receipts are history:
leave them, stop writing new ones there.

### F-7 · OD-33 — `build_release.ps1` cuts, it does not enumerate
It `git archive`s only the two composite archives, then records whatever `*.zip` sits in the output
directory as authoritative — six stale per-module archives nearly shipped under correct-looking
hashes. Cut all eight from the seal. A validator-authored defect; fix it properly.

### F-8 · OD-27 — refresh `module_source_registry.json`
Refresh debate, distillery and sow to the seal's provenance identities and fill tokencenter's
PENDING entry from its own record. `provenance_cross_hash_check` must go GREEN. It has been red
since CONVERGE-01 and is the last gate blocking release.

---

## §5 · PHASE G — measure at final bytes

All suites with measured counts; all six gates, **including provenance cross-hash, now GREEN**
(F-8). Archive delta accounted item by item. S-5 restoration hash-verified from a closed list.
S-12 governs.

**The acceptance that matters (S-17):** a captured end-to-end demonstration — Ollama up, a local
model at or under 8B selected, a SOW terminal reaching a live prompt, **the Conductor answering a
typed message from a local model**, a real Debate exchange, and a clean stop with no orphaned
processes. Screenshots or transcripts. Any leg you cannot drive from your seat becomes a precise
LIVE-VERIFICATION REQUEST naming exactly what must be observed and by whom.

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
[ ] D-3 Conductor model path scoped — what stands between it and a local session
[ ] D-4 H-10 redundancy measured, not reasoned
[ ] D-5 selection-to-prompt path traced
[ ] F-1 local models selectable, 8B ceiling enforced with stated reasons
[ ] F-2 SOW terminal reaches a live local prompt
[ ] F-3 Conductor offers the local library
[ ] F-3 Conductor launches and accepts typing, spawning nothing until sent
[ ] F-3 Conductor answers a typed message from a local model
[ ] F-3 frontier entries visible and honestly unavailable
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
dependency · E-6 queue exhausted. Every park gets a dossier naming the exact blocker and what would
unblock it. **F-3 is not parkable on construction grounds — see §4 F-3.**

## §9 · OUT OF SCOPE
`live_operation.json` and frontier authorization (operator ruled DENY) · any model above 8B · model
pulls of any size · `e2e8e55` and the assertion inversion (S-13, S-14) · N-19 BUILD-MANIFEST
staleness (OD-28, reviewer) · the SOW and Distillery dependency locks (S-11) · the 19 CANDIDATE
gates and Gate-5/5b (reviewer) · promotion to 1.0 (operator alone).

End every report with:
`BUILDER CLAIM: no gate submitted for reviewer evaluation; no PASS asserted.`

Execute BOOT now. Run D → F → G → H to convergence, then stop.

---END PASTE---

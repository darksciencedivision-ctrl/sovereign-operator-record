# EPC-04 — OpenCode as a pickable terminal slot

**Authority:** operator instruction, 2026-09-01. **Repository:** `release-worktree`, branch `main`.
**Seal at start:** `570a642`. **Mode:** surgical. One goal, one gate, stop at the gate.

---

## 0. The operator's instruction

> *"I want open code, the harness, to be one of the model selections in the multi model app so I
> can use it from the multi model app. The conductor can conduct it if needed, but I want open
> code, the harness, an available slot to be picked in one of the terminals, and then I'll load a
> model into it."*

---

## 1. THE GOAL, stated so it can be failed

**The operator opens a pane, picks OpenCode from the picker, chooses a local model, and gets a
working OpenCode session in that terminal — governed like every other pane.**

Done when all five hold, measured on the operator's host:

1. The picker offers OpenCode as a selectable option, with the local models it can drive.
2. Choosing it spawns a pane that registers as a Sovereign node — `class`, `locality: local`,
   a residency reservation, no subscription.
3. The pane reaches `READY` with an attested pid, like every other worker pane.
4. The session is confined to its own git worktree. The trunk working tree is unmodified after.
5. `worker_pane_spawn` no longer refuses the local coding role with U95.

**Not in scope, and not to be attempted:** the conductor driving OpenCode. The operator said "can
conduct it if needed" — that is permission for later, not this loop's goal. A pane he can pick and
use is the whole deliverable.

---

## 2. What already exists — measured 2026-09-01, before any code

This is not a build. It is a connection, and most of it is already written.

| Asset | State |
|---|---|
| `adapters/coding/opencode/harness.py` (312 lines) | `OpenCodeCliHarness`, `MockOpenCodeHarness`, `HarnessProbe`, `coding_capability_descriptors` |
| `adapters/coding/opencode/driver.py` (384 lines) | worktree-isolated drive, `git status` read-back INSIDE the worktree, trunk checked for leakage |
| `adapters/coding/opencode/candidate.py` (311 lines) | candidate publication |
| `opencode` binary | **installed**: `C:\Users\Sslaw\AppData\Roaming\npm\opencode` |
| Picker roles | `_LOCAL_ROLES = ("reasoning", "coding")` — the coding role is ALREADY in the menu |
| Local-model pin | `_require_local_model` + `_is_loopback_ollama_host` already enforce it in the harness |

**The blocker is one deliberate refusal**, `worker_pane_spawn.py:668`:

> *"a local CODING pane is the supervised OpenCode-harness path (worktree-isolated, Phase 10 /
> 14C), not a bare `ollama run` session — deferred, not unavailable (U95)"*

The picker already offers the role. The spawn path refuses it, on purpose, with the route named.
EPC-04 walks that named route.

---

## 3. THE ONE ARCHITECTURAL FACT THAT SHAPES THIS

**The existing harness is ONE-SHOT, not interactive.** `_opencode_run_argv` builds
`opencode run [--pure] [--auto] --dir <worktree> -m <model> --format json <prompt>` — a prompt in,
JSON out. Every other worker pane is an interactive ConPTY the operator types into.

So "a slot I can pick and load a model into" is **not** satisfied by calling the existing harness.
A decision is required, and it is the first work item, not an assumption:

- **Option A — interactive pane.** Spawn `opencode` in TUI mode in a ConPTY, exactly like
  `ollama run`. The operator types into it. Closest to what he asked for; the existing
  one-shot harness stays for the driver path and is NOT the thing in the pane.
- **Option B — one-shot behind a pane.** The pane is a surface over `opencode run` calls. Reuses
  everything; is not a terminal the operator can converse in.

**Measure before choosing.** `opencode --help` on this host decides whether a TUI mode exists and
what invokes it. If Option A is available, take it — it is what was asked for. If it is not,
Option B is delivered WITH a plain statement that it is not an interactive terminal, and the
operator rules.

---

## 4. Work items

| # | Item | Done when |
|---|---|---|
| **W-1** | Measure `opencode` on this host: version, TUI vs run-only, model selection flags | Option A or B chosen ON EVIDENCE and recorded |
| **W-2** | Picker: OpenCode appears as a selectable option with its local models | Visible in the picker feed; enumerated, not globbed |
| **W-3** | Worktree provisioning for a coding pane | Each coding pane gets its OWN worktree; two panes never share |
| **W-4** | Lift U95 in `_authorize_local` — route the coding role to the harness path | The refusal is replaced by an authorization, not deleted |
| **W-5** | Node registration for the coding pane | `class` non-null, `locality: local`, residency reservation, `subscription: null` |
| **W-6** | Live proof on the operator's host | Pane picked, model loaded, session reaches READY, trunk unmodified |

---

## 5. Untouchable

Unchanged from EPC-03 §3, plus two specific to this work:

- The honesty guard, the spend wall, the ceiling advisory, append-only ordering,
  `_assert_legs_honest`, path containment.
- **The worktree containment.** A coding pane is a model with WRITE HANDS. The refusal being
  lifted exists because "a coding role without a worktree would be a model with write hands and no
  containment." If containment cannot be provided, the refusal STAYS and the item parks. This is
  the one place where shipping the feature is worse than not shipping it.
- **The local-model pin.** `_require_local_model` and the loopback-Ollama check are why an
  OpenCode pane spends nothing. Neither is relaxed, and no frontier model becomes selectable here.

---

## 6. Method

1. **Measure first.** W-1 is a measurement item precisely because this programme has twice shipped
   a constant nobody had run (`0.5 MiB/1k` for the KV cache, `1024` tokens for a thinking model).
2. **Test against the real shape.** The node-log reader passed nine tests written against an
   invented log format while being wrong about the real one. Fixtures for W-5 come from the shape
   the product actually writes.
3. **Mutation-prove the containment.** W-3 is a security property. A guard that cannot fail proves
   nothing — show the trunk-leak check catching a write outside the worktree.
4. **Park honestly.** Anything that cannot be done without weakening §5 parks with a dossier.

---

## 7. Stop condition

**Stop at W-6.** Do not continue into conductor-driven OpenCode, and do not open a second front.
Report once, with the live measurement, and wait.

The standing items from punch list V8 are NOT part of this loop and do not get picked up in
passing: L5-5 is the operator's decision, the clean-room install is his to run, and 19 of 29 gates
remain unevaluated — the number this loop does not move.

---

BUILDER CLAIM: No gate is submitted for reviewer evaluation. No PASS status is asserted by the builder.

# EPC-03 — Conductor Orchestration Completion Loop

**Authority:** operator instruction, 2026-08-31, recorded verbatim as ENTRY 036.
**Scope:** Layers 3, 4, 5, 6 of the conductor capability.
**Repository:** `D:\producttion software 2\release-worktree`, branch `main`.
**Seal at start:** `45cfc92`.
**Mode:** continuous. The loop does not stop to ask. It reports at C.4.

---

## 0. The operator's instruction

> *"Give me a directive loop, same concept that we have been working off of in the past loops.
> I should not be asked for anything. It should run through the entire workload full
> permissions. I want it to be for layer three four five and six."*

The goal it serves, in the operator's own earlier words:

> *"the conductor is absolutely supposed to be talking to those models and communicating with
> them, injecting their prompts, telling them what to do. I should be able to tell the
> conductor to open up two more terminals, and then it automatically open them up… it's gotta
> be able to communicate with those extra workers."*

---

## 1. What "no asking" means, and what it cannot mean

The builder will not return for a decision. Every choice that would otherwise block is
pre-resolved in §2. Where something genuinely cannot proceed, the loop **parks it with a
written dossier and moves to the next item** — it never idles waiting for an answer.

Three things this directive **cannot** grant, because they are not the operator's to delegate
away in a sentence, and a loop that assumed otherwise would produce work that has to be
thrown out:

1. **Provider spend.** No frontier call, ever, under this loop. `live_operation.json` stays
   absent. This is not caution — a background loop that can spend money is a different kind of
   object entirely.
2. **A reviewer PASS.** The builder never writes one. Gate status stays `CANDIDATE`.
3. **Weakening a guard to make a result look green.** Named individually in §3.

Everything else is authorized.

---

## 2. Standing decisions — pre-answered so the loop never blocks

**D-1. Local panes ARE Sovereign nodes.** `test_a_local_pane_holds_no_terminal_and_gets_no_node_record`
asserts the opposite contract and is superseded. Invariant 2 says *every terminal is a
Sovereign node*; U313 says the two-provider wiring was the gap, not the design. **Invert that
test** to assert the new contract (local registers, no lease, `local_only`, no subscription),
and keep its `claude_code`/`codex` sibling asserting no-record — those remain genuinely
unwired.

**D-2. The conductor's local brain is `qwen3:8b`.** The only model at or under 8B on this host
reporting both `tools` and `thinking`. Workers are the four ≤4B models reporting `tools`:
`phi4-mini:3.8b`, `llama3.2:3b`, `qwen2.5:3b-instruct`, `qwen2.5-coder:3b-instruct`. The other
six ≤4B models are completion-only and are **not** delegation targets.

**D-3. A model may trigger a governed spawn, through the existing path only.** Layer 6 wires
the conductor to the *same* `emit_worker_launch` chain the operator's click uses — every gate,
the residency fence, the node record. It never gets a private spawn route. If a gate refuses,
the conductor is refused, and that refusal is surfaced rather than retried.

**D-4. Observation is the raw pane stream, bounded.** Per the operator's own framing — the
conductor should see what he sees. `LogRing` already strips ANSI, control characters and
redacts; Layer 4 adds a bounded window over that, never a second unredacted path.

**D-5. Presence keeps refusing `SPAWNING`.** Attestation stays a separate act with a live pid
behind it. If a pane never attests, presence says so; the loop does not lower the bar to make
a pane appear.

**D-6. Parking is always available.** Any item that cannot be completed honestly is parked with
a dossier naming the blocker and bounded options. Parking is a legitimate outcome; faking is
not.

---

## 3. Untouched, absolutely

- The honesty guard that rejects uncited answers.
- The spend wall (`live_operation.json` absence, the remote-execution refusal, the
  `_MIN_LOCAL_WEIGHTS_BYTES` check).
- The model-ceiling **advisory** — it recommends, it never selects; the operator pins the slate.
- Append-only log ordering, and the single-opener lock on `AppendOnlyEventLog`.
- `_assert_legs_honest` / `build_acceptance_packet` — a live worker leg stays unrepresentable
  without its own evidence record.
- Path containment (H-5), loopback-only binding, credential scrubbing.

A test may only be changed when the **contract** changed (D-1 is the sole pre-authorized case).
Changing a test because the code fails it is forbidden and is a parking condition.

---

## 4. The layers

### Layer 3 — the conductor knows which panes are up

| | Work | Done when |
|---|---|---|
| **L3-1** | Land the local-branch registration patch; apply D-1 to the superseded test | Suites green; local ticket registers |
| **L3-2** | Verify against a **real** pane opened in the app: `node_events.jsonl` in the real store, `seq=1`, `class` non-null, `locality: local` | A durable row exists that is not a pytest fixture |
| **L3-3** | Confirm `attest_spawned` fires on the existing path once a record exists; do not add a call, do not invent a pid | State leaves `SPAWNING` with a real pid |
| **L3-4** | Presence names the pane: `panes_live` carries it | Measured from the live registry |
| **L3-5** | Dispatch enumerates registered panes instead of `DEFAULT_WORKER_IDS` | `dispatched_to_live_panes: true` |

**L3-5 is the first point the operator's UI changes** — the banner should stop saying
`0 worker pane(s)`.

### Layer 4 — the conductor reads what the workers are doing

| | Work | Done when |
|---|---|---|
| **L4-1** | A bounded read API over each pane's `LogRing`: recent lines, size-capped, already redacted | Unit-tested against a fake ring |
| **L4-2** | Prove redaction survives the new path — a secret the ring hides must not appear in the window | Mutation-proven |
| **L4-3** | Expose the window to the conductor adapter as evidence, not as a leg | No `legs` field; presence-style honesty |
| **L4-4** | Bound it for an 8B context: the window must fit inside the derived `num_ctx` with room for the prompt | Measured, not assumed |

### Layer 5 — the conductor delegates and reads the result back

This is U58's outstanding half. The largest item.

| | Work | Done when |
|---|---|---|
| **L5-1** | Wire `OllamaConductorBackend` into a local conductor spawn path beside `spawn_claude_code_conductor` — same gates, same governor, no credential | A local conductor is constructible without the `claude` CLI |
| **L5-2** | The conductor selects a **registered live pane** as a delegate, by descriptor | Assignment names a real pane id |
| **L5-3** | Inject a prompt into that pane's ConPTY through the supervised path | The pane receives it; observable in its own output |
| **L5-4** | Read the result back and publish it as a CANDIDATE over MCP | `worker_evidence` names the pane |
| **L5-5** | `legs.workers` may become `live` **only** through the existing evidence derivation | `_assert_legs_honest` unchanged and still satisfied |

**L5-5 is the honesty line of this whole loop.** A live worker leg is derived from evidence by
the packet builder. If it cannot be derived, the leg stays `mock` and the loop says so.

### Layer 6 — "open two more terminals"

| | Work | Done when |
|---|---|---|
| **L6-1** | A spawn tool the conductor can call, routed through `emit_worker_launch` with every gate intact (D-3) | A refused gate refuses the conductor |
| **L6-2** | Tool-calling wired for `qwen3:8b` so the conductor can actually invoke it | A conductor turn can emit a tool call |
| **L6-3** | Operator-visible: every model-initiated spawn appears on the Approvals surface | Nothing spawns invisibly |
| **L6-4** | Bound it: a maximum concurrent pane count derived from measured VRAM, refusing beyond it with a stated reason | Measured from `nvidia-smi`, not hardcoded |

**L6-4 matters more than it looks.** Four 8B models cannot be co-resident in 8 GB; a conductor
that can spawn without a bound will thrash the card. The bound is derived, and it advises the
operator rather than silently capping him.

---

## 5. Method

Every item follows the loop discipline already in use:

1. **Criteria before code.** State what "done" means and how it will be measured.
2. **Measure the current state** before changing it. No fix rests on an assumption.
3. **Fix the class, not the instance.** Four containment sites and two registration gates were
   each found one at a time; when a defect has siblings, sweep them together.
4. **Mutation-prove every guard.** A test that cannot fail proves nothing.
5. **Run against the operator's live system** where the app is involved. The last stretch found
   five defects that no amount of reading would have surfaced.
6. **Commit with the reasoning**, including what was tried and rejected.

---

## 6. Reporting

The loop reports **once**, at C.4, unless it parks something the operator must decide. Interim
narration is not required and is not wanted.

The C.4 report states, per layer: what was built, what was measured, what was parked and why,
and what the operator's UI now does that it did not before.

---

## 7. Closing phase — R: the release is re-cut

Not in the operator's list, and included because the work is not delivered until it ships.

Thirteen commits have landed since the release artifacts were cut. Phase R re-runs the five
release gates, re-cuts the artifacts from the sealed commit, re-runs the CI lane, and updates
`RELEASE-ASSURANCE.md` with the measured figures — so that whatever Layers 3–6 achieve is in a
shippable state rather than only in the worktree.

---

BUILDER CLAIM: No gate is submitted for reviewer evaluation. No PASS status is asserted by the builder.

# EPC-02 — Orchestration Capability Directive

**Supersedes nothing.** EPC-01 is complete and closed; this is the next body of work.
**Authority:** operator instructions of 2026-08-30/31, recorded verbatim as ENTRY 030 in
`release-planning/OPERATOR-INSTRUCTIONS.log`.
**Repository:** `D:\producttion software 2\release-worktree`, branch `main`.
**Status:** **ACTIVE.** Two of the three §6 decisions are given (ENTRY 031): E-5 is
lifted, and llama.cpp is C-2 keep-as-is. Symptom detail was not supplied, so Items A
and B begin by reproducing from a blank start.

---

## 1. Why this directive exists

EPC-01 asked whether the workspace could be *shipped*. It answered that. This asks a
different question: whether the workspace can *do the thing it was built to do* — run a
conductor that opens, directs and closes other terminals, and let the operator put any model
from their library into any role.

Three items, one theme: **the orchestration surfaces are built but not proven.**

---

## 2. Item A — Conductor: full multi-terminal verification

**Operator statement:** *"the conductor can actually open up other terminals, close them,
speak to the other terminals, communicate, direct them as the conductor. All that needs to be
tested and verified… it doesn't seem to be working that well right now."*

### What already exists — do not rebuild it

Thirteen selfchecks already map onto the described capability:

| Capability | Existing check |
|---|---|
| open a terminal | `conductor-spawn-selfcheck.js`, `conductor-launch-selfcheck.js` |
| speak to it | `conductor-pty-selfcheck.js`, `pane-io-selfcheck.js` |
| direct it | `conductor-dispatch-selfcheck.js` |
| two-way communication | `conductor-roundtrip-selfcheck.js` |
| collaborate across workers | `orchestration-collaboration-selfcheck.js` |
| close / recover | `recovery-selfcheck.js`, `pane-guards-selfcheck.js` |
| identity and admission | `conductor-descriptor-selfcheck.js`, `conductor-admission.test.js` |

The work is to **run them live and record what actually happens**, not to write new ones
until a gap is proven.

### A-1 — ANSWERED 2026-08-31. The hypothesis was wrong; the real cause is worse.

**My hypothesis was that tool-calling was the limit. It is not.** Measured:

- `adapters/base/backend.py:100` — `OllamaBackend.generate()` posts to `/api/generate` with
  `{model, prompt, stream, options}`. **No `tools` array. Not `/api/chat`.** Plain text
  completion.
- Repo-wide search for a `tools` array sent to a model: **no such call path exists.**

So all eight ceiling-admitted models are equally capable of driving the conductor as far as
the protocol is concerned. The `tools` capability is irrelevant here. That part of this
directive's first draft was speculation dressed as analysis, and it is corrected rather than
quietly dropped.

**What is actually wrong: the live conductor has no local backend at all.**

| Evidence | What it shows |
|---|---|
| `adapters/conductor/adapter.py:42` | `self._backend = backend or MockReasoningBackend()` — with nothing injected, the conductor reasons against a deterministic **mock** |
| `node_runtime/supervisor/conductor_spawn.py:104` | raises `ClaudeCliUnavailable` — *"`claude` CLI not detected on host PATH — cannot spawn a live conductor (fail closed)"* |
| `conductor_spawn.py:118` | `worker_backend = backend or ClaudeCliBackend(model=resolved_model)`, wrapped in `ClaudeCodeConductorBackend` |
| `conductor_pane_spawn.py:290` | `spawn_conductor_pane(..., executable: str = "claude")` |
| Spawn entry points in that module | exactly two, and **both** are Claude-CLI paths |

**The live conductor is hardwired to the `claude` CLI — a frontier provider.** There is no
spawn path that backs a conductor with a local Ollama model.

This is not a small gap. `adapters/conductor/adapter.py` opens by declaring the opposite:

> *"The conductor is an INTERFACE; the current runtime selection (today: fable-5) backs it but
> the architecture knows only this contract (I-CN1)."*

And `control_plane/conductor/registry.py` already **derives local conductor seats** from the
same ceiling verdicts the picker uses — so the system *offers* the operator a local conductor
seat that no code path can then spawn.

**Why this presents as "it doesn't seem to be working that well":** running local-only, with
`live_operation.json` deliberately absent and no frontier authorization, the conductor either
fails closed at the CLI check or silently runs on `MockReasoningBackend`. Neither is a
conductor that conducts. Nothing is broken in the sense of a defect to patch — the capability
was never built for local models.

**Consequence for this directive.** A-2 as originally written (run the selfchecks live under
`qwen3:8b`) cannot pass, and running it first would only produce a list of failures whose
single shared cause is already known. Item A is re-planned below.

### Phases

| | Work | Output | State |
|---|---|---|---|
| **A-1** | Determine what the conductor's live dispatch actually requires | The finding above | **DONE** |
| **A-2** | Establish the honest baseline: run the thirteen selfchecks and record how each behaves with no frontier CLI present. Expected to be dominated by the A-1 cause; recorded anyway, because "expected to fail" is not evidence. | A live baseline | next |
| **A-3** | ~~Decision point~~ **DECIDED: A-3a**, build a local conductor backend (ENTRY 032) | An operator decision | **DONE** |
| **A-4** | If authorized: implement `OllamaConductorBackend` behind the existing `I-CN1` interface, and a local spawn path beside `spawn_claude_code_conductor` — same gates, same governor, same fail-closed discipline, no credential | Commits + tests | AUTHORIZED, queued behind Item B |
| **A-5** | Re-run the thirteen selfchecks against a local conductor; record the delta against A-2 | Closing evidence | after A-4 |

### Constraints that do not move

- **Zero provider spend.** Local models only. `live_operation.json` stays absent. The
  test-suite live-call guard stays armed.
- **Launch only via the ADR-004 selfcheck path**, with `SOW_CONDUCTOR_AUTOLAUNCH=0` and the
  `SHELL_SELFCHECK` value verified present in the compiled environment immediately before
  spawn. Absent or unverifiable → STOP. Never the normal operator/recovery path.
- **The frontier adapters stay configured.** The operator's design intent is an agnostic,
  scalable roster. Nothing in this directive removes a frontier model; they are simply not
  *exercised*, because exercising them costs money.

---

## 3. Item B — SOVEREIGN: the four slots, and the ceiling's real scope

**Operator statement:** *"the model picker for the local models is messed up between the four
spaces we can put in the models… make sure those model pickers are working where we can use
our entire library. You're only bound by the eight billion local models when you're doing the
testing, not me."*

### The four spaces, identified

`modules/sovereign/ui/ui_shell/src/types/model.ts` —

```ts
export type ConfigurableModelRole =
  | "PRIMARY_REASONER"
  | "ADVERSARIAL_CHALLENGER"
  | "CRITIC"
  | "SYNTHESIZER";
```

### The scope correction, and why it is not a one-line change

ENTRY 017's 8B ceiling has been implemented as a **global refusal**:
`adapters/local/model_ceiling.py` sets `CEILING_NAMEPLATE_B = 8` and every selector in both
products greys out anything above it — for everyone, always. Under that implementation the
operator cannot reach **52 of their 60 installed models**.

The operator's clarification is that the ceiling binds the **builder during testing**, not
the operator's own use. That is a change to a refusal surface, so it is scoped here and put
back for approval rather than applied quietly.

**The trap, and why it turns out to be safe.** The same authority refuses `glm-5.2:cloud` and
`deepseek-v4-pro:cloud` — entries that look local, hold no weights on this disk (290 and 323
bytes) and execute **remotely**. Admitting those would be provider spend, a mandatory STOP.

Measured in `model_ceiling.py`: the cloud refusal is **check #1**, testing
`size_bytes < _MIN_LOCAL_WEIGHTS_BYTES` (64 MB). The parameter ceiling is a **separate, later
check**. Scoping or raising the parameter ceiling therefore **cannot** admit a cloud pointer.
The two concerns are already independent in the code, which is what makes this safe to do at
all.

### Phases

| | Work |
|---|---|
| **B-1** | Reproduce the picker fault with SOVEREIGN running. The operator's word for it is "messed up between the four spaces"; the builder will not invent a symptom it has not observed. |
| **B-2** | Make the ceiling **scoped** rather than global: full library for the operator, 8B for automated testing. One authority, two audiences — never two copies of the rule. |
| **B-3** | Prove the cloud pointers stay refused at every ceiling setting, by mutation: raise the ceiling to admit a 1.65T model and assert the `:cloud` entries are still refused. |
| **B-4** | Fix the four-slot assignment defect, with a regression test |
| **B-5** | Verify each of the four roles can be independently assigned any admitted model, and that the assignment survives a restart |

---

## 4. Item C — llama.cpp: explain, then decide

**Operator statement:** *"explain this bottom llama CCP option to me. Uh, it was put in
without my knowledge."*

### What it is

A **candidate** second local inference runtime — an alternative to Ollama — pinned to port
5183, shipped as the workspace's one declared *optional* adapter. It is **stopped, not
installed, and not a production default.**

The pane is honest about its own state: *"Runtime not installed:
C:/sovereign-workspace/optional-runtimes/llama.cpp/current/llama-server.exe."* That path is a
**neutral placeholder, not a real location** — no llama.cpp binary or model ships in any
release archive, so that path exists on no machine, deliberately. `install.ps1` lists
`llamacpp` in `optional_adapters` and skips it.

### On "without my knowledge"

The written record traces it to CP-02, `docs/SWS-UI-001-v1.2-ADDENDUM-02.md`, and that
document cites the operator's own directive twice:

- §47: *"The operator's §10 suggests `D:\Product Software\Runtime\llama.cpp\`"* — and then
  **declines** that path because it sits outside `Production Workspace\`, where writes are
  forbidden.
- §115: *"No gate in this band promotes llama.cpp to production default — the operator's §76
  governs, and promotion is a separate operator act after reviewing CP-02 evidence."*

So the record says it originated in an operator directive and was deliberately built as
**non-default, pending an operator promotion that never happened**. The builder is not
telling the operator what they do or do not remember — it is reporting what the artifacts
say, and the operator judges.

### The decision

| Option | Effect |
|---|---|
| **C-1 Remove it** | Delete the adapter and its pane. Simplest surface; loses a second-runtime option. |
| **C-2 Keep as-is** | Stays a stopped, uninstalled candidate. Costs nothing but a pane. |
| **C-3 Promote it** | Requires the pinned-artifact acquisition CP-02 describes, plus the Band D validation gates. Substantial work. |

**DECIDED: C-2, keep as-is** (ENTRY 031). Nothing is removed, nothing is promoted, no
artifact is acquired. Revisit after Item A.

---

## 5. What is already fixed, going in

- **`shell/static/app.js`** rendered the literal string `undefined` for Distillery open
  questions — a regression from EPC-01 P3-2, found in the running shell, fixed and committed
  (`4e6f884`). It now names the state and the variable to set.

---

## 6. Decisions required before work begins

1. ~~**E-5 lift.**~~ **GIVEN** (ENTRY 031). Read narrowly: it lifts *whether* a GUI may be
   launched. It does not touch AGENTS.md §8, which still governs *how* — SOW only via the
   ADR-004 selfcheck path with `SOW_CONDUCTOR_AUTOLAUNCH=0` and a verified `SHELL_SELFCHECK`.
2. ~~**Item C.**~~ **GIVEN** — C-2, keep as-is.
3. **Symptom detail** — not supplied. Items A and B therefore begin by reproducing from a
   blank start, and will report what is observed rather than what was expected.

Item B-2 additionally changes a refusal surface; it will be implemented only after §6.1 is
answered and the B-3 mutation proof is in place, not before.

---

---

## 7. New decision, raised by the A-1 finding

**The conductor cannot conduct with local models, and making it able to is new capability.**

The operator's stated intent is that the roster is *"agnostic and scalable"* and *"working out
of all the local library right now"*. The architecture agrees in principle — `I-CN1` says the
conductor is an interface — but only a Claude-CLI implementation of that interface exists.

| Option | Effect |
|---|---|
| **A-3a Build a local conductor backend** | An `OllamaConductorBackend` behind the same interface, plus a local spawn path with the same gates. Makes the local library genuinely usable for orchestration. The largest piece of work in this directive. |
| **A-3b Authorize frontier for conductor only** | The conductor runs on Claude Code; workers stay local. Closest to what the code already does — but it means provider spend, which is currently a mandatory STOP and would need explicit written authorization. |
| **A-3c Neither yet** | Record the gap honestly in `LIMITATIONS.md`, finish Item B, and revisit. Nothing regresses; the conductor simply stays unusable locally. |

The builder recommends **A-3a**, because it is the only option matching the operator's stated
design intent and the only one that costs nothing to run. It should be scoped before it is
started.

BUILDER CLAIM: No gate is submitted for reviewer evaluation. No PASS status is asserted by the builder.

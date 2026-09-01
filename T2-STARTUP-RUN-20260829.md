# T-2 ATTENDED STARTUP RUN — SWS-REM-DIR-20260828 R2 · Annex A touchpoint T-2
## Seat: T-2 OBSERVER (claude-code, opus-5) · operator sam PRESENT
## Candidate under observation: seal `50b47326f91c5abab8b6d5250d59b1dba96a4d46`

**Operator:** paste the one-line kickoff, then follow along. This run is different from every
loop that came before it: **you are in the room, and asking you things is allowed.**

---BEGIN PASTE---

You are the **T-2 OBSERVER** seat for the Sovereign Workspace, under SWS-REM-DIR-20260828 R2
Annex A touchpoint T-2, authorized by OPERATOR-INSTRUCTIONS.log ENTRY 013.

## What makes this run different

Every previous run was a hermetic loop where you parked instead of asking. **T-2 is the one
touchpoint where the operator is present by design.** You may ask. You may not decide. Service
launches, module starts, and anything that loads a model are OPERATOR acts under R2 §1 — the
operator has pre-authorized you to launch the shell and start modules one at a time in the
sequence below, and to ask before anything outside it.

You are **not** a builder in this run. You write no product bytes. If you find a defect, you
record it; you do not fix it. Your write set is
`release-planning\bundles\T2-20260829\` only. The worktree is READ-ONLY to you — porcelain must
be identical at the end of this run to what it was at the start, and you prove that.

## BOOT

1. `git -C "D:\producttion software 2\release-worktree" rev-parse HEAD` == `50b47326f91c5abab8b6d5250d59b1dba96a4d46`
2. `git status --porcelain` empty **with exit status 0 captured** (empty output from a killed
   command is not a clean tree)
3. Read `release-planning\VALIDATOR-AUDIT-CLOSEOUT01-SEAL-20260829.md` — the open questions
   OBS-1 and OBS-2 below come from it
4. Record the starting porcelain and a `git stash list` to prove nothing was hidden

Mismatch → report and stop. Do not proceed against an unexpected tree.

## OBSERVATION SEQUENCE

Record for every step: the exact command, what you observed, and — where it matters — a
screenshot. Never write "works." Write what happened.

### S-1 · Cold-host baseline (BEFORE anything starts)
The operator's original complaint that started this whole workline was that something ate VRAM
before a single module was opened. Establish the truth first, with nothing running:
- GPU memory in use, by process (`nvidia-smi` or equivalent — read what the host actually offers,
  do not assume a tool exists)
- Any `python`, `node`, `electron`, `ollama` process already alive, with its path
- Listening sockets on 5175, 5180, 5183, 5184, 8700, 8765
This is the reference every later measurement is compared against.

### S-2 · Shell start
Operator launches `Sovereign Workspace.bat`, or you run
`.\Start-Shell.ps1` from the worktree (default port 5180). Record: preflight output, time to
serve, whether a browser opened, and the shell's own module list.
**Re-measure VRAM here.** If it moved with no module started, that is the original complaint
reproducing and it is the single most important finding of this run.

### S-3 · OBS-1 — the port tile
Open question carried since the first startup: the shell displayed **"port 5180 — free" in red
while the shell itself was serving on 5180.** Look at that tile now. Report which of these it is:
the tile is measuring the wrong thing; it is measuring correctly but colouring the result wrong;
or it is correct and the earlier reading was misread. Screenshot it either way.

### S-4 · OP-4 — Token Center embedded (NEW, never seen live by the operator)
Token Center should render **inside** the shell, not as a click-out. Confirm: the panel shows the
live app; the frame is not blank or blocked; the numbers populate. Then confirm the security
scoping actually holds — the shell allows framing only `127.0.0.1:8765`, and Token Center allows
being framed only by `127.0.0.1:5180`. Check the browser console for CSP violations. If the panel
is empty, capture the console and network tab; a silent empty frame is the failure mode this
feature was specifically built to avoid.

### S-5 · OP-3 — eight terminals (NEW)
Open SOW and create terminals one at a time up to 8. Record what happens at 8 and what happens on
the 9th. The operator asked for "at least eight"; report the actual enforced ceiling and whether
the layout holds at 8 without overlap or clipping.

### S-6 · The Conductor
Long-standing operator complaint: **the Conductor could not be typed into.** Try to type into it.
Try to talk to it. Report exactly what you can and cannot do. If it still cannot take input, that
is a live defect and it goes in the report as one.

### S-7 · N-14 — visual parity
Compare the running UI against `release-planning\UI-VISUAL-BASELINE-SPEC-20260828.md` and
`release-planning\design\UI-TARGET-20260828.png`. Screenshot the running shell. Report each
divergence specifically — "the background image does not load", not "close enough".

### S-8 · Module starts, one at a time, operator watching
For each of **sovereign, debate, distillery, tokencenter**: start it, wait for readiness, record
time-to-ready, then read its VRAM and process footprint. **Debate is the one to watch** — N-11
re-sized its default seat to qwen3:8b after phi4:14b was found straddling an 8 GB card at a 51/49
CPU/GPU split. Confirm which model actually loads.
**Ask the operator before starting anything that will pull a model that is not already local.**

### S-9 · llamacpp — the adapter that was neutralised
CLOSEOUT-01 X-4 repointed llamacpp to `C:/sovereign-workspace/optional-runtimes/llama.cpp/...`,
which does not exist on this host. It loads (the loader does not check existence) but it will not
start. Confirm the failure is **graceful and legible** — a clear "runtime not installed" state,
not a crash, a hang, or a silent nothing.
**OBS-2:** the install-time adapter rebase used to print one `SKIPPED-WITH-RECORD llamacpp` line
and now prints none. Report whether the optional-adapter skip is still visible anywhere a human
would look, or whether that signal has gone silent.

### S-10 · Clean stop and orphan sweep — the load-bearing one
Stop each module through the UI. Then stop the shell (Ctrl+C). Then re-run the S-1 census.
**Every process the shell started must be gone.** The shell holds modules in a Windows Job Object
specifically so this holds; this is the run that proves it does. Any survivor is named, with its
PID, command line and parent.
Then: `git status --porcelain` again. It must be byte-identical to BOOT. Running the product must
not dirty the release candidate — that was defect N-16, and this is its live proof.

### S-11 · OP-1 — the shortcut (operator's own hands)
The operator runs `tools\release\install_shortcut.ps1`. You do not — writing to a Desktop or Start
Menu is outside every builder and observer write set, and always has been. Record whether the
shortcut is created, whether it launches the shell, and whether a re-run replaces rather than
duplicates it.

### S-12 · Deferred, confirm only
Distillery's UI is untouched by design (OP-6, operator-deferred). Confirm its current state in one
line. Do not open work on it.

## OUTPUT

`release-planning\bundles\T2-20260829\T2-REPORT.md`: every step with its measurement, every
screenshot referenced by filename, a defect list separating **new** defects from **already-known**
ones, and an explicit list of the residual queue items this run **closes** versus those it leaves
open. Append a T-2 section to `release-planning\bundles\LOOP-RUN-REPORT.md`.

Close with:
`T-2 OBSERVER CLAIM: observations only; no gate submitted; no PASS asserted; no product bytes written.`

Execute BOOT now.

---END PASTE---

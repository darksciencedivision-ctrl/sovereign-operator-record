# PUNCH LIST v3 — CURRENT OPEN WORK · 2026-08-28
## Bound to worktree HEAD `5a52946` · supersedes v2.1's open remainder · incorporates the Codex audit (R3), Batch-4 scope, and today's live T-2 session findings

Closed and not repeated here: the 20 loop items (CANDIDATE, 21 commits, three-seat verified), AUD-1, the first live boot + three module-launch legs (T2-SESSION-LOG). This list is only what still needs addressing, organized by who can act.

---

## LANE A — OPERATOR: decisions and one-liners (nothing else moves these)

**A-1 · Batch-4 grant + OD-22.** One line into OPERATOR-INSTRUCTIONS.log unlocks all five Codex fixes: `OPEN SWS-REM-DIR-20260828 BATCH-4: P-D2 P-D3 P-D4 P-D5 P-D6 / OD-22: annotate / UTC: <ts>` (paste ready: OPENCODE-PASTE-BATCH4). Highest-leverage single act available.
**A-2 · OD-20 ruling.** H-5 canonical-lineage decision from the prepared dossier (`bundles\BATCH2\OD-20-LINEAGE-DOSSIER.md`). Blocks H-5 implementation and, downstream, the SOW release object.
**A-3 · OD-3 MT-09 signature** (physical act). Blocks SOW gate/phase-19 tag.
**A-4 · G26 attended debug** (OD-16c) — the one NOT_RUN caused by a defect, needs you present.
**A-5 · Debate deferral scope (OD-9)** — the standing Phase-7/8 deferrals decision, now plus **N-11** below folded into it.

## LANE B — BUILDER: engineering (each needs its grant; B-1 is drafted and waiting)

**B-1 · Batch 4 (from the Codex audit — R3 adjudicated):**
- P-D2 (HIGH) process_tree.py direct-exec import regression — clean managed processes currently crash; fix + the missing clean-success integration test.
- P-D3 (HIGH) provenance/RELEASE-MANIFEST regenerated against final bytes + full gate re-run + the new standing rule (manifests always last).
- P-D4 (MED) shell BUILD-MANIFEST still authenticates the corrupted pre-repair server.py blob.
- P-D5 (LOW) BUILD-DIRECTIVE table row corrected; ADR-003/DISCOVERY annotated per OD-22.
- P-D6 (MED) worktree cache cleanup + declared archive-vs-live gate policy.

**B-2 · NEW from today's live session — N-11 (MED): Debate's default seat is phi4:14b, a 12 GB model on an 8 GB card.** Ollama split it 51/49 CPU/GPU — measured live: ~5 GB VRAM held at idle keep-alive plus CPU-bound inference. Swap the seat default to a fully-resident model (qwen3:8b is installed and fits at 100% GPU), or make the seat VRAM-aware. Config change + release-notes line ("default seats sized for ≥8 GB residency"). Fold the decision into OD-9/A-5.

**B-3 · NEW priority raise — N-13: adapter path rebase.** All seven shell adapters point at the historical `D:\Product Software` tree; today's launch proved the consequence — the shell runs remediated bytes while every module Start launches old-tree code (old SOW still on Electron 31). Already Phase-6 scope, but it now blocks something concrete: live acceptance of the remediated modules and the worktree Electron UI band. Needs a controlled install/config rebase mechanism, not hand-edits.

**B-5 · N-14 (operator-directed, 2026-08-28): UI visual baseline.** The operator designated a capture (`design/UI-TARGET-20260828.png`, the build-2026-08-21 themed look: starfield background, breadcrumb chrome, PRE-FLIGHT dependency strip, two-column module grid, LOGS panel) as the required shell UI. Full spec: `UI-VISUAL-BASELINE-SPEC-20260828.md`. Step 1 is a parity check — the theme may already be in the candidate tree (THEME-BASELINE-v3 + Gate-5b visual set); implement only the delta. Presentation-only: CSP/self-only assets unchanged, no backend changes ride along, visual regression evidence required. Needs its own scope grant (proposed: fold into the Batch-4 grant line as `+ N-14`, or a one-line Batch-5).

**B-6 · N-15 (observed live, 2026-08-28): shell pre-flight npm detection false-negative.** The target capture shows `npm [WinError 2]` while npm is demonstrably installed and working (B3-2 used it). This is the original directive's "make runtime/tool detection accurate without invoking shell wrappers" defect manifesting: the probe looks for an executable where npm is a `.cmd` shim. Fix detection (resolve `.cmd` shims safely — the new L-1 `cmd_shim.py` guard is the sanctioned path), regression test with npm present-as-shim.

**B-4 · N-12 (fold into the llama.cpp lane item): runner/eviction authority.** Today produced a live near-instance of the orphaned-runner class (5 GB held by a runner the earlier snapshot caught; `/api/ps` empty minutes later). The punch item "one eviction authority, stable VRAM admission" is not theoretical on this card. Stays parked with the llama.cpp lane until that lane's authorized work opens.

## LANE C — REVIEWER (chatgpt seat): the largest single blocker, unchanged

**C-1 · The 19 CANDIDATE gates** (7a, 7b, 8a–8j, 9c–9h, 9j) — individually evaluated. **C-2 · Gate-5 STOP vs 5b adjudication.** **C-3 · 9a/9b/9i reconstruct-or-retire proposal.** **C-4 · The 20 loop CANDIDATE items** from sealed evidence (REVIEWER-PASTE + REVIEW-PACKAGE `9acced53…` ready; package needs a refresh after Batch 4 lands). **C-5 · Batch-4 review** once built.

## LANE D — T-2 SESSION (in progress today; remaining legs)

**D-0 · NEW: SOW "Failed: EXIT" investigation.** The operator's 10:49 capture shows SOW at `Failed: EXIT` (last check 10:06:49) after this morning's successful launch — the old-tree SOW process died. Pull the shell's Logs for the SOW module while the session is live; determine crash vs. user-close vs. supervision kill; record in the T-2 log. (Old-tree Electron 31 code — informative for N-13/B-3 priority, not a candidate-tree defect.)
**D-1 · Clean-stop leg:** when you Ctrl+C the shell — verify ports 5175/8700/5180 release, SOW Electron exits via Job Object, zero orphans (llama-server included). I verify on your word. **D-2 · Service legs G53/9d/9f detail-check** against today's launches (G34/G35-class evidence banked in T2-SESSION-LOG; map remaining leg definitions to what ran). **D-3 · jsonschema venv provisioning** for SOW so the gated security legs and the full 2382-test suite can run (E-6 environment; needs network/install authority on the host). **D-4 · Live Electron UI band** — blocked on B-3 rebase to be meaningful against worktree bytes. **D-5 · Screenshots** for the Phase-8 evidence requirement (browser pane must be displayed, or OS-level capture at T-2).

## LANE E — RELEASE ENGINEERING AHEAD (directive Phases 5–11, unstarted)

**E-1** SBOM + vulnerability disposition. **E-2** LICENSE files — still zero anywhere — plus model licenses/provisioning manifest. **E-3** Clean-room install path (build/install/verify/uninstall; depends on B-3). **E-4** H-6 composed-system proof — shell suite with all modules at ratified refs from lock-reprovisioned trees. **E-5** Module release objects: Distillery v1.1.0 (OD-2 already issued; W-4/W-5 execution awaits its batch), Token Center first object, shell tag, SOW after A-2/A-3, composed-system v1.0.0 identity. **E-6** Final deterministic re-run after last byte change; re-cut baselines from tags; cold-extraction verify. **E-7** Sealed-artifact independent review; operator acceptance + promotion (T-3). **E-8** No CI anywhere — minimal scripted deterministic runner so results stop being one-human-one-host.

## LANE OP — OPERATOR FEATURE REQUESTS (ENTRY 005, 2026-08-28; Batch-5 grant recorded)

**OP-1 · Desktop launcher.** No more PowerShell typing. Interim delivered: `D:\producttion software 2\Sovereign Workspace.bat` (right-click → Send to → Desktop to pin). Proper item: installed shortcut (icon, no lingering console, Start-Menu entry) as part of the install path.
**OP-2 · SOVEREIGN frontier-model agnosticism** (ChatGPT / Claude / Grok in the model list, like SOW). **Design dossier only this batch** — SOVEREIGN is deliberately local/loopback-only today, and frontier providers mean API keys, spend (OD-5 currently DENY), and a trust-boundary change. The dossier reuses SOW's existing provider-CLI adapters as the candidate mechanism and puts the spend/key/boundary decisions in front of the operator before any code.
**OP-3 · SOW terminal cap → minimum 8 concurrent** (currently 5). Locate the pane cap, raise, regression-test layout/recovery at 8.
**OP-4 · Token Center embedded live in the shell UI** — the main-UI section shows the running app itself (loopback iframe of 127.0.0.1:8765), not a click-out. Requires scoped CSP `frame-src` on the shell + a scoped frame-ancestors exception on Token Center (M-1 hardening currently forbids framing) — loopback-only, tested, with a graceful "not running" fallback.
**OP-5 · Conductor not interactive** (can't type into / talk to it) — operator-confirmed live symptom of G26 (CONDUCTOR_LAUNCH_PROCESS_DEATH). Routed to the OD-16c attended debug at T-2; not buildable blind.
**OP-6 · Distillery UI** — operator-deferred until after the system punch list.

## LANE R — RESEARCH TRACK (post-ratification program; does not block release)

**R-IMPORT-1** · Import `research/RESEARCH-SIG-COGNITIVE-PORTFOLIO-20260828.md` (operator-supplied, 2026-08-28) into the worktree at `docs/research/` in the next authorized batch — manifests-last rule applies. **R-PROG-1** · The document's Phase I–X program (SIG/SA metrics, intelligence retention, orchestration drag, ablation, cognitive-portfolio testing) becomes the post-ratification research roadmap; Phase I (measurement definition) can reuse existing instrumentation — Token Center telemetry, Debate/SOVEREIGN evidence lanes, the gate/receipt machinery. Level-0 per its own evidence hierarchy; no architectural change authorized by it; marketing claims gated on ≥Level-2 evidence per its §43.

## LANE F — YOUR MACHINE, not release blockers

**F-1** NVIDIA Overlay: ~1.38 GB dedicated across two processes — disable in NVIDIA App → Settings → Features unless you use it; biggest free VRAM recovery available. **F-2** Optional: set `OLLAMA_KEEP_ALIVE` deliberately (short for card headroom, long for latency) so model residency is a choice, not a default.

---

## Shortest path from here
A-1 (one line) → B-1 builds → C-lane runs in parallel on the existing package → your T-2 close-out (D-1 tonight when you shut down; A-2/A-3/A-4 in one sitting) → B-3 rebase → E-3/E-4 → release set → T-3. The reviewer lane remains the critical path; everything in Lane B is days, Lane C is the queue that nothing routes around.

VALIDATOR CLAIM: no gate submitted, no PASS asserted; grants in Lane A are the operator's alone.

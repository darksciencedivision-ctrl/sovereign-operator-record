# PUNCH LIST V5 — SOVEREIGN WORKSPACE
## Compiled by validator seat claude (cowork) · 2026-08-30T01:26:43Z
## Candidate seal `50b47326f91c5abab8b6d5250d59b1dba96a4d46` · version 1.0.0-rc.1
## Supersedes PUNCH-LIST-V4-20260830.md · ordered by what unblocks the most

---

# 1 · YOUR DESK — nothing below moves without these

| # | ruling | unblocks | recommendation |
|---|---|---|---|
| **OD-31** | Authorize live model operation: create `modules/sow/config/live_operation.json` naming ONE register row — **OP-6** (Claude + Codex) or **OP-12** (those + Grok + Antigravity) — and a terminal ceiling of 1–2 | **Every model in SOW.** The picker's "0/21 available" is this switch being off | Yours alone. Spend act; collides with OD-5. A builder may write the file, but only with the row named in your ruling. A config may narrow; any widening fails closed. |
| **OD-27** | Refresh `module_source_registry.json` for debate, distillery, sow | **Promotion.** `provenance_cross_hash_check` exits 1 until then | Rule it. It declares three modules' source identity, so no other seat may. CLOSEOUT-01 fixed the reproducibility half, which is what makes it safe to take now. |
| **OD-32** | Amend or scope **H-10** so the Conductor can launch under the shell — and say what quota protection replaces it | **The Conductor.** It is dark because the shell is *required* to keep it dark | Yours alone. A builder cannot weaken a guard the adapter schema enforces. |
| **OD-5** | Spend authority for frontier providers | OD-31, OP-2, OP-7 | Currently DENY. Everything frontier is inert until this moves. |
| OD-20 | Ratify or revert the `e2e8e55` lineage import | closes H-5 | **RATIFY** — byte-identical to the rollback-of-record baseline. |
| OD-23 | Assertion inversion in `test_states.py` | reviewer lane | No recommendation. Reviewer reads the inventory first. |
| OD-28 | N-19 `shell/BUILD-MANIFEST.txt` staleness | reviewer lane | Route to reviewer — I could not verify it and report no result. |
| OD-29 | Accept the X-4 partial or revert `9110390` | closes CLOSEOUT-01 | **ACCEPT.** |
| OD-30 | X-4 step 3 — the committed interpreter placeholder | cosmetic | Not urgent; values are overwritten at install. |

**Start with OD-31 and OD-27.** One gives you a working product to evaluate; the other is the only
thing standing between this candidate and a release.

---

# 2 · BUILDER BATCH — real defects, well scoped

| id | defect | sev |
|---|---|---|
| **N-21** | **Token Center embed never renders.** Both CSP halves correct, module healthy standalone, iframe `src` correct — the frame sits at `about:blank` and never navigates. No CSP violation. The exact failure OP-4 was written to prevent: a silent empty frame with no fallback. | **HIGH** |
| **N-25** | **No local models anywhere in the SOW picker.** `grep ollama apps/desktop/*.js` → nothing. The Electron picker is frontier-only by construction, while `adapters/roster.py` already has full Ollama support. Local costs nothing, so **OD-31 does not fix this.** | **HIGH** |
| **N-22** | **llamacpp vanished from the shell** — 5 modules listed, string absent from the page. Caused by validator-authored X-4 step 4. An uninstalled optional runtime must read as *present and unavailable*, never absent. Absorbs OBS-2. | MED |
| **N-23** | **"Open" is enabled only for SOW** — the one module with `open.kind: none` — while the three that DO have URLs are disabled. Two halves: the control shouldn't render enabled, and SOW is Electron so a real fix needs a **third `open.kind`** that raises a native window. | MED |
| **N-27** | **SOVEREIGN fails its readiness probe.** venv, package and `/v1/health` (server.py:1906) all verified present — obvious causes eliminated. **Cause undetermined.** Next evidence: the Logs button on the SOVEREIGN card. | MED |
| N-21b | `sandbox="allow-scripts allow-same-origin"` is an inert sandbox. Not a vulnerability on trusted loopback, but it undercuts OP-4's security framing. Fix alongside N-21. | LOW |
| N-19 | `shell/BUILD-MANIFEST.txt` stale — 19/67 hashes wrong, 8 files omitted (builder-measured; I could not reproduce). Predates CONVERGE-01. | MED |
| N-20 L1 | `module_source_registry.json` stale → release gate red. Gated by OD-27. | MED |

**None of N-21, N-22, N-23 is catchable by any gate in this program.** All suites were and are
green. The gates validate configuration and bytes; not one asserts that a control the UI offers is
one the product can perform.

---

# 3 · NOT DEFECTS — do not send these to a builder

| id | what you see | what it actually is |
|---|---|---|
| **N-24** | Picker: `0/21 available`, every provider `live DENIED (fail-closed)` | `live_operation.json` is **deliberately absent** — enforcement-by-absence, documented in the shipped template. Your authorization switch for billed sessions. → **OD-31** |
| **N-26** | Conductor cannot be typed into (raised 3×, now root-caused) | `adapter.py:344` forces `SOW_CONDUCTOR_AUTOLAUNCH=0` (H-10 quota guard); `main.js:2942` launches the Conductor only when that is **not** 0. From the shell it is guaranteed dark, by design. → **OD-32** |
| N-28 | Debate reaches no models | **Ollama is not running** — the red item in your pre-flight band. Host state. Start it. |

"Fixing" N-24 or N-26 would damage a deliberate protection.

---

# 4 · REVIEWER LANE — has never moved, all program

- 19 CANDIDATE gates + Gate-5/5b adjudication
- All CANDIDATE work from CONVERGE-01 and CLOSEOUT-01
- OD-23 assertion inventory · OD-28 N-19 verification
- Packages ready: `REVIEW-PACKAGE-CONVERGE01.zip` `93f1a15a…` · `REVIEW-PACKAGE-CLOSEOUT01.zip` `8e6987ca…`

**After OD-27, this is the longest pole in the project.**

---

# 5 · T-2 ATTENDED — remaining

| step | item | status |
|---|---|---|
| S-10 | **Clean stop, orphan sweep, porcelain unchanged** — N-16's live proof | **NOT DONE — highest remaining value** |
| S-1/S-2 | VRAM with the shell up, zero modules, vs the 1826 MiB cold baseline | not taken |
| S-5 | Eight concurrent terminals, and the 9th | not tested |
| S-8 | Module starts one at a time; confirm Debate loads qwen3:8b not phi4:14b | blocked — Ollama down |
| S-11 | You run `install_shortcut.ps1` | your own hands |
| S-7 | Visual parity detail vs UI-TARGET | background and dark surface confirmed live |
| S-3 | OBS-1 port tile | **appears resolved** — band reads `port 5180 · shell` |

---

# 6 · ENGINEERING DEFECTS — the enterprise-production blockers

| id | defect | why it matters |
|---|---|---|
| **E-1** | SOW has only unpinned ranges (`jsonschema>=3.2`, `pytest>=7`) — no lock | Reproducible provisioning **not achieved**. `pytest.ini` 2382/0/1 remains UNVERIFIED. |
| **E-2** | Distillery has no Python lock (`lockfiles: []`) | Same. A `.venv` cannot honestly be called lock-driven. |
| **E-3** | True clean-room install never performed | Every install proof is a same-host proxy on a machine that already has Python, node and Ollama. |

**E-1 and E-2 remain the single largest barrier to an enterprise-production claim.** No loop can
close them — the rules forbid a builder inventing a lock. They need your decision on where
authoritative locks come from.

---

# 7 · FEATURE PROGRAMS — scope and cost before any code

- **OP-7 (new, yours): model agnosticism across SOW, Debate and SOVEREIGN.** One selector offering
  local models AND frontier (Grok, Claude, ChatGPT) in each. Today SOW's picker is frontier-only,
  Debate is Ollama-only, SOVEREIGN is loopback-Ollama under SYSTEM_MANIFEST authority. Overlaps
  OP-2; gated by OD-5 and key custody. **A program, not a punch-list line.**
- OP-2 frontier-provider implementation (dossier written, inert until OD-5)
- OP-6 Distillery UI · Lane R research · canonical license text · upstream Distillery W-4 tagging
- H-6 composed-system proof · MT-09 signature · G26 attended debug · live-acceptance legs

---

# 8 · CLOSED SINCE V3 — verified, not merely reported

A-1 manifest circularity (structural fix) · A-2 spike exclusion · A-3 backup exclusion ·
A-4 release-identity convention · A-5 commit attribution · N-20 layer 2 (provenance records
reproduce themselves) · spike Electron risk (closed by exclusion) · OBS-1 (apparently) ·
OP-1 shortcut installer built · OP-3 cap raised in code · N-11 Debate seat resized ·
N-13b adapter rebase · N-14b UI adoption · N-16 runtime evidence isolation

---

# THE SHORT VERSION

**Two rulings from you unlock everything:** OD-31 (models work at all) and OD-27 (release stops
being blocked). **Five real defects** need a builder: N-21, N-25, N-22, N-23, N-27. **Two things
that look broken are locks you have never opened.** **Two dependency defects** keep this from being
enterprise-production, and only you can decide where the locks come from. **One reviewer lane** has
not moved since the program began.

# PUNCH LIST V4 — SWS-REM-DIR-20260828 R2
## Compiled by validator seat claude (cowork) · 2026-08-30T00:39:33Z
## Candidate: seal `50b47326f91c5abab8b6d5250d59b1dba96a4d46` · version 1.0.0-rc.1
## Supersedes PUNCH-LIST-V3-20260828.md

Status vocabulary: **BLOCKING** stops promotion. **OPEN** is real work nobody has done.
**RULING** waits on the operator. **DEFERRED** is parked by operator choice.

---

## LANE A — OPERATOR RULINGS

| id | decision | status | validator recommendation |
|---|---|---|---|
| **OD-27** | Refresh `module_source_registry.json` for debate, distillery, sow | **BLOCKING** | Rule it. `provenance_cross_hash_check` exits 1 until you do. It declares three modules' source identity, so no other seat may. CLOSEOUT-01 fixed the reproducibility half, which is what makes the ruling safe to take. |
| OD-20 | Ratify or revert the `e2e8e55` lineage import | RULING | **RATIFY** — the files are byte-identical to the rollback-of-record baseline in both of its internal locations. |
| OD-23 | Assertion inversion in `test_states.py` | RULING | No recommendation. Reviewer reads `V/assertion-inventory.md` first. |
| OD-28 | N-19 `shell/BUILD-MANIFEST.txt` staleness | RULING | Route to reviewer. Validator could not verify it and reports no result. |
| OD-29 | Accept the X-4 partial, or revert `9110390` | RULING | **ACCEPT** — but see N-22 below, which is a consequence of it and changes nothing about the recommendation. |
| OD-30 | X-4 step 3 — the committed interpreter placeholder | RULING | Not urgent. Values are overwritten at install. |
| **OD-31** | **Authorize live model operation in SOW** — create `modules/sow/config/live_operation.json`, naming ONE register row (OP-6 = claude+codex; OP-12 = those plus grok+antigravity) and a terminal ceiling of 1-2 | **RULING — blocks all model use** | Your call. The picker's "0/21 available" is this switch being off, by design. Spend act; collides with OD-5. A builder MAY create the file, but only with the row named in your ruling. |
| **OD-32** | **Amend or scope H-10** so the Conductor can launch under the shell, and say what replaces its quota protection | **RULING — blocks the Conductor** | Your call. H-10 forces `SOW_CONDUCTOR_AUTOLAUNCH=0`; the Conductor only starts when that is NOT 0. It is dark because the shell is required to keep it dark. |
| OD-5 | Spend authority for frontier providers | DEFERRED (DENY) | Blocks OP-2 and gates OD-31. Nothing to do until you change it. |

---

## LANE B — BUILDER WORK

| id | item | sev | status |
|---|---|---|---|
| **N-21** | **Token Center embed renders `about:blank`.** OP-4 does not work. Both CSP halves correct, Token Center standalone healthy, iframe `src` correct, frame never navigates. No CSP violation. | **HIGH** | **OPEN — new, found live** |
| **N-22** | **llamacpp absent from the shell entirely** — 5 modules listed, string absent from the page. Caused by validator-authored X-4 step 4. An uninstalled optional runtime must present as present-and-unavailable, never absent. Absorbs and enlarges OBS-2. | MEDIUM | **OPEN — new, found live** |
| **N-23** | **"Open" is enabled only for SOW — the one module with `open.kind: none`** — while the three modules that DO have open URLs are disabled. Operator clicks a live button wired to an empty string. Real fix needs a third `open.kind` to raise a native window; SOW is Electron, not a web surface. | MEDIUM | **OPEN — new, operator-reported** |
| N-21b | `sandbox="allow-scripts allow-same-origin"` is an inert sandbox — the console says so. Loopback content the shell already trusts, so not a vulnerability, but it undercuts OP-4's security framing. | LOW | OPEN — fix alongside N-21 |
| **N-25** | **No local models anywhere in the SOW picker.** `grep ollama apps/desktop/*.js` returns nothing — the Electron picker has no local path at all, while `adapters/roster.py` has full Ollama support. Schema admits `ollama_local`; nothing is wired to create one. Local costs nothing, so this is NOT fixed by OD-31. | **HIGH** | **OPEN — new** |
| **N-27** | SOVEREIGN Start and Run Startup Test fail: readiness probe not satisfied. venv, package and `/v1/health` (server.py:1906) all verified present — obvious causes eliminated. **Cause undetermined.** Next evidence: the shell's Logs button on the SOVEREIGN card. | MEDIUM | **OPEN — new, undiagnosed** |
| N-19 | `shell/BUILD-MANIFEST.txt` stale — 19 of 67 hashes wrong, 8 tracked files omitted (builder-measured; validator could not reproduce). Predates CONVERGE-01. | MEDIUM | OPEN, gated by OD-28 |
| N-20 L1 | `module_source_registry.json` stale → release gate red | MEDIUM | OPEN, gated by OD-27 |

**Note on N-21, N-22 and N-23.** None is catchable by any gate in this program. Every hermetic suite
passed and still passes. N-21's tests asserted the framing *policy*, not that the frame paints;
N-22's asserted that the adapter *compiles*, not that it appears; N-23's schema check asserted that
`open.kind` is a *permitted value*, not that the control the UI renders is one the shell can perform. Both were found by a person
looking at the running product within ten minutes. That is the argument for finishing T-2.

---

## LANE B2 — NOT DEFECTS: INTERLOCKS AWAITING AN OPERATOR RULING

**Do not send these to a builder as bugs.** Each is a fail-closed guard working as designed;
"fixing" them would damage a deliberate protection.

| id | what you see | what it is |
|---|---|---|
| **N-24** | Model picker: `0/21 available`, every provider `live DENIED (fail-closed)` | `config/live_operation.json` is **deliberately absent** — enforcement-by-absence. It is your authorization switch for billed CLI sessions. → **OD-31** |
| **N-26** | Conductor cannot be typed into (raised 3x, now root-caused) | `adapter.py:344` forces `SOW_CONDUCTOR_AUTOLAUNCH=0` (H-10 quota guard); `main.js:2942` launches the Conductor only when that is NOT 0. Launched from the shell it is **guaranteed dark, by design**. → **OD-32** |
| N-28 | Debate reaches no models | **Ollama is not running** on the host (pre-flight red, the 1 of 7 unavailable). Host state, not a product defect. Start Ollama. |

---

## LANE C — REVIEWER (chatgpt seat, untouched)

- The 19 CANDIDATE gates plus Gate-5/5b adjudication
- All CANDIDATE work from CONVERGE-01 and CLOSEOUT-01
- OD-23 assertion inventory; OD-28 N-19 verification
- Packages ready: `REVIEW-PACKAGE-CONVERGE01.zip` `93f1a15a…`, `REVIEW-PACKAGE-CLOSEOUT01.zip` `8e6987ca…`

**This lane has not moved for the entire program and is now the longest pole after OD-27.**

---

## LANE D — T-2 ATTENDED SESSION (in progress, operator present)

| step | item | status |
|---|---|---|
| S-1/S-2 | VRAM census cold, then shell-up-zero-modules. Cold baseline measured: 1826 MiB of 8151. | **shell-up reading NOT YET TAKEN** |
| S-3 | OBS-1 port tile — band reads `port 5180 · shell` | APPEARS RESOLVED, confirm |
| S-4 | OP-4 embed live | **FAILED → N-21** |
| S-5 | Eight concurrent terminals, and the 9th | NOT TESTED |
| S-4b | Module **Open** actions | **FAILED → N-23** (operator-reported, root-caused) |
| S-6 | **Conductor accepts typed input** — operator has raised this twice, never verified | NOT TESTED |
| S-7 | Visual parity vs UI-TARGET | background and dark surface live; detail comparison outstanding |
| S-8 | Module starts one at a time; confirm Debate loads qwen3:8b not phi4:14b | NOT TESTED (Ollama is down) |
| S-9 | llamacpp graceful-unavailable | **FAILED → N-22** |
| S-10 | Clean stop, orphan sweep, porcelain unchanged — N-16's live proof | **NOT DONE — highest remaining value** |
| S-11 | Operator runs `install_shortcut.ps1` | NOT DONE (operator's own hands) |
| S-12 | Distillery UI state confirm | observed: status only, 9 open questions, no compute on open |

Host note: **Ollama is not running** (pre-flight red, urlopen error, the 1 of 7 unavailable).
Nothing model-dependent can be tested until it is up.

---

## LANE E — GENUINE ENGINEERING DEFECTS, UNREMEDIATED

| id | item | why it matters |
|---|---|---|
| **E-1** | SOW has only unpinned ranges (`jsonschema>=3.2`, `pytest>=7`) — no lock | Reproducible provisioning **not achieved**. `pytest.ini` 2382/0/1 contract remains UNVERIFIED. |
| **E-2** | Distillery has no Python lock (`lockfiles: []`, only a `pyproject.toml`) | Same. A `.venv` cannot be truthfully called lock-driven. |
| **E-3** | True clean-room install never performed | Every install proof is a same-host proxy. Proves mechanics and layout only, on a host that already has Python, node and Ollama. |

**E-1 and E-2 together remain the largest genuine barrier to an enterprise-production claim.**
No loop can close them — S-11 forbids synthesizing a lock. They need an operator decision about
where authoritative locks come from.

---

## LANE F — DEFERRED BY OPERATOR

**OP-7 (NEW, operator-stated): model agnosticism across SOW, Debate and SOVEREIGN** — one selector
offering local models AND frontier (Grok, Claude, ChatGPT) in each. Today SOW's picker is
frontier-only, Debate is Ollama-only, SOVEREIGN is loopback-Ollama under SYSTEM_MANIFEST authority.
Overlaps OP-2; gated by the same spend and key-custody rulings. **A feature program to be scoped and
costed, not a punch-list line.**

OP-2 frontier providers (dossier written; inert until OD-5, key custody and role eligibility are
ruled) · OP-6 Distillery UI · Lane R research · canonical license text · upstream Distillery W-4
tagging · H-6 composed-system proof · MT-09 signature · G26 attended debug · live-acceptance legs

---

## NAMED ACCEPTED RISKS (carried into release notes)

nanoid GHSA-2v37-7h3g-55p8, lock-held · no Python vulnerability auditor in any retained lock, so
CVE disposition is unknown · Grok registry / G28 no-spend limitations
**Closed since V3:** the spike Electron/extract-zip risk, closed by archive exclusion in X-2.

---

## CLOSED SINCE V3 — verified, not merely reported

A-1 manifest circularity (structural) · A-2 spike exclusion · A-3 backup exclusion ·
A-4 release-identity convention · A-5 commit attribution · N-20 layer 2 (provenance records
reproduce themselves again) · OBS-1 (apparently) · OP-1 shortcut installer built · OP-3 cap raised
in code · N-11 Debate seat resized · N-13b adapter rebase · N-14b UI adoption · N-16 runtime
evidence isolation

---

## THE SHORT VERSION

One decision blocks release: **OD-27**. Five live defects block calling the product finished:
**N-21**, **N-22**, **N-23**, **N-25** and **N-27**. Two more things that look broken are not —
**N-24** and **N-26** are interlocks you have never opened (**OD-31**, **OD-32**). Two engineering defects block calling it enterprise-production:
**E-1** and **E-2**. Everything else is queue.

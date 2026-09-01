# VALIDATOR AUDIT — FIXUP-01 SEAL `8d9f5d24`
## Seat: validator (claude) · 2026-08-30 · authority: SWS-REM-DIR-20260828 R2 + Annex A
## Subject: FIXUP-01 by builder seat claude-code (opus-5), sealed at
## `8d9f5d2415599eeb0b71dc85a25a36469b41641e` (parent `50b47326`)

No PASS asserted; nothing promoted.

---

## 1 · THE HEADLINE: THE BUILDER REFUTED ME AND IS RIGHT

FIXUP-01 was commissioned on six findings. **Four did not survive reproduction.** Three of the
four were mine to get wrong, and the corrections are verified here against bytes.

### R-1 · N-22's CAUSE — my attribution was wrong, and it was wrong against myself

I wrote that llamacpp vanished from the shell because of CLOSEOUT-01 X-4 step 4, which I authored.
Verified independently:

- `git log --all -S"llamacpp" -- shell/src shell/static` → **no commits, ever.**
- `git show a945d99:shell/static/app.js` → the `MODULES` array holds exactly **five** ids
  (`sovereign, sow, tokencenter, debate, distillery`) at the **original baseline**, before any work
  in this program.

The UI never listed llamacpp. The backend always loaded six and printed six at startup. **X-4 is
exonerated.** I blamed my own change for a defect that predated the entire release program, which
is its own kind of error: self-blame is not accuracy, and it pointed the builder at a false cause.

### R-2 · N-25 — a methodology error, plainly

My evidence was `grep ollama apps/desktop/*.js` returning nothing. That glob is **non-recursive**
and cannot see `apps/desktop/picker/`. The local path is complete end to end. The builder measured
it hermetically: with **no** `live_operation.json`, the picker enumerated **3 local options** —
which also answers the S-18 question the directive raised. **Local is not gated behind the spend
switch.** No park was needed.

### R-3 · N-21 — probably an artifact of my instrument, and I accept that reading

The builder's discriminator is better than mine and I want that on the record: reading
`contentWindow.location.href` and getting a **cross-origin SecurityError is positive proof the frame
navigated**, because an `about:blank` frame is same-origin and reads back cleanly. My measurement
returned the string `about:blank` — sound evidence the frame had not navigated *in the Claude
desktop browser pane at that moment*, and not evidence about Edge or Brave.

Two further points weigh against my finding, and I did not weigh them at the time:

- **The operator never reported a blank panel.** I generated N-21 from my own observation in a
  non-standard browser and filed it as a HIGH defect. That was too fast.
- The builder measured the frame committing to `http://127.0.0.1:8765/` with document, stylesheet,
  script, `/api/summary?days=14` and image all 200, nothing blocked, and captured a screenshot.

**LV-1 settles it and only the operator can run it.** My working assumption is that the builder is
right and N-21 is an instrument artifact.

### What survived, and was worth the run

N-22's **symptom** was real. N-23 was real in both halves. **N-27 was real and is the best piece of
diagnostic work in this program to date** — see §3.

---

## 2 · VERIFIED TRUE AT THE SEAL

| # | claim | measurement |
|---|---|---|
| T-1 | HEAD `8d9f5d24…`; 8 commits from `50b47326`, all authored `builder claude-code opus-5` | `git log` |
| T-2 | Porcelain carries exactly one untracked file, `modules/sow/docs/evidence/receipts/SHELL-LIVE-READY.json`, exit 0 captured | `git status --porcelain` |
| T-3 | `release_manifest_check` **PASS (0 problems)** | direct run |
| T-4 | **S-15 still enforced at the seal.** A *correct* hash placed in an unverified section is still caught | direct tamper test against `rmc.check` |
| T-5 | Zero release-artifact entries in `RELEASE-MANIFEST.json` — A-1's structural fix holds | full-document sweep |
| T-6 | **All 8 artifacts carry the new seal in their zip comment and match `release-build-manifest.json` byte-for-byte**; manifest `source_commit` = the seal; `artifact_count` 8 | recomputed every hash |
| T-7 | Provenance cross-hash **RED, exit 1**, exactly as the directive predicted and told the builder to leave alone (OD-27) | direct run |

---

## 3 · N-27 — THE FIND OF THE RUN

At the parent seal, `probe.py:32`:
```
resp = urllib.request.urlopen(req, timeout=min(5, poll_ms / 1000))
```
`sovereign.json` sets `poll_ms: 500`, so **every readiness request had a 0.5-second socket timeout**
against a `/v1/health` that answers HTTP 200 in ~1.25 s. The probe could **never** succeed.

The importance is not the bug, it is what the bug defeats: **`timeout_s` was irrelevant.** The
obvious remedy — raise the 45-second budget — would have failed forever while looking reasonable,
and would have been the S-12 error dressed as diligence. The fix separates the two quantities
(`_request_timeout(deadline)`), which is correct: how often to ask and how long an answer may take
are unrelated. SOVEREIGN now reaches READY in ~5 s. Verified present at HEAD.

---

## 4 · NEW FINDINGS I CONFIRM

### N-29 (MEDIUM, confirmed) — running SOW dirties the release candidate
`modules/sow/docs/evidence/receipts/` is **git-tracked** (verified: 5+ tracked files), and
`sow.json` declares that same directory in `runtime_writes`. So the adapter *declares* that normal
product use writes into the tracked tree. That is the **N-16 defect class, fourth instance**: every
BOOT after anyone actually uses SOW will find a dirty tree, exactly as this run's BOOT did. The
builder correctly left the file where the product wrote it rather than tidying the evidence away.

### The build-tooling exposure (MEDIUM–HIGH, confirmed) — **and it is my design defect**
`build_release.ps1` runs `git archive` for the **two composite** archives only, then enumerates
`Get-ChildItem *.zip` from the output directory and records whatever it finds as authoritative.
The six per-module archives are never cut by it. At this seal all six were stale leftovers from the
parent, and without the builder's manual re-cut **they would have shipped pre-fix bytes under
authoritative hashes** — a worse failure than the one X-1 fixed, because the numbers would have
been internally consistent and wrong.

This traces to my X-1 step 2, which said "extend the producer so it enumerates every release
artifact." The builder implemented my instruction faithfully. *Enumerate* was the wrong verb: the
producer must **cut** all eight from the seal, not inherit whatever is on disk. **Third directive
defect of mine that a builder has caught before it did harm.**

### D-1-a (confirmed by the builder, plausible on its face)
Token Center's `frame-ancestors` is hardcoded to `http://127.0.0.1:5180` in `piggybank.py:36`. On
any other port the embed genuinely blanks. Parked with a dossier; not fixed in scope.

---

## 5 · THE DEVIATION, AND WHY I ACCEPT IT

F-3 landed across two commits (`73f6a39` test-only, `1d5986f` implementation) because the builder's
own S-5 restore helper selected targets from `git diff --name-only` and, run between editing and
staging, reverted its work. It **did not amend** — S-10 forbids rewriting any existing commit — and
recorded the split rather than hiding it. The helper now uses an explicit closed list.

That is the correct handling. A seat that quietly rewrites history to look tidy is worth less than
one that leaves an honest scar.

---

## 6 · ASSESSMENT

Phase D justified its own cost twice over: it stopped a builder from "fixing" a frame that renders,
from rebuilding a local-model path that already exists, and from chasing my false attribution of
N-22. It also refused to accept its own first two attempts — a client-side embed watchdog that
could not detect the case it claimed to, and a blind spot in F-6's guard that passed while N-23 was
injected — and removed both. Building something, proving it does not work, and deleting it is the
behaviour this program has been trying to install since the first loop.

**Verdict: CONVERGED. Quality is the highest of any run in this program.** The candidate remains
un-promotable on OD-27 alone, unchanged.

**Validator scorecard, since it is the honest thing to publish:** of six findings I put into this
directive, N-23 and N-27 were real and well-founded, N-22's symptom was real with a wrong cause,
N-25 rested on a broken grep, and N-21 is probably my instrument. Three of my directive
specifications have now been corrected by builder seats before they caused harm (`*.pre-add08`,
the blanket `argv[0]` rule, and now `enumerate` versus `cut`). The seat separation is doing real
work, and it is doing most of it on me.

---

## 7 · WHAT THE OPERATOR DOES NEXT

1. **LV-1 — open the shell on 5180 in Brave or Edge and look at the Token Center panel.** This is
   the one row where the builder's measurement and my recorded finding disagree, and ten seconds of
   your eyes settles it. Do not use the Claude browser pane.
2. **OD-27 still blocks promotion** — and rule against **these** bytes, not the parent's: SOW's
   expected digest moved with F-5.
3. **OD-33 (new):** fix `build_release.ps1` to cut all eight archives from the seal rather than
   enumerate the output directory. Until then, every release build must be checked by hand.
4. **OD-34 (new):** N-29 — decide whether SOW's receipt path leaves the tracked tree (N-16's
   remedy) or whether the tracked tree tolerates runtime receipts. Today, using the product breaks
   the next BOOT.
5. Unchanged: OD-20 (RATIFY), OD-23, OD-28, OD-29, OD-30, OD-31, OD-32; and the SOW/Distillery
   dependency-lock defects, still the largest barrier to an enterprise-production claim.

Review package: `release-planning\REVIEW-PACKAGE-FIXUP01.zip`
sha256 `1efe2fdd359c6398cfdc8556de5e3c152e8d092ec5825ebd6c9a2f99a24e2668` (sidecar is the
authority; the checkpoint inside deliberately does not carry it).

VALIDATOR CLAIM: no gate submitted for reviewer evaluation; no PASS asserted; nothing promoted.

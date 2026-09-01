# LOOP PROTOCOL — ANNEX A TO SWS-REM-DIR-20260828 R2
## Autonomous directive-loop execution · 2026-08-28

**Purpose.** The directive already specifies goals, scope, permitted work, required tests, and exit criteria. Therefore no seat has a legitimate reason to interrupt the operator between work items. This annex converts per-item ASK into per-item CHECK: seats self-evaluate against the written criteria and run to queue exhaustion. It amends nothing in R2's role separation — builder still never writes PASS, the reviewer seat still evaluates every gate, the operator still owns promotion. What changes is WHEN the operator is involved: three scheduled touchpoints plus a short exception list, instead of ad-hoc stops.

This annex is authorized by reference in the operator's token; the R2 directive hash (`9061c2e8…`) is unchanged.

## 1. The loop

Ordered queue = Punch List v2.1 items within the currently authorized batches, in the directive's batch order. For each item:

```
implement (builder) → fail-before evidence → minimal change → pass-after evidence
→ affected suite → seal evidence bundle (item ID, changed-file set, UTC, seat)
→ reviewer-seat check against the item's written pass criteria
→ PASS-equivalent: mark CANDIDATE-ACCEPTED-BY-REVIEWER, take next item
→ FAIL: builder fixes within the same authorized scope, re-check, max 3 cycles
→ 3 fails or criteria not mechanically evaluable: park item with dossier, take next item
```

**No seat stops the loop between items. Parked items do not stop the loop; they accumulate for T-2.** The reviewer seat runs continuously inside the loop — the 19 CANDIDATE gates are evaluated the same way: one continuous pass over all 19, one consolidated verdict table, zero operator interrupts.

## 2. Exceptions — the ONLY reasons any seat may halt and page the operator

- **E-1 Money.** Any action with possible billing, provider session, or GPU compute not covered by a recorded grant.
- **E-2 Irreversibility.** Any write outside the designated worktree and custody folder; any deletion; any mutation of an immutable input.
- **E-3 Spec contradiction.** The directive, punch list, and bytes disagree in a way that changes what the work IS (not how hard it is). Record, park, continue other items; halt only if the contradiction poisons the whole queue.
- **E-4 Security regression.** A change makes a verified security property worse.
- **E-5 Physical-operator dependency.** The item requires the operator's machine, signature, or attended presence (service launches, MT-09, G26 debug, live acceptance). These are QUEUED for T-2, never ad-hoc stops.
- **E-6 Queue exhausted / batch boundary not pre-authorized.** The loop finished everything it was allowed to do.

Anything not on this list is not a reason to stop. "I'm uncertain whether the operator would like this" is specifically not a reason to stop: the decision block below is the operator's stated will; apply it.

## 3. Three touchpoints — total operator involvement

- **T-1 (boot, one message):** operator accepts the decision block (§4), names any overrides, and authorizes the loop. Recorded verbatim + UTC in OPERATOR-INSTRUCTIONS.log by the validator.
- **T-2 (one attended session, scheduled when the queue is otherwise drained):** batched physical acts and parked-item rulings — service launches (OD-4 legs G34/G35/G53/9d/9f), MT-09 signature (OD-3), G26 attended debug (OD-16c), OD-20 ruling on the prepared H-5 lineage dossier, any parked items, any E-3 dossiers.
- **T-3 (final acceptance):** the R2 §9 nine-condition check, sealed artifacts, operator promotion. Unchanged from the directive.

## 4. Operator decision block — defaults

Accepting this block resolves every open OD that can be resolved without a physical act. Each line is the default applied by the loop unless the operator overrides it at T-1. Overrides are one line each ("OD-8: training runtime" etc.).

| OD | Default applied by the loop |
|---|---|
| OD-1 | SYSTEM baseline `75e4be…` = candidate; RESET `00ae9eef…` = rollback reference |
| OD-2 | Issue `AUTHORIZE SD-RBR-v1.0 W-4 W-5 RELEASE` (recorded at T-1 with the token; execution of W-4/W-5 happens in its authorized batch) |
| OD-3 | MT-09 signature → T-2 (physical act) |
| OD-4 | Service launches → T-2 (physical acts); affected legs stay NOT_RUN-with-cause until then |
| OD-5 | No provider spend this release; Grok-registry leg and G28 recorded as named accepted limitations |
| OD-6 | SOW CLAUDE.md correction approved; scope re-signature folded into MT-09 at T-2 |
| OD-7 | D-9, HG-3, G2–G6, F-26…F-30 scoped out as named limitations in release notes (never silent) |
| OD-8 | Distillery ships as status-only component this release; training authority deferred |
| OD-9 | Debate Phase-7/Phase-8 deferrals and extended soak stay out of this release; named in notes |
| OD-10 | Existing Token Center data archived unmodified beside the install; fresh canonical instance; no migration |
| OD-11 | SYSTEM_MANIFEST.json is the model-role authority; docs and hierarchy reconciled TO it; `qwen3.8:27b` tag verified against the local runtime and corrected if unresolvable |
| OD-12 | LATEST_MODULE_SOURCES becomes generated output of the canonical tree, not a release component |
| OD-13 | Ollama remains production default |
| OD-14 | 31-h hang recorded as UNREPRODUCED KNOWN OBSERVATION in release notes; non-blocking |
| OD-15 | Composed system version v1.0.0; Token Center v1.0.0 |
| OD-16 | (a) worktree `D:\producttion software 2\release-worktree` (b) reviewer seat: chatgpt (has held the reviewer function to date) (c) G26 → T-2 |
| OD-17 | GitHub debts deferred to the public-release track; not blocking internal ratification |
| OD-18 | Threat-model disclosure accepted as the 5.7 closure; `tests/security/` written in Batch 3 as hardening, non-blocking |
| OD-20 | CANNOT default. Loop produces the full H-5 lineage dossier with a recommendation; operator rules at T-2. Batch-2 H-5 implementation waits; everything else does not |
| OD-21 | BOM-free source policy adopted PROSPECTIVELY: hygiene, applied at next authorized re-cut of each artifact, never forcing a re-cut by itself |

## 5. Pass criteria rule

An item may only auto-advance if its pass criteria are written and mechanically checkable before implementation starts (the regression-test spec IS the criteria for defect items; the v2.1 text is the criteria for verification items). If the builder cannot state checkable criteria, the item parks under E-3/E-4 discipline — it does not become a conversation.

## 6. Custody cadence

The loop emits one consolidated CANDIDATE bundle per batch (changed files, evidence, reviewer verdict table, parked-item dossiers), exported to `D:\producttion software 2\release-planning\` — not one interrupt per item. The validator audits bundles against the frozen bytes on arrival. Ephemeral-seat rules from R2 §2 unchanged.

## 7. Boot ceremony — collapsed

The system's own acceptance standard is "a live-session instruction quoted verbatim with UTC." Therefore: the operator types ONE authorization line in the working session; the validator records it verbatim with UTC into `OPERATOR-INSTRUCTIONS.log` and generates the zero-placeholder harness paste from it. No fill-ins, no re-paste loop, no empty token slots.

Reference form (operator may edit any clause):

```
RUN SWS-REM-DIR-20260828 LOOP UNDER ANNEX A <annex sha256>.
ACCEPT DEFAULT DECISION BLOCK [EXCEPT: <overrides>].
DIRECTIVE SHA-256 9061c2e8a40bcff4258f77692ff0e23ffebde98eaa638446a22a7aa914b3691d.
SCOPE: PHASE 0 + BATCH 1 + BATCH 2 (H-5 held for OD-20) + BATCH 3, hermetic legs only; E-1..E-6 exceptions.
SEATS: BUILDER qwen3.8-max (deepseek), REVIEWER chatgpt, VALIDATOR claude (cowork), OPERATOR sam.
```

VALIDATOR CLAIM: this annex asserts no gate status, closes no punch item, and takes effect only when named in a recorded operator authorization.

# CROSS-SEAT ADJUDICATION — ROUND 2 — 2026-08-28

Validator seat. Inputs: builder response (v1 token withdrawn, corrected token + byte checks) and reviewer response (corrections accepted, one amendment proposed: M-2 downgrade). All new factual claims re-verified against `75e4be07…ac27138` bytes today.

## 1. Convergence check — both seats' byte claims verified

Builder's sovereign.db figures are **exact**: SQLite 3, 90,112 bytes, sessions 1 / messages 4 / jobs 2 / event_log 17 (plus tables research_checkpoints, self_snapshots, meta). Reviewer's BOM breakdown is **exact**: 37 total = 19 evidence + 3 dev + 14 active-SOVEREIGN + 1 active-Debate. Three seats have now independently derived identical numbers from the same frozen bytes. The record is converging, which is what this process is for.

## 2. M-2 amendment — ACCEPTED WITH ONE SPLIT

The reviewer's argument is correct as far as it goes: Python source decoding accepts a UTF-8 BOM, the file compiles, hash identity already covers the bytes, and **no written BOM policy exists** — I checked both .gitattributes files present in the archive (sow, distillery): they mandate `eol=lf` and binary markings only, nothing about BOMs. So for the 15 active `.py` files: **downgrade accepted**. M-2 becomes L-6 (encoding hygiene, normalize at the next otherwise-required re-cut, no Debate re-cut for this alone). OD-19 is reframed from "re-cut vs. limitation" to: *does the project adopt a BOM-free source policy?* If yes, L-6 becomes a conformance item at the next re-cut; if no, it is closed as accepted style.

**The split:** the BOM population is not only `.py`. Six JSON files in the active SOVEREIGN tree carry BOMs, and they are not incidental files:

- `modules/sovereign/SYSTEM_MANIFEST.json` — the file M-6 proposes as the single model-role authority
- `constitution/constitutional_rules.json`, `constitution/promotion_policy.json`, `constitution/risk_policy.json`
- `library/config/clu_runtime_policy.json`, `synthesis/synthesis_contract.json`

Strict `json.loads` rejects a BOM. These parse at runtime only because SOVEREIGN's own loaders compensate: 16 of 39 IO-performing SOVEREIGN `.py` files use `utf-8-sig` (system_manifest.py among them). The codebase has institutionalized the BOM rather than removed it. Two consequences: (a) any external strict tool — a release validator, jq, a provenance checker — fails on the system's own governance configs; (b) whether any of the 23 non-`sig` loaders ever reads one of these six files is unverified. This is not `.py` hygiene; it is the same machine-readability defect class as M-5's evidence envelopes, now shown to reach governance-bearing configuration. **Disposition: fold into M-5, extended**, with a cross-link into M-6 — "make SYSTEM_MANIFEST.json the single authority" must include "make it strictly parseable," or the authority file itself defeats mechanical validation.

## 3. C-1 three-way split — ACCEPTED

C-1a runtime DB + prompt/generation state: CRITICAL. C-1b active SOW phase17c log: MEDIUM. C-1c `.env.local` (no secret): LOW policy defect. One allow-list packaging gate closes all three; the split exists so nobody later discounts C-1 because its LOW component was benign. Recorded as the reviewer proposed.

## 4. C-3 manifest-enumerated provenance — ACCEPTED

The release manifest explicitly enumerates per module: current_provenance_path, superseded_provenance_paths[], source_ref/commit/archive/sha256, locks, build identity, artifact hash. Validators check what the manifest names; filename convention stops being an authority. This supersedes glob-based checking permanently and makes the `.previous.json` / `.json.previous` inconsistency a one-time normalization rather than a recurring hazard.

## 5. H-5 refinement — ACCEPTED

Copying `canonical_registry.py` and the llamacpp adapter from RESET into SYSTEM would be the wrong operation. The facts establish divergence, not direction. File-by-file lineage adjudication (intent of removal, callers, superseded capability, canonical disposition, regression test) — with the canonical ruling recorded under OD-20. Nothing in my record should be read as pre-deciding that those files belong in the release.

## 6. Corrected token — improved; three gaps before it is signable

The v2 token resolves D-1 (OD-1 explicit in body), D-2 (authority rooted in ZIP hash, sandbox commit dropped), and D-3 (directive hash + reviewer field present). Remaining:

1. **Custody of the directive bytes.** `CANDIDATE_REMEDIATION_DIRECTIVE.md` — the file that allegedly hashes to `860451fe…` — exists only in `artifacts/PHASE0_FREEZE/` on the builder's ephemeral sandbox. Neither the operator nor this validator has seen those bytes. An operator must never sign a hash of a document not in shared custody. **Required: deliver the directive file + its .sha256 sidecar + BUILDER_RESPONSE_TO_ADJUDICATION.md into `D:\producttion software 2\release-planning\` (or another operator-controlled location). I will then independently recompute the hash and confirm or refute `860451fe…` before any signing.**
2. **Sandbox mortality.** Same D-2 logic the builder accepted for the git commit applies to every PHASE0_FREEZE artifact: if the sandbox is reclaimed, the directive the token names ceases to exist anywhere. Export now, not at signing time.
3. **Reviewer seat identity.** `REVIEWER: REPLACE_WITH_SEAT` must name a seat distinct from the builder. In the current constellation the ChatGPT seat has been performing reviewer functions and the Grok seat builder functions; whatever the operator chooses, one seat cannot hold both roles for the same gates — that is the system's own AGENTS.md rule.

## 7. Standing state after Round 2

No authorization line recorded; no-mutation discipline held by all seats for a second round. Punch List v2.1 (issued alongside this record) incorporates: the M-2→L-6 downgrade with the M-5 JSON extension, the C-1a/b/c split, the C-3 manifest design, the H-5 refinement, OD-19 reframed, and new OD-21 (BOM policy adoption — subsumes OD-19's encoding half). V2 remains on disk as history. The reviewer's proposed treatment of v2.1 — reviewer baseline pending [R]/[U] legs — is consistent with this record.

The largest blocker is unchanged and predates every finding in this file: 19 CANDIDATE gates with no reviewer verdict.

VALIDATOR CLAIM: no gate submitted, no PASS asserted, no candidate designated, no authorization inferred.

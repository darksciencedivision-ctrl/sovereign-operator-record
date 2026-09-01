# CONVERGE-01 — SELF-REVIEW BEFORE HARDENING · validator seat · 2026-08-28

Adversarial read of my own R1 CONVERGE-01 directive. Ten weaknesses found; each becomes a change in R2. No new operator authority needed — R2 sharpens within ENTRY 006's recorded scope.

## Weaknesses in R1

**W-1 · C2 network grant is dangerously vague.** "Public package registries only" doesn't name hosts, and pip/npm resolve transitive deps from many CDNs. A loose grant re-opens the supply-chain surface the whole directive exists to close. → R2 names the exact allowed hosts, mandates lock-only installs (`npm ci`, pip from the resolved lock with `--require-hashes` where hashes exist), and forbids any install from a URL/VCS/local path not in a committed lock.

**W-2 · Locks treated as mutable.** R1 said "provision from locks" but didn't forbid regenerating a missing/broken lock — which would be a tracked-byte change mid-loop, exactly the staleness class the manifests-last rule fights. → R2: locks are READ-ONLY inputs; a missing, partial, or drifting lock PARKS the module, never regenerates it.

**W-3 · No supply-chain disposition after provisioning.** C2 installs real packages but R1 never scanned them. With network now granted, `npm audit` / `pip-audit` are available. → R2 adds a supply-chain step: audit each environment, disposition critical/high (fix within lock or park with a named accepted-risk), re-run the secrets scan over the tree, and confirm environments are gitignored so nothing installed reaches an artifact.

**W-4 · "Clean-room install" on the same host is a false claim.** True clean-room = an independent machine with no Python/node/Ollama/global state. Testing into a spaces-path dir on THIS host shares all of that. R1 implied clean-room; it isn't. → R2 renames C4's test a **self-host install PROXY**, states exactly what it does and does not prove, and lists true clean-room (fresh VM) as a residual T-2/reviewer item.

**W-5 · Shortcut-vs-uninstall contradiction.** R1's install writes shortcuts to the user's Desktop/Start Menu (outside `-Dest`) while its uninstall claims "nothing outside -Dest touched." Direct contradiction. → R2: the self-host proxy install uses `-TargetDir` to keep shortcuts INSIDE `-Dest`; the real install records every out-of-Dest artifact (shortcuts) in an install manifest, and uninstall removes exactly those and proves it against that manifest — the honest claim is "removes what it recorded," not "touched nothing outside Dest."

**W-6 · "Deterministic" undefined per module.** SOVEREIGN and Debate need Ollama models for real behavior; calling their suites "deterministic at final bytes" invites either a false green or a model load (forbidden). → R2 defines deterministic-hermetic per module and PARKS every model-dependent leg explicitly as E-6, so C5 can't accidentally claim coverage it doesn't have.

**W-7 · No mechanical exit gate.** "If anything buildable-hermetic remains you're not converged" is a judgment call, and judgment calls drift. → R2 adds a CONVERGENCE CHECKLIST the builder evaluates literally: every row must read DONE-CANDIDATE, PARKED(reason), or DEFERRED(operator) — no blanks — or the loop continues.

**W-8 · Not resumable.** A quota death (run 01's failure mode) would strand this long loop with no defined restart point. → R2 adds phase checkpoints: each phase ends with a `bundles\CONVERGE01\CHECKPOINT.json` recording completed phases + HEAD, so a resume seat knows exactly where to re-enter.

**W-9 · No disk preflight.** C2 provisions multiple venvs + node_modules + Electron ≈ several GB; on a full disk it fails mid-install and leaves partial state. → R2 opens C2 with a free-space check and a stated minimum, park-with-cause if short.

**W-10 · Rollback package unaddressed.** Directive Phase 10 wants a previous-release rollback package; R1 was silent. → R2 names the frozen RESET baseline (`00ae9eef…`) as the rollback artifact of record and cites it in the release notes, rather than manufacturing a new one.

## Also folded in
Third-party notices: an all-rights-reserved product that bundles OSS deps needs a `THIRD-PARTY-NOTICES` file (C3) — not doing this is a real distribution defect. And VERSION.json's `rc.1` suffix rationale is stated inline so no one mistakes the loop's output for the promoted 1.0.0.

R2 follows.

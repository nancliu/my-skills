# Reference — portable failure modes

Hard-won patterns from long AFK waves. No product or repo names. Use to calibrate judgement. When a mode earns a stable procedure, promote it into `human-gates.md` and bump `VERSION.md`.

## Premature owner exit

Feature owners that spawn explorers/helpers and return "in progress" leave an empty worktree. Brief must forbid stopping there. Replace with a fresh owner and the full consolidated brief; do not chain resume on a done summary that never opened a PR.

## Command apply that can no-op

Facades that "apply" a command may leave state unchanged when guards fail. UI that clears status, resets selection, or closes a dialog solely because apply did not raise will lie to the user. Gate side effects on a pre-check or a post-apply delta. Prefer encoding this in the repo's coding-standards via env retro.

## CI skips expensive surfaces

Pipelines often omit GUI, hardware, or live-network tests. Green CI then misses rotting smoke scripts. After changes on those surfaces, run the skipped suite (or the repo verify skill's equivalent) on a machine that can actually exercise them before land.

## Parallel edits on one seam

Two tickets that both extend a registry, router, or verify index will conflict. On rebase: union the entries; update tests that assumed the old "unsupported" path; keep unit tests on fakes so they never launch real engines or devices.

## Signing and rebase

Interactive or flaky commit signing stalls `commit` and `rebase --continue`. Use the repo-allowed non-interactive unsigned path for feature work, apply it at every rebase continue, and publish with lease-safe force only on the feature branch.

## Review / ship model outages

If one model family hits quota mid Spec/Standards/Shipping, switch to another available family and continue. Do not park the wave waiting for the original panel. Promote to a named gate when the fallback order is stable enough to prescribe.

## Silent stall after Shipping PASS

Build/verify/review finish, PRs sit merge-ready, and the orchestrator goes quiet because GATE-004 land was never granted and never asked. Fix: GATE-006 — ask once at wave start; one nudge when merge-ready; treat only explicit land phrases as grants, not bare "develop".

## Align done, labels still triage

Grill/spec finishes but tickets stay on a pre-ready triage label, so the ready-frontier query looks empty or the wave invents scope from chat. Fix: GATE-007 — retarget to the repo's AFK-ready label before Build.

## Invented to-spec split on a single ready cut

A ticket already labeled ready, with grill decisions on the issue, still gets a full `/to-spec` / `/to-tickets` pass that invents sibling tickets. Fix: Align step in `SKILL.md` — skip those tools unless the issue must become multiple tickets or parallel owners.

## TDD seam interview blocks AFK

Nested `/tdd` asks the human which seams to test; the long wave goes idle waiting for an answer that GATE-005 already allows the agent to choose. Fix: GATE-008 + Feature brief template — reuse existing seams, never ask, still red-green.

## Shared shell file parallelized anyway

Two "disjoint" tickets both rewrite the same GUI smoke test or verify index. Implementation looks parallel; land pays the merge tax. Prefer serialize when the shared shell is unavoidable; on conflict, union entries (Parallel-land hygiene).

## Orphan worktrees after land

Each ticket/arena/retro gets `git worktree add`, but land and env retro never remove the trees. Sibling folders pile up next to trunk; later agents open stale checkouts by mistake. Fix: **Worktree reclaim** after each wave land, plus completion-audit survivors-only-with-reason. On Windows, an empty locked directory after `worktree remove` is reported debt—do not block the next frontier waiting on the lock.

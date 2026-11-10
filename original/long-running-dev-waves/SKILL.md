---
name: long-running-dev-waves
description: >-
  Orchestrate long unattended development across a backlog of ready tickets:
  dependency-ordered waves, isolated worktrees, verify, two-axis review,
  independent ship verdict, merge, environment retro, then post-land hygiene
  (trunk refresh, worktree reclaim, local/remote branch prune, ticket close,
  stacked retarget, README when user-facing). Use for AFK multi-issue
  delivery, auto-advance by blocked-by, or session-length coding goals that
  must not invent product scope. Maintains a versioned human-gates register
  for ask-human and special-case exceptions.
disable-model-invocation: true
---

# Long-running development waves

**Version:** see [VERSION.md](VERSION.md) (current recorded there).

Run a **session-length goal** that lands every in-scope ready ticket the dependency graph allows. Do not invent product scope. Prefer the repo's `AGENTS.md` / agent docs when they conflict with this skill; this skill fills gaps when the repo only sketches the loop.

**Human gates:** before asking the user or handling an exceptional failure, read [human-gates.md](human-gates.md). Prefer an existing gate over improvising. New exceptions land in a skill version bump, not only in chat memory.

**Failure modes:** [reference.md](reference.md).

## Parse the goal

1. Restate: tracker, trunk branch tip (what is already landed), which tickets are in scope, wave/parallel rules, when to ask the human, and whether a GATE-004 land grant is already present in the invoke text or standing rules.
2. Arm one durable goal (`/goal` or CreateGoal). Do not redefine success down to the first wave. If a land grant is present, put it in the goal text so later turns do not lose it.
3. Apply gates in `human-gates.md` (baseline: product preference, verify FAIL needing scope change, irreversible ops; land grant, land-grant acquisition, and reversible defaults as recorded there). If no land grant yet, fire GATE-006 **before** spawning Feature owners so AFK waves are not blocked only at the end.

## Choose the frontier

1. List tickets labeled ready for an AFK agent (or the repo's equivalent). Do not treat `needs-triage` (or the repo's pre-ready label) as ready—apply GATE-007 first when Align just finished.
2. Honor explicit blockers in the ticket body. A ticket is frontier only when every blocker is closed or already on the PR base.
3. Form a **wave**: one ticket, or several **disjoint** tickets (separate worktrees or cloud agents). Never dual-drive one working tree.
4. When two tickets likely edit the same seam (shared dispatch table, shared index doc, shared facade, shared GUI smoke harness), parallelize at most one of them with a low-conflict companion—or serialize. Prefer serialize when both will rewrite the same shell file in the same wave.

## One wave (loop until the queue is empty)

### Align, then build

1. If the ticket is underspecified, Align first (grill / wayfinder—or the repo's named Align tools). Do not start Feature implementation while Align is still open. Skip `/to-spec` / `/to-tickets` when a single ticket is already `ready-for-agent` (or repo equivalent), grill/spec decisions are recorded, and the cut fits one Feature PR—use those tools only when the issue must split into multiple tickets or parallel owners.
2. When Align reaches shared understanding (grill accepted / spec settled), apply **GATE-007** before Build: retarget tracker labels from triage to the repo's AFK-ready label.
3. Build with the repo's Feature playbook (or Bug fix). One **fresh** Feature owner per ticket, isolated worktree:
   - `git fetch origin <trunk> && git worktree add <path> -b <branch> origin/<trunk>`
   - Brief **must** follow the **Feature brief template** below (GATE-008). Missing AFK/TDD lines is a brief defect—fix before spawn.
4. If an owner returns with only exploration and no commits/PR: spawn a **fresh** owner with consolidated scope. Do not resume a premature-done agent.

### Feature brief template

Every Feature-owner brief includes at least:

- Isolation: absolute worktree path, branch name, "do not touch other worktrees / do not dual-drive".
- Ticket: full title + body + accepted grill/spec decisions (or tracker pointers that resolve to them).
- Continuity: "do not stop after spawning explorers/helpers"; deliverable = open PR + head SHA + verify evidence.
- AFK TDD (GATE-005 / GATE-008): reuse existing public test seams in the repo; do **not** ask the human to confirm seams; reversible defaults are fine; record non-obvious defaults in the PR.
- Shell: repo's documented runner (script files / pytest); no fragile multi-line one-liners on shells that mangle them.
- Signing / rebase: repo-allowed non-interactive unsigned commit path if signing hangs; `GIT_EDITOR=true` (or equivalent) for non-interactive `rebase --continue`.
- Land: do not merge to trunk unless this session already has GATE-004 (owners open PRs only).
- Docs: if the change alters user-visible install, run, or feature surface, update the product `README` (and any repo-declared landing/user docs) in the **same PR** when cheap; otherwise open a tiny docs follow-up before claiming the ticket done. Do not invent marketing copy—keep docs accurate to what landed.

### Verify

1. Prefer the repo's runtime verify skill over disposable one-off scripts.
2. Run the worktree's unit/integration suite with the project's documented shell rules (script files / test runner; avoid fragile one-liners on shells that mangle them).
3. If CI deliberately skips a class of tests (GUI, hardware, live network), still run those suites **locally** before land when the change touches those surfaces.
4. Spot-check product README / landing docs against the landed UX when the ticket changed anything a new user would follow.

### Review, then fix blockers

1. After the PR is open—and after every tip that claims to fix a blocking finding—run a two-axis review against the **PR base** (three-dot diff):
   - **Spec**: linked ticket (+ design notes if the repo keeps them).
   - **Standards**: repo coding-standards doc + judgement/smell baseline.
2. Spec/Standards findings that block acceptance → fix on the same PR → re-review that tip. Non-blocking judgement smells alone do not stop Shipping.

### Babysit, then Shipping

1. Drive the PR to green / merge-ready (CI, conflicts). Babysit does not authorize merge by itself. If the repo/plugin babysit watcher needs a runtime (e.g. Bun) that is missing from PATH, `check` mode may use the forge CLI (`gh` / `origin`) for checks + mergeability + unresolved threads instead of stalling.
2. Independent ship verifier (an agent that did **not** author the tip): exercise the change the way a user or agent would; post exactly one of `PASS` / `PASS+NOTES` / `FAIL` on the PR.
3. Land only under GATE-004 after PASS/PASS+NOTES. Prefer squash-merge with head-branch deletion when the forge supports it (`gh pr merge --squash --delete-branch`, or the Origin equivalent). If the wave is merge-ready and still has no grant, apply GATE-006 (one nudge, then wait)—do not stall silently.
4. After each merge lands, read the PR back (`mergedAt`, `baseRefName`, head ref). Confirm the linked tracker ticket is **closed** (or wontfix). If the squash message omitted `Fixes #n` / equivalent and the ticket stayed open, close it with a comment citing the merge SHA—do not assume auto-close.
5. **Stacked / dependent PRs:** when a PR's base was a just-merged sibling (not trunk), retarget the remaining open PR to trunk, move its commits onto the new trunk tip, and publish with `--force-with-lease` on that feature branch only. Re-run verify/review as needed before its own land. Do not leave open PRs pointed at a deleted parent branch.

### Parallel-land hygiene

When a sibling merges first:

1. Rebase the waiting branch onto current trunk before land.
2. Shared catalog/index docs: keep **every** landed feature's name, not one side of the conflict.
3. Shared dispatch/facade seams: keep **every** variant the tickets jointly require; update cross-cutting tests; inject fakes/stubs so unit tests never hit real external engines, devices, or paid APIs.
4. If local commit signing hangs: use a non-interactive unsigned commit path the repo allows for feature branches, including each `rebase --continue`. Publish with `--force-with-lease` only on the feature branch after rebase—not on trunk.

### Environment retro

1. After the wave lands, retrospect the **agent environment** only (AGENTS pointers, standards judgement rules, verify skill, CI guardrails)—not product features.
2. If a lesson earns a durable guardrail, land a **small env-only PR**, then continue post-land hygiene for that retro PR too.
3. If the lesson is an AFK exception (ask-human or special-case), add or sharpen a gate in `human-gates.md` and bump this skill per `VERSION.md`—do not leave it only in chat.
4. Do not block the next ticket on a pure write-up unless a guardrail is already broken.

### Post-land hygiene

After the wave's feature PRs (and any env-retro PR) have landed—or the wave records "no env change earned"—run this sequence **before** opening the next frontier. Do not wait for a separate disk-cleanup ask. Commits stay reachable via the forge after refs are deleted.

1. **Refresh trunk.** In the trunk checkout: `git fetch origin` and fast-forward trunk to `origin/<trunk>` (`pull --ff-only` or equivalent). Next wave's `worktree add` must fork from the landed tip, not a stale local trunk. Apply **GATE-009** if the trunk working tree has unrelated dirty edits.
2. **Reclaim worktrees.** From the trunk checkout, `git worktree list`. Candidates = worktrees this wave created for feature / retro / arena (bakeoff) branches whose PRs are merged or closed as wontfix.
   - **Keep:** trunk checkout; worktrees whose branch still has an open PR; paths with uncommitted **tracked** edits (show the diff and pause); paths an active/pinned chat is still driving.
   - Per reclaimable path: `git worktree remove --force <path>`, then `git worktree prune`. If the directory survives (Windows locks / ignored build artifacts), retry delete once; if still locked, record the absolute path in the wave report and continue—do not stall the next frontier on an empty locked folder.
   - Arena / design-bakeoff siblings: reclaim once synthesis is landed or discarded. Keep only durable synthesis docs that earned a place in the product repo—not the scratch trees.
3. **Delete merged local branches.** For each this-wave branch that is fully landed (or arena scratch with no open PR): `git branch -d <branch>` after its worktree is gone. If `-d` refuses because the tip was squash-merged (not an ancestor), confirm the PR is merged on the forge, then `git branch -D <branch>`. Also drop local one-off verify refs (`pr-*-verify`, etc.) created for that wave. Never `-D` a branch that still has an open PR or unpushed unique commits you still need.
4. **Delete merged remote heads + prune.** If merge did not already delete the head: `git push origin --delete <branch>` (or forge CLI) for each landed this-wave branch. Then `git fetch --prune` so `: gone` tracking refs disappear. Prefer enabling the forge setting "delete head branch on merge" for the repo so this step is mostly a prune. Do **not** delete protected trunk or release tags.
5. **Trunk dirt check.** Uncommitted tracked edits on trunk that appeared during the wave: apply **GATE-009** (fold into a ticket / tiny follow-up PR, or discard only when clearly accidental). Untracked throwaway under ignored build/output dirs is not wave debt unless it blocks reclaim; do not invent a product cleanup ticket for caches.

## Completion audit

Treat completion as unproven until checked against live state:

1. No in-scope ready tickets remain open (or only tickets whose blockers are out of scope).
2. Every in-scope ticket is **closed** on the tracker (forge state, not chat memory); every feature PR is merged (or explicitly wontfix).
3. Latest wave retro merged, or recorded as "no env change earned".
4. Trunk checkout is fast-forwarded to `origin/<trunk>` (or the divergence is explained).
5. Session-created feature / retro / arena worktrees are removed, or each survivor has an explicit keep reason (open PR, tracked WIP, in-use chat). Locked empty dirs named in a wave report count as known debt, not as "still active".
6. This-wave merged local branches and remote heads are gone (or listed with keep reasons); `git fetch --prune` has run so stale tracking refs are not left as `: gone` noise.
7. No open PR in the wave still bases on a deleted/merged parent branch—all remaining open work bases on trunk (or an intentional live stack).
8. User-facing README / landing docs match what the wave shipped, or a docs follow-up is explicitly opened.
9. Prefer tracker + trunk tip evidence over chat memory.

## Anti-patterns

- Ending a Feature turn while nested explorers are the only progress.
- Treating green CI as a ship verdict—especially when CI skips expensive surfaces.
- Merging on the author's self-check without an independent PASS/PASS+NOTES.
- Resolving merge conflicts by dropping a sibling's feature from a shared index or dispatch table.
- Asking the human for reversible defaults covered by GATE-005.
- Starting Build while in-scope tickets still carry only `needs-triage` after Align finished (GATE-007).
- Spawning a Feature owner whose brief still asks the human to confirm TDD seams (GATE-008).
- Inventing a one-off human exception in chat without registering it in `human-gates.md` on the next skill bump.
- Leaving merged-wave worktrees, local branches, or remote heads on disk/forge after land; post-land hygiene is part of the wave, not a later optional cleanup playbook.
- Opening the next wave from a stale local trunk that is behind `origin/<trunk>`.
- Leaving stacked PRs aimed at a parent branch that already merged/deleted.
- Assuming a ticket closed because the PR merged, without reading tracker state.
- Shipping user-visible behavior while leaving the product README / install steps stale.

---
name: long-running-dev-waves
description: >-
  Orchestrate long unattended development across a backlog of ready tickets:
  dependency-ordered waves, isolated worktrees, verify, two-axis review,
  independent ship verdict, merge, then environment retro. Use for AFK
  multi-issue delivery, auto-advance by blocked-by, or session-length coding
  goals that must not invent product scope. Maintains a versioned human-gates
  register for ask-human and special-case exceptions.
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

### Verify

1. Prefer the repo's runtime verify skill over disposable one-off scripts.
2. Run the worktree's unit/integration suite with the project's documented shell rules (script files / test runner; avoid fragile one-liners on shells that mangle them).
3. If CI deliberately skips a class of tests (GUI, hardware, live network), still run those suites **locally** before land when the change touches those surfaces.

### Review, then fix blockers

1. After the PR is open—and after every tip that claims to fix a blocking finding—run a two-axis review against the **PR base** (three-dot diff):
   - **Spec**: linked ticket (+ design notes if the repo keeps them).
   - **Standards**: repo coding-standards doc + judgement/smell baseline.
2. Spec/Standards findings that block acceptance → fix on the same PR → re-review that tip. Non-blocking judgement smells alone do not stop Shipping.

### Babysit, then Shipping

1. Drive the PR to green / merge-ready (CI, conflicts). Babysit does not authorize merge by itself. If the repo/plugin babysit watcher needs a runtime (e.g. Bun) that is missing from PATH, `check` mode may use the forge CLI (`gh` / `origin`) for checks + mergeability + unresolved threads instead of stalling.
2. Independent ship verifier (an agent that did **not** author the tip): exercise the change the way a user or agent would; post exactly one of `PASS` / `PASS+NOTES` / `FAIL` on the PR.
3. Land only under GATE-004 after PASS/PASS+NOTES; confirm the ticket closed. If the wave is merge-ready and still has no grant, apply GATE-006 (one nudge, then wait)—do not stall silently.

### Parallel-land hygiene

When a sibling merges first:

1. Rebase the waiting branch onto current trunk before land.
2. Shared catalog/index docs: keep **every** landed feature's name, not one side of the conflict.
3. Shared dispatch/facade seams: keep **every** variant the tickets jointly require; update cross-cutting tests; inject fakes/stubs so unit tests never hit real external engines, devices, or paid APIs.
4. If local commit signing hangs: use a non-interactive unsigned commit path the repo allows for feature branches, including each `rebase --continue`. Publish with `--force-with-lease` only on the feature branch after rebase—not on trunk.

### Environment retro

1. After the wave lands, retrospect the **agent environment** only (AGENTS pointers, standards judgement rules, verify skill, CI guardrails)—not product features.
2. If a lesson earns a durable guardrail, land a **small env-only PR**, then open the next frontier.
3. If the lesson is an AFK exception (ask-human or special-case), add or sharpen a gate in `human-gates.md` and bump this skill per `VERSION.md`—do not leave it only in chat.
4. Do not block the next ticket on a pure write-up unless a guardrail is already broken.

## Completion audit

Treat completion as unproven until checked against live state:

1. No in-scope ready tickets remain open (or only tickets whose blockers are out of scope).
2. Every in-scope ticket is closed; every feature PR is merged (or explicitly wontfix).
3. Latest wave retro merged, or recorded as "no env change earned".
4. Prefer tracker + trunk tip evidence over chat memory.

## Anti-patterns

- Ending a Feature turn while nested explorers are the only progress.
- Treating green CI as a ship verdict—especially when CI skips expensive surfaces.
- Merging on the author's self-check without an independent PASS/PASS+NOTES.
- Resolving merge conflicts by dropping a sibling's feature from a shared index or dispatch table.
- Asking the human for reversible defaults covered by GATE-005.
- Starting Build while in-scope tickets still carry only `needs-triage` after Align finished (GATE-007).
- Spawning a Feature owner whose brief still asks the human to confirm TDD seams (GATE-008).
- Inventing a one-off human exception in chat without registering it in `human-gates.md` on the next skill bump.

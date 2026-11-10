# Human gates and special cases

Living register of situations that need a human, or that need a named special-case procedure instead of the default AFK loop. Grow this file over skill versions; do not dump one-off product facts here.

**Read when:** about to ask the user, about to take an irreversible action, or a wave hits a failure the main loop does not name.

## Entry template

Copy for each new gate (fill every field):

```markdown
### GATE-YYY — short title

- **since:** 0.x.0
- **kind:** `ask-human` | `special-case` | `never-ask`
- **when:** …
- **do:** …
- **do not:** …
- **override:** what the human can say to change the default
```

Kinds:

- **ask-human** — stop and ask; do not invent a product answer.
- **special-case** — do not ask; follow the named procedure, then continue.
- **never-ask** — must not escalate; apply the default and document it.

## Active gates

### GATE-001 — product preference

- **since:** 0.1.0
- **kind:** `ask-human`
- **when:** two or more product behaviors are compatible with the ticket and no default is stated in the ticket, product plan, or prior operator grant.
- **do:** ask once, with the options and the impact of each.
- **do not:** pick silently when the fork changes user-visible behavior the ticket left open.
- **override:** operator names the preferred option in plain language.

### GATE-002 — verify FAIL needs scope change

- **since:** 0.1.0
- **kind:** `ask-human`
- **when:** runtime or acceptance verify fails, and fixing it requires shrinking, expanding, or reinterpreting the ticket beyond a clear bugfix.
- **do:** stop the wave for that ticket; report evidence and the scope choices.
- **do not:** silently drop acceptance criteria or open unbounded follow-up scope.
- **override:** operator picks a scope amendment or marks the ticket needs-info / wontfix.

### GATE-003 — irreversible operations

- **since:** 0.1.0
- **kind:** `ask-human`
- **when:** force-push to shared trunk, delete production data, revoke credentials, or any action the repo/session marks irreversible.
- **do:** ask and wait.
- **do not:** use feature-branch `--force-with-lease` after a documented rebase as an excuse to force trunk.
- **override:** explicit operator approval for that action.

### GATE-004 — land grant

- **since:** 0.1.0
- **kind:** `special-case`
- **when:** the session (or standing user rule) already grants land-on-PASS. Recognized grants include plain language such as `PASS 可 land` / `PASS may land`, `合入授权`, `一直做到合入`, `AFK land`, `自动完成所有开发任务` (when said together with implement/land intent), or a standing user rule that Shipping PASS may merge.
- **do:** after independent Shipping PASS or PASS+NOTES, squash-merge (or repo land command) without asking again; confirm the ticket closed.
- **do not:** merge on green CI alone, or on the author agent's self-check. Do not treat bare `进行开发` / `实现` / `并行` / `自动审查` alone as a land grant.
- **override:** operator revokes the grant or requires an extra human approve step.

### GATE-005 — reversible operator defaults

- **since:** 0.1.0
- **kind:** `never-ask`
- **when:** the ticket or product plan implies a default, or the fork is reversible and does not change acceptance.
- **do:** apply the default; record it in the PR or design note; mention how to override.
- **do not:** block the wave asking "should I proceed?" on reversible work.
- **override:** operator names a different default later; open a follow-up if needed.

### GATE-006 — acquire land grant without silent stall

- **since:** 0.2.0
- **kind:** `ask-human`
- **when:** a long-running wave has no GATE-004 grant yet, or every in-scope PR already has Shipping PASS/PASS+NOTES and is merge-ready but still lacks a grant.
- **do:**
  1. **At goal arm / wave start** (no grant yet): ask **once**, yes/no — 「本波 Shipping PASS/PASS+NOTES 后是否可直接 land？」 Recommended default to offer: yes for AFK waves. Record the answer in the durable goal text.
  2. **When Shipping first makes the wave merge-ready** and still no grant: send **one** short nudge with PR links +「回复 PASS 可 land 即合入」. Then wait; do not keep building inventing scope.
- **do not:** stay silent for hours after merge-ready; ask repeatedly; merge without a grant; re-ask after a grant was already recorded this session.
- **override:** operator answers the start question or the nudge; or includes a GATE-004 phrase in the original invoke so this gate never fires.

### GATE-007 — Align closeout retargets ready labels

- **since:** 0.3.0
- **kind:** `special-case`
- **when:** Align for in-scope tickets just reached shared understanding (grill decisions accepted, spec/tickets settled) and those tickets still carry a pre-ready label such as `needs-triage`.
- **do:** before spawning Feature owners, remove the pre-ready triage label and add the repo's AFK-ready label (read the vocabulary from `AGENTS.md` / triage docs; common Matt default is `ready-for-agent`). Comment on the ticket only if the tracker needs an explicit handoff note.
- **do not:** leave Align-complete tickets on `needs-triage` and still call them the ready frontier; do not invent new label vocabulary when the repo already defines one.
- **override:** operator names a different ready label or keeps a ticket in `needs-info` / `ready-for-human` on purpose.

### GATE-008 — AFK Feature briefs skip seam interviews

- **since:** 0.3.0
- **kind:** `never-ask`
- **when:** this skill spawns a Feature (or Bug fix) owner, including when a nested `/tdd` skill would normally ask the human to confirm test seams.
- **do:** put the Feature brief template lines in the owner prompt: reuse existing public test seams; do not ask the human to confirm seams; apply GATE-005 for reversible defaults; note non-obvious defaults in the PR body.
- **do not:** block the wave on "which seams should we test?"; do not strip TDD—still red-green at those seams.
- **override:** operator explicitly demands a seam interview for that ticket (rare); then ask once and record the answer in the ticket.

### GATE-009 — orphan edits on trunk during a wave

- **since:** 0.6.0
- **kind:** `special-case`
- **when:** post-land hygiene finds uncommitted **tracked** edits on the trunk checkout that are not part of an intentional in-progress follow-up the operator already named.
- **do:** classify quickly: (a) fold into an existing open ticket or a tiny follow-up PR and open it; (b) if clearly accidental residue from a merged change, discard after showing the diff in the wave report; (c) if product preference is unclear, ask once (GATE-001) then continue.
- **do not:** silently leave dirty trunk and spawn the next wave from it; do not `reset --hard` without classifying; do not invent a large cleanup ticket for ignored build/output caches.
- **override:** operator says keep the dirty tree / names which paths to preserve.

## Deferred / to add in later versions

Use this list as a backlog for skill authors (not runtime noise). Move an item into **Active gates** only when the procedure is sharp enough to follow without improvising.

- Model-quota mid review/ship: switch family and continue (candidate special-case).
- Premature Feature owner exit: fresh owner + consolidated brief (candidate special-case; see `reference.md`).
- CI skips expensive surfaces: local re-run before land (candidate special-case).
- Apply/command no-op vs UI side effects (candidate standards pointer for env retro).
- Parallel seam union on rebase (already in main skill Parallel-land hygiene; promote only if a sharper gate is needed).
- Tracker comments via shell here-strings that break on CJK punctuation: prefer `--body-file` (candidate special-case).
- Babysit watcher runtime missing (Bun/`watch-pr`): forge-CLI check already in `SKILL.md`; promote only if a sharper ask/fallback order is needed.

# Version registry

**Current:** `0.6.0`

Semver for this skill only (not the product repo).

| Version | Kind | Summary |
| --- | --- | --- |
| `0.6.0` | minor | Post-land hygiene (trunk, branches, prune, stack retarget, ticket close, README); GATE-009 |
| `0.5.0` | minor | Worktree reclaim after each wave land; completion-audit survivors; orphan-worktree failure mode |
| `0.4.0` | minor | Strip product brand from GATE-007; skip to-spec on single ready cuts; GATE-004「自动完成」; babysit forge-CLI fallback |
| `0.3.0` | minor | GATE-007 Align→ready labels; GATE-008 AFK Feature brief / TDD seams; brief template |
| `0.2.0` | minor | GATE-006 land-grant acquisition; clearer GATE-004 phrases; no silent stall after PASS |
| `0.1.0` | initial | Portable AFK wave loop; baseline human gates; failure-mode reference |

## How to bump

1. Edit `SKILL.md` and/or disclosed refs (`human-gates.md`, `reference.md`).
2. Append a row to the table above (newest at top after Current).
3. Add a matching section under **Changelog** below.
4. Set **Current** to the new version.
5. If the change is a new ask-human exception or special-case handler, also add/update an entry in `human-gates.md` with `since: <version>`.
6. If the skill's one-line behavior summary changed, refresh `description` in `SKILL.md` frontmatter **and** the copies in `.claude-plugin/marketplace.json`, `original/INDEX.md`, and the matching blurb in the repo root `README.md`.

### Version kind

- **patch** (`0.1.x`): clarify wording; fix a broken pointer; no new gate.
- **minor** (`0.x.0`): new human-gate, new special-case procedure, or new anti-pattern that changes agent behavior.
- **major** (`x.0.0`): incompatible loop change (e.g. Shipping no longer required, or Align/Build order inverted).

## Changelog

### 0.6.0

- **Post-land hygiene** replaces bare worktree reclaim: refresh trunk, reclaim worktrees, delete merged local branches, delete/prune remote heads, trunk dirt check (GATE-009).
- **Shipping**: prefer `--delete-branch`; confirm tracker ticket closed; stacked PR retarget onto trunk.
- **Docs**: Feature brief + verify + completion audit require product README / landing docs to match user-visible landings.
- **GATE-009**: orphan tracked edits on trunk during a wave.
- `reference.md`: stale trunk/refs, stacked dead base, open ticket after squash, README lag.
- Skill-author note: bump must refresh marketplace / INDEX / root README when the description changes.

### 0.5.0

- **Worktree reclaim**: after feature (+ env-retro) land, remove this wave's feature / retro / arena worktrees before the next frontier; keep trunk, open-PR, tracked WIP, and in-use chats.
- **Completion audit**: session-created isolation trees must be gone or named with a keep reason; locked empty dirs are reported debt, not active worktrees.
- **Anti-pattern**: leaving merged-wave worktrees for a later disk-cleanup playbook.
- `reference.md`: orphan worktrees after land.

### 0.4.0

- **GATE-007**: drop product brand name; read AFK-ready label from repo docs (Matt default still cited as common example).
- **Align**: skip `/to-spec` / `/to-tickets` for a single already-ready cut with recorded grill/spec; document the invented-split failure mode.
- **GATE-004**: recognize `自动完成所有开发任务` (with implement/land intent); bare `自动审查` alone is not a grant.
- **Babysit**: forge-CLI check fallback when the plugin watcher runtime is missing from PATH.

### 0.3.0

- **GATE-007**: after Align shared understanding, retarget `needs-triage` → `ready-for-agent` (repo vocabulary) before Build.
- **GATE-008**: Feature owners must not ask humans to confirm TDD seams; reuse existing seams under GATE-005.
- **Feature brief template** in `SKILL.md`; frontier guidance to serialize shared shell-file edits.
- `reference.md`: Align-label stall, TDD seam interview stall, shared-shell false parallelism.

### 0.2.0

- **GATE-006**: ask once at wave start for land-on-PASS; one nudge when merge-ready without a grant; forbid silent multi-hour stalls.
- **GATE-004**: list recognized grant phrases; bare「进行开发」is not a land grant.
- Parse-goal / Shipping steps and `reference.md` updated for the stall mode.

### 0.1.0

- Initial portable skill: frontier waves, worktree Feature owners, verify, Spec/Standards, independent Shipping, parallel-land hygiene, env retro, completion audit.
- Baseline human gates recorded in `human-gates.md`.
- Portable failure modes in `reference.md`.

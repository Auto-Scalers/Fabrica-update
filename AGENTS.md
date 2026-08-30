# Fabrica-update — Worker Instructions (AGENTS.md)

## What This Folder Is
This is the **Fabrica update pipeline** sub-project — the workspace for analyzing the Orca↔Fabrica diff and planning/syncing upstream Orca updates into the Fabrica fork(s). It is **read-only by default** (discovery + analysis + planning). No code changes to any Fabrica project are made from here. Workers produce analysis artifacts (diff maps, sync plans) inside this repo.

## Scope (per PM 2026-08-29)
**In scope** (to analyze for the Orca→Fabrica sync):
- `Fabrica-app/` — the forked desktop app (baseline = `.backup/orca`)
- `Fabrica-plugins/` — the plugin marketplace index
- All plugin repos inside `Fabrica-plugins/` — the 8 plugin repos (added as submodules)

**Out of scope** (built fresh, not forked from Orca):
- `Fabrica-web/`, `Fabrica-relay/`, `Fabrica-atlas/`, `Fabrica-marketing/`

The upstream scope: `https://github.com/stablyai/orca` (the app) and `https://github.com/stablyai` (the org — for the plugin repos' upstream sources).

## Sources in this workspace
- `orca-baseline/` — the **frozen Orca source Fabrica was rebranded from** (copy of `.backup/orca` from Fabrica-app). **Read-only reference. Do not edit.**
- `upstream-orca/` — the **current upstream Orca** (`https://github.com/stablyai/orca`, cloned). **Read-only comparison source. Do not edit.**
- `.Fabrica-update-board/` — the task file + the **historical roadmaps & task files** copied in for study (the detailed record of every change we made during the rebrand, including archived logs in `.archive/`).
- Historical docs in `.Fabrica-update-board/historical/` — root roadmap/DNA, App/Plugins task files + archives, for pattern study.

## Tech Stack / Commands
This sub-project is primarily a **read-only analysis workspace**. No app to build or test. Workers use:
- `git diff` / `diff -ru` for structural diffs.
- `grep` / `rg` for content searches.
- File reads for historical docs.
- Standard CLI tools. No build step.

## Plan
The master plan lives at `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md`. It defines:
- 3 sources (`.backup/orca` baseline, current Fabrica, new upstream Orca).
- 3 phases of analytics: (1) rebrand diff, (2) upstream diff, (3) implementation mapping.
- The "massive file" output: `REBRAND-DIFF-MAP` — line-mapped diff with intent tags + our patterns.
- Phase 4: sync script + runbook + CI guard.
- Strict rules: preserve everything we changed; adapt updates to our patterns; read-only on `.backup/orca` + upstream clone.

All work here follows that plan.

## Definition of Done
A task is DONE only when ALL of these hold:
1. The deliverable artifact exists in the correct location (per the plan: `REBRAND-DIFF-MAP.md`/`.json`, `UPSTREAM-DIFF-MAP.md`/`.json`, `SYNC-IMPLEMENTATION-PLAN.md`, `UPDATE-SYNC-RUNBOOK.md`).
2. The artifact is grounded in the actual source code (`.backup/orca`, `upstream-orca/`, the in-scope Fabrica repos) with line-level evidence.
3. Historical task files + archived logs were consulted to capture our rebrand patterns.
4. **No code changes** to any Fabrica project were made — this sub-project is read-only on others. No commits/pushes to other repos.
5. Tracking files updated in the same edit: task status + Rollup recount + Checkpoint + Session Ledger in `.Fabrica-update-board/Fabrica-update-tasks.md`.

## What You Do NOT Do
- **Do NOT edit** `orca-baseline/` or `upstream-orca/` (read-only references). Do not commit/push to those.
- **Do NOT edit** any file in `Fabrica-app/`, `Fabrica-plugins/`, or the plugin repos. This sub-project is **discovery + analysis only**. No code changes to other projects.
- **Do NOT edit** `.backup/`/`_sources/` in any project.
- **Do NOT commit or push** the in-scope Fabrica repos from here.
- **Do NOT add new dependencies** without explicit instruction.

## Key Directories
```
orca-baseline/                         — frozen Orca fork baseline (read-only)
upstream-orca/                         — current upstream Orca (read-only, for comparison)
.Fabrica-update-board/                 — task file + historical docs + our outputs
  Fabrica-update-tasks.md              — single source of truth for our work
  historical/                          — copied roadmaps + task files + archives (study)
  REBRAND-DIFF-MAP.md / .json          — Phase 1 output (T1)
  UPSTREAM-DIFF-MAP.md / .json         — Phase 2 output (T3)
  SYNC-IMPLEMENTATION-PLAN.md          — Phase 3 output (T4)
  UPDATE-SYNC-RUNBOOK.md               — Phase 4 output (T5)
```

## Parallelism & Anti-Overlap Policy
Standard. One task = one worker. Claim `IN_PROGRESS` + record handle in Session Ledger before starting. No overlap. Quality bar: no DONE without evidence; Rollup updated in the same edit as any status change.

## Resume Protocol
On heartbeat kick or session resume: read `.Fabrica-update-board/Fabrica-update-tasks.md` Checkpoint FIRST. Continue from Next Action.

## How to Send Results
Workers report via standard mechanisms (the root orchestrator dispatches and reviews). On completion the orchestrator verifies (grep/read) before promoting.

## Orchestration IDs
Standard `task_xxx` / `ctx_xxx` / `term_xxx` IDs, recorded in this file's Session Ledger.

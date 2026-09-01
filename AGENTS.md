# Fabrica-update — Worker Instructions (AGENTS.md)

## What This Folder Is

This is the **Fabrica update pipeline** sub-project — the workspace for analyzing the Orca↔Fabrica diff and planning upstream Orca updates into Fabrica. It is **read-only by default** (discovery + analysis + planning). No code changes to any Fabrica project are made from here.

## What Exists

| Directory/File | What It Is |
|---|---|
| `orca-baseline/` | Frozen Orca source Fabrica was rebranded from (copy of `.backup/orca`). **Read-only.** |
| `upstream-orca/` | Current upstream Orca (`stablyai/orca`). **Read-only.** |
| `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md` | Master plan: two-diff framework, 4 phases, strict preservation rules. |
| `.Fabrica-update-board/Fabrica-update-tasks.md` | Task file — single source of truth for execution. |

No deliverables produced yet. This is a read-only analysis workspace.

## Scope

**In scope:** `Fabrica-app/`, `Fabrica-plugins/`, and all 8 plugin repos inside `Fabrica-plugins/`.
**Out of scope:** `Fabrica-web/`, `Fabrica-relay/`, `Fabrica-atlas/`, `Fabrica-marketing/`.

Upstream sources: `https://github.com/stablyai/orca` (the app) and `https://github.com/stablyai` (the org — for plugin upstreams).

## Tech Stack / Commands

Read-only analysis workspace. No build step. Workers use:
- `git diff` / `diff -ru` for structural diffs.
- `grep` / `rg` for content searches.
- File reads for code analysis.

## Plan

Master plan: `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md`. Defines:
- 3 sources (`.backup/orca` baseline, current Fabrica, new upstream Orca).
- 4 phases: (1) rebrand diff, (2) upstream diff, (3) implementation mapping, (4) sync workflow.
- Strict rules: preserve everything we changed; adapt updates to our patterns; read-only on `orca-baseline/` + `upstream-orca/`.

## Definition of Done

A task is DONE only when:
1. The deliverable artifact exists in the correct location (per the plan).
2. The artifact is grounded in the actual source code with line-level evidence.
3. **No code changes** to any Fabrica project — read-only on others.
4. Tracking files updated in the same edit: task status + Rollup + Checkpoint + Session Ledger.

## What You Do NOT Do

- **Do NOT edit** `orca-baseline/` or `upstream-orca/` (read-only references).
- **Do NOT edit** any file in `Fabrica-app/`, `Fabrica-plugins/`, or the plugin repos.
- **Do NOT edit** `.backup/`/`_sources/` in any project.
- **Do NOT commit or push** any Fabrica repo from here.
- **Do NOT add new dependencies** without explicit instruction.

## Key Directories

```
orca-baseline/                         — frozen Orca fork baseline (read-only)
upstream-orca/                         — current upstream Orca (read-only, for comparison)
.Fabrica-update-board/
  Fabrica-update-tasks.md              — single source of truth for our work
  UPDATE-PIPELINE-PLAN.md              — the master plan
  .archive/                            — archived/old versions of tracking files
```

## Parallelism & Anti-Overlap Policy

Standard. One task = one worker. Claim `IN_PROGRESS` + record handle in Session Ledger before starting. No overlap. Quality bar: no DONE without evidence; Rollup updated in the same edit as any status change.

## Resume Protocol

On heartbeat kick or session resume: read `.Fabrica-update-board/Fabrica-update-tasks.md` Checkpoint FIRST. Continue from Next Action.

## How to Send Results

Workers report via standard mechanisms (the root orchestrator dispatches and reviews). On completion the orchestrator verifies (grep/read) before promoting.

## Orchestration IDs

Standard `task_xxx` / `ctx_xxx` / `term_xxx` IDs, recorded in this file's Session Ledger.

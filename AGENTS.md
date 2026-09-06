# Fabrica-update — Worker Instructions (AGENTS.md) (v2)

## What This Folder Is

This is the **Fabrica update pipeline** sub-project — the workspace that keeps the new `Auto-Scalers/Fabrica` repo (a clean fork of `stablyai/orca`) in sync with upstream, while layering our rebrand and custom logic on top.

**Strategy (v2, since 2026-09-03):** we **fork** upstream into a brand-new `Fabrica/` repo, then apply rebrand substitutions and re-implement custom logic on top of that clean fork. We no longer try to "port" upstream content into the old `Fabrica-app/` fork.

**Mode:** Read-only on `orca-baseline/`, `upstream-orca/`, and (until T1) the existing `Fabrica-app/` repo. Writes happen in the new `Fabrica/` repo (created at T1) and in the tracking files under `.Fabrica-update-board/`.

## What Exists

| Directory/File | What It Is |
|---|---|
| `orca-baseline/` | Frozen Orca source the **old** `Fabrica-app` was rebranded from. Used as the "what we did" reference. **Read-only.** |
| `upstream-orca/` | Current upstream Orca (`stablyai/orca`) at a pinned commit. The starting point for the new `Fabrica` fork. **Read-only.** |
| `Fabrica-app/` | Old Fabrica fork (v0.0.6, `0a5d258`). The source of truth for "what we did" (rebrand + custom logic). **Read-only.** |
| `../Fabrica/` | New repo (fresh clone of upstream). All v2 work happens here. |
| `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md` | Master plan v2: fork-from-upstream, rebrand pass, custom-logic re-apply. |
| `.Fabrica-update-board/Fabrica-update-tasks.md` | Task file — single source of truth for execution. |

## Scope

**In scope:** new `Fabrica/` repo (post-T1), `Fabrica-plugins/`, and all 8 plugin repos.
**Out of scope:** `Fabrica-web/`, `Fabrica-relay/`, `Fabrica-atlas/`, `Fabrica-marketing/`.

Upstream sources: `https://github.com/stablyai/orca` (the app), `https://github.com/stablyai/orca-plugins` (the plugins).

## Tech Stack / Commands

Analysis + transformation. Workers use:
- `git diff` / `diff -ru` for structural diffs.
- `grep` / `rg` for content searches (incl. rebrand residual verification).
- File reads for code analysis.
- After T1: writes to the new `Fabrica/` repo (case-sensitive, longest-match substitutions per the rebrand table in the plan).

## Plan

Master plan: `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md` (v2). Defines:
- 3 sources: `orca-baseline/` (what we did), `upstream-orca/` (what Orca did now), new `Fabrica/` (where we apply it).
- 8 tasks (T0–T7): pin → fork → rebrand intent diff → upstream diff → custom-logic map → apply rebrand → re-implement custom logic → final verification.
- Strict rules: case-sensitive longest-match substitutions, idempotent rebrand pass, no commits/pushes by workers.

## Definition of Done

A task is DONE only when:
1. The deliverable artifact exists in the correct location (per the plan).
2. The artifact is grounded in the actual source code with line-level evidence.
3. No code changes to read-only workspaces (`orca-baseline/`, `upstream-orca/`, `Fabrica-plugins/` until promoted).
4. Tracking files updated in the same edit: task status + Rollup + Checkpoint + Session Ledger.
5. **Rebrand verification:** residual `orca`/`stably`/`stablyai` count = 0 in the new `Fabrica/` after T5 and T6.

## What You Do NOT Do

- **Do NOT edit** `orca-baseline/` or `upstream-orca/` (read-only references).
- **Do NOT edit** `Fabrica-app/` (old fork — frozen at `3e3ff56`; it's a read-only reference for T2).
- **Do NOT edit** `Fabrica-plugins/` or the plugin repos directly from this orchestrator — they have their own worktrees.
- **Do NOT edit** `.backup/`/`_sources/` in any project.
- **Do NOT commit or push** any repo from here — orchestrator handles git.
- **Do NOT add new dependencies** without explicit instruction.

## Key Directories

```
orca-baseline/                         — frozen old-fork baseline (read-only)
upstream-orca/                         — current upstream Orca (read-only)
Fabrica-app/                           — old Fabrica fork v0.0.6 (read-only reference)
.Fabrica-update-board/
  Fabrica-update-tasks.md              — single source of truth for our work
  UPDATE-PIPELINE-PLAN.md              — master plan v2
  .archive/                            — archived v1 plan, old logs, snapshots
../Fabrica/                            — new repo (fresh clone of upstream, target of v2 work)
```

## Parallelism & Anti-Overlap Policy

Standard. One task = one worker. Claim `IN_PROGRESS` + record handle in Session Ledger before starting. No overlap. Quality bar: no DONE without evidence; Rollup updated in the same edit as any status change.

## Resume Protocol

On heartbeat kick or session resume: read `.Fabrica-update-board/Fabrica-update-tasks.md` Checkpoint FIRST. Continue from Next Action.

## How to Send Results

Workers report via standard mechanisms (the root orchestrator dispatches and reviews). On completion the orchestrator verifies (grep/read) before promoting.

## Orchestration IDs

Standard `task_xxx` / `ctx_xxx` / `term_xxx` IDs, recorded in this file's Session Ledger.

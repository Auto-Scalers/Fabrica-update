# Fabrica-update

Read-only analysis workspace for the Fabrica update pipeline — maps the Orca↔Fabrica diff surface and plans how to sync upstream Orca updates without breaking the rebrand.

## What This Is

- `orca-baseline/` — frozen Orca source Fabrica was rebranded from (copy of `.backup/orca`). Read-only reference.
- `upstream-orca/` — current upstream Orca (`stablyai/orca`). Read-only comparison source.
- `.Fabrica-update-board/` — task file, master plan, and analysis outputs.

No code is written here. This workspace produces analysis artifacts (diff maps, sync plans) that guide how upstream Orca changes land in Fabrica.

## Scope

**In scope:** `Fabrica-app/`, `Fabrica-plugins/`, and all 8 plugin repos inside `Fabrica-plugins/`.
**Out of scope:** `Fabrica-web/`, `Fabrica-relay/`, `Fabrica-atlas/`, `Fabrica-marketing/` (built fresh, not forked from Orca).

## Structure

```
orca-baseline/                          # Frozen Orca fork baseline (read-only)
upstream-orca/                          # Current upstream Orca (read-only)
.Fabrica-update-board/
  Fabrica-update-tasks.md               # Task file — single source of truth
  UPDATE-PIPELINE-PLAN.md               # Master plan: 2-diff framework, 4 phases
  .archive/                             # Archived/old tracking files
AGENTS.md                               # Worker instructions
```

## Pipeline Phases

| Phase | What | Output |
|-------|------|--------|
| 1 | Rebrand diff (`orca-baseline` vs `Fabrica-app`) | `REBRAND-DIFF-MAP.md` + `.json` |
| 2 | Upstream diff (`orca-baseline` vs `upstream-orca`) | `UPSTREAM-DIFF-MAP.md` + `.json` |
| 3 | Implementation mapping (cross-reference both diffs) | `SYNC-IMPLEMENTATION-PLAN.md` |
| 4 | Sync script + runbook | `UPDATE-SYNC-RUNBOOK.md` |

## Related

- Master plan: `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md`
- Roadmap status: `.Fabrica-board/Fabrica-Roadmap.md`

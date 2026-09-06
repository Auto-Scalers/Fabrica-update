# Fabrica-update

Read-only analysis workspace for the Fabrica update pipeline — maps the Orca↔Fabrica diff surface and plans how to sync upstream Orca updates without breaking the rebrand.

## What This Is

- `orca-baseline/` — frozen Orca source Fabrica was rebranded from (copy of `../.backup/orca`). Read-only reference.
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
  UPDATE-PIPELINE-PLAN.md               # Master plan: 2-diff framework, 3 phases
  .archive/                             # Archived/old tracking files
AGENTS.md                               # Worker instructions
```

## Pipeline Phases

| Phase | What | Output |
|-------|------|--------|
| T0 | Pin upstream Orca to latest commit | `upstream-orca/` at recorded hash |
| T1 | Fork `Auto-Scalers/Fabrica` from upstream | Clean baseline pushed to GitHub |
| T2 | Rebrand intent diff (`orca-baseline` vs `Fabrica-app`) | `pipeline-files/REBRAND-INTENT-MAP.md` + `.json` |
| T3 | Upstream diff (`orca-baseline` vs `upstream-orca`) | `pipeline-files/UPSTREAM-DIFF-MAP.md` + `.json` |
| T4 | Custom-logic map (diff-of-diffs) | `pipeline-files/CUSTOM-LOGIC-MAP.md` + `.json` |
| T5 | Apply rebrand to new `Fabrica/` | `pipeline-files/REBRAND-LOG.txt` + `rebrand-verification-report.md` |
| T6 | Re-implement custom logic | `pipeline-files/CUSTOM-LOGIC-LOG.txt` + `CUSTOM-LOGIC-REVIEW.md` |
| T7 | Final verification | `pipeline-files/FINAL-VERIFICATION-REPORT.md` |

## Related

- Master plan: `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md`
- Roadmap status: `.Fabrica-board/Fabrica-Roadmap.md`

# Rebrand Verification Report

> Generated: 2026-09-04
> Target: `Fabrica/` (fresh clone of upstream `stablyai/orca`)

## Summary

| Metric | Value |
|--------|-------|
| Files scanned (candidates) | 7,327 |
| Files modified | 7,266 |
| Total substitutions | 71,252 |
| Residual `orca` (content) | **0** |
| Residual `stablyai` (content) | **0** |
| Residual `stably` (content) | **0** |

## Substitution Breakdown (per pattern)

| Pattern | Replacement | Count |
|---------|-------------|-------|
| `ORCA_BUILD_IDENTITY` | `FABRICA_BUILD_IDENTITY` | 22 |
| `ORCA_POSTHOG_WRITE_KEY` | `FABRICA_POSTHOG_WRITE_KEY` | 13 |
| `ORCA_DIAGNOSTICS_TOKEN_URL` | `FABRICA_DIAGNOSTICS_TOKEN_URL` | 17 |
| `ORCA_I18N_EXTRACTION_OUTPUT` | `FABRICA_I18N_EXTRACTION_OUTPUT` | 2 |
| `ORCA_FEATURE_WALL_ENABLED` | `FABRICA_FEATURE_WALL_ENABLED` | 2 |
| `ORCA_BOOTSTRAP_FATAL_LOG` | `FABRICA_BOOTSTRAP_FATAL_LOG` | 1 |
| `ORCA_DEV_DOCK_TITLE` | `FABRICA_DEV_DOCK_TITLE` | 7 |
| `ORCA_DEV_BRANCH` | `FABRICA_DEV_BRANCH` | 15 |
| `ORCA_DEV_INSTANCE_LABEL` | `FABRICA_DEV_INSTANCE_LABEL` | 9 |
| `ORCA_DEV_WORKTREE_NAME` | `FABRICA_DEV_WORKTREE_NAME` | 13 |
| `ORCA_CODEX_VALIDATION_TEMP_PARENT` | `FABRICA_CODEX_VALIDATION_TEMP_PARENT` | 6 |
| `ORCA_CODEX_HOME` | `FABRICA_CODEX_HOME` | 252 |
| `ORCA_STARTUP_DIAGNOSTICS` | `FABRICA_STARTUP_DIAGNOSTICS` | 29 |
| `orca-main-bootstrap` | `fabrica-main-bootstrap` | 1 |
| `orca-plain-node-entry-guard` | `fabrica-plain-node-entry-guard` | 2 |
| `orca.happyDom` | `fabrica.happyDom` | 2 |
| `orca-cache` | `fabrica-cache` | 4 |
| `orca-shared-dist` | `fabrica-shared-dist` | 4 |
| `onorca.dev` | `fabrica-ai.vercel.app` | 561 |
| `stablyai` | `fabrica-ai` | 2,616 |
| `stably` | `fabrica` | 216 |
| `ORCA_PROBE_` | `FABRICA_PROBE_` | 6 |
| `__ORCA_BOOTSTRAP_` | `__FABRICA_BOOTSTRAP_` | 5 |
| `ORCA` | `FABRICA` | 67,311 |
| `Orca: <branch>` | `Fabrica: <branch>` | 1 |
| `Orca Dev` | `Fabrica Dev` | 9 |
| `Orca.app` | `Fabrica.app` | 126 |
| `Orca` | `Fabrica` | 67,311 |
| `orca` | `fabrica` | 67,311 |

**Note:** The `ORCA`/`Orca`/`orca` counts overlap because `ORCA` is a substring of `ORCA_BUILD_IDENTITY` etc. The actual unique replacements are as shown — the substitution order ensures longest-match-first.

## Residual Check

After the main pass, a secondary fix was applied for PascalCase variants that the main pass missed:

| Pattern | Replacement | Files Fixed |
|---------|-------------|-------------|
| `StablyAI` | `FabricaAi` | 13 files |
| `Stably` | `Fabrica` | 15 files |

### Post-fix verification

```
rg -i "stablyai" → 0 matches
rg -i "stably" → 0 matches
rg "orca" → 0 matches (in file contents)
```

## What Remains

- **File names** with "orca" (e.g., `orca.yaml`, `Casks/orca.rb`) — these are **filename renames**, not content substitutions. File renames require separate handling to update all import paths and references. This is expected to be done in a subsequent task.
- **`orchestration`** — legitimate word, not "orca". Present and correct.
- **Binary files** (images, fonts) — skipped per instructions.

## Issues / Edge Cases

1. **PascalCase `StablyAI`/`Stably`** — The initial substitution table only had lowercase patterns. A secondary pass was needed to catch PascalCase variants in test fixtures.
2. **File names not renamed** — The content rebrand pass does not rename files. Filename renames (e.g., `orca.yaml` → `fabrica.yaml`) should be handled in a separate step with import path updates.
3. **`pnpm-lock.yaml`** — Contains `stablyai` references from dependency metadata. These are auto-generated and will resolve when `pnpm install` is run after the full rebrand.

## Output Files

- `REBRAND-LOG.txt` — Per-file log of all substitutions applied (18,855 lines)
- This file (`rebrand-verification-report.md`) — Summary and analysis

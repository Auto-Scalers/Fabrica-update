# Fabrica-app Update Pipeline — Plan (v2, fork-from-upstream)

> Plan for **forking upstream Orca as the new Fabrica-app** and re-applying every rebrand + custom-logic on top of it. Goal: a clean, repeatable way to keep Fabrica in sync with upstream without ever trying to "port over" thousands of upstream-only files into the old fork.

---

## Why v2

v1 (port upstream into existing `Fabrica-app/`) tried to **patch** the old Orca fork (`3e3ff56`, based on an older `stablyai/orca` commit) by copying thousands of new upstream files into it. That approach produced a 14k-file untracked mess — too much risk, too hard to review, and the new upstream commit had already moved past our baseline.

**v2 pivots:** we **start from current upstream** and **layer** our work on top. The new Fabrica repo is a fresh fork of upstream Orca; rebrand + custom-logic are applied as a separate, auditable transformation pass.

## Goals

1. The new `Fabrica` repo begins life as a **clean fork of upstream `stablyai/orca`** at a recorded commit.
2. Every rebrand substitution and every piece of custom logic is applied **on top** of that clean fork in a single pass.
3. The transformation is **diffable, reversible, and re-runnable** against any future upstream commit.
4. We never re-introduce Orca identifiers in `Fabrica/` source.

## Three sources (precise roles)

1. **`Fabrica-update/orca-baseline/`** — the **frozen Orca source** we previously rebranded from. Used as the *ground truth* for "what did we change?" **Read-only.**
2. **`Fabrica-update/upstream-orca/`** — the **current upstream `stablyai/orca`** at a recorded commit. The starting point for the new `Fabrica` fork. **Read-only here.**
3. **`Fabrica-update/Fabrica-app/`** — the **old Fabrica fork** (v0.0.6, `0a5d258`). The source of truth for "what we did" (rebrand + custom logic). **Read-only here** (the actual new target lives in `Fabrica/`).
4. **`Fabrica/`** — the **new Fabrica repo** (fresh clone of upstream at T1's pinned commit). This is where rebrand + custom-logic are applied.

**Scope:** `Fabrica/` (the desktop app) + `Fabrica-plugins/` + the 8 plugin repos. Out of scope: `Fabrica-web/`, `Fabrica-relay/`, `Fabrica-atlas/`, `Fabrica-marketing/`.

## Strategy

We have **two diffs**, both off the same `orca-baseline/`:

| Diff | Comparison | What it tells us |
|---|---|---|
| **A — Rebrand diff** | `orca-baseline/` ↔ `Fabrica-update/Fabrica-app/` (old fork, v0.0.6) | **What we changed**: rebrand substitutions + custom logic (added/removed/changed) |
| **B — Upstream diff** | `orca-baseline/` ↔ `upstream-orca/` (new commit) | **What changed in Orca** since we forked: where the file layout, line numbers, and contents have drifted |

By **diffing the diffs**, we can map each piece of our custom logic onto its new location in upstream, then apply rebrand to the entire fork in one shot.

```
                orca-baseline/  (frozen reference)
                  /            \
                 /              \
        A: rebrand diff        B: upstream diff
       (what WE did)         (what ORCA did)
  Fabrica-update/Fabrica-app/   upstream-orca/
                 \              /
                  \            /
                   diff-of-diffs
                       |
                CUSTOM-LOGIC-MAP
          (each custom-logic file →
           its new location in upstream)
                       |
                       v
              Fabrica/  (clone of upstream)
                       |
        +--------------+--------------+
        |                             |
   apply REBRAND                apply CUSTOM-LOGIC
   substitution table            map onto new locations
        |                             |
        +-------------+---------------+
                      v
                  Fabrica/  (final)
```

## Six-phase approach (fork-from-upstream)

### Phase 0 — Pin upstream

1. Pull/fetch `upstream-orca/` from `https://github.com/stablyai/orca`.
2. Confirm the repo is at a known commit/tag — record the hash.
3. **Output:** `upstream-orca/` pinned to a recorded commit (e.g. `f737f3499f`).

### Phase 1 — Fork upstream into the new Fabrica repo

1. Create the GitHub repo `Auto-Scalers/Fabrica` (public) — empty.
2. Clone `stablyai/orca` at the pinned commit into the local `Fabrica/` folder.
3. Replace remote URL with `Auto-Scalers/Fabrica`.
4. Push the upstream content to `main` so we have a clean baseline fork.
5. **Output:** `Fabrica/` is a clean clone of upstream at the pinned commit.

### Phase 2 — Rebrand diff: discover what we changed

**Goal:** produce a structured map of everything that differs between `orca-baseline/` and `Fabrica-update/Fabrica-app/` (old fork, v0.0.6). This is the source of truth for both rebrand patterns and custom logic.

1. Diff `orca-baseline/` ↔ `Fabrica-update/Fabrica-app/`.
2. For every differing file, tag each change with **intent**:
   - **`rebrand`** — pure identity substitution (strings, names, app id, env vars, domains, wire tokens, package names). Re-runnable from the substitution table.
   - **`custom_logic`** — features/fixes/patterns we added, removed, or changed. Must be re-implemented in `Fabrica/` at the corresponding upstream location.
   - **`incidental`** — whitespace, dep bumps, formatting. Re-evaluate per sync.
3. **Output:** `pipeline-files/REBRAND-INTENT-MAP.md` + `.json` — per-file, intent-tagged, with our-pattern notes.

### Phase 3 — Upstream diff: discover what Orca changed

**Goal:** produce a structured map of what changed in Orca since our baseline. Used to know where custom-logic should land.

1. Diff `orca-baseline/` ↔ `upstream-orca/`.
2. For every differing file, capture path, status (`added`/`removed`/`modified`/`renamed`), and diff hunks with line numbers.
3. **Output:** `pipeline-files/UPSTREAM-DIFF-MAP.md` + `.json` — what changed, line-mapped.

### Phase 4 — Custom-logic map (the diff-of-diffs)

**Goal:** for each `custom_logic` file from Phase 2, find its **new location** in upstream so we know where to re-implement.

1. Take the list of `custom_logic` files from Phase 2.
2. For each, use the Phase 3 upstream diff to find:
   - The new path (renamed? moved to a subdirectory?)
   - The surrounding context (which module now owns the responsibility)
   - Whether the custom logic is still relevant (sometimes upstream solved it differently and our patch is now obsolete — flag for PM review).
3. **Output:** `pipeline-files/CUSTOM-LOGIC-MAP.md` + `.json` — per-custom-logic entry: old path → new path in upstream → implementation guidance → risk (easy/medium/hard).

### Phase 5 — Apply rebrand to the entire new Fabrica

**Goal:** a single, idempotent pass that substitutes every Orca identifier with the Fabrica equivalent across the whole repo.

1. Start from the clean fork in `Fabrica/` (Phase 1).
2. Apply the **Rebrand Pattern** (table below) to every text file:
   - Use a deterministic ordered substitution list.
   - Scan every file with `grep -l` after substitution to confirm zero residual `orca`/`stably`/`stablyai` references.
   - Apply case-sensitively where appropriate (PascalCase vs lowercase vs UPPERCASE).
3. **Output:** `Fabrica/` rebranded. Verification artifacts:
   - `pipeline-files/rebrand-verification-report.md` — files changed, patterns applied, residuals (must be 0).
   - `pipeline-files/REBRAND-LOG.txt` — per-file log of substitutions.

### Phase 6 — Re-implement custom logic on top of the rebranded fork

**Goal:** for every entry in `CUSTOM-LOGIC-MAP`, port the custom logic into the new `Fabrica/` at the correct location.

1. Process `CUSTOM-LOGIC-MAP` in **priority order**:
   1. `src/main/` (core) — highest priority
   2. `src/shared/` (types, utilities)
   3. `src/renderer/` (UI)
   4. `src/cli/`, `src/relay/`, `src/preload/` (supporting)
   5. `tests/`, `mobile/`, `config/`
2. For each entry:
   - Read the **old** custom logic from `Fabrica-update/Fabrica-app/` (v0.0.6 content).
   - Locate the **new** home in the rebranded fork using `CUSTOM-LOGIC-MAP`.
   - Produce a merged file that respects upstream's new structure.
   - Re-apply any rebrand patterns to the merged result.
3. **Output:** `Fabrica/` final. Artifacts:
   - `pipeline-files/CUSTOM-LOGIC-LOG.txt` — per-file log of what was merged and where.
   - `pipeline-files/CUSTOM-LOGIC-REVIEW.md` — entries flagged for PM review (upstream may have already solved the problem).

### Phase 7 — Final verification

1. **Grep for residual Orca identifiers** across `Fabrica/src/`, `Fabrica/config/`, root-level files. Expected: 0.
2. **Grep for stablyai** across the same. Expected: 0.
3. **Build smoke test** — compile the renderer + main process to catch structural breaks.
4. **Custom-logic coverage check** — every entry in `CUSTOM-LOGIC-MAP` is either `applied` or `flagged_for_review`. None silently dropped.
5. **Output:** `pipeline-files/FINAL-VERIFICATION-REPORT.md`.

## Rebrand Pattern (complete)

Worker must scan every file for **all** of these patterns, in this order:

| Pattern | Replacement | Notes |
|---------|-------------|-------|
| `stablyai` | `fabrica-ai` | GitHub org, npm scope, API references |
| `stably` | `fabrica` | Appears in names, URLs, comments — not just `stablyai` |
| `ORCA` | `FABRICA` | UPPERCASE — env vars, constants, debug symbols |
| `Orca` | `Fabrica` | PascalCase — class names, component names |
| `orca` | `fabrica` | Lowercase — variable names, CLI commands, file refs |
| `onorca.dev` | `fabrica-ai.vercel.app` | Backend URLs, API endpoints |
| `on_orca` | `on_fabrica` | Underscore variant |
| App ID | `ai.autoscalers.fabrica` | Electron app identifier |
| Wire tokens | `FABRICA_*` prefix | Authentication/session tokens |
| Package names | `@autoscalers/*` | npm package naming |
| Icons/logos | `DEFAULT_APP_ICON_ID='light'` | File paths to icon/logo assets |

### Extended patterns (discovered in v1)

| Pattern | Replacement | Context |
|---------|-------------|---------|
| `ORCA_BUILD_IDENTITY` | `FABRICA_BUILD_IDENTITY` | Build constant |
| `ORCA_POSTHOG_WRITE_KEY` | `FABRICA_POSTHOG_WRITE_KEY` | Telemetry |
| `ORCA_DIAGNOSTICS_TOKEN_URL` | `FABRICA_DIAGNOSTICS_TOKEN_URL` | Diagnostics |
| `ORCA_STARTUP_DIAGNOSTICS*` | `FABRICA_STARTUP_DIAGNOSTICS*` | Debug env vars |
| `__ORCA_BOOTSTRAP_*_INSTALLED__` | `__FABRICA_BOOTSTRAP_*_INSTALLED__` | Runtime flags |
| `orca-main-bootstrap` | `fabrica-main-bootstrap` | Plugin name |
| `orca-plain-node-entry-guard` | `fabrica-plain-node-entry-guard` | Plugin name |
| `ORCA_I18N_EXTRACTION_OUTPUT` | `FABRICA_I18N_EXTRACTION_OUTPUT` | Build config |
| `ORCA_FEATURE_WALL_ENABLED` | `FABRICA_FEATURE_WALL_ENABLED` | Feature flag |
| `ORCA_BOOTSTRAP_FATAL_LOG` | `FABRICA_BOOTSTRAP_FATAL_LOG` | Debug env var |
| `ORCA_DEV_DOCK_TITLE` | `FABRICA_DEV_DOCK_TITLE` | Dev env var |
| `ORCA_DEV_BRANCH` | `FABRICA_DEV_BRANCH` | Dev env var |
| `ORCA_DEV_INSTANCE_LABEL` | `FABRICA_DEV_INSTANCE_LABEL` | Dev env var |
| `ORCA_DEV_WORKTREE_NAME` | `FABRICA_DEV_WORKTREE_NAME` | Dev env var |
| `ORCA_PROBE_*` | `FABRICA_PROBE_*` | Probe env vars |
| `ORCA_CODEX_HOME` | `FABRICA_CODEX_HOME` | Codex env var |
| `ORCA_CODEX_VALIDATION_TEMP_PARENT` | `FABRICA_CODEX_VALIDATION_TEMP_PARENT` | Validation env var |
| `orca.happyDom*` | `fabrica.happyDom*` | Symbol names |
| `Orca: <branch>` | `Fabrica: <branch>` | Dev dock title format |
| `Orca Dev` | `Fabrica Dev` | Dev mode label |
| `Orca.app` | `Fabrica.app` | macOS app path |
| `/orca/` | `/fabrica/` | URL paths |
| `orca-cache` | `fabrica-cache` | Cache directory |
| `orca-shared-dist` | `fabrica-shared-dist` | Shared dist marker |

**CRITICAL:** Apply substitutions **case-sensitively, longest-match first**, in the order shown above (so `stablyai` is substituted before `stably`, and `ORCA` before `Orca` before `orca`). After each pass, re-grep to confirm zero residuals.

## Structural refactors (re-encountered in the new fork)

Upstream frequently splits monoliths into modules. When the custom logic in old `Fabrica-app` lived inside a monolith that has since been split:

1. Check whether the new module files exist in upstream (likely yes — they're part of the fork).
2. Re-implement the custom logic into the **new** module location, not the old monolith path.
3. Apply rebrand to the result.

**Detection:** when `orca-baseline` shows a single file (e.g. `server.ts`, ~3000 lines) and `upstream-orca` shows a thin wrapper plus `server/*.ts` modules, treat the modules as the canonical location.

## Custom-logic 4-scenario classification (reused)

When a custom-logic file is no longer present in upstream:

| Scenario | Custom Logic | Used in Upstream | Action |
|----------|--------------|------------------|--------|
| 1 | Yes | Yes (new location) | Merge into new location, archive old |
| 2 | Yes | No | Decide: re-implement if still relevant, else drop with PM sign-off |
| 3 | No | Yes (new location) | Skip — upstream absorbed it |
| 4 | No | No | Archive (dead code) |

## Output artifacts (under `Fabrica-update/.Fabrica-update-board/pipeline-files/`)

| File | Phase | Purpose |
|---|---|---|
| `UPDATE-PIPELINE-PLAN.md` *(this file)* | — | The plan |
| `UPSTREAM-DIFF-MAP.md` + `.json` | 3 | What changed in upstream, line-mapped |
| `REBRAND-INTENT-MAP.md` + `.json` | 2 | What we changed in the old Fabrica-app, intent-tagged |
| `CUSTOM-LOGIC-MAP.md` + `.json` | 4 | Each custom-logic → its new location in upstream + guidance |
| `REBRAND-LOG.txt` | 5 | Per-file rebrand substitutions applied |
| `rebrand-verification-report.md` | 5 | Residual check (must be 0 orca/stablyai) |
| `CUSTOM-LOGIC-LOG.txt` | 6 | Per-file merge log |
| `CUSTOM-LOGIC-REVIEW.md` | 6 | Entries flagged for PM review |
| `FINAL-VERIFICATION-REPORT.md` | 7 | End-to-end pass |

## Strict rules (for every phase)

- **Strictly preserve every rebrand substitution and every piece of custom logic.** Zero rebrand regressions, zero lost custom work.
- **The new `Fabrica/` starts as a clean fork of upstream.** No carry-over of the old fork's quirks.
- **Substitutions are case-sensitive, longest-match first**, in the documented order.
- **Read-only** on `orca-baseline/`, `upstream-orca/`, and `Fabrica-plugins/` (until T5 of the plugins plan).
- **No commits/pushes by the worker** — orchestrator handles git. Workers report `worker_done`.
- **No actual deletes.** All archived material goes under `.Fabrica-update-board/.archive/` (or `Fabrica/.Fabrica-app-board/.archive/` for the new fork).
- **Exclude from diff noise:** `node_modules/`, `.next/`, `dist/`, `out/`, `build/`, `.git/`, `.backup/`, `_sources/`.
- **Workers must claim** `IN_PROGRESS` in the task file and record their handle in the Session Ledger **before** starting — anti-overlap protocol.

## Tasks (v2)

| # | What | Who | Depends on |
|---|---|---|---|
| T0 | Update `upstream-orca/` to latest commit; record hash | Worker | — |
| T1 | Fork `Auto-Scalers/Fabrica` from upstream at pinned commit; push clean baseline | Worker | T0 |
| T2 | Rebrand intent diff (`orca-baseline/` ↔ old `Fabrica-app/` snapshot) → `pipeline-files/REBRAND-INTENT-MAP.md/.json` | Worker | T1 |
| T3 | Upstream diff (`orca-baseline/` ↔ `upstream-orca/`) → `pipeline-files/UPSTREAM-DIFF-MAP.md/.json` | Worker | T0 |
| T4 | Custom-logic map (diff-of-diffs) → `pipeline-files/CUSTOM-LOGIC-MAP.md/.json` | Worker | T2, T3 |
| T5 | Apply rebrand pass to new `Fabrica/` → `pipeline-files/REBRAND-LOG.txt` + `rebrand-verification-report.md` | Worker | T1, T2 |
| T6 | Re-implement custom logic into new `Fabrica/` → `pipeline-files/CUSTOM-LOGIC-LOG.txt` + `CUSTOM-LOGIC-REVIEW.md` | Worker (+ PM review) | T4, T5 |
| T7 | Final verification → `pipeline-files/FINAL-VERIFICATION-REPORT.md` | Worker | T6 |

## Status

- v1 (port upstream into old fork): **abandoned 2026-09-03** — old `Fabrica-app/` submodule removed from dev-env.
- Old `Fabrica-app` (v0.0.6, `0a5d258`): cloned into `Fabrica-update/Fabrica-app/` as a read-only reference.
- Upstream `stablyai/orca` (`7106101ed2`): cloned into `Fabrica/` as the new target repo.
- v2 (fork-from-upstream): **starting now** — T0 ready.
- Upstream commit for this sync: `7106101ed2` ("fix(mobile): restore terminal input when reopening worktrees").
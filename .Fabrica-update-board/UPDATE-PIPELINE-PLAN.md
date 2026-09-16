# Fabrica-app Update Pipeline — Plan (v3, fork-from-upstream)

> Plan for **forking upstream Orca as the new Fabrica-app** and re-applying every rebrand + custom-logic on top of it. Goal: a clean, repeatable way to keep Fabrica in sync with upstream without ever trying to "port over" thousands of upstream-only files into the old fork.

---

## Why v3

v2 (fork-from-upstream) produced a working pipeline but revealed **13 patterns** that v2 didn't account for. The C-phase audits (14 audits, 60 findings) and I-phase implementation (26 tasks) showed that content rebranding alone is insufficient — filenames, binary assets, shared directories, app IDs, auth endpoints, dependencies, build configs, and documentation all need explicit handling.

**v3 adds:** 4 new phases (filename/binary rebrand, dependency audit, build/CI verification, coexistence analysis), refines the verification phase into a 7-check matrix, and codifies every pattern discovered so future syncs get it right the first time.

## Why v2

v1 (port upstream into existing `Fabrica-app/`) tried to **patch** the old Orca fork (`3e3ff56`, based on an older `stablyai/orca` commit) by copying thousands of new upstream files into it. That approach produced a 14k-file untracked mess — too much risk, too hard to review, and the new upstream commit had already moved past our baseline.

**v2 pivots:** we **start from current upstream** and **layer** our work on top. The new Fabrica repo is a fresh fork of upstream Orca; rebrand + custom-logic are applied as a separate, auditable transformation pass.

## Goals

1. The new `Fabrica` repo begins life as a **clean fork of upstream `stablyai/orca`** at a recorded commit.
2. Every rebrand substitution and every piece of custom logic is applied **on top** of that clean fork in a single pass.
3. The transformation is **diffable, reversible, and re-runnable** against any future upstream commit.
4. We never re-introduce Orca identifiers in `Fabrica/` source.
5. **(v3)** Filenames, binary assets, dependencies, build configs, and shared directories are all handled explicitly — not just file contents.

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

## Eleven-phase approach (fork-from-upstream, v3)

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

### Phase 5 — Apply content rebrand to the entire new Fabrica

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

### Phase 7 — Filename & binary rebrand ← NEW IN v3

**Goal:** Phase 5 only changes file CONTENTS. Filenames with "orca" remain on disk. This phase renames every file and directory that still carries an Orca identifier.

**Pattern discovered in v2→v3:** T5 grep confirmed zero `orca` in file contents, but `find . -name "*orca*"` found 372 files + 17 directories. Build broke because code imports `fabrica-blue.png` but file on disk was `orca-blue.png`.

1. **Scan for orca-named files:**
   - `find Fabrica/ -name "*orca*" -o -name "*Orca*" -o -name "*ORCA*" | grep -v node_modules`
   - `find Fabrica/ -name "*stably*" -o -name "*Stably*" -o -name "*STABLY*" | grep -v node_modules`
2. **Classify each rename:**
   - **Source files** (`.ts`, `.tsx`, `.js`, `.mjs`) — rename + update all import/require paths
   - **Resource files** (`.png`, `.ico`, `.icns`, `.svg`) — rename + update all code references
   - **Config files** (`.yaml`, `.json`, `.cjs`, `.mjs`) — rename + update references
   - **Directory names** — rename + update all path references
   - **Native files** (`.swift`, `.cs`, `.m`) — rename + update project references
3. **Verify import paths:** after every rename, grep for the old filename to ensure no broken references.
4. **Output:** `pipeline-files/FILENAME-REBRAND-LOG.txt` — per-file rename log.

### Phase 8 — Dependency & config audit ← NEW IN v3

**Goal:** verify that package.json, build configs, and CI workflows are correctly rebranded and free of dead dependencies.

**Pattern discovered in v2→v3:** `@supabase/supabase-js` was added but never imported (dead code). `@fabrica-ai/playwright-test` was a thin wrapper that should be `@playwright/test`. Build channels (hourly/daily/adhoc) were Orca-specific and needed removal.

1. **package.json audit:**
   - Search for `@stablyai/*` — must be zero (replaced by `@fabrica-ai/*` or removed)
   - Search for dead dependencies (added but never imported)
   - Verify `homepage` points to `Auto-Scalers/Fabrica`
   - Verify `name` field is correct
2. **Build config audit (`electron-builder.config.cjs`):**
   - Verify app ID = `ai.autoscalers.fabrica`
   - Drop Orca-specific build channels (hourly/daily/adhoc) if present
   - Verify all env vars use `FABRICA_*` prefix
3. **CI workflow audit (`.github/workflows/`):**
   - Verify all env vars use `FABRICA_*` prefix (not `ORCA_*`)
   - Verify SignPath project-slug is `fabrica` (not `orca`)
   - Drop Orca-specific workflow files (hourly/daily/adhoc builds)
   - Verify all GitHub Actions references are correct
4. **Gitignore audit:**
   - Verify patterns use `.fabrica` (not `.stably` or `.orca`)
5. **Homebrew cask audit:**
   - Verify `Casks/fabrica.rb` exists (not `Casks/orca.rb`)
   - Verify cask references are correct
6. **Output:** `pipeline-files/DEPENDENCY-AUDIT-REPORT.md`

### Phase 9 — Coexistence & conflict analysis ← NEW IN v3

**Goal:** if a user has both Orca and Fabrica installed on the same device, identify what conflicts and what doesn't.

**Pattern discovered in v2→v3:** Fabrica is a rebrand of Orca. Both apps may be installed simultaneously. The shared `~/.agents/skills/` directory is the primary conflict point — both apps read/write to it.

1. **Map all user-facing paths:**
   - User data directories (`~/.fabrica/`, `~/.fabrica-relay/`, etc.)
   - CLI commands (`fabrica`, `fabrica-dev`)
   - App identifiers (`ai.autoscalers.fabrica`)
   - Named pipes (`\\.\pipe\fabrica-*`)
   - Auth endpoints (`fabrica-ai.vercel.app`)
2. **Check for shared resources:**
   - `~/.agents/skills/` — shared skills directory (by design, but skill names must not collide)
   - Worktree-level files (`fabrica.yaml`, `.fabrica/drops/`)
   - Runtime state files, lock files, daemon ports
3. **Classify conflicts:**
   - **Safe:** different paths, different names, different identifiers
   - **Conflict:** same paths, same filenames, same lock files
   - **By-design shared:** `~/.agents/skills/` (acceptable if skill names are unique)
4. **Output:** `pipeline-files/COEXISTENCE-ANALYSIS.md`

### Phase 10 — Verification matrix ← REFINED FROM v2 Phase 7

**Goal:** comprehensive verification across 7 dimensions. v2 only checked 3 (grep orca, grep stablyai, build smoke). v3 checks everything.

| # | Check | Method | Expected |
|---|-------|--------|----------|
| 1 | Content rebrand residual | `grep -ri "orca" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.json" --include="*.yml"` in `Fabrica/src/`, `Fabrica/config/`, root | 0 matches (excluding audit/doc files) |
| 2 | Stably residual | `grep -ri "stablyai\|stably" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.json"` | 0 matches |
| 3 | Filename rebrand residual | `find Fabrica/ -name "*orca*" -o -name "*Orca*" -o -name "*ORCA*" \| grep -v node_modules \| grep -v .git` | 0 matches |
| 4 | Binary asset verification | Check `resources/app-icons/fabrica-*.png`, `resources/tray/fabrica-*.png`, `resources/*/bin/fabrica*` exist; old orca-named files gone | All present, none orphaned |
| 5 | App ID consistency | Verify `ai.autoscalers.fabrica` in: `package.json`, `electron-builder.config.cjs`, `local-build-compatibility-contract.json`, `mobile/app.json`, `mobile/SIGNING.md` | All match |
| 6 | Import path integrity | After all renames, grep for old filenames in import/require statements | 0 stale imports |
| 7 | Custom-logic coverage | Every entry in `CUSTOM-LOGIC-MAP` is `applied` or `flagged_for_review` | None silently dropped |

**Output:** `pipeline-files/FINAL-VERIFICATION-REPORT.md`

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

### Extended patterns (discovered in v1 and v2)

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

### Filename rebrand patterns (discovered in v2→v3, applied in Phase 7)

| Old Pattern | New Pattern | Scope |
|-------------|-------------|-------|
| `orca-*` (file prefix) | `fabrica-*` | All files: source, resource, config, native |
| `Orca-*` (PascalCase prefix) | `Fabrica-*` | Component files, native source |
| `orcad-*` (variant) | `fabricada-*` | Daemon-related files |
| `stablyai.orca-*` (plugin prefix) | `fabrica-*` | Plugin directories |
| `orca.yaml` (config file) | `fabrica.yaml` | Root workspace config |
| `Casks/orca.rb` | `Casks/fabrica.rb` | Homebrew cask files |

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

## Lessons learned (codified from v2 C-phase + I-phase)

These patterns were discovered during the first v2 execution and are now mandatory checks for every future sync:

### L1: Content rebrand ≠ filename rebrand
Phase 5 changes file contents. It does NOT rename files. A separate Phase 7 pass is required to rename files and directories. Build will break if code imports `fabrica-blue.png` but the file on disk is still `orca-blue.png`.

### L2: Binary assets need explicit verification
Phase 5 skips binary files (PNG, ICO, ICNS, compiled binaries). Phase 7 must verify that icon/tray/binary filenames match what the code imports. P0 build-breaking mismatches are common.

### L3: App ID must be consistent everywhere
The app ID (`ai.autoscalers.fabrica`) appears in 5+ locations: `package.json`, `electron-builder.config.cjs`, `local-build-compatibility-contract.json`, `mobile/app.json`, `mobile/SIGNING.md`. A mismatch in any one breaks builds or auth.

### L4: Dead dependencies accumulate silently
Upstream may add dependencies that our custom logic doesn't use (e.g., `@supabase/supabase-js`). Phase 8 must scan for packages that are imported nowhere in the codebase.

### L5: Build channels are Orca-specific
Hourly/daily/adhoc build channels are Orca infrastructure. Fabrica should drop them. Their workflow files and config references must be removed.

### L6: Shared directories need coexistence analysis
The `~/.agents/skills/` directory is a community standard shared by multiple agents (Claude, Codex, Cursor, Fabrica). Both Orca and Fabrica write to it. Skill names must not collide.

### L7: Auth endpoints change between versions
The auth endpoint moved from `login.fabrica-ai.vercel.app` to `fabrica-ai.vercel.app`. Phase 8 must verify the correct endpoint is configured.

### L8: UTF-8 BOM characters accumulate
Locale JSON files may gain BOM characters from Windows editors. Phase 8 must strip BOMs for RFC 8259 compliance.

### L9: Locale files need explicit handling
New locales (e.g., Arabic) may need to be added. Existing locales may need encoding fixes. This is separate from content rebrand.

### L10: CSS font migrations need completion
Font-face declarations may be incomplete after merge. Phase 8 must verify that `@font-face` is fully declared and `--app-font-family` is updated.

### L11: Homebrew cask files need renaming
`Casks/orca.rb` → `Casks/fabrica.rb`. References in `homebrew-bump.yml` must also be updated.

### L12: Documentation with old brand references
Docs in `docs/reference/` may still reference "Orca". Phase 8 must grep docs for old brand names.

### L13: Skill registries need rebrand + dedup
`snapshot-registry.json`, `current-manifest.json`, and `release-mapping.json` must be rebranded. Extra Fabrica-only entries (duplicates of existing skills) must be removed.

## Output artifacts (under `Fabrica-update/.Fabrica-update-board/pipeline-files/`)

| File | Phase | Purpose |
|---|---|---|
| `UPDATE-PIPELINE-PLAN.md` *(this file)* | — | The plan |
| `UPSTREAM-DIFF-MAP.md` + `.json` | 3 | What changed in upstream, line-mapped |
| `REBRAND-INTENT-MAP.md` + `.json` | 2 | What we changed in the old Fabrica-app, intent-tagged |
| `CUSTOM-LOGIC-MAP.md` + `.json` | 4 | Each custom-logic → its new location in upstream + guidance |
| `REBRAND-LOG.txt` | 5 | Per-file rebrand substitutions applied |
| `rebrand-verification-report.md` | 5 | Residual check (must be 0 orca/stablyai in contents) |
| `CUSTOM-LOGIC-LOG.txt` | 6 | Per-file merge log |
| `CUSTOM-LOGIC-REVIEW.md` | 6 | Entries flagged for PM review |
| `FILENAME-REBRAND-LOG.txt` | 7 | Per-file rename log (orca→fabrica filenames) |
| `DEPENDENCY-AUDIT-REPORT.md` | 8 | Dead deps, build config, CI workflows, gitignore, casks |
| `COEXISTENCE-ANALYSIS.md` | 9 | Orca↔Fabrica coexistence on same device |
| `FINAL-VERIFICATION-REPORT.md` | 10 | 7-dimension verification matrix |

## Strict rules (for every phase)

- **Strictly preserve every rebrand substitution and every piece of custom logic.** Zero rebrand regressions, zero lost custom work.
- **The new `Fabrica/` starts as a clean fork of upstream.** No carry-over of the old fork's quirks.
- **Substitutions are case-sensitive, longest-match first**, in the documented order.
- **Read-only** on `orca-baseline/`, `upstream-orca/`, and `Fabrica-plugins/` (until T5 of the plugins plan).
- **No commits/pushes by the worker** — orchestrator handles git. Workers report `worker_done`.
- **No actual deletes.** All archived material goes under `.Fabrica-update-board/.archive/` (or `Fabrica/.Fabrica-app-board/.archive/` for the new fork).
- **Exclude from diff noise:** `node_modules/`, `.next/`, `dist/`, `out/`, `build/`, `.git/`, `.backup/`, `_sources/`.
- **Workers must claim** `IN_PROGRESS` in the task file and record their handle in the Session Ledger **before** starting — anti-overlap protocol.
- **(v3)** Filename rebrand (Phase 7) is mandatory after content rebrand (Phase 5). Do not skip it.
- **(v3)** Verification (Phase 10) must pass all 7 checks before marking done.

## Tasks (v3)

| # | What | Who | Depends on |
|---|---|---|---|
| T0 | Update `upstream-orca/` to latest commit; record hash | Worker | — |
| T1 | Fork `Auto-Scalers/Fabrica` from upstream at pinned commit; push clean baseline | Worker | T0 |
| T2 | Rebrand intent diff (`orca-baseline/` ↔ old `Fabrica-app/` snapshot) → `pipeline-files/REBRAND-INTENT-MAP.md/.json` | Worker | T1 |
| T3 | Upstream diff (`orca-baseline/` ↔ `upstream-orca/`) → `pipeline-files/UPSTREAM-DIFF-MAP.md/.json` | Worker | T0 |
| T4 | Custom-logic map (diff-of-diffs) → `pipeline-files/CUSTOM-LOGIC-MAP.md/.json` | Worker | T2, T3 |
| T5 | Apply content rebrand to new `Fabrica/` → `pipeline-files/REBRAND-LOG.txt` + `rebrand-verification-report.md` | Worker | T1, T2 |
| T6 | Re-implement custom logic into new `Fabrica/` → `pipeline-files/CUSTOM-LOGIC-LOG.txt` + `CUSTOM-LOGIC-REVIEW.md` | Worker (+ PM review) | T4, T5 |
| T7 | **Filename & binary rebrand** → `pipeline-files/FILENAME-REBRAND-LOG.txt` | Worker | T5, T6 |
| T8 | **Dependency & config audit** → `pipeline-files/DEPENDENCY-AUDIT-REPORT.md` | Worker | T7 |
| T9 | **Coexistence & conflict analysis** → `pipeline-files/COEXISTENCE-ANALYSIS.md` | Worker | T7 |
| T10 | **Verification matrix** (7 checks) → `pipeline-files/FINAL-VERIFICATION-REPORT.md` | Worker | T8, T9 |

## Status

- v1 (port upstream into old fork): **abandoned 2026-09-03** — old `Fabrica-app/` submodule removed from dev-env.
- Old `Fabrica-app` (v0.0.6, `0a5d258`): cloned into `Fabrica-update/Fabrica-app/` as a read-only reference.
- Upstream `stablyai/orca` (`7106101ed2`): cloned into `Fabrica/` as the new target repo.
- v2 (fork-from-upstream): **completed 2026-09-08** — all 48 tasks done (T0-T7, C1-C14, I-01 to I-26).
- v3 (refined pipeline): **ready** — 11-phase approach with lessons codified.
- Upstream commit for this sync: `7ed86a98ae` ("fix(mobile): restore terminal input when reopening worktrees").

# Upstream Diff Map

> Diff between orca-baseline/ and upstream-orca/ — what Orca changed since our fork.
> Generated: 2026-09-04

## Summary

| Metric | Count |
|--------|-------|
| **Total files in baseline** | 13,195 |
| **Total files in upstream** | 23,562 |
| **Common files** | 12,779 |
| **Added (upstream only)** | 10,783 |
| **Deleted (baseline only)** | 416 |
| **Modified** | 5,511 |
| **Identical** | 7,268 |
| **Potential renames** | 152 |

### Breakdown by Top-Level Directory

| Directory | Added | Deleted | Modified |
|-----------|-------|---------|----------|
| `src/` | 9,541 | 378 | 4,843 |
| `mobile/` | 357 | 6 | 354 |
| `cloud/` | 339 | 0 | 0 |
| `tests/` | 212 | 4 | 131 |
| `docs/` | 146 | 3 | 14 |
| `config/` | 142 | 6 | 107 |
| `.github/` | 43 | 0 | 28 |
| `native/` | 3 | 0 | 6 |
| `skill-guides/` | 0 | 0 | 8 |
| `skills/` | 0 | 0 | 3 |
| `.husky/` | 0 | 17 | 0 |
| Root files | 0 | 2 | 12 |

---

## Key Findings

### 1. Massive New Module: `cloud/` (339 files, entirely new)

The entire `cloud/` directory is new in upstream. Contains:
- `cloud/dev/scripts/` — 117 build/dev scripts
- `cloud/apps/relay/` — 111 relay server files
- `cloud/apps/relay-ops/` — 34 relay operations files
- `cloud/infra/terraform/` — 23 infrastructure-as-code files
- `cloud/packages/relay-contract/` — 20 relay contract types
- `cloud/apps/relay-fence-broker/` — 13 fence broker files

**Impact:** This is the relay server split — upstream extracted relay functionality into a separate cloud deployment package.

### 2. Huge `src/` Growth (9,541 added + 4,843 modified)

The `src/` directory exploded in size. Key subdirectories with most additions:

| Subdirectory | Added | Modified | Description |
|--------------|-------|----------|-------------|
| `src/renderer/src/` | 4,032 | 2,680 | Major renderer UI additions |
| `src/main/runtime/` | 1,139 | 284 | Runtime system expansion |
| `src/main/ipc/` | 555 | 215 | IPC handler additions |
| `src/main/browser/` | 424 | 42 | Browser pane expansion |
| `src/main/native-chat/` | 193 | 27 | Native chat system |
| `src/main/git/` | 170 | 35 | Git integration |
| `src/main/daemon/` | 162 | 102 | Daemon service |
| `src/main/skills/` | 155 | 0 | Skills system |
| `src/main/codex/` | 151 | 61 | Codex integration |
| `src/preload/api/` | 138 | 0 | Preload API |
| `src/main/github/` | 127 | 43 | GitHub integration |
| `src/main/persistence/` | 121 | 0 | Persistence layer |
| `src/main/ssh/` | 110 | 85 | SSH support |
| `src/main/providers/` | 95 | 48 | Provider system |

### 3. Baseline Files Are Much Larger (Custom Fabrica Code)

Several baseline files are dramatically larger than upstream, reflecting our custom additions:

| File | Baseline | Upstream | Ratio |
|------|----------|----------|-------|
| `src/renderer/src/store/slices/worktrees.ts` | 249,579 | 6,933 | 36x |
| `src/main/index.ts` | 145,894 | 4,734 | 31x |
| `src/renderer/src/App.tsx` | 127,644 | 5,839 | 22x |
| `package.json` | 19,919 | 24,603 | upstream 1.2x |

### 4. Deleted Files (416)

Key deletions in upstream (files removed from baseline but present in upstream):

**Deleted by category:**
- `.husky/_/` — 17 husky hook files removed
- `src/main/browser/` — 42 browser pane files removed (moved/refactored)
- `src/renderer/src/components/browser-pane/` — ~60 browser pane UI files removed
- `src/renderer/src/components/editor/` — ~20 editor files removed
- `src/renderer/src/components/right-sidebar/` — ~30 source control files removed
- `src/shared/` — ~40 shared utility files removed
- `config/patches/` — 4 xterm patch files removed
- `src/main/` — Various `.test.ts` files removed across modules

### 5. Massive Config Growth

| File | Baseline | Upstream | Delta |
|------|----------|----------|-------|
| `config/reliability-gates.jsonc` | 851,567 | 1,607,419 | +755,852 bytes |
| `config/max-lines-baseline.txt` | 17,919 | 844 | -17,075 bytes |
| `pnpm-workspace.yaml` | 335 | 1,855 | +1,520 bytes |
| `package.json` | 19,919 | 24,603 | +4,684 bytes |

---

## File-by-File Analysis (Key Modified Files)

### package.json
- **Status:** modified
- **Hunks:** 92 insertions, 62 deletions
- **Key changes:** New dependencies added (30+), version bumps, workspace config changes. Significant dependency growth.

### pnpm-workspace.yaml
- **Status:** modified
- **Hunks:** 46 insertions, 0 deletions
- **Key changes:** Expanded from 335 to 1,855 bytes. Added workspace package definitions for `cloud/` directory (relay, relay-ops, relay-fence-broker, relay-contract, dev scripts).

### electron.vite.config.ts
- **Status:** modified
- **Hunks:** 14 insertions, 7 deletions
- **Key changes:** Build configuration updates, likely for new modules and entry points.

### AGENTS.md
- **Status:** modified
- **Hunks:** 31 insertions, 4 deletions
- **Key changes:** Expanded agent instructions, new capabilities documented.

### README.md
- **Status:** modified
- **Hunks:** 8 insertions, 4 deletions
- **Key changes:** Updated project documentation.

### config/reliability-gates.jsonc
- **Status:** modified
- **Hunks:** 12,780 insertions, 5,476 deletions
- **Key changes:** Massive expansion of reliability gate definitions. Grew from 851KB to 1.6MB. New test contracts, coverage requirements, and quality gates added across the entire codebase.

### config/max-lines-baseline.txt
- **Status:** modified
- **Hunks:** 335 deletions
- **Key changes:** Reduced from 17,919 to 844 bytes. Most line-count baselines removed (files likely moved to reliability-gates.jsonc or deprecated).

### config/electron-builder.config.cjs
- **Status:** modified
- **Hunks:** Significant changes
- **Key changes:** Grew from 27,339 to 34,735 bytes (+7,396). New build targets, packaging configuration for new modules.

### mobile/pnpm-workspace.yaml
- **Status:** modified
- **Hunks:** Significant changes
- **Key changes:** Expanded from 67 to 254 bytes. Mobile workspace restructured.

---

## Renderer UI Changes (4,032 added + 2,680 modified)

The renderer (`src/renderer/src/`) saw the largest changes:

### New Components (added)
- `components/browser-pane/` — Entirely new browser pane with remote browser streaming, markup tools, annotation system
- `components/editor/` — Combined diff viewer, file tree, section loading
- `components/right-sidebar/` — Source control panel, git history, AI code review, PR creation flow
- `components/skills/` — Skill card component
- `components/TaskPage.tsx` — New task page
- `components/terminal-pane/` — Agent completion coordinator, parked terminal mode
- `components/workspace-cleanup/` — Workspace cleanup dialog and session management

### Modified Components
- `store/slices/` — Editor, GitHub, tabs, UI, worktrees state management
- `components/sidebar/` — Worktree list improvements (virtualization, filtering, groups)
- `components/terminal-pane/` — PTY transport, link handlers, global effects

---

## Deleted Files by Category

### .husky/_/ (17 files)
All husky hook templates removed from upstream.

### src/main/browser/ (42 files)
Browser manager, guest UI, agent browser bridge, grab functionality — all removed (likely moved to new location or refactored).

### src/renderer/src/components/browser-pane/ (~60 files)
Entire browser pane component tree removed. Includes:
- Address bar, annotation, toolbar, find, import hints
- Remote browser streaming (lifecycle, restart, status, tokens)
- Markup tools (drawing, shapes, screenshots, canvas)
- Webview registry, grab mode

### src/renderer/src/components/editor/ (~20 files)
Combined diff viewer components removed (likely replaced by new implementations).

### src/renderer/src/components/right-sidebar/ (~30 files)
Source control components, git history, AI prompts, branch context — all removed.

### src/shared/ (~40 files)
GitHub utilities, worktree management, Linear integration, attribution, types — all removed.

### config/patches/ (4 files)
xterm addon patches removed (addon-ligatures, addon-serialize, addon-webgl, xterm core).

---

## Summary of Architectural Changes

1. **Relay server split** — New `cloud/` directory with relay server, ops, fence broker, and terraform infrastructure. This is a major architectural extraction.

2. **Browser pane rebuild** — The entire browser pane was deleted from the old locations and rebuilt with new components. Remote browser streaming, markup/annotation tools, and webview management are new.

3. **Source control expansion** — Git history panel, PR creation flow, AI code review, and branch management completely rewritten.

4. **Runtime system growth** — 1,139 new files in `src/main/runtime/` suggest major runtime architecture changes.

5. **IPC expansion** — 555 new IPC handlers indicate significant new main↔renderer communication.

6. **Skills system** — 155 new files in `src/main/skills/` indicate a new plugin/skill architecture.

7. **Config explosion** — `reliability-gates.jsonc` doubled in size with comprehensive test/quality gate definitions.

8. **Dependencies grew** — `package.json` increased by 4,684 bytes (92 insertions, 62 deletions) with many new packages.

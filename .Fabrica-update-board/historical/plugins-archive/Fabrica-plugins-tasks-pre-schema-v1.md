# Fabrica-plugins — Tasks

> Single source of truth for plugin marketplace work. The Roadmap (`.Fabrica-Board/Fabrica-Roadmap.md`) tracks cross-cutting status only — this file owns execution details.

---

## Status Legend

- **DONE** — implemented and verified
- **VERIFY** — implemented, needs verification
- **PARTIAL** — partially implemented
- **TODO** — planned, not started
- **BLOCKED** — waiting on dependency

---

## Orca Source Study

> Clone and study the original Orca plugin repos to understand structure before building Fabrica equivalents.

| # | Task | Status | Notes |
|---|------|--------|-------|
| P0a | Clone `stablyai/orca-plugins` into `_sources/orca-plugins/` | **DONE** | Cloned to `_sources/orca-plugins/` |
| P0b | Clone bundled plugin repos into `_sources/` | **DONE** | Cloned: `orca-portuguese`, `orca-navigation-shortcuts`, `orca-multipass-recipes` |
| P0c | Clone theme/skill plugin repos into `_sources/` | **DONE** | Cloned: `orca-solarized-terminal`, `orca-minimal-icons`, `orca-nord-theme`, `orca-midnight-theme`, `orca-workflow-skills` |
| P0d | Study marketplace index format from cloned repo | **DONE** | Schema documented in `.Fabrica-plugins-board/P0-source-study.md` |
| P0e | Study bundled plugin manifest format | **DONE** | `orca-plugin.json` format documented in `.Fabrica-plugins-board/P0-source-study.md` |
| P0f | Document rename strategy | **DONE** | Rename strategy documented in `.Fabrica-plugins-board/P0-source-study.md` |

---

## Marketplace Index

| # | Task | Status | Notes |
|---|------|--------|-------|
| P1 | Initialize marketplace index JSON | **DONE** | `marketplace-index.json` created with 8 bundled plugins, following `stablyai`→`autoscalers` rename strategy. |
| P2 | Add bundled plugins to index | **DONE** | All 8 bundled plugins verified present in `marketplace-index.json`. |
| P3 | Plugin submission guidelines | **DONE** | Documented in `.Fabrica-plugins-board/P3-plugin-submission-guidelines.md`. |

---

## Plugin Schema

| # | Task | Status | Notes |
|---|------|--------|-------|
| P4 | Define plugin manifest schema | **DONE** | `fabrica-plugin.json` schema documented in `.Fabrica-plugins-board/P4-plugin-manifest-schema.md`. |
| P5 | Plugin validation rules | **DONE** | Documented in `.Fabrica-plugins-board/P5-plugin-validation-rules.md`. |

---

## Quality & Trust

| # | Task | Status | Notes |
|---|------|--------|-------|
| P6 | Plugin review process | **DONE** | Documented in `.Fabrica-plugins-board/P6-plugin-review-process.md`. |
| P7 | Kill-list management | **DONE** | Documented in `.Fabrica-plugins-board/P7-kill-list-management.md`. |
| P8 | Plugin signing (future) | **DONE** | Research complete. Orca never signed plugins (SHA-256 content hashing + GitHub provenance). Recommendation: zero-cost Tier-1 baseline (signed Git tags + GPG keys + SHA-256 in marketplace). Apple Dev Program ($99/yr) is hard blocker for macOS. Report: `.Fabrica-plugins-board/P8-plugin-signing-research.md` |

---

## App Integration

| # | Task | Status | Notes |
|---|------|--------|-------|
| P9 | Plugin loader reads from marketplace | **DONE** | Already fully implemented — marketplace fetches via Git clone, caches snapshots, bundles bootstrap to filesystem, discovery finds them, IPC handlers registered, startup wires it all. Verified by audit in Fabrica-app. |
| P10 | Plugin update mechanism | **DONE** | Already fully wired — previewMarketplaceUpdate, "Check for update" button, installMarketplacePlugin IPC with rollback, seedOfficialSource on startup. Verified by audit. |

---

## What Needs Verification

- [~] GitHub repo created (`Auto-Scalers/Fabrica-plugins`)

---

## Session Ledger

> Tracks orchestration sessions and workers for this task file. Updated when sessions are created, released, or worktrees merged.

| Session Handle | Type | Task/Group | Status | Created | Worktree Branch | Merged |
|---------------|------|-----------|--------|---------|----------------|--------|
| `term_24ff1a27-8b86-43f8-9206-73367917448f` | orchestrator | plugins-orchestrator | **active** | Aug 2026 | `main` (Fabrica-plugins/) | — |

**Rules:**
- Only the main orchestrator creates sessions in this ledger
- Workers are released after review
- Worktrees are merged immediately after approval
- Never leave orphaned sessions

---

_Created: Aug 2026_

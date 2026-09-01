# Fabrica-update — Tasks

> Single source of truth for the **update pipeline** sub-project. Master plan: `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md`. Status: ⬜ TODO · 🔶 IN_PROGRESS · 👀 VERIFY · ✅ DONE · 🚫 BLOCKED · ❌ CANCELLED.

**Pipeline order (upstream-first):** T0 (update upstream) → T1 (upstream diff) → T2 (targeted rebrand diff, scoped to upstream-touching files only) → T3 (implementation mapping) → T4 (sync runbook).

## What Exists in This Workspace

| Directory/File | What It Is |
|---|---|
| `orca-baseline/` | Frozen Orca source Fabrica was rebranded from (copy of `.backup/orca`). Read-only. |
| `upstream-orca/` | Current upstream Orca (`stablyai/orca`). Read-only comparison source. |
| `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md` | Master plan: two-diff framework, 4 phases, strict preservation rules. |

No deliverables produced yet. Read-only analysis workspace — no code changes to any Fabrica project.

## Scope

**In scope:** `Fabrica-app/`, `Fabrica-plugins/`, and all 8 plugin repos inside `Fabrica-plugins/`.
**Out of scope:** `Fabrica-web/`, `Fabrica-relay/`, `Fabrica-atlas/`, `Fabrica-marketing/`.

Upstream sources: `https://github.com/stablyai/orca` (the app) and `https://github.com/stablyai/orca-plugins` (the plugins).

---

## Rollup

| Metric | Value |
|---|---|
| Total tasks | 5 |
| ✅ DONE | 0 |
| 🔶 IN_PROGRESS | 0 |
| 👀 VERIFY | 0 |
| ⬜ TODO | 5 |
| 🚫 BLOCKED | 0 |
| ❌ CANCELLED | 0 |
| Completion | 0% |

_Last recount: 2026-09-01_

---

## Tasks

| # | Task | Status | Notes |
|---|------|--------|-------|
| T0 | Update `upstream-orca/` to latest commit; record hash | ⬜ | Pull/fetch latest from confirmed repo + branch (Q1). Record exact commit hash. |
| T1 | Phase 1 — Upstream diff (`orca-baseline/` vs `upstream-orca/`) → `UPSTREAM-DIFF-MAP.md` + `.json` | ⬜ | Depends on T0. Risk tags: safe / rebrand-touching / conflicting / incompatible. |
| T2 | Phase 2 — Targeted rebrand diff (`orca-baseline/` vs `Fabrica-app/`, scoped to upstream-touching files) → `REBRAND-DIFF-MAP.md` + `.json` | ⬜ | Depends on T1. Only diff rebrand in files where upstream changed. Intent tags: rebrand / custom logic / debrand cleanup / incidental. |
| T3 | Phase 3 — Cross-reference upstream map + rebrand map → `SYNC-IMPLEMENTATION-PLAN.md` | ⬜ | Depends on T1 + T2. Per-change action: port verbatim / port+rebrand / merge / skip+rewrite. |
| T4 | Phase 4 — Sync script + runbook → `UPDATE-SYNC-RUNBOOK.md` | ⬜ | Depends on T1+T2+T3. Repeatable per-release procedure. |

---

## Session Ledger

| Task | Session name | task_id | dispatch_id (ctx) | terminal (term) | Status |
|------|--------------|---------|-------------------|-----------------|--------|
| init | sub-project-init | task_fabrica_update_init | ctx_local | term_local_fabrica_update_init | ✅ done 2026-08-29 — AGENTS.md + tasks file written; `orca-baseline/` populated; `upstream-orca/` cloned. |

---

## Checkpoint

| Field | Value |
|---|---|
| **Current Phase** | Phase 0 — Update upstream-orca |
| **Current Task** | T0 ready. No progress. |
| **Next Action** | Dispatch T0 (update `upstream-orca/` to latest commit from `stablyai/orca`). |
| **Blockers** | None — Q1 answered (upstream = `stablyai/orca`, scope includes `stablyai/orca-plugins`). |
| **Last Checkpoint** | 2026-09-01 |

# Fabrica-update — Tasks

> Single source of truth for the **update pipeline** sub-project. Master plan: `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md`. Status: ⬜ TODO · 🔶 IN_PROGRESS · 👀 VERIFY · ✅ DONE · 🚫 BLOCKED · ❌ CANCELLED.

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

---

## Rollup

| Metric | Value |
|---|---|
| Total tasks | 4 |
| ✅ DONE | 0 |
| 🔶 IN_PROGRESS | 0 |
| 👀 VERIFY | 0 |
| ⬜ TODO | 4 |
| 🚫 BLOCKED | 0 |
| ❌ CANCELLED | 0 |
| Completion | 0% |

_Last recount: 2026-08-31_

---

## Tasks

| # | Task | Status | Notes |
|---|------|--------|-------|
| T1 | Phase 1 — Rebrand diff (`orca-baseline/` vs `Fabrica-app/`) → `REBRAND-DIFF-MAP.md` + `.json` | ⬜ | Line-level map of everything we changed. Intent tags: rebrand / custom logic / debrand cleanup / incidental. |
| T2 | Phase 2 — Upstream diff (`orca-baseline/` vs `upstream-orca/`) → `UPSTREAM-DIFF-MAP.md` + `.json` | ⬜ | Depends on T1. Risk tags: safe / rebrand-touching / conflicting / incompatible. |
| T3 | Phase 3 — Cross-reference rebrand map + upstream map → `SYNC-IMPLEMENTATION-PLAN.md` | ⬜ | Depends on T1 + T2. Per-change action: port verbatim / port+rebrand / merge / skip+rewrite. |
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
| **Current Phase** | Phase 1 — Rebrand Diff |
| **Current Task** | T1 ready. No progress. |
| **Next Action** | Dispatch T1 (rebrand diff: orca-baseline vs Fabrica-app → REBRAND-DIFF-MAP). |
| **Blockers** | None. Discovery + analysis + plans only. |
| **Last Checkpoint** | 2026-08-31 |

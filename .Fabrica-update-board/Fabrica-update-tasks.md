# Fabrica-update — Tasks

> Single source of truth for the **update pipeline** sub-project. Master plan: `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md`. Status: ⬜ TODO · 🔶 IN_PROGRESS · 👀 VERIFY · ✅ DONE · 🚫 BLOCKED · ❌ CANCELLED.

**Pipeline order (upstream-first):** T0 (update upstream) → T1 (upstream diff) → T2 (targeted rebrand diff, scoped to upstream-touching files only) → T3 (implementation mapping).

## What Exists in This Workspace

| Directory/File | What It Is |
|---|---|
| `orca-baseline/` | Frozen Orca source Fabrica was rebranded from (copy of `../.backup/orca`). Read-only. |
| `upstream-orca/` | Current upstream Orca (`stablyai/orca`). Read-only comparison source. |
| `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md` | Master plan: two-diff framework, 3 phases, strict preservation rules. |

No deliverables produced yet. Read-only analysis workspace — no code changes to any Fabrica project.

## Scope

**In scope:** `Fabrica-app/`, `Fabrica-plugins/`, and all 8 plugin repos inside `Fabrica-plugins/`.
**Out of scope:** `Fabrica-web/`, `Fabrica-relay/`, `Fabrica-atlas/`, `Fabrica-marketing/`.

Upstream sources: `https://github.com/stablyai/orca` (the app) and `https://github.com/stablyai/orca-plugins` (the plugins).

---

## Rollup

| Metric | Value |
|---|---|
| Total tasks | 4 |
| ✅ DONE | 4 |
| 🔶 IN_PROGRESS | 0 |
| 👀 VERIFY | 0 |
| ⬜ TODO | 0 |
| 🚫 BLOCKED | 0 |
| ❌ CANCELLED | 0 |
| Completion | 100% |

_Last recount: 2026-09-02_

---

## Tasks

| # | Task | Status | Notes |
|---|------|--------|-------|
| T0 | Update `upstream-orca/` to latest commit; record hash | ✅ | Commit `f737f3499f` (2026-09-02, "fix(relay): stream an oversized fs.listFiles reply instead of refusing it (#17954)") — updated from `d7123591ce` |
| T1 | Phase 1 — Upstream diff (`orca-baseline/` vs `upstream-orca/`) → `UPSTREAM-DIFF-MAP.md` + `.json` | ✅ | 9,875 added / 5,269 modified / 423 deleted / 7,503 unchanged. Bulk in `src/` (9,052 added, 4,641 modified) and `mobile/` (329 added, 331 modified). Commit `f737f3499f`. |
| T2 | Phase 2 — Targeted rebrand diff (`orca-baseline/` vs `Fabrica-app/`, scoped to upstream-touching files) → `REBRAND-DIFF-MAP.md` + `.json` | ✅ | 1,894 rebrand-modified / 3,302 rebrand-unchanged / 73 not in Fabrica. Intent: 507 rebrand, 1,361 custom logic, 26 incidental. |
| T3 | Phase 3 — Cross-reference upstream map + rebrand map → `SYNC-IMPLEMENTATION-PLAN.md` | ✅ | 13,250 port_verbatim / 507 port_and_rebrand / 1,361 merge / 26 skip / 423 verify_and_remove. |

---

## Session Ledger

| Task | Session name | task_id | dispatch_id (ctx) | terminal (term) | Status |
|------|--------------|---------|-------------------|-----------------|--------|
| init | sub-project-init | task_fabrica_update_init | ctx_local | term_local_fabrica_update_init | ✅ done 2026-08-29 — AGENTS.md + tasks file written; `orca-baseline/` populated; `upstream-orca/` cloned. |
| T0 | update-upstream-orca | ses_fa023ba56ffeOOLCORRcjQx5zS | — | — | ✅ done 2026-09-02 — upstream-orca/ updated to `f737f3499f` (605 files changed, +32,702/-6,649). |
| T1 | upstream-diff-map | task_fabrica_update_t1 | ctx_local | — | ✅ done 2026-09-02 — 9,875 added / 5,269 modified / 423 deleted. UPSTREAM-DIFF-MAP.md + .json produced. |
| T2 | rebrand-diff-map | task_fabrica_update_t2 | ctx_local | — | ✅ done 2026-09-02 — 1,894 rebrand-modified / 3,302 unchanged / 73 not in Fabrica. REBRAND-DIFF-MAP.md + .json produced. |
| T3 | sync-implementation-plan | task_fabrica_update_t3 | ctx_local | — | ✅ done 2026-09-02 — 13,250 port_verbatim / 507 port_and_rebrand / 1,361 merge / 26 skip / 423 verify_and_remove. SYNC-IMPLEMENTATION-PLAN.md produced. |

---

## Checkpoint

| Field | Value |
|---|---|
| **Current Phase** | Pipeline complete |
| **Current Task** | All tasks done. |
| **Next Action** | Review SYNC-IMPLEMENTATION-PLAN.md. The 1,361 "merge" files need PM review. 507 "port_and_rebrand" files need Orca→Fabrica substitution applied. |
| **Blockers** | None |
| **Last Checkpoint** | 2026-09-02 |

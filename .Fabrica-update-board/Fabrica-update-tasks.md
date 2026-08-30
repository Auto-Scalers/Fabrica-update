# Fabrica-update — Tasks

> Single source of truth for the **update pipeline** sub-project. The master plan lives at `Fabrica-app/.Fabrica-app-board/UPDATE-PIPELINE-PLAN.md` — this file owns execution details. Status enum: ⬜ TODO · 🔶 IN_PROGRESS · 👀 VERIFY · ✅ DONE · 🚫 BLOCKED · ❌ CANCELLED.

## High-Level Goals
1. Map the Orca↔Fabrica diff surface (rebrand diff + upstream diff + implementation mapping).
2. Produce the "massive file" of mapped code lines (`REBRAND-DIFF-MAP`).
3. Build the repeatable sync workflow + runbook + CI guard.
4. First end-to-end sync run producing a reviewable PR.
5. **Strict preservation:** every change we made in Fabrica is preserved verbatim; updates are adapted to our patterns.

## Scope (per PM 2026-08-29)
**In scope:** `Fabrica-app/`, `Fabrica-plugins/`, and all 8 plugin repos inside `Fabrica-plugins/`.
**Out of scope:** `Fabrica-web/`, `Fabrica-relay/`, `Fabrica-atlas/`, `Fabrica-marketing/`.
**Upstream sources:** `https://github.com/stablyai/orca` (app) + `https://github.com/stablyai` (org, for plugin upstreams).

## PM constraints (2026-08-29) — READ-ONLY DISCOVERY PHASE
- **For now: discovery + analysis + plans ONLY. No code changes** to any project (Fabrica-app, Fabrica-plugins, plugin repos, or any other).
- Use the **historical task files + archived logs** (the `.archive/` folders) to capture our rebrand patterns — that's the main reason they exist.
- The historical record is the ground truth for "what we changed and how."

---

## Rollup
| Metric | Value |
|---|---|
| Total tasks | 6 |
| ✅ DONE | 1 |
| 🔶 IN_PROGRESS | 0 |
| 👀 VERIFY | 0 |
| ⬜ TODO | 5 |
| 🚫 BLOCKED | 0 |
| ❌ CANCELLED | 0 |
| Completion | 17% |

_Last recount: 2026-08-29 (sub-project initialized: AGENTS.md + tasks file written; `orca-baseline/` copied from `.backup/orca`; `upstream-orca/` cloned from `https://github.com/stablyai/orca`; historical docs copied to `.Fabrica-update-board/historical/`. T2 (clone) effectively done as part of init. T1/T3/T4/T5/T6 pending dispatch.)_

---

## Tasks (from UPDATE-PIPELINE-PLAN.md)

| # | Task | Status | Notes |
|---|------|--------|-------|
| T1 | Phase 1 — Rebrand diff (`.backup/orca` vs in-scope Fabrica repos) → `REBRAND-DIFF-MAP.md` + `.json` (the "massive file" of mapped code lines + intent tags + our patterns) | ⬜ | **Primary deliverable.** Line-level diff across the in-scope repos; consult historical task files + `.archive/` for our patterns. No code changes. |
| T2 | Confirm upstream Orca repo + branch; clone read-only | ✅ | `https://github.com/stablyai/orca` cloned to `upstream-orca/` (part of init). For the 8 plugin repos, the worker will identify each plugin's upstream source under `https://github.com/stablyai` during T3 and clone as needed. |
| T3 | Phase 2 — Upstream diff (`.backup/orca` vs upstream-orca; and for each plugin repo, upstream-plugin vs current) → `UPSTREAM-DIFF-MAP.md` + `.json` (risk tags + rebrand pattern to apply) | ⬜ | Depends on T1 (rebrand patterns inform risk tagging). Read-only. |
| T4 | Phase 3 — Cross-reference rebrand map + upstream map → `SYNC-IMPLEMENTATION-PLAN.md` (per-change action: port verbatim / port + rebrand / merge / skip+rewrite) | ⬜ | Depends on T1 + T3. |
| T5 | Phase 4 — Sync script + runbook + CI guard → `UPDATE-SYNC-RUNBOOK.md` (and supporting script) | ⬜ | Depends on T1+T3+T4. The runbook turns the maps into a repeatable per-release procedure. |
| T6 | First end-to-end sync run against the latest upstream Orca release → reviewable PR | ⬜ | Depends on T5. First real sync. |

---

## Open PM questions (carried from the plan)
- Q1 ✅ — Upstream: `https://github.com/stablyai/orca` + `https://github.com/stablyai` (org).
- Q2 ✅ — Scope: `Fabrica-app/`, `Fabrica-plugins/`, + all 8 plugin repos inside.
- Q3 — Conflicting upstream changes (overlap our custom logic): worker proposes options, PM decides.
- Q4 — Incompatible changes (upstream assumes Orca infra we removed): worker proposes a Fabrica-native option, PM decides.

---

## Session Ledger
| Task | Session name | task_id | dispatch_id (ctx) | terminal (term) | Status |
|------|--------------|---------|-------------------|-----------------|--------|
| init | sub-project-init | task_fabrica_update_init | ctx_local | term_local_fabrica_update_init | ✅ done 2026-08-29 — AGENTS.md + tasks file written; `orca-baseline/` populated from `.backup/orca`; `upstream-orca/` cloned from `stablyai/orca`; historical docs copied to `.Fabrica-update-board/historical/` (root roadmap/DNA, Fabrica-app + Fabrica-plugins task files + archives). |

---

## Checkpoint (Current State)
| Field | Value |
|---|---|
| **Current Group** | Setup |
| **Current Task** | Sub-project initialized (T2 effectively done as part of init). Ready to dispatch T1. |
| **Last Action** | Created GitHub repo `Auto-Scalers/Fabrica-update`, local folder, AGENTS.md, tasks file, copied `orca-baseline/`, cloned `upstream-orca/`, copied historical docs. |
| **Next Action** | Dispatch T1 (rebrand diff → `REBRAND-DIFF-MAP`). No code changes anywhere. |
| **Blockers** | None. PM constraints: discovery + analysis + plans only. |
| **Last Checkpoint** | 2026-08-29 (sub-project initialized) |

# Fabrica-update — Tasks

> Single source of truth for the **update pipeline** sub-project. The master plan lives at `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md` — this file owns execution details. Status enum: ⬜ TODO · 🔶 IN_PROGRESS · 👀 VERIFY · ✅ DONE · 🚫 BLOCKED · ❌ CANCELLED.

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
| ✅ DONE | 2 |
| 🔶 IN_PROGRESS | 0 |
| 👀 VERIFY | 0 |
| ⬜ TODO | 4 |
| 🚫 BLOCKED | 0 |
| ❌ CANCELLED | 0 |
| Completion | 33% |

_Last recount: 2026-08-30 (T1 DONE: REBRAND-DIFF-MAP.md + .json produced via 6 parallel workers. T2 DONE as part of init. T3/T4/T5/T6 pending.)_

---

## Tasks (from UPDATE-PIPELINE-PLAN.md)

| # | Task | Status | Notes |
|---|------|--------|-------|
| T1 | Phase 1 — Rebrand diff (`.backup/orca` vs in-scope Fabrica repos) → `REBRAND-DIFF-MAP.md` + `.json` (the "massive file" of mapped code lines + intent tags + our patterns) | ✅ | **DONE 2026-08-30.** 6 workers dispatched in parallel (T1-A through T1-E + T1-FINAL assembly). REBRAND-DIFF-MAP.md (308 lines) + REBRAND-DIFF-MAP.json (40KB) produced. ~872 files compared, ~1800 hunks mapped, 22 rebrand patterns, 28 custom logic items, 14 debrand cleanup items, 10 incidental, 2 bugs found. No code changes to any project. |
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
| T1 rebrand-diff | t1-rebrand-diff | task_2bfde97ba08d | ctx_88e55163b457 | term_bdbcf7d7-7eda-4588-a449-a079603645c0 | ❌ failed 2026-08-30 — terminal exited with no output (dispatched to wrong worktree root) |
| T1-A config | t1a-config-diff | task_f6e212f7b2b6 | ctx_1b1092bca289 | term_f09bc245-cf95-44aa-b25d-5e49339c2b20 | ✅ done 2026-08-30 — config files + Casks + .github + docs diff. Findings: `.Fabrica-update-board/T1-A-findings.md` (11 file categories, ~120 diff hunks, all rebrand except 2 custom-logic additions: Supabase dep + env injection) |
| T1-B main+cli | t1b-main-cli-diff | task_2c4be8205c39 | ctx_8b6ec21a773f | term_b20120f9-6bb3-4552-9c39-6157cb216c40 | ✅ done 2026-08-30 — src/main/ + src/cli/ diff complete; findings written to `.Fabrica-update-board/T1-B-findings.md` (196 main + 72 cli files modified; 2 bugs found; pattern registry for T3/T4) |
| T1-C renderer+shared | t1c-renderer-shared-diff | task_f84373ccc456 | ctx_d01629a02782 | term_83b66140-9f05-418f-ae9b-f8df8a03f9dd | ✅ done 2026-08-30 — renderer files diff (10 file pairs, all rebrand). Findings: `.Fabrica-update-board/T1-C-findings.md` |
| T1-D preload+mobile | t1d-preload-mobile-diff | task_a76f4cae700d | ctx_ef9eb55478f7 | term_08bda283-085c-47c5-9720-71b9a93ff79c | ✅ done 2026-08-30 — src/preload/ + src/relay/ + src/types/ + renderer/assets + mobile/ + native/ diff → `.Fabrica-update-board/T1-D-findings.md` |
| T1-E intent-ref | t1e-intent-reference | task_9f9416799d03 | ctx_a72af232aaf1 | term_cb0408d4-7478-4c22-b8ca-b2de2142ee47 | ✅ done 2026-08-30 — T1-INTENT-REFERENCE.md written (60+ rebrand, 22 custom, 14 debrand, 10 incidental) |
| T1-FINAL assemble | t1-final-assembly | task_a7381f1d137a | ctx_7aca2ca2eb44 | term_232b0918-47d1-40b1-96af-5c5e67410f0c | 🔶 dispatched 2026-08-30 — assembling REBRAND-DIFF-MAP.md + .json from all findings |

---

## Checkpoint (Current State)
| Field | Value |
|---|---|
| **Current Group** | Phase 2 — Upstream Diff |
| **Current Task** | T1 DONE. Ready to dispatch T3 (upstream diff). |
| **Last Action** | T1 complete: 6 parallel workers produced REBRAND-DIFF-MAP.md (308 lines) + REBRAND-DIFF-MAP.json (40KB). ~872 files compared, ~1800 hunks, 22 patterns, 28 custom logic, 14 debrand, 10 incidental, 2 bugs. |
| **Next Action** | Dispatch T3 (upstream diff: orca-baseline vs upstream-orca → UPSTREAM-DIFF-MAP). Depends on T1 (patterns now available). |
| **Blockers** | None. PM constraints: discovery + analysis + plans only. |
| **Last Checkpoint** | 2026-08-30 (T1 DONE, T3 ready) |

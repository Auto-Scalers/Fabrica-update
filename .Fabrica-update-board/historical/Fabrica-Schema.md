# Fabrica Tracking Schema v1

> **The single canonical schema** for all roadmap files, task files, and AGENTS.md
> files across every Fabrica sub-project. If any file conflicts with this spec,
> this file wins.
>
> Owner: Orchestrator. Changes require PM approval.
> Structure base: Roadmap-02 style (Checkpoint + Autonomous Work System).

---

## 1. Status Enum (the ONLY allowed statuses)

| Status | Emoji | Meaning |
|---|---|---|
| `TODO` | ⬜ | Not started |
| `IN_PROGRESS` | 🔶 | Started, partially done |
| `VERIFY` | 👀 | Implemented, awaiting orchestrator review |
| `DONE` | ✅ | Implemented AND verified by reviewer |
| `BLOCKED` | 🚫 | Waiting on dependency/decision |
| `CANCELLED` | ❌ | Dropped |

Rules:
- One status per task. Never two systems in one file (no mixing bold words,
  `[x]`, `[~]`, or free text like "dead (hung)" inside the Status column).
- Extra detail (dead handles, verification evidence) goes in **Notes**, not Status.
- Legacy mapping: `PARTIAL→IN_PROGRESS`, `✅→DONE`, `⬜→TODO`, `🔶→IN_PROGRESS`,
  `🚫→BLOCKED`, `[x]→DONE`, `[~]→VERIFY`.

## 2. Task ID Format

`<PROJ>-<LOCAL_ID>` — project prefix keeps its historical local IDs:

| Prefix | Project | Examples |
|---|---|---|
| `APP-` | Fabrica-app | `APP-A1`, `APP-F3` |
| `WEB-` | Fabrica-web | `WEB-W14`, `WEB-W25` |
| `MKT-` | Fabrica-marketing | `MKT-M24` |
| `PLG-` | Fabrica-plugins | `PLG-P9` |
| `REL-` | Fabrica-relay | `REL-R31` |
| `R2-` | Roadmap 02 items | `R2-1.1`, `R2-3.2` |

Task IDs are permanent. Never reuse a retired ID; new tasks take the next number.
Cross-project duplicates (`W13b` lived in both web and marketing) must be resolved:
one owner keeps the ID, the other references it.

## 3. Standard File Structure (all roadmap + task files)

```markdown
# <Project> — <Tasks | Roadmap>
> One-line purpose + pointer to parent/sibling tracking files.

## Dashboard            (roadmap only — counts copied from Rollups)
## Right Now            (roadmap only)
## Rollup               REQUIRED in every task file — see §4

## Group/Phase N — <Name>
> WHAT THIS GROUP DOES:        (bullet list)
> WHAT THIS GROUP DOES NOT DO: (bullet list)

| # | Task | Status | Output/Notes |
|---|------|--------|--------------|

## Checkpoint (Current State)   REQUIRED — see §5
## Autonomous Work System       resume rules for heartbeat kicks
## Parallelism & Anti-Overlap   REQUIRED — see §9 (canonical block)
## Dependencies & Coordination Rules
## What Needs Verification      checkbox list `- [ ]`
## Session Ledger               REQUIRED — see §6

---
_Last updated: YYYY-MM-DD_
```

Roadmap-only sections are omitted in task files; everything else is mandatory.

## 4. Rollup Block (kills count drift)

Every task file carries exactly one Rollup block near the top. Counts MUST be
recountable from that file's own task tables. The Roadmap Dashboard copies these
numbers verbatim — it never recomputes or estimates.

```markdown
## Rollup

| Metric | Value |
|---|---|
| Total tasks | N |
| ✅ DONE | n |
| 🔶 IN_PROGRESS | n |
| 👀 VERIFY | n |
| ⬜ TODO | n |
| 🚫 BLOCKED | n |
| ❌ CANCELLED | n |
| Completion | NN% |

_Last recount: YYYY-MM-DD_
```

Sync rule: whenever a task status changes, update the Rollup in the same edit.

## 5. Checkpoint Table (resume state)

Every tracking file that drives autonomous work has a Checkpoint table updated
after every significant action:

```markdown
## Checkpoint (Current State)

| Field | Value |
|---|---|
| **Current Group** | <group/phase name> |
| **Current Task** | <task ID> — <one-line state> |
| **Last Action** | <what just happened> |
| **Next Action** | <exactly what to do on resume> |
| **Blockers** | <none or list> |
| **Last Checkpoint** | ISO timestamp |
```

Resume rule: any agent kicked by a heartbeat reads Checkpoint FIRST, then the
task tables, then continues from Next Action.

## 6. Session Ledger (one canonical column set)

```markdown
## Session Ledger

| Handle | Type | Task ID | Orchestration IDs | Status | Created | Branch | Merged |
|---|---|---|---|---|---|---|---|
```

- `Type`: `orchestrator` or `worker`.
- `Orchestration IDs`: `run_xxx / task_xxx / ctx_xxx` when they exist, else `—`.
- `Status` uses ONLY the §1 enum plus `RELEASED` (worker finished + released)
  and `DEAD` (terminal lost; note cause in Orchestration IDs column).
- Stale handles are pruned to an `.archive` table monthly or when the roadmap
  master ledger is reconciled.

## 7. AGENTS.md Skeleton (all sub-projects)

Every sub-project AGENTS.md contains these sections, in this order:

1. **What This Folder Is**
2. **Tech Stack**
3. **Commands** — build / lint / typecheck / test commands workers must run
   before claiming DONE (project-specific; no placeholders)
4. **Conventions** — style rules specific to this repo
5. **Definition of Done** — grep checks + commands that must pass + tracking
   files that must be updated (Rollup, Checkpoint, Ledger row)
6. **What You Do NOT Do**
7. **Key Directories / Files**
8. **Resume Protocol** — on heartbeat kick: read task-file Checkpoint first,
   then continue Next Action; never restart completed work
9. **Task File & Schema** — path to the task file + pointer to
   `.Fabrica-board/Fabrica-Schema.md` (this file)
10. **How to Send Results** — worker_done / escalation examples
11. **Orchestration IDs** — the task_/ctx_/term_ reference table

## 8. Sync Rules (who updates what, when)

| Event | File updated | By |
|---|---|---|
| Worker finishes a task | Task status → VERIFY, Notes updated | Worker session |
| Orchestrator review passes | VERIFY → DONE, Rollup recount, Checkpoint update | Orchestrator |
| Session created/released/merged | Session Ledger row + Roadmap master ledger | Orchestrator |
| Any status change | Rollup in same edit | Whoever edits |
| Heartbeat kick dispatched | Run Log in `.Fabrica-board/Heartbeat.md` §5 | Heartbeat automation |
| New sub-project | New AGENTS.md + task file per this schema | Orchestrator |

The Roadmap Dashboard is a mirror, never a source. If a number disagrees,
the task file's Rollup is correct until recounted.

## 9. Parallelism & Anti-Overlap Section (REQUIRED in every tracking file + AGENTS.md)

Every roadmap, task file, and sub-project AGENTS.md carries this canonical block
(verbatim, or a project-adapted version that keeps every rule):

```markdown
## Parallelism & Anti-Overlap Policy

> This project runs REAL 24/7 multi-terminal orchestration. Parallelism is the
> default: unlimited tokens, multi-terminal app, massive project, close deadline.

- **Minimum fleet:** the orchestrator keeps AT LEAST 3 active worker terminals at
  all times. Fewer than 3 on resume or cycle end => launching more comes FIRST,
  chosen from the highest-priority TODO/VERIFY tasks in this file, focused on
  high-level goals and principles, not micro-edits.
- **One task = one worker:** claim a task by setting its status IN_PROGRESS and
  recording your terminal handle in the Session Ledger BEFORE starting. Claimed
  tasks are forbidden to everyone else.
- **One folder = one orchestrator:** never work another slot's folder.
- **One file = one writer:** two live workers never edit the same file; such tasks
  run sequentially.
- **Claim-before-work:** confirm your Task ID is still unclaimed before executing;
  if done or claimed, stop and report instead of duplicating.
- **Cross-project dependencies:** record them as notes in the OTHER project's task
  file; never edit another project directly.
- **Quality bar unchanged under deadline pressure:** no DONE without verified
  evidence; status change and Rollup update happen in the same edit.
```

Sync rule: if this block is missing from any tracking file or AGENTS.md, the next
session touching that file adds it in the same edit.

---

_Created: 2026-08-22. Migration of existing files tracked as R2 follow-up tasks._

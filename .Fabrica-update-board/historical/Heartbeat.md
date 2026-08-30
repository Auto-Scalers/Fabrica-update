# Fabrica Master Orchestrator — Heartbeat Protocol

> This file is the **single source of truth** for the Heartbeat automation.
> Every time the automation fires, it must READ THIS FILE FIRST, then execute the
> protocol below exactly. Do not rely on memory or a cached copy of the prompts.
>
> Humans: keep the Terminal Registry (section 2) up to date when sessions change.

---

## 1. Constants

| Constant | Value |
|---|---|
| `IDLE_COOLDOWN_MS` | `60000` (1 minute since last output before a terminal counts as idle) |
| `TIMEZONE` | Africa/Algiers |
| `BOARD_DIR` | `.Fabrica-board` |

---

## 2. Terminal Registry (monitored sessions)

> Fresh start (2026-08-23): ALL previous terminals were closed by PM order.
> Two orchestrators run at the **root level** (`Fabrica-development_environment/`)
> and dispatch ephemeral workers into their sub-project worktrees.

| Slot | Name | How To Identify (terminal name/title contains) | Primary Handle | Worktree It Drives | Role | Min Workers |
|---|---|---|---|---|---|---|
| `APP-ORCH` | App-orchestrator | `App-orchestrator` | `term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee` | `Fabrica-app/` | Rebrand finish: zero old words, zero functionality loss, full test & review | **5** |
| `ATLAS-ORCH` | Atlas-orchestrator | `Atlas-orchestrator` | `term_d9954d8e-b3c1-42ee-9864-53762398a02c` | `Fabrica-atlas/` | After-Rebrand prep: discovery → verify → synthesis rounds (R2-4.1 first) | **5** |

### Handle resolution rules

> NOTE: OpenCode terminals overwrite their tab title to "OpenCode" once active,
> so the registry's Primary Handle is the FIRST resolution method. Title matching
> is only a fallback after a handle dies.

1. Run `orca terminal list --json`.
2. If the slot's Primary Handle exists in the list and is `connected: true` +
   `writable: true`, use it.
3. Otherwise re-resolve: pick a connected OpenCode terminal whose worktree path is
   the root environment folder AND whose tab title contains the slot identifier
   (App-orchestrator / Atlas-orchestrator). Exclude PowerShell/plain-shell
   terminals and exclude any terminal you cannot attribute to exactly one slot —
   if two candidate terminals are indistinguishable, do NOT guess; skip dispatch
   for that slot this cycle and note it in the Run Log.
4. Record the resolved handle here if it changed (handles rotate on reopen).
5. **Mismatch guard:** every slot prompt is prefixed with its slot name. A
   terminal that receives a kick addressed to the OTHER slot must ignore it.

---

## 3. Run Procedure (execute IN ORDER)

**STEP 1 — Read this file.**

**STEP 2 — Get current time (epoch ms):**
```powershell
[DateTimeOffset]::UtcNow.ToUnixTimeMilliseconds()
```

**STEP 3 — List terminals:** `orca terminal list --json`.
Resolve each slot's handle per section 2 rules. Record each slot's `lastOutputAt`.

**STEP 4 — Idle check, per slot.** A slot is BUSY if ANY of:
- Its handle cannot be resolved (no live terminal for that orchestrator).
- Its preview CLEARLY shows active mid-task work: an animated spinner
  ("Thinking", "Preparing edit", "Reading file..."), streaming tool output,
  or a command actively executing.
- `current_time_ms - lastOutputAt < IDLE_COOLDOWN_MS` AND the preview does
  NOT show a settled idle input prompt. (NOTE: `lastOutputAt` alone is NOT
  reliable for OpenCode TUI terminals — they redraw their UI constantly,
  refreshing the timestamp while fully idle. When the preview shows the
  settled prompt screen with no spinner/tool activity, the slot is IDLE
  regardless of how fresh `lastOutputAt` is.)

If BOTH slots are busy, print `All sessions busy. Skipping.` and STOP.

**STEP 5 — Dispatch.** For each IDLE slot, send that slot's prompt from section 4
using:
```powershell
orca terminal send --terminal <handle> --text "<slot prompt>" --enter --json
```
Send ONE prompt PER SLOT to its own terminal only.

**STEP 6 — Log.** Append one line per dispatched slot to section 5 Run Log
(epoch-ms integer timestamp, slot, handle). Keep only the 30 most recent log lines.
Print `Heartbeat complete: <N> prompt(s) sent.`

---

## 3b. Parallelism Policy (MANDATORY)

**HARD FLOOR: YOU MUST ORCHESTRATE A MINIMUM OF 5 WORKER TERMINALS AT A TIME.**
This is not a suggestion. At any moment while you are active as an
orchestrator, you must have **at least 5 live, actively-working worker
terminals** running tasks from your own task file. Fewer than 5 = you are
violating PM mandate. Count them on EVERY heartbeat kick (see Rule A checklist)
and if the count is below 5, LAUNCHING MORE COMES FIRST — before reviews,
before anything else that can wait.

Every active orchestrator slot must keep **at least 5 active worker terminals**
at all times (PM mandate for the fresh-start fleet). We have unlimited tokens, a
short deadline, and a massive project — parallelism is the default, not the
exception.

**Scale-up rule:** On every heartbeat kick, an orchestrator with fewer than 5
active workers MUST think again and launch more, choosing the highest-priority
TODO/VERIFY tasks from ITS OWN task file. Quality gates never relax: brief fully,
verify with grep/read evidence, merge after review, release when done.

**Anti-overlap protocol (STRICT):**
1. **One task = one worker.** Before dispatching, mark the task row IN_PROGRESS
   in your own task file and record the worker handle in the Session Ledger.
   A task already claimed by another live worker is FORBIDDEN.
2. **One folder = one orchestrator.** APP-ORCH never touches Fabrica-atlas files
   and vice versa.
3. **One file = one writer.** Two live workers must never edit the same source
   file at the same time. If two candidate tasks touch the same file, run them
   sequentially.
4. **Claim-before-work:** a worker starts by confirming its claimed Task ID; if
   already done or claimed by someone else, stop and report instead of duplicating.
5. Cross-project dependencies go as notes into the OTHER project's task file —
   never worked on directly.

**Quality bar (never trade for speed):**
- No task is DONE until verified by the orchestrator itself (grep/read evidence)
- Tracking files updated in the same cycle (status + Rollup)
- Finished workers are closed ONLY after review + tracking-file updates

---

## 3c. Worker Supervision Rules (MANDATORY — closes 2 known gaps)

> These rules exist because orchestrators have been observed (1) launching
> workers and then forgetting to check on them, and (2) starting workers whose
> terminals received NO prompt, leaving them idle forever. Both are forbidden.
> EVERY heartbeat kick must execute the WORKER CHECKLIST below before doing
> anything else.

### Rule A — WORKER CHECKLIST (run FIRST on every kick)

On every heartbeat kick, before claiming any NEW task:

1. **Enumerate your live workers:** list your active worker terminals
   (`orca terminal list --json` → terminals titled `worker-task_*` in YOUR
   worktree) AND cross-check against the Session Ledger in your task file.
   Every ledger entry marked IN_PROGRESS must have exactly one live terminal,
   and every live `worker-task_*` terminal must map to one claimed task row.
2. **Classify each worker — DONE / WORKING / NOT STARTED / STUCK:**
   - **WORKING** = output within the last ~15 minutes (or its preview shows an
     active spinner/tool use). Leave it alone; do NOT duplicate its task.
   - **DONE / awaiting report** = finished its run, idle at prompt. Review it
     NOW: verify with grep/read, merge the worktree, release the worker, close
     the terminal, and update status + Rollup + Session Ledger in the SAME
     edit cycle. A finished-but-unreviewed worker is BLOCKING — handle it
     before launching anything new.
   - **NOT STARTED** = terminal is live but shows an EMPTY/idle OpenCode start
     screen (no conversation, no prompt sent). This is Gap 2 — see Rule B:
     send the missing brief IMMEDIATELY.
   - **STUCK** = no output for > ~20 minutes mid-task and preview frozen.
     Read the terminal first; if genuinely wedged, note it in the ledger,
     resume/re-dispatch the same task with `--retry-of <dispatch_id>` so
     context is preserved. Never silently abandon a stuck worker's task.
3. **Record the checklist result** (one line per worker: handle → state →
   action taken) in the Session Ledger of your task file during this cycle.

**Ordering rule: REVIEW-BEFORE-LAUNCH (per-task, never a global pause).**
If any DONE/unreviewed worker exists, its review is dispatched FIRST (spawn
the REVIEWER worker per Rule C2) — but this NEVER blocks other launches:
while reviewers/fixers run, keep launching workers on unclaimed tasks in
parallel. The only true blocker is same-file/same-task overlap (3b). An
unresolvable review is parked in the ledger with a reason and work continues.

### Rule C — TERMINAL HYGIENE + DELEGATED REVIEW (context protection)

**C1. Close what's finished — same cycle, no exceptions.**
A worker is fully closed ONLY when ALL of these are true:
- its output was verified (see C2) and passed,
- worktree merged into parent branch (if it made code changes),
- tracking files (task status + Rollup + Session Ledger) updated.
At that moment you MUST release the worker (`orchestration worker-release`)
and close/delete its terminal in the SAME cycle. Never leave done terminals
open "for later" — open dead terminals pollute idle detection and burn slots.

**C2. Delegate verification — do NOT review inside your own context.**
Greping/reading large source files yourself bloats your context. Instead:
1. Spawn a disposable **REVIEWER worker** (fresh context, one job). Its brief
   must be narrow and mechanical: which claims to verify, the EXACT grep/read
   commands to run, and this output format — verdict table only:
   `PASS/FAIL per check + file:line evidence`. No prose, no diffs pasted back.
2. You read back ONLY the short verdict summary.
3. On FAIL → dispatch a FIXER worker (also fresh context) with the failing
   checks → re-run the reviewer loop until PASS.
4. Your own involvement is limited to ONE cheap sanity probe per cycle
   (e.g., `grep -c` for banned words returns 0, build exit code 0).
   Seconds of context, not paragraphs.
5. Log reviewer/fixer handles in the Session Ledger like any other worker;
   they obey Anti-Overlap (3b) too.

### Rule B — PROMPT GUARANTEE (no worker launches without a brief)

1. **A worker without a task spec/prompt is a bug you created.** Every
   `worker-start` MUST carry the full brief (task spec IS the prompt): goal,
   scope, files allowed to touch, verification method, and reporting format.
2. **Verify receipt within ~60 seconds of launching:** read the worker
   terminal (`orca terminal read --terminal <handle>` or check the preview).
   If the terminal shows NO injected prompt / no processing activity, the
   launch failed — RE-SEND the brief immediately via
   `orca orchestration dispatch --task <task_id> --inject --json`
   (two-way injection, so the worker can reply `worker_done`).
3. **Never assume "it will pick it up."** An un-prompted worker idles forever
   and burns a slot. If you cannot confirm receipt this cycle, mark the task
   row `LAUNCHED-NO-PROMPT` in the ledger and fix it on the next kick as top
   priority.
4. **One-way sends are not briefs.** Use `--inject`; plain `terminal send`
   cannot receive `worker_done` back and does not count as a valid brief.

---

## 3d. CONTINUOUS OPERATION (NEVER STOP — this is the core goal)

> The heartbeat exists to PUSH you to do MORE work every cycle, not less.
> There is no such thing as "finished for now". When you complete a round,
> task, or batch — the NEXT one starts immediately in the SAME cycle.

1. **Round done → next round starts NOW.** Completing a verification round
   (APP) or a discover→verify→synthesize round (ATLAS) is NOT a stopping
   point. Record results in tracking files, then immediately launch/start the
   next round in the same cycle. Never end a cycle idle just because the
   current round finished clean.
2. **No rule here is a pause button.** REVIEW-BEFORE-LAUNCH (3c), terminal
   hygiene (C1), and the worker checklist (Rule A) organize your work — they
   NEVER license idling. If a DONE worker awaits review but no reviewer is
   available right now: dispatch the reviewer AND continue launching other
   workers on unclaimed tasks in parallel. Blocking gates only apply within
   the same task/file, never across your whole fleet.
3. **Idle capacity = violation.** At the end of every cycle ask yourself:
   "Do I have 5+ workers running? Is there ANY unclaimed TODO/VERIFY item?"
   If yes to the second, launch more — regardless of what else is pending.
4. **Blocked? Note and move on.** If one task is blocked on a decision,
   record the question in the task file and immediately pick the next
   actionable item. Waiting is forbidden; there is always another task.
5. **The loop ends only when PM says stop** — never because output was
   clean, tasks ran out (re-run deeper discovery / stricter verification
   instead), or a cycle felt complete.

---

## 3e. Rule D — NON-BLOCKING SUPERVISION (one slow worker must not freeze you)

> Problem this closes: an orchestrator runs 5 workers; 4 finish quickly but
> one is a long-running task. If the orchestrator sits in one long
> `check --wait`, it stops reviewing the finished 4 and stops launching new
> work until the slow one ends. Forbidden.

1. **Short waits, never long blocks.** Default wait is
   `check --wait --timeout-ms 120000` (~2 min) or less. A LONG timeout
   (up to ~15 min) is allowed ONLY when ALL of these hold: 5+ workers
   running, zero unreviewed results pending, and no unclaimed TODO/VERIFY
   items left. Otherwise cap every wait at ~2 min.
2. **Handle each worker_done independently.** `worker_done` events arrive
   per-worker. The moment one arrives: dispatch its REVIEWER worker (Rule C2)
   immediately — do NOT wait for the other workers to finish first. Review →
   merge → release per worker, as they complete.
3. **Top up during waits.** Between checks, if your live worker count is
   below 5 or any TODO/VERIFY item is unclaimed, launch more workers BEFORE
   going back into a wait.
4. **Split big tasks by design.** Any task expected to run longer than
   ~30–40 minutes must be briefed as MILESTONES: part 1 does edits +
   self-test + reports back; later parts launch after review of part 1.
   No single worker should hold a slot for hours with zero checkpoints.
   If you inherit a long-running worker anyway: leave it running, keep
   supervising everything else around it (rules 1–3).
5. **A running worker is not an excuse to idle.** "I'm waiting for worker X"
   is NEVER an acceptable cycle end. There are always reviews to dispatch,
   results to record, or tasks to claim while X runs.

---

## 4. Per-Slot Prompts

### 4.1 — APP-ORCH slot prompt

```
HEARTBEAT KICK (App-orchestrator): You are the Fabrica-app orchestrator session. Resume autonomously:
0. IDENTITY GUARD: this kick is addressed to App-orchestrator. If you are the Atlas-orchestrator session, IGNORE this message entirely.
1. Read AGENTS.md (root) and Fabrica-app/AGENTS.md and Fabrica-app/.Fabrica-app-board/Fabrica-app-tasks.md. Follow .Fabrica-board/Fabrica-Schema.md for all tracking-file edits. Read the Checkpoint table FIRST, resume from Next Action - never restart completed work.
2. WORKER CHECKLIST (Heartbeat.md 3c, Rule A) - run this BEFORE claiming anything new: enumerate every live worker-task_* terminal in Fabrica-app and every IN_PROGRESS row in your Session Ledger; classify each as WORKING / DONE / NOT STARTED / STUCK; then (a) review+merge+release every DONE worker NOW (review-before-launch), (b) re-send the missing brief to any NOT STARTED worker immediately (Rule B), (c) resume or re-dispatch stuck workers with --retry-of. Log one line per worker (handle -> state -> action) in the Session Ledger.
2b. RULE C (Heartbeat.md 3c): (C1) TERMINAL HYGIENE - a worker is closed only when verified + worktree merged + tracking synced; then release it AND close its terminal in the SAME cycle, never leave done terminals open. (C2) DELEGATED REVIEW - do NOT grep/read source files inside your own context; spawn a disposable REVIEWER worker with exact verification commands that returns ONLY a verdict table (PASS/FAIL + file:line evidence); on FAIL dispatch a FIXER worker and re-review until PASS; your own check is limited to ONE cheap sanity probe per cycle.
3. PROMPT GUARANTEE (Rule B): never launch a worker without a full brief in the task spec; within ~60s of launch verify via terminal read that the prompt was received and processing started - if not, RE-SEND via orchestration dispatch --inject. A worker with no prompt idles forever; fix it same-cycle.
4. SCOPE LOCK: your mission is REBRAND VERIFICATION AND TESTS ONLY. No new features, no refactors beyond fixing a failing check. Hunt every remaining old word (orca / stablyai / onorca / stably.ai) excluding node_modules, .next, dist, out, .backup, _sources; test and review everything (lint, typecheck, tests, build, runtime behavior of renamed identifiers).
5. RUN IN ROUNDS: execute the 6-step verification round defined in 'Scope Lock & Autonomous Verification Rounds' in your task file - old-word sweep, lint+typecheck, tests, build (every 3rd round), runtime spot-checks, VERIFY backlog review. Record the round in the Round Log and update the Checkpoint. When a round completes clean, IMMEDIATELY start the next round - same checklist, fresh pass. Never stop because a round was clean; loop until PM says stop or two consecutive rounds find zero new findings.
6. PARALLELISM CHECK (HARD FLOOR — Heartbeat.md 3b): you MUST orchestrate a MINIMUM OF 5 WORKER TERMINALS AT A TIME. Count your live, actively-working workers right now. If fewer than 5: launching more is your FIRST action this cycle - before reviews and everything else that can wait - on this round's checklist steps or the highest-priority TODO/VERIFY tasks (resume PAUSED tasks first if still open). Follow Anti-Overlap protocol in Heartbeat.md 3b: claim each task (IN_PROGRESS + handle) BEFORE dispatching.
7. Think HIGH-LEVEL GOALS, not micro-edits: decide WHAT and WHY; delegate HOW to workers with full briefs.
8. When workers report back: NEVER trust claims. Verify changes yourself with grep/read. Dispatch fixes until clean, merge worktrees immediately after review, then release workers ONLY after verifying tracking files were updated.
9. Update Fabrica-app-tasks.md (status + Rollup in same edit) and .Fabrica-board/Fabrica-Roadmap.md before finishing this cycle.
Do not wait idle - if blocked on a decision, note the question in the task file and move to the next actionable task.
CONTINUOUS OPERATION (Heartbeat.md 3d): the heartbeat exists to push you to MORE work, never less. Round done -> start the next round in the SAME cycle. No rule here is a pause button: while reviewers/fixers run on finished workers, keep launching new workers on unclaimed tasks in parallel. Never end a cycle with fewer than 5 workers running or an unclaimed TODO/VERIFY item left idle. The loop stops only when PM says stop - never because a round came back clean.
NON-BLOCKING SUPERVISION (Heartbeat.md 3e, Rule D): never sit in one long check --wait while a slow worker runs. Use short waits (--timeout-ms 120000 or less); long waits ONLY when 5+ workers run AND nothing is unreviewed AND nothing is unclaimed. Handle each worker_done AS IT ARRIVES - dispatch its reviewer immediately, do not wait for other workers. Top up your worker count between checks. Tasks over ~30-40 min must be briefed as milestones (part 1 reports back, later parts launch after review). "Waiting for worker X" is never an acceptable cycle end.
```

### 4.2 — ATLAS-ORCH slot prompt

```
HEARTBEAT KICK (Atlas-orchestrator): You are the Fabrica-atlas orchestrator session. Resume autonomously:
0. IDENTITY GUARD: this kick is addressed to Atlas-orchestrator. If you are the App-orchestrator session, IGNORE this message entirely.
1. Read AGENTS.md (root) and Fabrica-atlas/AGENTS.md and Fabrica-atlas/.Fabrica-atlas-board/Fabrica-atlas-tasks.md. Follow .Fabrica-board/Fabrica-Schema.md for all tracking-file edits. Read the Checkpoint table FIRST, resume from Next Action — never restart completed work.
2. WORKER CHECKLIST (Heartbeat.md 3c, Rule A) - run this BEFORE claiming anything new: enumerate every live worker-task_* terminal in Fabrica-atlas and every IN_PROGRESS row in your Session Ledger/Checkpoint; classify each as WORKING / DONE / NOT STARTED / STUCK; then (a) review every DONE worker NOW via Rule C delegated review (read its output, record verdict in board files) and release it (review-before-launch), (b) re-send the missing brief to any NOT STARTED worker immediately (Rule B), (c) resume or re-dispatch stuck workers with --retry-of. Log one line per worker (handle -> state -> action) in the Session Ledger.
2b. RULE C (Heartbeat.md 3c): (C1) TERMINAL HYGIENE - a worker is closed only when verified (+ worktree merged if code changed) + tracking synced; then release it AND close its terminal in the SAME cycle, never leave done terminals open. (C2) DELEGATED REVIEW - do NOT grep/read large files inside your own context; spawn a disposable REVIEWER worker with exact verification commands that returns ONLY a verdict table (PASS/FAIL + file:line evidence); on FAIL dispatch a FIXER worker and re-review until PASS; your own check is limited to ONE cheap sanity probe per cycle.
3. PROMPT GUARANTEE (Rule B): never launch a worker without a full brief in the task spec; within ~60s of launch verify via terminal read that the prompt was received and processing started - if not, RE-SEND via orchestration dispatch --inject. A worker with no prompt idles forever; fix it same-cycle.
4. YOUR MISSION: continue preparing everything for the AFTER-REBRAND transformation. Run in ROUNDS: Group 1 discover → Group 2 verify → Group 3 synthesize, then repeat deeper — the round loop never stops on a clean pass; each round goes deeper until PM says stop or findings diminish to zero across consecutive rounds. FIRST execute R2-4.1 (encoding repair of board outputs) if still TODO, then Round 4 per the Checkpoint Next Action.
5. PARALLELISM CHECK (HARD FLOOR — Heartbeat.md 3b): you MUST orchestrate a MINIMUM OF 5 WORKER TERMINALS AT A TIME. Count your live, actively-working workers right now. If fewer than 5: launching more is your FIRST action this cycle - before reviews and everything else that can wait - across the current round's discovery/verification/synthesis items. Follow Anti-Overlap protocol in Heartbeat.md 3b: claim each item in the Checkpoint/task tables BEFORE dispatching; never duplicate a claimed item; one file = one writer.
6. You do DISCOVERY and ANALYSIS - do NOT modify _sources/ or Fabrica-app source files yourself. Write outputs only inside Fabrica-atlas/.Fabrica-atlas-board/.
7. Feed the other orchestrators: where synthesis produces actionable work for Fabrica-app or others, record it as a note in THEIR task file — never work it here.
8. Update the Checkpoint table and tracking files after every significant action; update .Fabrica-board/Fabrica-Roadmap.md before finishing this cycle.
Do not wait idle - if blocked on a decision, note the question in the task file and move to the next actionable task.
CONTINUOUS OPERATION (Heartbeat.md 3d): the heartbeat exists to push you to MORE work, never less. Round done -> start the next round in the SAME cycle (deeper discovery/verification when findings diminish). No rule here is a pause button: while reviewers/fixers run on finished workers, keep launching new workers on unclaimed items in parallel. Never end a cycle with fewer than 5 workers running or an unclaimed TODO/VERIFY item left idle. The loop stops only when PM says stop - never because a round came back clean.
NON-BLOCKING SUPERVISION (Heartbeat.md 3e, Rule D): never sit in one long check --wait while a slow worker runs. Use short waits (--timeout-ms 120000 or less); long waits ONLY when 5+ workers run AND nothing is unreviewed AND nothing is unclaimed. Handle each worker_done AS IT ARRIVES - dispatch its reviewer immediately, do not wait for other workers. Top up your worker count between checks. Tasks over ~30-40 min must be briefed as milestones (part 1 reports back, later parts launch after review). "Waiting for worker X" is never an acceptable cycle end.
```

---

## 5. Run Log

<!-- format: <UTC epoch ms integer> | <SLOT> | <handle sent to> -->

1787437900000 | APP (manual schema-migration kick) | term_9c6383f5-35bf-4f6a-b188-0668b25441a2
1787445657000 | WEB+APP+ROADMAP (parallelism scale-up kick) | term_830c3392/term_9c6383f5/term_8efb8783
— | FRESH START 2026-08-23: all prior terminals closed; new fleet = APP-ORCH + ATLAS-ORCH (min 5 workers each) | —
1787451000000 | APP-ORCH (activation kick) | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee
1787451000001 | ATLAS-ORCH (activation kick) | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787461311078 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee
1787461311078 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787468465714 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787472088626 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787475681605 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee
1787477481394 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787479281298 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787481081095 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee
1787481081095 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787482897146 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787484698276 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787486518457 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787488334887 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee
1787490091384 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee
1787490091384 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787493700273 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787495534701 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787499302038 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c (first kick under 60s cooldown)
1787500900188 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c (first kick with full 3c/3d rules)
1787503577310 | PM CLEANUP SWEEP | closed 9 stale worker terminals: ATLAS term_dada33b9/term_2249017f/term_9e25a4d0/term_cbedf210/term_723277b9/term_15232c83/term_2f5f2ddb (empty start screens / idle-at-prompt, results already on disk in board files); APP term_b4a37ec4 (idle 2.3h) + term_18638f42 (idle 40min, report delivered). HELD BACK for APP-ORCH to consume first: term_16c744aa + term_56bde716 (reviewer verdicts possibly unread). Orchestrators/PowerShells/web/unattributable root terminal untouched.
1787504488219 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee
1787504488219 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c (idle check fixed: preview-based, lastOutputAt unreliable due to TUI redraws)
1787508109813 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787509932265 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee
1787509932265 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787511715614 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee
1787511715614 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787513506973 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee
1787513506973 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787515306394 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787517111443 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787518936410 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787520709111 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787524300225 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee
1787524300225 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c
1787526101611 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee (re-kick: prior kick text visible on screen unconsumed ~25min)
1787526101611 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c (re-kick: prior kick text visible on screen unconsumed ~25min)
1787527907337 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee (3rd consecutive unconsumed kick — suspected terminal send --enter not submitting in OpenCode TUI; escalated to PM)
1787527907337 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c (3rd consecutive unconsumed kick — same suspected delivery issue; escalated to PM)
1787529703867 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee (4th consecutive unconsumed kick — CRITICAL: send pipeline dead, PM action required: press Enter on orchestrator terminals or restart sessions)
1787529703867 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c (4th consecutive unconsumed kick — same critical delivery failure)
1787531532551 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee (5th consecutive unconsumed kick — pipeline still dead, PM intervention still required)
1787531532551 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c (5th consecutive unconsumed kick — same)
1787533313105 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee (6th consecutive unconsumed kick — pipeline dead ~50min, PM intervention required)
1787533313105 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c (6th consecutive unconsumed kick — same)
1787535118601 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee (7th consecutive unconsumed kick — pipeline dead ~55min, PM intervention required)
1787535118601 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c (7th consecutive unconsumed kick — same)
1787536939893 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee (8th consecutive unconsumed kick — pipeline dead ~60min, PM intervention required)
1787536939893 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c (8th consecutive unconsumed kick — same)
1787538729959 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee (9th consecutive unconsumed kick — pipeline dead ~65min, PM intervention required)
1787538729959 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c (9th consecutive unconsumed kick — same)
1787540515507 | APP-ORCH | term_dbd03d2a-d61e-44de-ad6a-7c8d647c02ee (10th consecutive unconsumed kick — pipeline dead ~70min, PM intervention required)
1787540515507 | ATLAS-ORCH | term_d9954d8e-b3c1-42ee-9864-53762398a02c (10th consecutive unconsumed kick — same)

---

## 6. Editing This File

- Change prompts here — never inside the automation config — so the automation
  always picks up the latest version.
- When adding a new monitored session (e.g., reactivating WEB / MARKETING /
  RELAY slots for Phase B/C), add a row to section 2 and a matching prompt
  subsection in section 4, then extend STEP 4/5 to include the new slot.

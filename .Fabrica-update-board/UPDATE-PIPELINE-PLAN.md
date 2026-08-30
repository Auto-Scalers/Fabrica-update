# Fabrica-app Update Pipeline — Plan

> Plan for Roadmap **#17** (map the Orca↔Fabrica diff surface) and **#18** (build the repeatable sync workflow). Goal: a safe, repeatable way to pull new Orca features into Fabrica-app while **strictly preserving** the rebrand and our custom logic.

---

## Why
Fabrica-app is a fork of Orca. Upstream Orca keeps moving. Today there is no machine to sync upstream into Fabrica safely — every Orca release would be a manual, error-prone merge, with high risk of clobbering the rebrand (app id, icons, names, domains, env vars, wire tokens) or our custom logic. This plan builds that machine.

## Sources (three codebases, precise roles)
1. **`.backup/orca/`** *(inside Fabrica-app)* — the **frozen Orca source Fabrica was rebranded from**. This is the **fork baseline**. **Read-only. Never edited.**
2. **`Fabrica-app/`** — the **current Fabrica code** (rebrand + custom logic + post-rebrand changes). The live product. **Strictly preserve everything we changed.**
3. **New upstream Orca (GitHub)** — the **current upstream** we want to pull updates from. *(PM to confirm exact repo + branch — see Q1.)* Cloned read-only to a sibling dir (e.g. `_sources/upstream-orca/`) for comparison.

## The two-comparison framework
Everything is built on **two diffs** off the same baseline (`.backup/orca`):

- **Rebrand diff** = `.backup/orca` vs `Fabrica-app/` → **what we changed** (the set of edits we **strictly preserve** during any sync).
- **Upstream diff** = `.backup/orca` vs **new upstream Orca** → **what changed in Orca since we forked** (available updates, fixes, features, security patches).

Their intersection/union is where the sync pipeline operates.

## Three-phase approach

### Phase 1 — Rebrand diff analytics (`.backup/orca` vs `Fabrica-app/`)
**Goal:** a complete, line-level map of everything we changed, so the sync knows what to preserve.
1. Recursive structural diff `.backup/orca/` ↔ `Fabrica-app/` (read-only on `.backup/`).
2. For every differing file, capture: path, status (`added`/`removed`/`modified`/`renamed`), diff hunks with line numbers (backup line ↔ Fabrica line).
3. Tag each change with **intent** (worker judgment, PM-reviewed):
   - **rebrand** — identity-only (Orca→Fabrica strings, logos, names, app id, env vars, domains, wire tokens, package names). **Preserve verbatim.**
   - **custom logic** — features/fixes/patterns we added. **Preserve.**
   - **debrand cleanup** — Orca-only config/scripts we removed. Can be dropped on sync.
   - **incidental** — whitespace, dep bumps. Re-evaluate per sync.
4. **Output:** `REBRAND-DIFF-MAP.md` (human) + `REBRAND-DIFF-MAP.json` (machine) — the **"massive file"** PM asked for: every changed file, diff hunks, mapped line ranges, intent tag, and a short **"our pattern"** note (e.g. "we rewrote `login.onorca.dev` → `fabrica-ai.vercel.app`; any future Orca ref to that domain must use ours").

### Phase 2 — Upstream diff analytics (`.backup/orca` vs new upstream Orca)
**Goal:** a complete map of what changed in Orca since the fork — the available updates.
1. Clone the new upstream Orca to a read-only sibling dir. Confirm exact repo + branch with PM (Q1).
2. Structural diff `.backup/orca/` ↔ upstream clone, same shape as Phase 1.
3. Tag each upstream change with **rebrand-risk** (worker, PM-reviewed):
   - **safe** — in code paths we did not rebrand → port verbatim.
   - **rebrand-touching** — references an identifier we renamed → must apply our pattern.
   - **conflicting** — overlaps our custom logic → merge decision.
   - **incompatible** — assumes Orca infra we removed (e.g. `onorca.dev` backends) → skip or Fabrica-native rewrite.
4. **Output:** `UPSTREAM-DIFF-MAP.md` + `.json` — per-file hunks, risk tag, the rebrand pattern to apply.

### Phase 3 — Implementation mapping (per upstream change → how to land in Fabrica)
**Goal:** for every upstream change, a concrete "how to implement in Fabrica" decision that respects our patterns.
1. Look up the upstream change in `UPSTREAM-DIFF-MAP` (hunks + risk).
2. Cross-reference `REBRAND-DIFF-MAP` for the matching rebrand pattern in that area (e.g. "this is where we rewrote the domain; port the new endpoint to our domain and follow our env-var / config pattern").
3. Decide action:
   - **port verbatim** (safe) — include in the sync PR as-is.
   - **port + rebrand** (rebrand-touching) — port the logic, apply our substitutions.
   - **merge** (conflicting) — 3-way merge note for PM/worker review.
   - **skip / rewrite** (incompatible) — note why; propose a Fabrica-native option.
4. **Output:** `SYNC-IMPLEMENTATION-PLAN.md` — per-upstream-change table: upstream hunk, rebrand pattern to apply, action, ready-to-review diff.

## Phase 4 — The sync workflow (Roadmap #18, built on the maps)
Turn the maps into a tool + runbook + guard:
- **Script:** given a new upstream Orca commit/tag, regenerate the upstream diff against the fork baseline, cross-reference the rebrand map, and emit a PR-ready patch series.
- **Runbook (`UPDATE-SYNC-RUNBOOK.md`):** humans follow each Orca release — regenerate → review auto-resolved safe ports → triage conflicts → land the PR.
- **CI guard:** a job that re-runs the diff on every Fabrica commit and flags any accidental rebrand-introducing change (e.g. a stray `onorca.dev`).

## Output artifacts (under `Fabrica-app/.Fabrica-app-board/`)
| File | Phase | Purpose |
|---|---|---|
| `UPDATE-PIPELINE-PLAN.md` *(this file)* | — | The plan. |
| `REBRAND-DIFF-MAP.md` + `.json` | 1 | Our changes, line-mapped, intent-tagged, with "our pattern" notes (the massive file). |
| `UPSTREAM-DIFF-MAP.md` + `.json` | 2 | Upstream changes, line-mapped, risk-tagged, with rebrand pattern to apply. |
| `SYNC-IMPLEMENTATION-PLAN.md` | 3 | Per-upstream-change action + rebrand adaptation + ready diffs. |
| `UPDATE-SYNC-RUNBOOK.md` | 4 | Repeatable procedure per Orca release. |

## Strict rules (for every phase & sync)
- **Strictly preserve everything we changed in Fabrica** during any sync. Zero rebrand regressions, zero lost custom logic.
- **Adapt updates to our patterns** (rebrand substitutions, env vars, domains, naming, infra). Never reintroduce Orca identifiers.
- Read-only on `.backup/orca/` and the upstream clone. All writes are in `Fabrica-app/` and the new plan/map files under `.Fabrica-app-board/`.
- No commits/pushes by the worker; orchestrator handles git.
- Exclude from diff noise: `node_modules/`, `.next/`, `dist/`, `out/`, `build/`, `.git/`, `.backup/_sources/` (frozen).

## Tasks (initial decomposition)
| # | What | Who | Depends on |
|---|---|---|---|
| T1 | Phase 1 — produce `REBRAND-DIFF-MAP` (md + json). | Worker | — |
| T2 | Confirm upstream Orca repo + branch with PM; clone read-only. | Orchestrator | Q1 |
| T3 | Phase 2 — produce `UPSTREAM-DIFF-MAP` (md + json). | Worker | T2 |
| T4 | Phase 3 — produce `SYNC-IMPLEMENTATION-PLAN`. | Worker (+ PM review) | T1, T3 |
| T5 | Phase 4 (#18) — sync script + runbook + CI guard. | Worker | T1, T3, T4 |
| T6 | First end-to-end sync run against latest upstream Orca release → reviewable PR. | Worker + PM | T5 |

## Open questions for PM
- **Q1.** What is the exact URL + branch of the "new" upstream Orca repo? *(Original `stablyai/orca` default branch? Has it moved?)* — needed for Phase 2.
- **Q2.** Scope: is the pipeline **Fabrica-app only** (the forked app + mobile), or does it also cover Fabrica-web / Fabrica-plugins / Fabrica-relay? *(Default: Fabrica-app only — the others were built fresh, not forked from Orca.)*
- **Q3.** For **conflicting** upstream changes (overlap our custom logic): worker proposes options, PM decides? *(Default: yes.)*
- **Q4.** For **incompatible** changes (upstream assumes Orca infra we removed): should the worker propose a **Fabrica-native rewrite**, or just skip and note? *(Default: propose a native option; PM decides.)*

## Status
- **DRAFT 2026-08-29** — plan created per PM request; captures the two-comparison framework (`.backup/orca` baseline → rebrand diff + upstream diff), the three analytics phases, the "massive file" of mapped code lines, the strict-preservation rule, and the sync workflow.
- **Awaiting:** PM answers to Q1–Q4 and go-ahead to dispatch **T1** (rebrand diff → `REBRAND-DIFF-MAP`).

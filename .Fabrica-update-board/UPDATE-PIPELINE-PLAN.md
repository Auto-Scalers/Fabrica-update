# Fabrica-app Update Pipeline — Plan

> Plan for Roadmap **#17** (map the Orca↔Fabrica diff surface) and **#18** (build the repeatable sync workflow). Goal: a safe, repeatable way to pull new Orca features into Fabrica-app while **strictly preserving** the rebrand and our custom logic.

---

## Why
Fabrica-app is a fork of Orca. Upstream Orca keeps moving. Today there is no machine to sync upstream into Fabrica safely — every Orca release would be a manual, error-prone merge, with high risk of clobbering the rebrand (app id, icons, names, domains, env vars, wire tokens) or our custom logic. This plan builds that machine.

## Sources (three codebases, precise roles)
1. **`.backup/orca/`** *(inside Fabrica-app)* — the **frozen Orca source Fabrica was rebranded from**. This is the **fork baseline**. **Read-only. Never edited.**
2. **`Fabrica-app/`** — the **current Fabrica code** (rebrand + custom logic + post-rebrand changes). The live product. **Strictly preserve everything we changed.**
3. **New upstream Orca (GitHub)** — the **current upstream** we want to pull updates from. Two repos:
   - `https://github.com/stablyai/orca` → the app (cloned to `upstream-orca/`)
   - `https://github.com/stablyai/orca-plugins` → the plugins (cloned to `upstream-orca-plugins/`)

## The two-comparison framework
Everything is built on **two diffs** off the same baseline (`.backup/orca`):

- **Rebrand diff** = `.backup/orca` vs `Fabrica-app/` → **what we changed** (the set of edits we **strictly preserve** during any sync).
- **Upstream diff** = `.backup/orca` vs **new upstream Orca** → **what changed in Orca since we forked** (available updates, fixes, features, security patches).

Their intersection/union is where the sync pipeline operates.

## Four-phase approach (upstream-first)

The order is: **see what Orca changed** → **then check our rebrand only where it matters** → **map actions** → **build the sync tool**. This focuses effort on the intersection of upstream changes and our rebrand, instead of mapping everything we changed (most of which has no upstream conflict).

### Phase 0 — Update upstream Orca
**Goal:** get the latest upstream Orca source for comparison.
1. Pull/fetch the latest `upstream-orca/` from the confirmed repo + branch (Q1).
2. Confirm the repo is at a known commit/tag (record the hash).
3. **Output:** `upstream-orca/` at a specific, recorded commit.

### Phase 1 — Upstream diff analytics (`.backup/orca` vs updated `upstream-orca/`)
**Goal:** a complete map of what changed in Orca since the fork — the available updates.
1. Structural diff `.backup/orca/` ↔ `upstream-orca/`, excluding noise dirs (`node_modules/`, `.next/`, `dist/`, etc.).
2. For every differing file, capture: path, status (`added`/`removed`/`modified`/`renamed`), diff hunks with line numbers.
3. Tag each upstream change with **rebrand-risk** (worker, PM-reviewed):
   - **safe** — in code paths we did not rebrand → port verbatim.
   - **rebrand-touching** — references an identifier we renamed → must apply our pattern.
   - **conflicting** — overlaps our custom logic → merge decision.
   - **incompatible** — assumes Orca infra we removed (e.g. `onorca.dev` backends) → skip or Fabrica-native rewrite.
4. **Output:** `UPSTREAM-DIFF-MAP.md` + `.json` — per-file hunks, risk tag, the rebrand pattern to apply.

### Phase 2 — Targeted rebrand diff (`.backup/orca` vs `Fabrica-app/`, scoped to upstream-touching areas)
**Goal:** map our rebrand changes only in files/areas where upstream actually changed. No point mapping rebrand in files upstream never touched.
1. Take the file list from Phase 1 (files that differ in upstream).
2. For each upstream-changed file, diff `.backup/orca/` ↔ `Fabrica-app/` to see what we rebranded in that same area.
3. Tag each rebrand change with **intent** (worker, PM-reviewed):
   - **rebrand** — identity-only (Orca→Fabrica strings, logos, names, app id, env vars, domains, wire tokens, package names). **Preserve verbatim.**
   - **custom logic** — features/fixes/patterns we added. **Preserve.**
   - **debrand cleanup** — Orca-only config/scripts we removed. Can be dropped on sync.
   - **incidental** — whitespace, dep bumps. Re-evaluate per sync.
4. **Output:** `REBRAND-DIFF-MAP.md` + `.json` — focused map of our changes in upstream-touching files, intent-tagged, with "our pattern" notes.

### Phase 3 — Implementation mapping (per upstream change → how to land in Fabrica)
**Goal:** for every upstream change, a concrete "how to implement in Fabrica" decision that respects our patterns.
1. Look up the upstream change in `UPSTREAM-DIFF-MAP` (hunks + risk).
2. Cross-reference `REBRAND-DIFF-MAP` for the matching rebrand pattern in that area.
3. Decide action:
   - **port verbatim** (safe) — include in the sync PR as-is.
   - **port + rebrand** (rebrand-touching) — port the logic, apply our substitutions.
   - **merge** (conflicting) — 3-way merge note for PM/worker review.
   - **skip / rewrite** (incompatible) — note why; propose a Fabrica-native option.
4. **Output:** `SYNC-IMPLEMENTATION-PLAN.md` — per-upstream-change table: upstream hunk, rebrand pattern to apply, action, ready-to-review diff.

### Phase 4 — The sync workflow (Roadmap #18, built on the maps)
Turn the maps into a tool + runbook + guard:
- **Script:** given a new upstream Orca commit/tag, regenerate the upstream diff against the fork baseline, cross-reference the rebrand map, and emit a PR-ready patch series.
- **Runbook (`UPDATE-SYNC-RUNBOOK.md`):** humans follow each Orca release — regenerate → review auto-resolved safe ports → triage conflicts → land the PR.
- **CI guard:** a job that re-runs the diff on every Fabrica commit and flags any accidental rebrand-introducing change (e.g. a stray `onorca.dev`).

## Output artifacts (under `Fabrica-update/.Fabrica-update-board/`)
| File | Phase | Purpose |
|---|---|---|
| `UPDATE-PIPELINE-PLAN.md` *(this file)* | — | The plan. |
| `UPSTREAM-DIFF-MAP.md` + `.json` | 1 | Upstream changes, line-mapped, risk-tagged, with rebrand pattern to apply. |
| `REBRAND-DIFF-MAP.md` + `.json` | 2 | Our changes in upstream-touching files, intent-tagged, with "our pattern" notes. |
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
| T0 | Update `upstream-orca/` to latest commit; record hash. | Worker | Q1 answered |
| T1 | Phase 1 — produce `UPSTREAM-DIFF-MAP` (md + json). | Worker | T0 |
| T2 | Phase 2 — produce `REBRAND-DIFF-MAP` (md + json) — scoped to upstream-touching files only. | Worker | T1 |
| T3 | Phase 3 — produce `SYNC-IMPLEMENTATION-PLAN`. | Worker (+ PM review) | T1, T2 |
| T4 | Phase 4 (#18) — sync script + runbook + CI guard. | Worker | T1, T2, T3 |
| T5 | First end-to-end sync run against latest upstream Orca release → reviewable PR. | Worker + PM | T4 |

## Open questions for PM
- **Q1.** What is the exact URL + branch of the "new" upstream Orca repo? → **Answered:** `https://github.com/stablyai/orca` (default branch).
- **Q2.** Scope: is the pipeline **Fabrica-app only** (the forked app + mobile), or does it also cover Fabrica-web / Fabrica-plugins / Fabrica-relay? → **Answered:** Fabrica-app (`stablyai/orca`) + Fabrica-plugins (`stablyai/orca-plugins`). Out of scope: web, relay, atlas, marketing.
- **Q3.** For **conflicting** upstream changes (overlap our custom logic): worker proposes options, PM decides? → **Answered:** Yes — worker proposes, PM decides.
- **Q4.** For **incompatible** changes (upstream assumes Orca infra we removed): should the worker propose a **Fabrica-native rewrite**, or just skip and note? → **Answered:** Propose native option; PM decides.

## Status
- **DRAFT 2026-08-29** — plan created per PM request; captures the two-comparison framework, four phases, strict-preservation rule, and sync workflow.
- **UPDATED 2026-09-01** — reordered to upstream-first: T0 (update upstream) → T1 (upstream diff) → T2 (targeted rebrand diff, scoped to upstream-touching files) → T3 (implementation mapping) → T4 (sync tooling) → T5 (first sync run).
- **UPDATED 2026-09-01** — PM answered Q1–Q4. Q1: upstream = `stablyai/orca`. Q2: scope = Fabrica-app + Fabrica-plugins (via `stablyai/orca-plugins`). Q3: worker proposes, PM decides. Q4: propose native option, PM decides. **All blockers cleared.**
- **Ready to execute:** T0 is unblocked.

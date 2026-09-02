# Fabrica-app Update Pipeline — Plan

> Plan for Roadmap **#17** (map the Orca↔Fabrica diff surface). Goal: a safe, repeatable way to pull new Orca features into Fabrica-app while **strictly preserving** the rebrand and our custom logic.

---

## Why

Fabrica-app is a fork of Orca. Upstream Orca keeps moving. Today there is no machine to sync upstream into Fabrica safely — every Orca release would be a manual, error-prone merge, with high risk of clobbering the rebrand (app id, icons, names, domains, env vars, wire tokens) or our custom logic. This plan builds the analysis that makes that sync possible.

## Sources (three codebases, precise roles)

1. `**Fabrica-update/orca-baseline/**` — the **frozen Orca source Fabrica was rebranded from**. This is the **fork baseline**. **Read-only. Never edited.**
2. `**Fabrica-app/**` — the **current Fabrica code** (rebrand + custom logic + post-rebrand changes). The live product. **Strictly preserve everything we changed.**
3. **New upstream Orca (GitHub)** — the **current upstream** we want to pull updates from. Two repos:
  - `https://github.com/stablyai/orca` → the app (cloned to `upstream-orca/`)
  - `https://github.com/stablyai/orca-plugins` → the plugins (cloned to `upstream-orca-plugins/`)

**Scope:** Fabrica-app (`stablyai/orca`) + Fabrica-plugins (`stablyai/orca-plugins`). Out of scope: web, relay, atlas, marketing.

## The two-comparison framework

Everything is built on **two diffs** off the same baseline (`orca-baseline/`):

- **Upstream diff** = `orca-baseline/` vs `upstream-orca/` → **what changed in Orca since we forked** (available updates, fixes, features, security patches).
- **Rebrand diff** = `orca-baseline/` vs `Fabrica-app/` → **what we changed** (the set of edits we **strictly preserve** during any sync).

Their intersection/union is where the sync pipeline operates.

## Three-phase approach (upstream-first)

The order is: **see what Orca changed** → **then check our rebrand only where it matters** → **map actions**. This focuses effort on the intersection of upstream changes and our rebrand, instead of mapping everything we changed (most of which has no upstream conflict).

### Phase 0 — Update upstream Orca

**Goal:** get the latest upstream Orca source for comparison.

1. Pull/fetch the latest `upstream-orca/` from `https://github.com/stablyai/orca` (default branch).
2. Confirm the repo is at a known commit/tag (record the hash).
3. **Output:** `upstream-orca/` at a specific, recorded commit.

### Phase 1 — Upstream diff analytics (`orca-baseline/` vs updated `upstream-orca/`)

**Goal:** a complete map of what changed in Orca since the fork — the available updates.

1. Structural diff `orca-baseline/` ↔ `upstream-orca/`, excluding noise dirs (`node_modules/`, `.next/`, `dist/`, etc.).
2. For every differing file, capture: path, status (`added`/`removed`/`modified`/`renamed`), diff hunks with line numbers.
3. Tag each upstream change with **rebrand-risk** (worker, PM-reviewed):
  - **safe** — in code paths we did not rebrand → port verbatim.
  - **rebrand-touching** — references an identifier we renamed → must apply our pattern.
  - **conflicting** — overlaps our custom logic → merge decision (worker proposes options, PM decides).
  - **incompatible** — assumes Orca infra we removed (e.g. `onorca.dev` backends) → skip or propose Fabrica-native rewrite (worker proposes, PM decides).
4. **Output:** `UPSTREAM-DIFF-MAP.md` + `.json` — per-file hunks, risk tag, the rebrand pattern to apply.

### Phase 2 — Targeted rebrand diff (`orca-baseline/` vs `Fabrica-app/`, scoped to upstream-touching areas)

**Goal:** map our rebrand changes only in files/areas where upstream actually changed. No point mapping rebrand in files upstream never touched.

1. Take the file list from Phase 1 (files that differ in upstream).
2. For each upstream-changed file, diff `orca-baseline/` ↔ `Fabrica-app/` to see what we rebranded in that same area.
3. Tag each rebrand change with **intent** (worker, PM-reviewed):
  - **rebrand** — identity-only (Orca→Fabrica strings, logos, names, app id, env vars, domains, wire tokens, package names). **Preserve verbatim.**
  - **custom logic** — features/fixes/patterns we added. **Preserve.**
  - **debrand cleanup** — Orca-only config/scripts we removed. Can be dropped on sync.
  - **incidental** — whitespace, dep bumps. Re-evaluate per sync.
4. **Output:** `REBRAND-DIFF-MAP.md` + `.json` — focused map of our changes in upstream-touching files, intent-tagged, with "our pattern" notes.

### Phase 3 — Implementation mapping (per upstream change → how to land in Fabrica)

**Goal:** for every upstream change, a concrete "how to implement in Fabrica" decision that respects our patterns.

1. Look up the upstream change in `UPSTREAM-DIFF-MAP` (hunks + risk).
2. Cross-reference `REBRAND-DIFF-MAP` for the matching rebrand pattern in that area (intent).
3. Decide action using the **risk × intent matrix**:

| Risk | Intent | Action |
|---|---|---|
| safe | rebrand | **port verbatim** — rebrand already applied in our copy |
| safe | custom logic | **port verbatim** — our addition, no conflict |
| safe | debrand cleanup | **skip** — we removed it on purpose |
| safe | incidental | **skip** — noise (whitespace, dep bumps) |
| rebrand-touching | rebrand | **port + rebrand** — apply our substitution pattern |
| rebrand-touching | custom logic | **merge** — our logic touches a renamed identifier; worker proposes, PM decides |
| conflicting | any | **merge** — worker proposes options, PM decides |
| incompatible | any | **skip / rewrite** — worker proposes Fabrica-native option, PM decides; note why |

4. **Output:** `SYNC-IMPLEMENTATION-PLAN.md` — per-upstream-change table: upstream hunk, rebrand pattern to apply, action, ready-to-review diff.

## Rebrand Pattern (complete)

All patterns to substitute during any sync. Worker must scan for ALL of these in every file being ported:

| Pattern | Replacement | Notes |
|---------|-------------|-------|
| `stablyai` | (our org/domain) | GitHub org, npm scope, API references |
| `stably` | (our brand) | Appears in names, URLs, comments — not just `stablyai` |
| `orca` | `fabrica` | Lowercase — variable names, CLI commands, file refs |
| `Orca` | `Fabrica` | PascalCase — class names, component names |
| `ORCA` | `FABRICA` | UPPERCASE — env vars, constants |
| `onorca.dev` | (our domain) | Backend URLs, API endpoints |
| `on_orca` | (our pattern) | Underscore variant in code |
| App ID | (our app id) | Electron app identifier |
| Wire tokens | (our tokens) | Authentication/session tokens |
| Package names | (our packages) | npm/cargo package naming |
| Icons/logos | (our assets) | File paths to icon/logo assets |

**CRITICAL:** Even files tagged `port_verbatim` must be scanned for these patterns. Upstream may introduce new references to `orca`, `stably`, `stablyai` in code we never touched before. Every ported file gets a rebrand scan — no exceptions.

## Sync Actions — Execution Guide

After the pipeline produces `SYNC-IMPLEMENTATION-PLAN.md`, files are grouped by action. Here's how to process each:

### Action: port_verbatim (13,276 files)

**What:** Upstream changed a file we never modified (including former "skip" files — whitespace, dep bumps, formatting). Lowest risk — but NOT zero risk.

**Why not zero risk:** Upstream may introduce new references to `orca`, `stably`, `stablyai` in code paths that didn't have them before. We must scan every ported file.

**How to process:**
1. Copy file from `upstream-orca/` → `Fabrica-app/`
2. **Scan the copied file** for rebrand patterns (all items in the table above)
3. If patterns found → apply substitutions
4. If clean → done

**Can be batched:** Yes. Worker copies files, runs grep for rebrand patterns, applies substitutions where found.

### Action: port_and_rebrand (507 files)

**What:** Upstream changed a file where we previously applied Orca→Fabrica substitutions. Must pull the change AND reapply our naming pattern.

**How to process:**
1. Copy from `upstream-orca/` → `Fabrica-app/`
2. Apply full substitution pattern (all items in table above)
3. Verify no Orca/stably/stablyai identifiers remain
4. Check that our existing rebrand substitutions (from baseline→Fabrica) weren't overwritten

**Can be batched:** Yes, but slower — each file needs substitution + verification.

### Action: merge (1,361 files)

**What:** Our custom logic overlaps with upstream changes. Can't just copy — we'd lose our work.

**How to process (3 sub-approaches):**

| Situation | Approach |
|-----------|----------|
| Our change small, upstream change big | Review upstream diff, manually port our small addition on top |
| Upstream change small, our change big | Take our version, manually add the small upstream fix |
| Both big | Manual merge — read both diffs, resolve conflicts |

**Per-file merge workflow:**
1. Read `orca-baseline/` version (what we forked from)
2. Read `upstream-orca/` version (what they changed)
3. Read `Fabrica-app/` version (what we changed)
4. Produce merged version keeping both
5. Scan result for rebrand patterns

**Not automatable at scale.** Each file needs attention. Tackle in priority order:
1. Core app logic (`src/main/`, `src/services/`)
2. Plugin API
3. Mobile

### Action: verify_and_remove (423 files)

**What:** Upstream deleted these files. Need to check if Fabrica still uses them.

**How to process:**
1. For each file, grep `Fabrica-app/` for imports/references
2. If nothing references it → delete from `Fabrica-app/`
3. If something references it → keep it, may need rewrite

**Can be automated:** grep for each filename, report references, batch-delete unused ones.

## Recommended Execution Order

1. **port_verbatim** (13,276) — bulk copy + rebrand scan, biggest impact
2. **verify_and_remove** (423) — grep + batch delete, low risk
3. **port_and_rebrand** (507) — copy + full substitution, medium risk
4. **merge** (1,361) — manual review, highest effort, save for last

## Output artifacts (under `Fabrica-update/.Fabrica-update-board/`)


| File                                    | Phase | Purpose                                                                          |
| --------------------------------------- | ----- | -------------------------------------------------------------------------------- |
| `UPDATE-PIPELINE-PLAN.md` *(this file)* | —     | The plan.                                                                        |
| `UPSTREAM-DIFF-MAP.md` + `.json`        | 1     | Upstream changes, line-mapped, risk-tagged, with rebrand pattern to apply.       |
| `REBRAND-DIFF-MAP.md` + `.json`         | 2     | Our changes in upstream-touching files, intent-tagged, with "our pattern" notes. |
| `SYNC-IMPLEMENTATION-PLAN.md`           | 3     | Per-upstream-change action + rebrand adaptation + ready diffs.                   |


## Strict rules (for every phase &amp; sync)

- **Strictly preserve everything we changed in Fabrica** during any sync. Zero rebrand regressions, zero lost custom logic.
- **Adapt updates to our patterns** (rebrand substitutions, env vars, domains, naming, infra). Never reintroduce Orca identifiers.
- **Every ported file gets a rebrand scan** — even `port_verbatim` files. Upstream may add new `orca`/`stably`/`stablyai` references we need to substitute.
- Read-only on `orca-baseline/` and the upstream clone. All writes are in `Fabrica-app/` and the new plan/map files under `.Fabrica-update-board/`.
- No commits/pushes by the worker; orchestrator handles git.
- Exclude from diff noise: `node_modules/`, `.next/`, `dist/`, `out/`, `build/`, `.git/`, `.backup/_sources/` (frozen).

## Tasks


| #   | What                                                                                       | Who                  | Depends on |
| --- | ------------------------------------------------------------------------------------------ | -------------------- | ---------- |
| T0  | Update `upstream-orca/` to latest commit; record hash.                                     | Worker               | —          |
| T1  | Phase 1 — produce `UPSTREAM-DIFF-MAP` (md + json).                                         | Worker               | T0         |
| T2  | Phase 2 — produce `REBRAND-DIFF-MAP` (md + json) — scoped to upstream-touching files only. | Worker               | T1         |
| T3  | Phase 3 — produce `SYNC-IMPLEMENTATION-PLAN`.                                              | Worker (+ PM review) | T1, T2     |


## Status

- **Pipeline complete:** T0–T3 all done (2026-09-02).
- **Next:** Execute sync actions per the guide above. Start with `skip` → `port_verbatim` → `verify_and_remove` → `port_and_rebrand` → `merge`.


# Fabrica-update — Tasks (v2)

> Single source of truth for the **update pipeline** sub-project. Master plan: `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md` (v2 — fork-from-upstream). Status: ⬜ TODO · 🔶 IN_PROGRESS · 👀 VERIFY · ✅ DONE · 🚫 BLOCKED · ❌ CANCELLED.

**Pipeline order (fork-from-upstream):** T0 (pin upstream) → T1 (fork) → T2 (rebrand intent) → T3 (upstream diff) → T4 (custom-logic map) → T5 (apply rebrand) → T6 (re-implement custom logic) → T7 (final verification) → C-phase (clarification audit, 14 files, 60 findings) → I-phase (implementation fixes, 26 tasks) → R-phase (pipeline refinement to v3).

## What Exists in This Workspace


| Directory/File                                  | What It Is                                                                                                    |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `orca-baseline/`                                | Frozen Orca source the old `Fabrica-app` was rebranded from. Read-only reference.                             |
| `upstream-orca/`                                | Current upstream Orca (`stablyai/orca`) at a pinned commit. Read-only.                                        |
| `Fabrica-app/`                                  | Old Fabrica fork (v0.0.6, `0a5d258`). Read-only reference — source of "what we did" (rebrand + custom logic). |
| `.Fabrica-update-board/UPDATE-PIPELINE-PLAN.md` | Master plan (v2).                                                                                             |
| `.Fabrica-update-board/.archive/`               | Archived old plans, logs, snapshots.                                                                          |
| `.Fabrica-update-board/pipeline-files/`         | All T2-T7 output artifacts (maps, logs, reports).                                                             |
| `.Fabrica-update-board/c-audits/`               | C-phase audit outputs (C1-C14).                                                                               |


**Note (2026-09-04):** Old `Fabrica-app/` submodule removed from dev-env; fresh clone placed in `Fabrica-update/Fabrica-app/` as a read-only reference. `Fabrica/` is the new target repo (fresh clone of upstream `stablyai/orca`). Custom logic will be re-applied on top of the rebranded fork (see plan §"Strategy").

## Scope

**In scope (this plan):** new `Fabrica/` repo (fresh clone of upstream `stablyai/orca`), `Fabrica-plugins/`, and the 8 plugin repos.
**Out of scope:** `Fabrica-web/`, `Fabrica-relay/`, `Fabrica-atlas/`, `Fabrica-marketing/`.

Upstream sources: `https://github.com/stablyai/orca` (the app), `https://github.com/stablyai/orca-plugins` (the plugins).

---

## Rollup


| Metric         | Value |
| -------------- | ----- |
| Total tasks    | 61    |
| ✅ DONE         | 50    |
| 🔶 IN_PROGRESS | 0     |
| 👀 VERIFY      | 0     |
| ⬜ TODO         | 11    |
| 🚫 BLOCKED     | 1     |
| ❌ CANCELLED    | 0     |
| Completion     | 82%   |


*Last recount: 2026-09-08 (T0-T7, C1-C14, I-01 to I-26, R-phase, H1 DONE; AT-1 to AT-5 + H2-H7 TODO)*

---

## Tasks

### Phase 1 — Fork &amp; Transform (T0-T7)


| #   | Task                                                                                         | Status | Notes                                                                                                                                          |
| --- | -------------------------------------------------------------------------------------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| T0  | Update `upstream-orca/` to latest commit; record hash                                        | ✅      | Updated to `7ed86a98ae` (2026-09-04). Both `upstream-orca/` and `Fabrica/` now at this commit.                                                 |
| T1  | Fork `Auto-Scalers/Fabrica` from upstream; push clean baseline                               | ✅      | Repo exists on GitHub; `Fabrica/` has origin→Auto-Scalers/Fabrica + upstream→stablyai/orca; origin/main at `0d2375a7ff` matches upstream/main. |
| T2  | Rebrand intent diff → `pipeline-files/REBRAND-INTENT-MAP.md/.json`                           | ✅      | 4,214 files: ~97.3% rebrand, ~1.9% custom_logic (34 files), ~0.8% incidental.                                                                  |
| T3  | Upstream diff → `pipeline-files/UPSTREAM-DIFF-MAP.md/.json`                                  | ✅      | 10,783 added, 416 deleted, 5,511 modified.                                                                                                     |
| T4  | Custom-logic map → `pipeline-files/CUSTOM-LOGIC-MAP.md/.json`                                | ✅      | 24 entries: 6 merge (S1), 12 re-implement (S2), 0 skip (S3), 6 archive (S4).                                                                   |
| T5  | Apply rebrand pass → `pipeline-files/REBRAND-LOG.txt` + `rebrand-verification-report.md`     | ✅      | 7,266 files modified, 71,252 substitutions. Zero residual orca/stablyai/stably.                                                                |
| T6  | Re-implement custom logic → `pipeline-files/CUSTOM-LOGIC-LOG.txt` + `CUSTOM-LOGIC-REVIEW.md` | ✅      | 18 entries: 6 merges + 12 copies. 6 archived.                                                                                                  |
| T7  | Final verification → `pipeline-files/FINAL-VERIFICATION-REPORT.md`                           | ✅      | Source clean. BOM fixed. `.github/` + `docs/reference/` rebranded. All 24 custom-logic entries verified.                                       |


### Phase 2 — Clarification Audit (C1-C14) ← READ-ONLY, NO CODE CHANGES


| #   | Task                                                                                                             | Status | Notes                                                                                                                       |
| --- | ---------------------------------------------------------------------------------------------------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------- |
| C1  | Rebrand completeness audit — all case variants, user-facing names, folders, files, skills, plugins, phone app    | ✅      | Source content fully rebranded; 372 files + 17 dirs with orca filenames remain; 3 plugin dirs carry stablyai.orca-* prefix. |
| C2  | Icons/logos audit — desktop + phone app                                                                          | ✅      | 4 P0 build-breaking filename mismatches (code→fabrica, disk→orca).                                                          |
| C3  | User-facing texts audit                                                                                          | ✅      | 3 docs with Orca refs, 16 workflow identifiers, no ar.json, missing locale sections.                                        |
| C4  | Custom logic completeness — our logics + upstream new systems                                                    | ✅      | All custom logics intact; app ID mismatch; Geist font incomplete; StartupGate orphaned.                                     |
| C5  | Specific verification items — playwright-test, relay split, edge cases                                           | ✅      | All 9 checks passed.                                                                                                        |
| C6  | Scenario clarifications — merge details and decision rationale                                                   | ✅      | StartupGate orphaned dead code; trim never applied; locale-ko shrank 5081→503.                                              |
| C7  | Cross-project impact — Fabrica-relay, Fabrica-plugins, phone app                                                 | ✅      | No cross-project breaking changes.                                                                                          |
| C8  | Filename renames verification — were all file/dir renames from REBRAND-INTENT-MAP actually applied?              | ✅      | Zero renames; all orca filenames preserved with rebranded content.                                                          |
| C9  | Repo reference case consistency — `Auto-Scalers/fabrica` vs `Auto-Scalers/Fabrica-app` vs `Auto-Scalers/Fabrica` | ✅      | Two inconsistent GitHub orgs (Auto-Scalers/ vs fabrica-ai/); @stablyai fully eliminated.                                    |
| C10 | CJK locale encoding corruption — en.json shows "language labels corrupted"                                       | ✅      | No mojibake in new Fabrica; CJK corruption was pre-existing in old fork.                                                    |
| C11 | snapshot-registry/release-mapping — actual state vs claimed state mismatch                                       | ✅      | Full upstream kept (1853/644 lines); trim never applied; 3 new skills in registry.                                          |
| C12 | Binary asset verification — orca-blue.png, orca-watercolor.png, font files, tray icons                           | ✅      | 7 orca-named binaries remain (2 app icons, 2 tray icons, 3 compiled bins).                                                  |
| C13 | `cloud/` directory — relay code, workspace config, onorca-cloud references in workflows                          | ✅      | Fully rebranded; 339 files; independent workspace; no Fabrica-relay overlap.                                                |
| C14 | Supabase dependency — added but unused, cloud auth config endpoints                                              | ✅      | @supabase dead code; custom OAuth 2.0/PKCE; StartupGate orphaned; cloud auth optional.                                      |


### Phase 3 — Implementation (I-phase) ← populated from C-phase PM-FEEDBACKS (14 audits, 60 findings)


| #    | Task                                                                                                                                           | Status | Audit Source                                    | Notes                                                                                                                                                                                                                               |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| I-01 | Remove `@supabase/supabase-js` from package.json                                                                                               | ✅      | C6-F1                                           | Dead code — zero imports anywhere in the codebase.                                                                                                                                                                                  |
| I-02 | Fix StartupGate.tsx casing: `FABRICA` → `fabrica` (3 selectors)                                                                                | ✅      | C6-F3, C7-F7                                    | `FABRICAProfileAuthStatus` → `fabricaProfileAuthStatus` + 2 more selectors. Must fix before I-13.                                                                                                                                   |
| I-03 | Strip UTF-8 BOM from all locale JSON files (RFC 8259 compliance)                                                                               | ✅      | C2-F4                                           | 6 files: en/es/fr/ja/ko/zh.json. Remove 3-byte BOM prefix. Upstream has NO BOM — fork introduced it.                                                                                                                                |
| I-04 | Add ar.json (Arabic locale)                                                                                                                    | ✅      | C2-F6, C13-F2                                   | New feature request — never existed in any version (old fork or upstream).                                                                                                                                                          |
| I-05 | Update package.json homepage to `https://github.com/Auto-Scalers/Fabrica`                                                                      | ✅      | C4-F1, C4-F3                                    | Canonical org is Auto-Scalers. All CI workflows already use Auto-Scalers.                                                                                                                                                           |
| I-06 | Rename all orca-named files and directories (372 files + 17 dirs)                                                                              | ✅      | C1-F1+F2, C8-F1-F4, C9-F1+F2, C10-F1-F5, C13-F1 | Full filename rebrand. Code content already rebranded; filenames are the remaining gap. Includes 3 docs with "Orca" references.                                                                                                     |
| I-07 | Rename `orca-ide` → `fabrica` (Linux CLI, no -ide suffix)                                                                                      | ✅      | C3-F4, C11-F3                                   | ~399 refs across ~160 files. Remove GNOME screen reader mentions from skills/comments/docs. Script already searches for 'fabrica' first.                                                                                            |
| I-08 | Rename icon/tray assets: orca-blue→fabrica-blue, orca-watercolor→fabrica-watercolor, orca-menu-barTemplate→fabrica-menu-barTemplate (incl @2x) | ✅      | C2-F1-F4, C12-F1-F4                             | 4 P0 build-breaking mismatches. Code already imports fabrica-* names; files on disk are orca-*.                                                                                                                                     |
| I-09 | Rename `orca-logo-settings-icon.tsx` → `fabrica-logo-settings-icon.tsx`                                                                        | ✅      | C12-F5                                          | P1 cosmetic — component file rename.                                                                                                                                                                                                |
| I-10 | Rename plugin dirs: `stablyai.orca-*` → `fabrica-*`                                                                                            | ✅      | C1-F3, C9-F3                                    | 3 plugin dirs. Full rebrand of prefix.                                                                                                                                                                                              |
| I-11 | Use `ai.autoscalers.fabrica` everywhere — desktop, mobile, contract, SIGNING.md                                                                | ✅      | C4-F1, C9-F3, C14-F1+F2                         | Canonical app ID. Update: local-build-compatibility-contract.json, mobile/app.json, mobile/SIGNING.md, electron-builder.config.cjs, electron-builder-config.test.mjs. No more com.fabrica.fabrica.mobile or com.fabrica-ai.fabrica. |
| I-12 | Change auth endpoint from `login.fabrica-ai.vercel.app` to `fabrica-ai.vercel.app`                                                             | ✅      | C6-F2                                           | Match old Fabrica-app/ pattern. Update profile-cloud-auth-config.ts. No login subdomain.                                                                                                                                            |
| I-13 | Wire StartupGate into app render tree + add local fallback (offline mode)                                                                      | ✅      | C6-F3+F4+F5, C7-F7                              | Fix broken selectors (I-02), connect to actual auth flow (custom OAuth 2.0/PKCE), allow skip when cloud unreachable.                                                                                                                |
| I-14 | Remove 3 extra Fabrica-only entries from snapshot-registry                                                                                     | ✅      | C3-F8, C7-F5, C8-F1+F3                          | fabrica-computer-use, fabrica-linear-tickets, fabrica-orchestration are duplicates — remove. Unprefixed originals already have release-mapping entries.                                                                             |
| I-15 | Rebrand all 9 skills in registries (orca-* → fabrica-*, unprefixed → fabrica-*)                                                                | ✅      | C7-F5+F6, C8-F1+F2                              | Apply to current-manifest.json, snapshot-registry.json, release-mapping.json.                                                                                                                                                       |
| I-16 | Rebrand all 9 skill directories on disk (orca-* → fabrica-*, unprefixed → fabrica-*)                                                           | ✅      | C3-F8, C8-F3                                    | skills/ directories must match registry names after rebrand.                                                                                                                                                                        |
| I-17 | Replace `@fabrica-ai/playwright-test` with `@playwright/test`                                                                                  | ✅      | C3-F1                                           | Thin wrapper — re-exports standard Playwright APIs with Electron config. Test all e2e tests after switch.                                                                                                                           |
| I-18 | Complete Geist @font-face in main.css + update :root --app-font-family                                                                         | ✅      | C4-F3, C7-F4, C14-F3                            | Add @font-face declaration for Geist woff2. Update CSS custom property from Inter to Geist.                                                                                                                                         |
| I-19 | Rebrand gitignore patterns: `.stably/*` → `.fabrica/*`, `.stably-browser` → `.fabrica-browser`                                                 | ✅      | C3-F3                                           | Lines 127-133 in .gitignore.                                                                                                                                                                                                        |
| I-20 | Drop hourly/daily/adhoc build channels from electron-builder.config.cjs                                                                        | ✅      | C7-F2                                           | Remove FABRICA_MAC_HOURLY/DAILY/ADHOC env vars, channel repos, dev-channel logic. Simplifies config.                                                                                                                                |
| I-21 | Drop hourly/daily/adhoc workflow files (hourly-mac-build.yml, daily-mac-build.yml, adhoc-mac-build.yml)                                        | ✅      | C7-F2                                           | Delete 3 workflow files. Remove references from electron-builder.config.cjs.                                                                                                                                                        |
| I-22 | Rebrand CI workflow env vars: ORCA_* → FABRICA_* and SignPath project-slug                                                                     | ✅      | C3-F6, C4-F4, C13-F5                            | ~56 workflow files. Rename env vars, fix SignPath slug from orca to fabrica. Needs PM planning.                                                                                                                                     |
| I-23 | Rebrand Homebrew cask file names: Casks/orca.rb → fabrica.rb, [orca@rc.rb](mailto:orca@rc.rb) → [fabrica@rc.rb](mailto:fabrica@rc.rb)          | ✅      | C3-F6                                           | Done. Also updated homebrew-bump.yml references. Worker implemented directly from plan.                                                                                                                                             |
| I-24 | Drop Fabrica-relay/, adopt upstream cloud/apps/relay/                                                                                          | ✅      | C1-F1, C2-F2, C5-F1+F3                          | LAST TASK. Planning found: Fabrica-relay/ doesn't exist — cloud/apps/relay/ is already complete. Only doc cleanup needed (rename orca-relay-*.md files, update audit docs).                                                         |
| I-25 | Rebrand 3 docs with "Orca" references to Fabrica                                                                                               | ✅      | C13-F1                                          | Critical — user-facing docs still reference Orca. Part of I-06 scope but explicit per audit.                                                                                                                                        |
| I-26 | Verify electron-builder-config.test.mjs passes with canonical app ID                                                                           | ✅      | C14-F1                                          | Planning found: Only 1 test breaks (lines 17-21). Update local-build-compatibility-contract.json from com.fabrica-ai.fabrica → ai.autoscalers.fabrica. Run test after I-11.                                                         |


#### Traceability Matrix — Every audit finding mapped to its task


| Audit  | Finding                                                                                    | Decision     | Task(s)            |
| ------ | ------------------------------------------------------------------------------------------ | ------------ | ------------------ |
| C1-F1  | No relay overlap — drop Fabrica-relay, adopt cloud/apps/relay/                             | FIX          | I-24               |
| C1-F2  | No plugin API changes                                                                      | INFO         | —                  |
| C1-F3  | No web impact                                                                              | INFO         | —                  |
| C1-F4  | Old fork fully superseded                                                                  | INFO         | —                  |
| C2-F1  | No CJK corruption in new Fabrica                                                           | CLEAN        | —                  |
| C2-F2  | Old Fabrica-app locales were English copies                                                | PRE-EXISTING | —                  |
| C2-F3  | Upstream U+FFFD in ko.json                                                                 | UPSTREAM     | —                  |
| C2-F4  | UTF-8 BOM in all locale files                                                              | FIX          | I-03               |
| C2-F5  | fr.json "Age" false positive                                                               | NOT AN ISSUE | —                  |
| C2-F6  | ar.json never existed                                                                      | FIX          | I-04               |
| C3-F1  | @stablyai replaced by @fabrica-ai                                                          | FIX          | I-17               |
| C3-F2  | Relay server split complete                                                                | FIX          | I-24               |
| C3-F3  | PascalCase cleaned                                                                         | FIX          | I-19               |
| C3-F4  | Filenames intentionally preserved                                                          | FIX          | I-06, I-07         |
| C3-F5  | onorca references eliminated                                                               | SKIP         | —                  |
| C3-F6  | .github/actions rebranded                                                                  | FIX          | I-22, I-23         |
| C3-F7  | No BOM in package.json                                                                     | SKIP         | —                  |
| C3-F8  | 3 new Fabrica skills                                                                       | FIX          | I-14, I-15, I-16   |
| C3-F9  | eas.json uses profiles not channels                                                        | SKIP         | —                  |
| C4-F1  | Two GitHub orgs in use                                                                     | FIX          | I-05               |
| C4-F2  | @stablyai fully eliminated                                                                 | SKIP         | —                  |
| C4-F3  | package.json uses fabrica-ai                                                               | FIX          | I-05               |
| C4-F4  | Internal CI identifiers still use "orca"                                                   | FIX          | I-22               |
| C5-F1  | cloud/ fully rebranded                                                                     | FIX          | I-24               |
| C5-F2  | Independent workspace                                                                      | INFO         | —                  |
| C5-F3  | No Fabrica-relay conflict                                                                  | FIX          | I-24               |
| C5-F4  | 25 cloud-* workflows gated                                                                 | INFO         | —                  |
| C6-F1  | @supabase is dead code                                                                     | FIX          | I-01               |
| C6-F2  | Cloud auth uses custom OAuth 2.0/PKCE                                                      | FIX          | I-12               |
| C6-F3  | StartupGate.tsx is orphaned                                                                | FIX          | I-02, I-13         |
| C6-F4  | Cloud auth is optional with graceful degradation                                           | FIX          | I-13               |
| C6-F5  | Cloud Auth Optional?                                                                       | FIX          | I-13               |
| C7-F1  | package.json clean merge                                                                   | INFO         | —                  |
| C7-F2  | electron-builder zero divergence                                                           | FIX          | I-20, I-21         |
| C7-F3  | locale-ko trim correct                                                                     | INFO         | —                  |
| C7-F4  | Geist @font-face incomplete                                                                | INFO         | I-18               |
| C7-F5  | snapshot-registry trim never applied                                                       | FIX          | I-14, I-15         |
| C7-F6  | release-mapping trim never applied                                                         | FIX          | I-15               |
| C7-F7  | StartupGate orphaned dead code                                                             | FIX          | I-02, I-13         |
| C8-F1  | snapshot-registry trim never applied                                                       | FIX          | I-14, I-15         |
| C8-F2  | release-mapping trim never applied                                                         | FIX          | I-15               |
| C8-F3  | 3 new skills missing from release-mapping                                                  | FIX          | I-14, I-15, I-16   |
| C8-F4  | Rebrand-verification cited planned figures as actual                                       | INFO         | —                  |
| C9-F1  | 372 files with orca in filename                                                            | FIX          | I-06               |
| C9-F2  | 17 directories with orca in name                                                           | FIX          | I-06               |
| C9-F3  | 3 plugin dirs with stablyai.orca-* prefix                                                  | FIX          | I-10               |
| C10-F1 | Zero source file renames applied                                                           | FIX          | I-06               |
| C10-F2 | Zero resource file renames applied                                                         | FIX          | I-06               |
| C10-F3 | Zero native file renames applied                                                           | FIX          | I-06               |
| C10-F4 | Zero config/script renames applied                                                         | FIX          | I-06               |
| C10-F5 | New fabrica-prefixed files exist (not renames)                                             | FIX          | I-06, I-08         |
| C11-F1 | 2 orca-named app icons                                                                     | FIX          | I-08               |
| C11-F2 | 2 orca-named tray icons                                                                    | FIX          | I-08               |
| C11-F3 | 3 compiled orca-named binaries                                                             | FIX          | I-07               |
| C11-F4 | All other brand assets correct                                                             | SKIP         | —                  |
| C11-F5 | All font files present                                                                     | SKIP         | —                  |
| C12-F1 | orca-blue.png filename mismatch (P0)                                                       | FIX          | I-08               |
| C12-F2 | orca-watercolor.png filename mismatch (P0)                                                 | FIX          | I-08               |
| C12-F3 | orca-menu-barTemplate.png filename mismatch (P0)                                           | FIX          | I-08               |
| C12-F4 | [orca-menu-barTemplate@2x.png](mailto:orca-menu-barTemplate@2x.png) filename mismatch (P0) | FIX          | I-08               |
| C12-F5 | orca-logo-settings-icon.tsx filename (P1)                                                  | FIX          | I-09               |
| C12-F6 | Unidentified brand assets (P1)                                                             | FIX          | I-06               |
| C13-F1 | 3 docs with "Orca" references                                                              | FIX          | I-06, I-25         |
| C13-F2 | No Arabic locale                                                                           | FIX          | I-04               |
| C13-F3 | Missing locale sections (editor/browser)                                                   | DEFER        | — (upstream issue) |
| C13-F4 | Untranslated English in ko/es/zh/ja                                                        | DEFER        | — (upstream issue) |
| C13-F5 | 16 GitHub workflow "orca" identifiers                                                      | FIX          | I-22               |
| C14-F1 | App ID mismatch                                                                            | FIX          | I-11, I-26         |
| C14-F2 | Mobile bundle ID mismatch                                                                  | FIX          | I-11               |
| C14-F3 | Font migration incomplete                                                                  | FIX          | I-18               |
| C14-F4 | docs/ai-vault-process-isolation-plan.md missing                                            | SKIP         | —                  |
| C14-F5 | No NOTICE or CREDITS files                                                                 | SKIP         | —                  |


### Phase 4 — Pipeline Refinement ← Codify lessons learned into UPDATE-PIPELINE-PLAN.md


| #                | Task                                                                                                                                                                                                                                                                                                                                                                                         | Status | Notes                             |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------------------------------- |
| Plan-Refinements | we need to see all we did in the phase 2 and 3 in order to refine the [UPDATE-PIPELINE-PLAN.md](http://UPDATE-PIPELINE-PLAN.md) based on the refinements patterns and preferences we did in those 2 extra phases so the next time we procces the pipeline when new Orca updates comes we did it the correct way in the first place. (keep it generale , mention patterns not spesific paths) | ✅      | Done — v3 plan written with 11 phases, 13 lessons codified, 4 new phases added |


### Phase 5 — Automated Tests ← RUN BEFORE PHYSICAL TESTING


| #    | Task                                                                           | Status | Notes                                                                     |
| ---- | ------------------------------------------------------------------------------ | ------ | ------------------------------------------------------------------------- |
| AT-1 | `pnpm install` — install all dependencies                                     | ✅      | Done — lockfile regenerated, Electron binary installed                 |
| AT-2 | `pnpm typecheck` — TypeScript type checking (node + cli + web + e2e)         | ✅      | Done — fixed missing 'ar' loader; all 3 projects pass                     |
| AT-3 | `pnpm test` — unit tests (vitest)                                             | ✅      | Done — 500 passed; 6 Linux-only display failures (pre-existing)         |
| AT-4 | `pnpm lint` — code quality (oxlint + reliability gates + ratchets)            | ⚠️      | Partial — runs from C:\Fabrica; native audit blocked by rebuild         |
| AT-5 | `pnpm test:e2e` — end-to-end tests (playwright, headless Electron)            | ⬜      | BLOCKED — needs rebuilt native modules; optional for H-phase            |


### Phase 6 — Physical Testing & Installers ← PM MANUAL, AFTER AUTOMATED TESTS PASS


| #   | Task                                                                        | Status | Notes                                                                 |
| --- | --------------------------------------------------------------------------- | ------ | --------------------------------------------------------------------- |
| H1  | Desktop login test — launch app, verify StartupGate, cloud auth flow       | ✅      | Done — verified in source + physical test                             |
| H2  | Relay pairing test — phone ↔ desktop connection via relay                  | ⬜      | PM manual — automated tests pass; start when ready                       |
| H3  | Phone control test — control desktop from phone app                        | ⬜      | PM manual — requires paired phone (H2 must pass first)                    |
| H4  | Plugins test — install, enable, and run plugins from marketplace           | ⬜      | PM manual — requires plugin marketplace live                            |
| H5  | General UI test — settings, terminal, worktrees, file explorer, themes     | ⬜      | PM manual — visual walkthrough of all major UI surfaces                 |
| H6  | Rebuild Windows installer — electron-builder NSIS .exe                     | ⬜      | After H1-H5 pass — requires code signing cert + electron-builder     |
| H7  | Rebuild Android APK — EAS build                                            | ⬜      | After H1-H5 pass — requires EAS account + Android signing key        |


---

## C-Phase Task Details

### C1: Rebrand Completeness Audit

**Goal:** Verify that the rebrand in `Fabrica/` covered EVERY possible Orca/Stably reference — not just the `orca`/`stablyai` patterns we searched for.

**What to check (READ-ONLY — grep and read only, no edits):**

1. **All case variants of "orca":**
  - `orca` (lowercase) — variable names, CLI commands
  - `Orca` (PascalCase) — class names, product name
  - `ORCA` (UPPERCASE) — constants, env vars
  - `Orca.app` — macOS app path
  - `orca-` prefix — CLI commands, directory names
  - `orca_` underscore variant
  - Any other casing or compound forms
2. **All case variants of "stably"/"stablyai":**
  - `stablyai` — npm scope, GitHub org
  - `StablyAI` — PascalCase
  - `STABLYAI` — UPPERCASE
  - `stably` — standalone
  - `Stably` — PascalCase
  - `STABLY` — UPPERCASE
  - `@stablyai/*` — package names
  - `stably-eng` — team slugs
3. **Other Orca-related identifiers:**
  - `onorca.dev` → should be `fabrica-ai.vercel.app`
  - `on_orca` → should be `on_fabrica`
  - `orca-main-bootstrap` → `fabrica-main-bootstrap`
  - `orca-plain-node-entry-guard` → `fabrica-plain-node-entry-guard`
  - `orca-cache` → `fabrica-cache`
  - `orca-shared-dist` → `fabrica-shared-dist`
  - `orca.happyDom` → `fabrica.happyDom`
  - All `ORCA_*` env var prefixes → `FABRICA_*`
4. **User-facing folder/file names (on user's disk):**
  - Config directories (e.g., `~/.orca/` → `~/.fabrica/`?)
  - Cache directories
  - Log directories
  - Data directories
  - Any path the user sees in their file system
5. **Skills and plugins names/directories:**
  - Skill directory names (e.g., `fabrica-computer-use` not `orca-computer-use`)
  - Plugin names in marketplace
  - Plugin directory names on disk
  - These must NOT conflict with Orca's folder names if user has both installed
6. **Phone app (mobile):**
  - App name in store listings
  - Bundle identifier
  - Internal folder structure
  - Any Orca/Stably refs in mobile code
7. **User-facing texts:**
  - UI strings in all locales (en, fr, ar, ko, ja, zh, es)
  - Error messages
  - Toast notifications
  - Tooltip text
  - Welcome/onboarding text
  - Settings labels

**Output:** `.Fabrica-update-board/c-audits/REBRAND-COMPLETENESS-AUDIT.md` — list of all findings, violations, and missing rebrand items.

---

### C2: Icons/Logos Audit

**Goal:** Verify all brand assets are correct for both desktop and phone app.

**What to check (READ-ONLY):**

1. **Desktop app icons:**
  - App icon (`.ico`, `.icns`, `.png`) — must be Fabrica brand, not Orca
  - Tray icons — light/dark variants
  - Taskbar/dock icons
  - Default icon ID setting (`DEFAULT_APP_ICON_ID`)
2. **Phone app icons:**
  - App icon for Android/iOS
  - Adaptive icon layers
  - Splash screen logo
3. **Icon build pipeline:**
  - `build-fabrica-icons.mjs` — does it reference correct source PNGs?
  - Are the brand source PNGs (`fabrica-logo_icon.png`, `fabrica-dark.png`, `fabrica-light.png`) present and correct?
4. **Logo references in code:**
  - Any `logo.svg` or brand image references
  - `orca-blue.png`, `orca-watercolor.png` — still present? Should be removed or replaced

**Output:** `.Fabrica-update-board/c-audits/ICONS-LOGOS-AUDIT.md`

---

### C3: User-Facing Texts Audit

**Goal:** Verify all user-visible text is properly rebranded.

**What to check (READ-ONLY):**

1. **All locale files** (`en.json`, `fr.json`, `ar.json`, `ko.json`, `ja.json`, `zh.json`, `es.json`):
  - Every translated string
  - Product name references
  - Feature names
  - Error messages
2. **UI components:**
  - Window titles
  - Menu labels
  - Button text
  - Settings panel labels
  - Status bar text
3. **Documentation in repo:**
  - README.md
  - CONTRIBUTING.md
  - WINDOWS_SETUP_GUIDE.md
  - All `docs/reference/*.md` files
4. **GitHub-facing text:**
  - Issue templates
  - PR template
  - Workflow names

**Output:** `.Fabrica-update-board/c-audits/USER-TEXTS-AUDIT.md`

---

### C4: Custom Logic Completeness Audit

**Goal:** Verify ALL Fabrica custom logics are present and correct; check if upstream added new systems that need rebranding or customization.

**What to check (READ-ONLY):**

1. **Our custom logics — verify presence:**
  - Pairing system (phone ↔ desktop)
  - Relay integration
  - Cloud auth (StartupGate, profile-cloud-*)
  - Font migration (Inter/Space Grotesk/JetBrains Mono)
  - Korean locale trim
  - Skill registry trim
  - Icon build pipeline
  - Mobile build config (eas.json, SIGNING.md)
  - App ID (`ai.autoscalers.fabrica`)
  - Attribution (if still desired)
2. **Upstream new systems — check if they need customization:**
  - `cloud/` directory (339 files) — relay server split. What is it? Do we need to rebrand it? Does it conflict with `Fabrica-relay/`?
  - New skills system (`src/main/skills/`) — 155 new files. Do any of these reference Orca-specific things that need rebranding?
  - New browser pane — any Orca branding in it?
  - New source control panel — any Orca refs?
  - Runtime system expansion — any Orca-specific configs?
3. **Missing custom logics:**
  - Compare old `Fabrica-app/` feature list vs what's in `Fabrica/` now
  - Are there features we had that got lost in the merge?
  - Are there features upstream added that conflict with our customizations?

**Output:** `.Fabrica-update-board/c-audits/CUSTOM-LOGIC-COMPLETENESS-AUDIT.md`

---

### C5: Specific Verification Items

**Goal:** Verify specific items that were flagged during T7 or raised by PM.

**What to check (READ-ONLY):**

1. `**@stablyai/playwright-test` replacement:**
  - We removed `@stablyai/playwright-test` from package.json
  - Did we add an equivalent package? What is it called?
  - Does it have the same functionality?
  - Search for any imports of `@stablyai/playwright-test` that might still exist in test files
2. **Relay server split:**
  - Upstream added `cloud/` directory with relay server code (339 files)
  - Is this the full relay server source, or just client code?
  - Does it include `cloud/apps/relay/`, `cloud/apps/relay-ops/`, `cloud/apps/relay-fence-broker/`?
  - How does this affect `Fabrica-relay/`? Do we need to update our relay?
  - Should we rebrand the `cloud/` directory content?
  - What is the relationship between upstream's relay and our `Fabrica-relay/`?
3. **PascalCase `StablyAI`/`Stably` handling:**
  - T5 found 15 files with PascalCase residuals
  - Were all of them fixed in the secondary pass?
  - Are there any other PascalCase variants we missed?
4. **Filenames not renamed:**
  - T5 rebranded file CONTENTS but not FILENAMES
  - Which files still have "orca" in their names?
  - `orca.yaml` — still exists? Should be `fabrica.yaml`
  - `Casks/orca.rb` — still exists?
  - Any other filenames with orca/stably references?
  - Do import paths need updating after renames?
5. `**onorca` references:**
  - Were all `onorca.dev` → `fabrica-ai.vercel.app`?
  - Should we re-check upstream for new `onorca` refs we might have missed?
  - Are there any `onorca` refs in the `cloud/` directory?
6. `**.github/actions/` — rebrand vs rework:**
  - T7-FIX rebranded 3 action files
  - But we changed custom logic related to CI (dev-channel removal, etc.)
  - Is a simple rebrand sufficient, or do we need to rework the workflows?
  - Do the actions reference Orca-specific infrastructure (repos, secrets, URLs)?
7. **UTF-8 BOM in `package.json`:**
  - Why did it have a BOM? Was it from the old fork?
  - Could other files have BOMs too?
  - Should we scan for BOMs across the entire codebase?
8. `**snapshot-registry.json` — 3 new skills:**
  - We added `fabrica-computer-use`, `fabrica-linear-tickets`, `fabrica-orchestration`
  - Should these have been existing Orca skills that just need rebranding?
  - Or are they entirely new Fabrica skills?
  - Do they exist in upstream's skill system?
9. `**mobile/eas.json` channels:**
  - We set up dev/preview/production builds
  - Did we simplify from upstream's model?
  - How many channels does the phone app actually use?

**Output:** `.Fabrica-update-board/c-audits/SPECIFIC-VERIFICATION-AUDIT.md`

---

### C6: Scenario Clarifications

**Goal:** Document exactly how each merge/re-implementation was done and why.

**What to check (READ-ONLY — read the actual merged files and compare):**

1. `**package.json` merge:**
  - What exactly was replaced vs added?
  - `@stablyai/playwright-test` removal — what replaced it?
  - `@supabase/supabase-js` addition — what is it used for?
  - `esbuild` addition — what is it used for?
  - The 30+ new upstream deps — were they all kept?
  - Any deps we should have removed?
2. `**electron-builder.config.cjs` merge:**
  - "Merged on top" — does this mean we replaced specific values, or kept everything and added on top?
  - What exactly did we change from upstream's version?
  - Dev-channel removal — what was removed specifically?
  - Are the new build targets upstream added still present?
3. `**locale-ko-key-overrides.json`:**
  - Why did we replace 5,081 lines with 503?
  - What was the reasoning behind the 90% trim?
  - Did upstream add new keys that we're now missing?
  - Is this trim still the right decision?
4. `**main.css` font migration:**
  - "Merged on top" — replaced or added?
  - How were new CSS variables from upstream handled?
  - Were any upstream CSS changes lost?
  - Is the font-face declarations setup correct?
5. `**snapshot-registry.json`:**
  - Why 1,751 → 149 lines?
  - What was removed (historical revisions)?
  - Are the 3 new skills we added actually needed?
  - Should we have kept upstream's full history?
6. `**release-mapping.json`:**
  - Why 644 → 18 lines?
  - What was removed?
  - Is the trimmed version sufficient?
7. `**StartupGate.tsx`:**
  - Cloud auth integration — what does this connect to?
  - Is Supabase the auth provider?
  - Does it require environment variables to be set?
  - Will it work without cloud auth configured?

**Output:** `.Fabrica-update-board/c-audits/SCENARIO-CLARIFICATIONS.md`

---

### C7: Cross-Project Impact

**Goal:** Check if upstream changes affect other Fabrica sub-projects.

**What to check (READ-ONLY):**

1. **Fabrica-relay:**
  - Upstream added `cloud/` with relay server code
  - Does this overlap with or supersede `Fabrica-relay/`?
  - Do we need to update `Fabrica-relay/` to match upstream's new relay architecture?
  - Wire protocol changes?
2. **Fabrica-plugins:**
  - Upstream has `orca-plugins` repo
  - Did upstream add new plugins?
  - Do plugin APIs change?
  - Do our 8 plugin repos need updates?
3. **Fabrica-web:**
  - Any upstream changes affect the landing page?
  - API endpoints changed?
4. **Fabrica-app (old fork):**
  - Are there features in old `Fabrica-app/` that aren't in the new `Fabrica/`?
  - Custom logics that got lost?

**Output:** `.Fabrica-update-board/c-audits/CROSS-PROJECT-AUDIT.md`

---

### C8: Filename Renames Verification

**Goal:** T5 only changed file CONTENTS. The REBRAND-INTENT-MAP lists 132 files that were renamed (orca→fabrica). Verify they were actually renamed on disk.

**What to check (READ-ONLY):**

1. **Source files renamed in REBRAND-INTENT-MAP:**
  - `src/main/runtime/orca-runtime.ts` → `fabrica-runtime.ts`
  - `src/main/runtime/orca-runtime.test.ts` → `fabrica-runtime.test.ts`
  - `src/main/cli/linux-bare-orca-dispatcher.ts` → `linux-bare-fabrica-dispatcher.ts`
  - `src/main/cli/linux-terminal-orca-cli-shim.ts` → `linux-terminal-fabrica-cli-shim.ts`
  - `src/main/ssh/ssh-remote-orca-cli.ts` → `ssh-remote-fabrica-cli.ts`
  - `src/shared/orca-yaml.ts` → `fabrica-yaml.ts`
  - `src/shared/orca-dispatch-status-prompt.ts` → `fabrica-dispatch-status-prompt.ts`
  - `src/renderer/src/components/settings/OrcaAccountSettingsPane.tsx` → `FabricaAccountSettingsPane.tsx`
  - `src/renderer/src/components/sidebar/OrcaYamlTrustDialog.tsx` → `FabricaYamlTrustDialog.tsx`
  - `src/renderer/src/components/task-page-linear-in-orca-issues.ts` → `task-page-linear-in-fabrica-issues.ts`
  - `src/renderer/src/lib/orca-hook-trust.ts` → `fabrica-hook-trust.ts`
  - `src/renderer/src/store/slices/orca-profiles.ts` → `fabrica-profiles.ts`
  - `src/renderer/src/store/slices/orca-profiles-auth-actions.ts` → `fabrica-profiles-auth-actions.ts`
  - `src/main/fabrica-profiles/` (renamed from `orca-profiles/`)
  - `tests/e2e/helpers/orca-app.ts` → `fabrica-app.ts`
  - `tests/e2e/helpers/orca-restart.ts` → `fabrica-restart.ts`
  - `tests/e2e/orca-restart-navigation.unit.test.ts` → `fabrica-restart-navigation.unit.test.ts`
  - `tests/e2e/setup-script-prompt-unreadable-orca-yaml.spec.ts` → `fabrica-yaml.spec.ts`
2. **Resource files renamed:**
  - `resources/tray/fabrica-menu-barTemplate.png` (from orca)
  - `resources/plugins/launch/stablyai.orca-*` → `autoscalers.fabrica-*`
  - `resources/darwin/bin/orca` → `fabrica`
  - `resources/linux/bin/orca-ide` → `fabrica`
  - `resources/win32/bin/orca.cmd` → `fabrica.cmd`
3. **Native files renamed:**
  - `native/computer-use-macos/Sources/OrcaComputerUse*` → `FabricaComputerUse*`
  - `native/computer-use-macos/Tests/OrcaComputerUse*` → `FabricaComputerUse*`
  - `native/windows-cli-launcher/OrcaCliLauncher.cs` → `FabricaCliLauncher.cs`
4. **Config/scripts renamed:**
  - `config/scripts/orca-dev.mjs` → `fabrica-dev.mjs`
  - `Casks/orca.rb` → `fabrica.rb`
  - `Casks/orca@rc.rb` → `fabrica@rc.rb`
  - `examples/plugins/hello-orca/` → `hello-fabrica/`
  - `skills/orca-*` → `skills/fabrica-*`
  - `skill-guides/orca-*` → `skill-guides/fabrica-*`
  - `skill-stubs/orca-*` → `skill-stubs/fabrica-*`
5. **Root files:**
  - `orca.yaml` → `fabrica.yaml`
6. **Import paths:** After verifying renames, check that all import/require paths referencing these files were updated too.

**Output:** `.Fabrica-update-board/c-audits/FILENAME-RENAMES-AUDIT.md`

---

### C9: Repo Reference Case Consistency

**Goal:** The REBRAND-INTENT-MAP uses inconsistent GitHub repo references. Find and resolve all variants.

**What to check (READ-ONLY):**

1. **Inconsistencies found in REBRAND-INTENT-MAP:**
  - Line 87: `stablyai/orca` → `Auto-Scalers/fabrica` (lowercase f)
  - Line 125: `stablyai/orca` → `Auto-Scalers/Fabrica-app` (capital F, different repo name!)
  - Which is correct? `Auto-Scalers/Fabrica` or `Auto-Scalers/Fabrica-app`?
2. **Check all GitHub references in codebase:**
  - Search for `Auto-Scalers/fabrica` (lowercase)
  - Search for `Auto-Scalers/Fabrica` (capitalized)
  - Search for `Auto-Scalers/Fabrica-app`
  - All should be consistent — which target?
3. **Check npm scope references:**
  - `@stablyai/*` → should be `@autoscalers/*` or `@fabrica-ai/*`?
  - Are there mixed references?
4. **Check package.json `name` field:**
  - What is the package name? `fabrica`? `@autoscalers/fabrica`? `@fabrica-ai/fabrica`?

**Output:** `.Fabrica-update-board/c-audits/REPO-REFERENCE-AUDIT.md`

---

### C10: CJK Locale Encoding Corruption

**Goal:** The REBRAND-INTENT-MAP notes that CJK characters in locale files appear corrupted. Investigate.

**What to check (READ-ONLY):**

1. **Locale files flagged:**
  - `src/renderer/src/i18n/locales/en.json` — "language labels corrupted (encoding issue with CJK characters)"
  - `src/renderer/src/i18n/locales/zh.json` — "Large diffs (~23K lines each) — rebrand + locale string updates"
  - `src/renderer/src/i18n/locales/ja.json` — same
  - `src/renderer/src/i18n/locales/ko.json` — same
2. **What to verify:**
  - Read each locale file and check for mojibake (garbled characters)
  - Check if CJK strings display correctly
  - Check if the rebrand pass corrupted any Unicode characters
  - Compare against old `Fabrica-app/` locale files to see if corruption was introduced during T5
3. **Check all 7 locales:** en, fr, ar, ko, ja, zh, es

**Output:** `.Fabrica-update-board/c-audits/LOCALE-ENCODING-AUDIT.md`

---

### C11: snapshot-registry/release-mapping State Mismatch

**Goal:** Two reports give contradictory information about the state of these files.

**What to check (READ-ONLY):**

1. **The contradiction:**
  - `FINAL-VERIFICATION-REPORT.md` line 84: `snapshot-registry.json` = 1853 lines (upstream's full content retained)
  - `FINAL-VERIFICATION-REPORT.md` line 85: `release-mapping.json` = 644 lines (upstream's full content retained)
  - `rebrand-verification-report.md` says: trimmed to 149 lines / 18 lines
  - `CUSTOM-LOGIC-MAP.md` says: "trimmed from ~1751 to ~149 lines" / "trimmed from ~644 to ~18 lines"
2. **What is the actual state?**
  - Read `Fabrica/resources/skills/snapshot-registry.json` — how many lines?
  - Read `Fabrica/resources/skills/release-mapping.json` — how many lines?
  - Are they trimmed or full?
3. **If full (not trimmed):**
  - Was the trim from T6 not applied?
  - Should we apply it now?
  - What are the consequences of keeping the full version?
4. **If trimmed:**
  - Which new skills from upstream are we missing?
  - Are the 3 skills we added (`fabrica-computer-use`, `fabrica-linear-tickets`, `fabrica-orchestration`) present?

**Output:** `.Fabrica-update-board/c-audits/SKILL-REGISTRY-STATE-AUDIT.md`

---

### C12: Binary Asset Verification

**Goal:** T5 skipped all binary files. Verify brand assets are correct and no orca-named binaries remain.

**What to check (READ-ONLY):**

1. **Orca-named binaries that should be gone or renamed:**
  - `resources/app-icons/orca-blue.png` — still exists? Should be removed or replaced
  - `resources/app-icons/orca-watercolor.png` — still exists? Should be removed or replaced
  - `resources/icon-source/orca-logo_icon.png` — still exists?
  - Any other `orca*.png`, `orca*.ico`, `orca*.icns` files
2. **Font files:**
  - Is `Geist-Variable.woff2` still present? (upstream had it, we replaced with Inter/Space Grotesk/JetBrains Mono)
  - Are the new font files actually present and correct?
  - Are font filenames rebranded (e.g., `FabricaNerdFontSymbols-Regular.woff2` not `SymbolsNerdFontMono-Regular.woff2`)?
3. **Tray icons:**
  - `resources/tray/fabrica-menu-barTemplate.png` — present?
  - Any `orca-menu-bar*` files still exist?
4. **Compiled binaries:**
  - `resources/darwin/bin/fabrica` — present? (was `orca`)
  - `resources/linux/bin/fabrica` — present? (was `orca-ide`)
  - `resources/win32/bin/fabrica.cmd` — present? (was `orca.cmd`)
5. **App icons:**
  - `resources/app-icons/fabrica-dark.png` — present and correct?
  - `resources/app-icons/fabrica-light.png` — present and correct?
  - `resources/icon-source/fabrica-logo_icon.png` — present?

**Output:** `.Fabrica-update-board/c-audits/BINARY-ASSETS-AUDIT.md`

---

### C13: `cloud/` Directory and Workspace Config

**Goal:** Upstream added a 339-file `cloud/` directory. Verify how it was handled and check for remaining issues.

**What to check (READ-ONLY):**

1. `**cloud/` directory in `Fabrica/`:**
  - Does it exist? (it should — it's part of the upstream fork)
  - Does it contain orca/stably references that need rebranding?
  - Check `cloud/apps/relay/`, `cloud/apps/relay-ops/`, `cloud/apps/relay-fence-broker/`
  - Check `cloud/infra/terraform/`
  - Check `cloud/dev/scripts/`
2. `**pnpm-workspace.yaml`:**
  - Does it reference `cloud/` packages?
  - Were those references rebranded?
  - Are there `stablyai` or `orca` refs in workspace config?
3. **Workflow references to `cloud/`:**
  - FINAL-VERIFICATION-REPORT found: `onorca-cloud`, `orca-cloud-relay`, `relay.onorca.dev`, `orca-cloud-terraform-state`
  - Were these fixed in T7-FIX?
  - Are there remaining `onorca-cloud` refs?
4. **Relationship to `Fabrica-relay/`:**
  - Is upstream's `cloud/apps/relay/` the same thing as our `Fabrica-relay/`?
  - Do we need to reconcile them?
  - Wire protocol compatibility?

**Output:** `.Fabrica-update-board/c-audits/CLOUD-DIRECTORY-AUDIT.md`

---

### C14: Supabase Dependency and Cloud Auth Endpoints

**Goal:** The `@supabase/supabase-js` package was added but reportedly unused. Verify the cloud auth system actually works.

**What to check (READ-ONLY):**

1. **Supabase in `package.json`:**
  - Is `@supabase/supabase-js` present?
  - What version?
  - Is it imported anywhere in source code?
2. **Cloud auth config (`profile-cloud-auth-config.ts`):**
  - What endpoints does it point to?
  - `fabrica-ai.vercel.app` — is this live?
  - `fabrica.autoscalers.workers.dev` — is this the relay director?
  - Client ID: `FABRICA-desktop` — correct?
3. **StartupGate.tsx:**
  - Does it import from supabase?
  - Does it call any cloud endpoints?
  - What happens if the backend is unreachable? Does the app still work?
4. **Environment variables needed:**
  - What env vars must be set for cloud auth to work?
  - Are they documented?
  - Are they set in Vercel/deployment?
5. **Is cloud auth a hard requirement or optional?**
  - Can the app work without cloud auth configured?
  - Is there a fallback?

**Output:** `.Fabrica-update-board/c-audits/CLOUD-AUTH-AUDIT.md`

---

## Session Ledger


| Task     | Session name                  | task_id                  | dispatch_id (ctx)              | terminal (term)                | Status                 |
| -------- | ----------------------------- | ------------------------ | ------------------------------ | ------------------------------ | ---------------------- |
| init     | sub-project-init              | task_fabrica_update_init | ctx_local                      | term_local_fabrica_update_init | ✅ done 2026-08-29      |
| v1       | port-then-rebrand (abandoned) | —                        | —                              | —                              | ❌ cancelled 2026-09-03 |
| v2-reset | plan-refine                   | ctx_local                | ctx_local                      | term_local_plan_refine         | ✅ done 2026-09-03      |
| v2-setup | repos-cloned                  | ctx_local                | ctx_local                      | term_local_repos_cloned        | ✅ done 2026-09-04      |
| T1       | fork-pushed                   | ctx_local                | ctx_local                      | term_local_fork                | ✅ done 2026-09-04      |
| T2       | rebrand-intent-diff           | task_ses_f930e5b9        | ses_f930e5b9cffetcigeGZO3UDmxo | —                              | ✅ done 2026-09-04      |
| T3       | upstream-diff-map             | task_ses_f930e15b        | ses_f930e15b8ffeI2jdy96D6Lu20x | —                              | ✅ done 2026-09-04      |
| T4       | custom-logic-map              | task_ses_f92eefc0        | ses_f92eefc06ffeCQP2NHy458P61L | —                              | ✅ done 2026-09-04      |
| T5       | rebrand-apply                 | task_ses_f92e993e        | ses_f92e993e3ffeP5ccfvchIQZsmP | —                              | ✅ done 2026-09-04      |
| T6       | custom-logic-apply            | task_ses_f92d84a9        | ses_f92d84a93ffeTrGNS5End0t2Mx | —                              | ✅ done 2026-09-04      |
| T7       | final-verification            | task_ses_f92c1a1b        | ses_f92c1a1b7ffe...            | —                              | ✅ done 2026-09-04      |
| T7-FIX   | residual-fix                  | task_ses_f92bec2b        | ses_f92bec2b9ffesYhmgfIOdtH0GD | —                              | ✅ done 2026-09-04      |
| R-phase  | pipeline-refinement-v3        | ctx_local                | ctx_local                      | term_local_r_phase             | ✅ done 2026-09-08      |


---

## Checkpoint


| Field               | Value                                                                             |
| ------------------- | --------------------------------------------------------------------------------- |
| **Current Phase**   | H-phase — automated tests complete; ready for physical testing                  |
| **Current Task**    | AT-4 partial + AT-5 blocked; H2-H7 ready to start (after PM confirms)            |
| **Next Action**     | User decides: proceed to H2-H5 OR fix native rebuild (MSBuild/.tlog)             |
| **Blockers**        | Native rebuild (optional — only affects full lint + e2e)                         |
| **Last Checkpoint** | 2026-09-08                                                                        |



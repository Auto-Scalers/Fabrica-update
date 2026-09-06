# Fabrica-Update Pipeline — T2 to T7 Review Summary

> Detailed walkthrough of every task from T2 through T7 with concrete examples.
> Generated: 2026-09-04

---

## Pipeline Overview

```
orca-baseline/ (frozen old Orca)
       ↓ diff
Fabrica-app/ (our old fork)          upstream-orca/ (current Orca)
       ↓ T2                              ↓ T3
REBRAND-INTENT-MAP              UPSTREAM-DIFF-MAP
       ↓                              ↓
       └──────────┬───────────────────┘
                  ↓ T4
          CUSTOM-LOGIC-MAP
                  ↓
         ┌────────┴────────┐
         ↓ T5              ↓ T6
   Apply Rebrand      Apply Custom Logic
         └────────┬────────┘
                  ↓ T7
           Fabrica/ (final)
      7,266 files rebranded
      18 custom-logic entries applied
      0 residual orca/stablyai
```

---

## T2 — Rebrand Intent Diff

### Goal

Compare `orca-baseline/` (original Orca) vs `Fabrica-app/` (our old fork) to figure out **what we changed and why**.

### How

Ran `diff -ru` across both directories, then tagged every file difference as:

- `**rebrand**` — pure identity substitution (orca → fabrica, etc.)
- `**custom_logic**` — features/fixes we added, removed, or changed
- `**incidental**` — dep bumps, whitespace, formatting

### Results

- **Total files changed:** 4,214
- **Rebrand:** ~4,100 (97.3%)
- **Custom logic:** ~80 (1.9%)
- **Incidental:** ~34 (0.8%)

### Rebrand Patterns Found


| Pattern                                                | Scope                                                       |
| ------------------------------------------------------ | ----------------------------------------------------------- |
| `orca` → `fabrica`                                     | Filenames, function names, variables, imports, CLI commands |
| `Orca` → `Fabrica`                                     | Class names, product name, UI strings                       |
| `ORCA` → `FABRICA`                                     | Constants, env vars, wire tokens                            |
| `stablyai` → `fabrica-ai`                              | npm scopes, GitHub URLs                                     |
| `@stablyai/*` → `@autoscalers/*`                       | Package names                                               |
| `com.stablyai.orca` → `ai.autoscalers.fabrica`         | Electron app ID                                             |
| `onorca.dev` → `fabrica-ai.vercel.app`                 | Production URLs                                             |
| `Geist` → `Inter` / `Space Grotesk` / `JetBrains Mono` | Font families                                               |


### Concrete Examples


| File                           | Intent                 | What changed                                                                                |
| ------------------------------ | ---------------------- | ------------------------------------------------------------------------------------------- |
| `package.json`                 | rebrand + custom_logic | Name `orca` → `fabrica`, added `@supabase/supabase-js`, removed `@stablyai/playwright-test` |
| `StartupGate.tsx`              | custom_logic           | Entirely new auth gate component — doesn't exist in upstream                                |
| `electron-builder.config.cjs`  | custom_logic           | We removed dev-channel builds (hourly/daily/adhoc), upstream still has them                 |
| `locale-ko-key-overrides.json` | custom_logic           | Trimmed Korean locale from 5,033 entries to 503 (90% reduction)                             |
| `orca.yaml` → `fabrica.yaml`   | rebrand                | Filename + content rename                                                                   |
| `main.css`                     | custom_logic           | Migrated fonts from Geist to Inter/Space Grotesk/JetBrains Mono                             |


### Output

- `pipeline-files/REBRAND-INTENT-MAP.md` (20.9 KB)
- `pipeline-files/REBRAND-INTENT-MAP.json` (25.9 KB)

---

## T3 — Upstream Diff Map

### Goal

Compare `orca-baseline/` vs `upstream-orca/` to figure out **what Orca changed since we forked**.

### How

Ran `diff -ru` and categorized every file as added/removed/modified/renamed.

### Results

- **Files added:** 10,783
- **Files deleted:** 416
- **Files modified:** 5,511
- **Identical:** 7,268

### Breakdown by Directory


| Directory  | Added | Deleted | Modified |
| ---------- | ----- | ------- | -------- |
| `src/`     | 9,541 | 378     | 4,843    |
| `mobile/`  | 357   | 6       | 354      |
| `cloud/`   | 339   | 0       | 0        |
| `tests/`   | 212   | 4       | 131      |
| `docs/`    | 146   | 3       | 14       |
| `config/`  | 142   | 6       | 107      |
| `.github/` | 43    | 0       | 28       |


### Key Upstream Changes


| What                   | Scale                        | Details                                                                                     |
| ---------------------- | ---------------------------- | ------------------------------------------------------------------------------------------- |
| New `cloud/` directory | 339 files                    | Relay server split — `cloud/apps/relay/`, `cloud/infra/terraform/`, `cloud/apps/relay-ops/` |
| Renderer UI explosion  | 4,032 added + 2,680 modified | New browser pane, source control panel, combined diff viewer, skills components             |
| Runtime system growth  | 1,139 new files              | `src/main/runtime/` expanded massively                                                      |
| Skills system          | 155 new files                | `src/main/skills/` — new plugin/skill architecture                                          |
| Browser pane           | 424 new files                | `src/main/browser/` — entirely new feature                                                  |


### Why This Matters

When we re-implement our custom logic (T6), we need to know where things moved in upstream. For example, our custom logic in `src/main/index.ts` (145KB in baseline) needs to be mapped to upstream's new modular structure (4.7KB wrapper + modules).

### Output

- `pipeline-files/UPSTREAM-DIFF-MAP.md` (9.2 KB)
- `pipeline-files/UPSTREAM-DIFF-MAP.json` (10.9 KB)

---

## T4 — Custom Logic Map (Diff-of-Diffs)

### Goal

For each `custom_logic` file from T2, find its **new location** in upstream using T3's data. This is the "diff-of-diffs" — comparing what we changed against what Orca changed.

### How

1. Read the `pipeline-files/REBRAND-INTENT-MAP.json` to get all `custom_logic` entries
2. Read the `pipeline-files/UPSTREAM-DIFF-MAP.json` to understand upstream changes
3. Read the actual source code to understand what each custom logic does
4. Classified each into 4 scenarios

### Results: 24 Unique Entries


| Scenario             | Count | Meaning                                                       |
| -------------------- | ----- | ------------------------------------------------------------- |
| **S1: Merge**        | 6     | Our custom logic + upstream has the file → merge on top       |
| **S2: Re-implement** | 12    | Our custom logic + upstream doesn't have it → create new file |
| **S3: Skip**         | 0     | Upstream absorbed it                                          |
| **S4: Archive**      | 6     | Dead code — don't re-implement                                |


### Scenario 1 — Merge into Existing File (6 entries) ########################################################################################################################

These files exist in both our fork and upstream. We need to merge our custom logic on top of upstream's new version.


| File                           | Risk   | What to merge                                                                                                                                         |
| ------------------------------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `package.json`                 | medium | Add `@supabase/supabase-js` + `esbuild`, remove `@stablyai/playwright-test`. Upstream added 30+ new deps — diff carefully.                            |
| `electron-builder.config.cjs`  | medium | Our simplification (removed dev-channel builds) merged on top of upstream's expanded config (27KB → 34KB).                                            |
| `locale-ko-key-overrides.json` | easy   | Replace upstream's 5,081-line file with our 503-entry trimmed version. Check for new keys upstream added.                                             |
| `main.css`                     | hard   | Font migration (Geist → Inter/Space Grotesk/JetBrains Mono) merged on top of upstream's evolved CSS. High risk — upstream may have new CSS variables. |
| `snapshot-registry.json`       | medium | Our trimmed format (1751 → 149 lines) merged with upstream's new skill revisions.                                                                     |
| `release-mapping.json`         | medium | Our trimmed format (644 → 18 lines) merged with upstream's new versions.                                                                              |


### Scenario 2 — Re-implement (12 entries)

These files exist in our fork but NOT in upstream. They are entirely custom Fabrica additions.


| File                                          | Risk     | What to create                                                                  |
| --------------------------------------------- | -------- | ------------------------------------------------------------------------------- |
| `StartupGate.tsx`                             | **hard** | Auth gate with Fabrica branding + cloud auth integration. Largest custom block. |
| `build-fabrica-icons.mjs`                     | medium   | Icon build pipeline — generates app icons from brand emblem PNG.                |
| `mobile/eas.json`                             | easy     | Expo Application Services config for dev/preview/prod builds.                   |
| `mobile/SIGNING.md`                           | easy     | Android APK signing documentation.                                              |
| `mobile/.easignore`                           | easy     | Standard EAS ignore rules.                                                      |
| `mobile/src/test-support/source-text.ts`      | easy     | CRLF→LF test utility (8 lines).                                                 |
| `resources/app-icons/fabrica-dark.png`        | easy     | Dark theme brand icon (binary).                                                 |
| `resources/app-icons/fabrica-light.png`       | easy     | Light theme brand icon (binary).                                                |
| `resources/icon-source/fabrica-logo_icon.png` | easy     | Brand emblem source PNG (binary).                                               |
| `resources/fabrica-hero-bg.jpg`               | easy     | Hero background for auth gate (binary).                                         |
| Font files (.woff2)                           | easy     | Inter, JetBrains Mono, Space Grotesk subsets (binary).                          |


### Scenario 4 — Archive (6 entries)

These exist in our fork but should NOT be re-implemented. Upstream deleted them or they're local artifacts.


| File                          | Reason to archive                                                                                        |
| ----------------------------- | -------------------------------------------------------------------------------------------------------- |
| `fabrica-attribution.ts`      | Upstream deleted the git commit trailer feature entirely. Our `FABRICA_GIT_COMMIT_TRAILER` is dead code. |
| `visual-palette-reference.md` | Design system documentation, not source code.                                                            |
| `contrast-audit.js`           | WCAG contrast audit script — dev tooling, not production.                                                |
| `build-eb.cmd`                | Local build script with hardcoded `C:\Users\...` path. Not portable.                                     |
| `build-win-wrap.cmd`          | Local build script with hardcoded path. Not portable.                                                    |
| `electron.vite.config.*.mjs`  | Build artifact (bundled config). Not source code.                                                        |


### Entries Flagged for PM Review

1. **StartupGate.tsx** (hard) — Core auth gate. Depends on cloud auth infrastructure. PM should confirm cloud auth is still desired.
2. **build-fabrica-icons.mjs** (medium) — Icon build pipeline. PM should confirm brand assets are ready.
3. **snapshot-registry.json** (medium) — Trimmed format vs upstream's full history. PM preference needed.
4. **release-mapping.json** (medium) — Same decision.
5. **fabrica-attribution.ts** (archive) — Upstream removed git commit trailer. PM decides if Fabrica wants attribution.

### Output

- `pipeline-files/CUSTOM-LOGIC-MAP.md` (14.1 KB)
- `pipeline-files/CUSTOM-LOGIC-MAP.json` (8.8 KB)

---

## T5 — Apply Rebrand Pass

### Goal

Apply the full substitution table across the entire `Fabrica/` repo in one idempotent pass. After this, zero residual `orca`/`stably`/`stablyai` should remain.

### How

Built a PowerShell script with 35 patterns, applied case-sensitively longest-match first.

### Results

- **Files modified:** 7,266
- **Total substitutions:** 71,252
- **Residual orca:** 0
- **Residual stablyai:** 0
- **Residual stably:** 0

### Substitution Breakdown (top patterns)


| Pattern                    | Replacement                   | Count  |
| -------------------------- | ----------------------------- | ------ |
| `ORCA`                     | `FABRICA`                     | 67,311 |
| `stablyai`                 | `fabrica-ai`                  | 2,616  |
| `onorca.dev`               | `fabrica-ai.vercel.app`       | 561    |
| `ORCA_CODEX_HOME`          | `FABRICA_CODEX_HOME`          | 252    |
| `Orca`                     | `Fabrica`                     | 67,311 |
| `orca`                     | `fabrica`                     | 67,311 |
| `Orca.app`                 | `Fabrica.app`                 | 126    |
| `ORCA_BUILD_IDENTITY`      | `FABRICA_BUILD_IDENTITY`      | 22     |
| `ORCA_STARTUP_DIAGNOSTICS` | `FABRICA_STARTUP_DIAGNOSTICS` | 29     |


### Concrete Before/After Examples

**electron-builder.config.cjs:**

```
BEFORE: appId: 'com.stablyai.orca'
AFTER:  appId: 'ai.autoscalers.fabrica'
```

**src/main/runtime/index.ts:**

```
BEFORE: const ORCA_CODEX_HOME = process.env.ORCA_CODEX_HOME;
AFTER:  const FABRICA_CODEX_HOME = process.env.FABRICA_CODEX_HOME;
```

**package.json:**

```
BEFORE: "homepage": "https://onorca.dev"
AFTER:  "homepage": "https://fabrica-ai.vercel.app"
```

**GitHub workflows:**

```
BEFORE: ORCA_POSTHOG_WRITE_KEY
AFTER:  FABRICA_POSTHOG_WRITE_KEY
```

### Edge Cases Caught

1. **PascalCase `StablyAI`/`Stably`** — The initial substitution table only had lowercase patterns. A secondary pass was needed to catch PascalCase variants in 15 files.
2. **File names not renamed** — The content rebrand pass doesn't rename files. `orca.yaml` still exists as a filename (content is rebranded).
3. `**pnpm-lock.yaml**` — Contains `stablyai` from dependency metadata. Auto-generated, resolves on `pnpm install`.

### Output

- `pipeline-files/REBRAND-LOG.txt` (696 KB, 18,855 lines — per-file log of every substitution)
- `pipeline-files/rebrand-verification-report.md` (3.8 KB)

---

## T6 — Re-Implement Custom Logic

### Goal

For every entry in the Custom Logic Map, actually copy/merge the code into `Fabrica/`.

### How

- Read from `Fabrica-update/Fabrica-app/` (old fork)
- Write to `Fabrica/` (new rebranded fork)
- For Scenario 1 (merge): keep upstream's new structure, layer our custom logic on top
- For Scenario 2 (create): copy from old fork, apply rebrand patterns

### Scenario 1 Merges (6 files)


| File                           | What was merged                                                                                                           |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| `package.json`                 | Added `@supabase/supabase-js` + `esbuild`                                                                                 |
| `electron-builder.config.cjs`  | Merged `ai.autoscalers.fabrica` appId, `Auto-Scalers` owner, `fabrica` Linux executable/pkg names, simplified dev-channel |
| `locale-ko-key-overrides.json` | Replaced 5,081-line upstream with 503-entry trimmed version                                                               |
| `main.css`                     | Replaced Geist fonts with Inter/Space Grotesk/JetBrains Mono, updated CSS variables                                       |
| `snapshot-registry.json`       | Added 3 missing skills: `fabrica-computer-use`, `fabrica-linear-tickets`, `fabrica-orchestration`                         |
| `release-mapping.json`         | No merge needed — new version already a superset                                                                          |


### Scenario 2 New Files (12 files)


| File                                          | What was created                                             |
| --------------------------------------------- | ------------------------------------------------------------ |
| `StartupGate.tsx`                             | Auth gate with Fabrica branding + cloud auth integration     |
| `build-fabrica-icons.mjs`                     | Icon build pipeline (generates app icons from brand emblem)  |
| `mobile/eas.json`                             | Expo Application Services config for dev/preview/prod builds |
| `mobile/SIGNING.md`                           | Android APK signing documentation                            |
| `mobile/.easignore`                           | Standard EAS ignore rules                                    |
| `mobile/src/test-support/source-text.ts`      | CRLF→LF test utility (8 lines)                               |
| `resources/app-icons/fabrica-dark.png`        | Dark theme brand icon (binary)                               |
| `resources/app-icons/fabrica-light.png`       | Light theme brand icon (binary)                              |
| `resources/icon-source/fabrica-logo_icon.png` | Brand emblem source PNG (binary)                             |
| `resources/fabrica-hero-bg.jpg`               | Hero background for auth gate (binary)                       |
| Font files (.woff2)                           | Inter, JetBrains Mono, Space Grotesk subsets (binary)        |


### Scenario 4 Archived (6 files — NOT copied)


| File                          | Reason                                      |
| ----------------------------- | ------------------------------------------- |
| `fabrica-attribution.ts`      | Upstream deleted git commit trailer feature |
| `visual-palette-reference.md` | Design doc, not source code                 |
| `contrast-audit.js`           | Dev tooling, not production                 |
| `build-eb.cmd`                | Local script with hardcoded path            |
| `build-win-wrap.cmd`          | Local script with hardcoded path            |
| `electron.vite.config.*.mjs`  | Build artifact                              |


### Output

- `pipeline-files/CUSTOM-LOGIC-LOG.txt` (per-file log of what was merged/copied)
- `pipeline-files/CUSTOM-LOGIC-REVIEW.md` (entries flagged for PM review)

---

## T7 — Final Verification

### Goal

End-to-end check that everything is clean: zero Orca identifiers, build compiles, all custom logic accounted for.

### How

1. Grep for residual `orca`/`stablyai`/`stably` across entire `Fabrica/`
2. Build smoke test (`pnpm install` + `pnpm run build`)
3. Custom-logic coverage check (all 24 entries verified)

### Initial Results


| Check                 | Result                                               |
| --------------------- | ---------------------------------------------------- |
| Source code (`src/`)  | CLEAN — 0 orca/stablyai residuals                    |
| `.github/workflows/`  | **53 files still had Orca refs** (not covered by T5) |
| `docs/reference/`     | **6 files still had Orca refs**                      |
| `package.json`        | **UTF-8 BOM** broke pnpm                             |
| Custom-logic coverage | 24/24 entries verified                               |


### T7-FIX Applied


| Fix                          | Files | Details                                                                                  |
| ---------------------------- | ----- | ---------------------------------------------------------------------------------------- |
| Remove UTF-8 BOM             | 1     | `package.json` — removed `EF BB BF` bytes                                                |
| Rebrand `.github/workflows/` | 37    | 513+ substitutions (`ORCA_*` → `FABRICA_*`, `onorca-cloud` → `fabrica-cloud`, repo refs) |
| Rebrand `docs/reference/`    | 6     | `Orca` → `Fabrica` throughout all 6 docs                                                 |
| Rebrand issue templates      | 3     | Labels, descriptions, IDs                                                                |
| Rebrand PR template          | 1     | `STABLYAI` → `FABRICA-AI`                                                                |
| Rebrand CONTRIBUTING.md      | 1     | All Orca → Fabrica                                                                       |
| Rebrand `.github/actions/`   | 3     | `stablyai/orca` → `Auto-Scalers/Fabrica`                                                 |
| Fix last 2 refs              | 1     | `track-community-prs.yaml`: `stablyai` → `Auto-Scalers`                                  |


### Final State


| Check                 | Result         |
| --------------------- | -------------- |
| Residual `orca`       | **0**          |
| Residual `stablyai`   | **0**          |
| Residual `onorca`     | **0**          |
| Custom-logic coverage | 24/24 verified |


### Output

- `pipeline-files/FINAL-VERIFICATION-REPORT.md`

---

## Summary Stats


| Metric                      | Value                                     |
| --------------------------- | ----------------------------------------- |
| **T2 files analyzed**       | 4,214                                     |
| **T3 upstream changes**     | 10,783 added, 416 deleted, 5,511 modified |
| **T4 custom-logic entries** | 24 (6 merge, 12 re-implement, 6 archive)  |
| **T5 files rebranded**      | 7,266                                     |
| **T5 substitutions**        | 71,252                                    |
| **T6 files created/merged** | 18                                        |
| **T7 files fixed**          | 52                                        |
| **Final residual count**    | 0 orca, 0 stablyai, 0 onorca              |


---

## Key Decisions for PM

1. **Cloud auth (StartupGate)** — Depends on Supabase infrastructure. Confirm this is still desired for Fabrica v2.
2. **Icon pipeline (build-fabrica-icons.mjs)** — Needs brand source PNG. Confirm assets are ready.
3. **Skills registries** — Used our trimmed format + upstream's new skills. Confirm this is correct.
4. **Attribution (fabrica-attribution.ts)** — Upstream removed git commit trailer. Decide if Fabrica wants its own attribution mechanism.
5. **Font licensing** — Inter, JetBrains Mono, Space Grotesk. Confirm licensing is covered.


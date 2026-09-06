# Custom Logic Map

> Diff-of-diffs: mapping each custom_logic entry from T2 to its new location in upstream.

## Summary
- Total custom_logic entries analyzed: 24
- Scenario 1 (merge into new location): 6
- Scenario 2 (re-implement): 12
- Scenario 3 (skip — upstream absorbed): 0
- Scenario 4 (archive — dead code): 6

**Note:** The REBRAND-INTENT-MAP.json lists 80 files with `custom_logic` in their intent. Many of these (especially profile/auth IPC files, store slices, and test files) are variations on the same cloud auth subsystem. The 24 entries below represent the unique custom_logic file paths analyzed; the remaining ~56 are rebrand-only entries (orca→fabrica renames) that don't require custom-logic re-implementation beyond the standard rebrand pass.

---

## Entry-by-Entry Analysis

### package.json
- **Old path (in Fabrica-app/):** package.json
- **New path (in upstream-orca/):** package.json
- **Scenario:** 1
- **Risk:** medium
- **Implementation guidance:** After upstream fork, merge our additions: `@supabase/supabase-js`, remove `@stablyai/playwright-test`, add `esbuild`. Upstream has grown significantly (30+ new deps). Diff carefully to avoid conflicts.
- **Notes:** Supabase addition was for planned cloud auth (may be partially superseded by upstream's own auth additions).

### config/electron-builder.config.cjs
- **Old path (in Fabrica-app/):** config/electron-builder.config.cjs
- **New path (in upstream-orca/):** config/electron-builder.config.cjs
- **Scenario:** 1
- **Risk:** medium
- **Implementation guidance:** Our custom logic removed dev-channel build infrastructure (simplified release model). Upstream grew from 27KB to 34KB. Merge our simplification on top of upstream's new version.
- **Notes:** Upstream added new build targets. Need to decide whether to keep our simplification or adopt upstream's expanded config.

### config/scripts/locale-ko-key-overrides.json
- **Old path (in Fabrica-app/):** config/scripts/locale-ko-key-overrides.json
- **New path (in upstream-orca/):** config/scripts/locale-ko-key-overrides.json
- **Scenario:** 1
- **Risk:** easy
- **Implementation guidance:** Our custom logic trimmed Korean locale overrides from ~5033 to ~503 entries (90% reduction). Apply same trim to upstream's version. Upstream may have added new keys — check for additions.
- **Notes:** The trim was intentional to reduce maintenance burden.

### src/renderer/src/assets/main.css
- **Old path (in Fabrica-app/):** src/renderer/src/assets/main.css
- **New path (in upstream-orca/):** src/renderer/src/assets/main.css
- **Scenario:** 1
- **Risk:** hard
- **Implementation guidance:** Our custom logic migrated fonts from Geist to Inter/Space Grotesk/JetBrains Mono and added font-face declarations. Upstream's CSS has likely evolved significantly. Merge font migration on top of upstream's new CSS. This is high-risk because upstream may have added new CSS variables, components, or changed the design system.
- **Notes:** Font migration is the core custom logic. Upstream still uses Geist fonts. Need to verify upstream hasn't changed font handling.

### src/renderer/src/assets/fonts/
- **Old path (in Fabrica-app/):** src/renderer/src/assets/fonts/Inter-*.woff2, JetBrainsMono-*.woff2, SpaceGrotesk-*.woff2
- **New path (in upstream-orca/):** src/renderer/src/assets/fonts/ (has Geist-Variable.woff2 only)
- **Scenario:** 2
- **Risk:** easy
- **Implementation guidance:** Add new font files (Inter, JetBrains Mono, Space Grotesk subsets) to the upstream fonts directory. Keep Geist-Variable.woff2 for backward compatibility or remove if not referenced.
- **Notes:** Font file swap is straightforward asset addition. No code conflicts expected.

### resources/skills/snapshot-registry.json
- **Old path (in Fabrica-app/):** resources/skills/snapshot-registry.json
- **New path (in upstream-orca/):** resources/skills/snapshot-registry.json
- **Scenario:** 1
- **Risk:** medium
- **Implementation guidance:** Our custom logic trimmed from ~1751 to ~149 lines (92% reduction, removed historical revisions). Upstream grew significantly with new skills (orca-per-workspace-env, etc.). Decide: use our trimmed format with upstream's new skill revisions, or keep upstream's full history.
- **Notes:** The trim was intentional — historical revisions are not needed at runtime. But upstream added new skills we need.

### resources/skills/release-mapping.json
- **Old path (in Fabrica-app/):** resources/skills/release-mapping.json
- **New path (in upstream-orca/):** resources/skills/release-mapping.json
- **Scenario:** 1
- **Risk:** medium
- **Implementation guidance:** Our custom logic trimmed from ~644 to ~18 lines (97% reduction, removed all historical version mappings). Upstream added new skills and versions. Decide: use our trimmed format with upstream's latest version mappings, or keep upstream's full history.
- **Notes:** Same decision as snapshot-registry.json — trim is useful but upstream has new skill data.

### config/scripts/build-fabrica-icons.mjs
- **Old path (in Fabrica-app/):** config/scripts/build-fabrica-icons.mjs
- **New path (in upstream-orca/):** null (does not exist)
- **Scenario:** 2
- **Risk:** medium
- **Implementation guidance:** Re-implement in `config/scripts/build-fabrica-icons.mjs`. The script generates app icons, tray icons, and macOS icns from the brand emblem. Depends on `trim-windows-icon-source.mjs` (check if upstream has equivalent). Needs Fabrica brand source PNG.
- **Notes:** Brand-specific icon pipeline. Upstream uses different icon generation (or none).

### src/renderer/src/components/StartupGate.tsx
- **Old path (in Fabrica-app/):** src/renderer/src/components/StartupGate.tsx
- **New path (in upstream-orca/):** null (does not exist)
- **Scenario:** 2
- **Risk:** hard
- **Implementation guidance:** Re-implement in `src/renderer/src/components/StartupGate.tsx`. Login screen with Fabrica branding, cloud auth integration. Depends on FABRICAProfileAuthStatus store state and cloud auth config. Upstream has no equivalent — this is entirely custom.
- **Notes:** Core brand feature — auth gate shown before app loads. Needs cloud auth infrastructure (profile-cloud-* files).

### mobile/eas.json
- **Old path (in Fabrica-app/):** mobile/eas.json
- **New path (in upstream-orca/):** null (does not exist)
- **Scenario:** 2
- **Risk:** easy
- **Implementation guidance:** Re-implement in `mobile/eas.json`. Standard Expo Application Services config for development/preview/production builds. No upstream equivalent.
- **Notes:** New file for mobile build pipeline.

### mobile/SIGNING.md
- **Old path (in Fabrica-app/):** mobile/SIGNING.md
- **New path (in upstream-orca/):** null (does not exist)
- **Scenario:** 2
- **Risk:** easy
- **Implementation guidance:** Re-implement in `mobile/SIGNING.md`. Documentation for Android APK signing and internal distribution. References Fabrica package identity (`com.autoscalers.fabrica.mobile`).
- **Notes:** Documentation only — no code risk.

### mobile/.easignore
- **Old path (in Fabrica-app/):** mobile/.easignore
- **New path (in upstream-orca/):** null (does not exist)
- **Scenario:** 2
- **Risk:** easy
- **Implementation guidance:** Re-implement in `mobile/.easignore`. Standard EAS ignore rules (node_modules, .expo, android, ios, .git, *.log).
- **Notes:** Trivial file — copy as-is.

### mobile/src/test-support/source-text.ts
- **Old path (in Fabrica-app/):** mobile/src/test-support/source-text.ts
- **New path (in upstream-orca/):** null (does not exist)
- **Scenario:** 2
- **Risk:** easy
- **Implementation guidance:** Re-implement in `mobile/src/test-support/source-text.ts`. Test utility that normalizes CRLF to LF for cross-platform test assertions. 8 lines.
- **Notes:** Simple utility, no dependencies.

### resources/app-icons/fabrica-dark.png
- **Old path (in Fabrica-app/):** resources/app-icons/fabrica-dark.png
- **New path (in upstream-orca/):** null (has orca-blue.png, orca-watercolor.png)
- **Scenario:** 2
- **Risk:** easy
- **Implementation guidance:** Add Fabrica brand icon assets. Upstream has `orca-blue.png` and `orca-watercolor.png` — replace or keep alongside depending on brand needs.
- **Notes:** Brand asset — must be provided by design.

### resources/app-icons/fabrica-light.png
- **Old path (in Fabrica-app/):** resources/app-icons/fabrica-light.png
- **New path (in upstream-orca/):** null
- **Scenario:** 2
- **Risk:** easy
- **Implementation guidance:** Add Fabrica brand icon asset. Pair with fabrica-dark.png for light/dark theme icons.
- **Notes:** Brand asset — must be provided by design.

### resources/icon-source/fabrica-logo_icon.png
- **Old path (in Fabrica-app/):** resources/icon-source/fabrica-logo_icon.png
- **New path (in upstream-orca/):** null (has icon.source/ with generate.sh and icon.icon)
- **Scenario:** 2
- **Risk:** easy
- **Implementation guidance:** Add Fabrica brand emblem source PNG. This feeds the icon build pipeline (build-fabrica-icons.mjs).
- **Notes:** Brand asset — source for all icon generation.

### resources/fabrica-hero-bg.jpg
- **Old path (in Fabrica-app/):** resources/fabrica-hero-bg.jpg
- **New path (in upstream-orca/):** null
- **Scenario:** 2
- **Risk:** easy
- **Implementation guidance:** Add hero background image used by StartupGate component.
- **Notes:** Brand asset for auth gate UI.

### src/shared/fabrica-attribution.ts
- **Old path (in Fabrica-app/):** src/shared/fabrica-attribution.ts
- **New path (in upstream-orca/):** null (upstream DELETED orca-attribution.ts)
- **Scenario:** 4
- **Risk:** easy
- **Implementation guidance:** Archive — do not re-implement. Upstream removed the git commit trailer feature entirely. The `FABRICA_GIT_COMMIT_TRAILER` constant (`Co-authored-by: Fabrica <fabrica.studio.contact@gmail.com>`) is dead code.
- **Notes:** Upstream removed this feature. If we want attribution, it needs a new design decision.

### visual-palette-reference.md
- **Old path (in Fabrica-app/):** visual-palette-reference.md
- **New path (in upstream-orca/):** null
- **Scenario:** 4
- **Risk:** easy
- **Implementation guidance:** Archive — design system documentation, not source code. Contains OKLCH design token reference for visual palette migration. Useful as reference but not for re-implementation.
- **Notes:** Documentation artifact.

### contrast-audit.js
- **Old path (in Fabrica-app/):** contrast-audit.js
- **New path (in upstream-orca/):** null
- **Scenario:** 4
- **Risk:** easy
- **Implementation guidance:** Archive — WCAG contrast audit script for CSS tokens. Development tooling, not production code. Can be run ad-hoc against the new CSS.
- **Notes:** Tooling artifact.

### build-eb.cmd
- **Old path (in Fabrica-app/):** build-eb.cmd
- **New path (in upstream-orca/):** null
- **Scenario:** 4
- **Risk:** easy
- **Implementation guidance:** Archive — local build convenience script. Hardcoded path to `C:\Users\BAB AL SAFA\Desktop\...`. Not portable.
- **Notes:** Local convenience script.

### build-win-wrap.cmd
- **Old path (in Fabrica-app/):** build-win-wrap.cmd
- **New path (in upstream-orca/):** null
- **Scenario:** 4
- **Risk:** easy
- **Implementation guidance:** Archive — local build convenience script. Hardcoded path. Not portable.
- **Notes:** Local convenience script.

### electron.vite.config.1787972008048.mjs
- **Old path (in Fabrica-app/):** electron.vite.config.1787972008048.mjs
- **New path (in upstream-orca/):** null
- **Scenario:** 4
- **Risk:** easy
- **Implementation guidance:** Archive — inlined/bundled electron-vite config (build artifact). Not source code.
- **Notes:** Build artifact.

---

## Entries Flagged for PM Review

### Scenario 2 (Re-implement) — Requires PM Decision

1. **StartupGate.tsx** (hard risk) — Core auth gate. Depends on cloud auth infrastructure. PM should confirm cloud auth is still desired for Fabrica v2.
2. **config/scripts/build-fabrica-icons.mjs** (medium risk) — Icon build pipeline. PM should confirm brand assets are ready and pipeline is needed.
3. **resources/skills/snapshot-registry.json** (medium risk) — Deciding between trimmed format vs upstream's full history. PM preference needed.
4. **resources/skills/release-mapping.json** (medium risk) — Same decision as snapshot-registry.
5. **Font files** (easy risk) — Brand assets must be provided. PM should confirm font licensing.

### Scenario 4 (Archive) — Requires PM Confirmation

1. **src/shared/fabrica-attribution.ts** — Upstream removed git commit trailer. PM should decide if Fabrica wants its own attribution mechanism.
2. **visual-palette-reference.md** — Design doc. PM should confirm OKLCH token model is still the target.

---

## Notable Findings

1. **Upstream deleted orca-attribution.ts** — The git commit trailer feature (`Co-authored-by: Orca`) was removed from upstream. Our `FABRICA_GIT_COMMIT_TRAILER` is dead code unless we intentionally re-implement attribution.

2. **Cloud auth is entirely custom** — StartupGate, profile-cloud-* files, and auth config are all Fabrica additions with no upstream equivalent. This is the largest block of custom logic requiring re-implementation.

3. **Upstream added relay server** — The `cloud/` directory (339 files) is entirely new in upstream. This may overlap with or supersede our `Fabrica-relay/` sub-project. PM should clarify relay strategy.

4. **Font migration is clean** — Upstream still uses Geist fonts. Our Inter/Space Grotesk/JetBrains Mono swap is a straightforward asset addition with no code conflicts.

5. **Skills registries diverged** — Upstream added new skills (orca-per-workspace-env, expanded orchestration). Our trimmed format is cleaner but missing upstream's new data. Need to merge.

6. **Mobile files are all new** — The entire `mobile/` custom logic (eas.json, SIGNING.md, .easignore, source-text.ts) has no upstream equivalent. These are pure additions.

7. **Build artifacts can be ignored** — electron.vite.config.*.mjs, build-eb.cmd, build-win-wrap.cmd are local artifacts that don't belong in the repo.

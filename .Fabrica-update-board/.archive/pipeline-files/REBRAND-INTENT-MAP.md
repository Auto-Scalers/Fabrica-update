# Rebrand Intent Map

> Diff between `orca-baseline/` and `Fabrica-app/` (v0.0.6, commit `0a5d258`), intent-tagged.
> Generated from `git diff --no-index` analysis.

## Summary

- **Total files changed:** 4,214 (excluding `.git/`, `out/`, `node_modules/`)
- **Renamed (R*):** 132 files — all rebrand
- **Added (A):** 70 files
- **Deleted (D):** 48 files
- **Modified (M):** 3,937 files
- **Binary files:** 42 (fonts, icons, images)
- **Text files:** 4,172

### Intent Breakdown (estimated)

| Intent | Files | Notes |
|--------|-------|-------|
| **rebrand** | ~4,100 | Identity substitution only (orca→fabrica, stablyai→fabrica-ai, etc.) |
| **custom_logic** | ~80 | Features/fixes/assets added or removed |
| **incidental** | ~34 | Dep bumps, whitespace, formatting |

## Rebrand Patterns Found

These substitutions appear across the codebase. Every file tagged `rebrand` contains one or more of these:

| Pattern | Scope |
|---------|-------|
| `orca` → `fabrica` | Filenames, function names, variables, imports, CLI commands |
| `Orca` → `Fabrica` | Class names, product name, UI strings |
| `ORCA` → `FABRICA` | Constants, env vars, wire tokens |
| `orca-profiles` → `fabrica-profiles` | Directory/module rename |
| `stablyai` → `fabrica-ai` | npm scopes, GitHub URLs |
| `stablyai/orca` → `Auto-Scalers/Fabrica-app` | GitHub repo references |
| `@stablyai/*` → `@autoscalers/*` | Package names |
| `com.stablyai.orca` → `ai.autoscalers.fabrica` | Electron app ID |
| `onorca.dev` → `fabrica-ai.vercel.app` | Production URLs |
| `orca-cli` → `fabrica-cli` | CLI command name |
| `orca-ide` → `fabrica` | Linux executable name |
| `Geist` → `Inter` / `Space Grotesk` / `JetBrains Mono` | Font family names |
| `Orca Nerd Font Symbols` → `Fabrica Nerd Font Symbols` | Font family |
| `SymbolsNerdFontMono-Regular.woff2` → `FabricaNerdFontSymbols-Regular.woff2` | Font file |

## File-by-File Analysis

### Root Files

#### `package.json`
- **Intent:** rebrand + custom_logic
- **What changed:** Name, author, homepage, CLI binary names, build env vars, added `@supabase/supabase-js` dep, removed `@stablyai/playwright-test`, added `esbuild`
- **Our pattern:** `orca` → `fabrica`, `stablyai` → `Auto-Scalers`, `ORCA_MAC_RELEASE` → `FABRICA_MAC_RELEASE`
- **Notes:** Supabase addition suggests planned cloud auth integration

#### `electron.vite.config.ts`
- **Intent:** rebrand
- **What changed:** Rebrand constants, env var names
- **Our pattern:** `ORCA_*` → `FABRICA_*`

#### `orca.yaml` → `fabrica.yaml`
- **Intent:** rebrand (renamed)
- **What changed:** Filename rename + content rebrand

#### `AGENTS.md`
- **Intent:** rebrand
- **What changed:** Product name references

#### `README.md`
- **Intent:** rebrand
- **What changed:** Product name, repo links

#### `pnpm-lock.yaml`
- **Intent:** rebrand + incidental
- **What changed:** Lock file reflects dep changes (supabase added, stablyai removed)

### Config Files

#### `config/electron-builder.config.cjs`
- **Intent:** rebrand + custom_logic
- **What changed:** App ID, product name, artifact names, env vars, signing identities, Linux package names, GitHub publish config. Removed dev-channel build infrastructure (hourly/daily/adhoc builds).
- **Our pattern:** `com.stablyai.orca` → `ai.autoscalers.fabrica`, `ORCA_*` → `FABRICA_*`, `stablyai` → `Auto-Scalers`
- **Notes:** Dev-channel removal is custom_logic (simplification for Fabrica's release model)

#### `config/reliability-gates.jsonc`
- **Intent:** rebrand
- **What changed:** GitHub URL references updated
- **Our pattern:** `stablyai/orca` → `Auto-Scalers/fabrica`

#### `config/scripts/locale-ko-key-overrides.json`
- **Intent:** rebrand + custom_logic
- **What changed:** Korean locale overrides trimmed from ~5,033 to ~503 entries
- **Notes:** Massive trim is custom_logic — likely removed keys no longer in use

#### `config/scripts/build-fabrica-icons.mjs`
- **Intent:** custom_logic (new file)
- **What changed:** New icon build script for Fabrica brand assets
- **Notes:** Generates app icons, tray icons, and macOS icns from brand emblem

#### `config/scripts/orca-dev.mjs` → `config/scripts/fabrica-dev.mjs`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content rebrand

### Source: `src/main/`

#### `src/main/index.ts`
- **Intent:** rebrand
- **What changed:** Import paths (orca-profiles→fabrica-profiles), function names, class names
- **Our pattern:** `OrcaRuntimeService` → `FABRICARuntimeService`, `initOrcaProfilePaths` → `initFABRICAProfilePaths`

#### `src/main/runtime/orca-runtime.ts` → `fabrica-runtime.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + all internal references

#### `src/main/runtime/orca-runtime.test.ts` → `fabrica-runtime.test.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + all mock names, env vars, function references

#### `src/main/runtime/runtime-rpc.test.ts`
- **Intent:** rebrand
- **What changed:** Equal insertions/deletions — pure identity substitution

#### `src/main/github/client.ts`
- **Intent:** rebrand
- **What changed:** GitHub API calls, star check endpoint
- **Our pattern:** `stablyai/orca` → `Auto-Scalers/Fabrica-app`

#### `src/main/github/client.test.ts`
- **Intent:** rebrand
- **What changed:** Test assertions updated for new repo names

#### `src/main/ipc/pty.test.ts`
- **Intent:** rebrand
- **What changed:** Mock paths, env vars, function names

#### `src/main/providers/local-pty-shell-ready.test.ts`
- **Intent:** rebrand
- **What changed:** Test mock updates

### Source: `src/main/fabrica-profiles/` (renamed from `orca-profiles/`)

#### All files in `src/main/fabrica-profiles/`
- **Intent:** rebrand (renamed directory)
- **What changed:** Directory rename + all internal references
- **Our pattern:** `OrcaProfile` → `FABRICAProfile`, `orca-profiles` → `fabrica-profiles`

#### `src/main/fabrica-profiles/profile-cloud-auth-config.ts`
- **Intent:** rebrand + custom_logic
- **What changed:** Production URLs point to `fabrica-ai.vercel.app`, client ID is `FABRICA-desktop`, relay director at `fabrica.autoscalers.workers.dev`
- **Notes:** Custom cloud auth infrastructure endpoints

#### `src/main/fabrica-profiles/profile-cloud-client.ts`
- **Intent:** rebrand
- **What changed:** Type names, function names

#### `src/main/fabrica-profiles/profile-cloud-service.ts`
- **Intent:** rebrand
- **What changed:** Function names, error message strings

### Source: `src/main/cli/`

#### `src/main/cli/linux-bare-orca-dispatcher.ts` → `linux-bare-fabrica-dispatcher.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### `src/main/cli/linux-terminal-orca-cli-shim.ts` → `linux-terminal-fabrica-cli-shim.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

### Source: `src/main/ssh/`

#### `src/main/ssh/ssh-remote-orca-cli.ts` → `ssh-remote-fabrica-cli.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

### Source: `src/shared/`

#### `src/shared/orca-yaml.ts` → `fabrica-yaml.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### `src/shared/fabrica-yaml-file-limit.ts`
- **Intent:** rebrand (new file, rebrand of orca-yaml-file-limit)
- **What changed:** Constants renamed `ORCA_YAML_*` → `FABRICA_YAML_*`

#### `src/shared/fabrica-attribution.ts`
- **Intent:** custom_logic (new file)
- **What changed:** Single constant: `FABRICA_GIT_COMMIT_TRAILER`
- **Notes:** Brand-specific git commit trailer

#### `src/shared/fabrica-cli-command-name.ts`
- **Intent:** rebrand (new file, rebrand of orca-cli-command-name)
- **What changed:** Function returns `fabrica` / `fabrica.cmd` instead of `orca` / `orca.cmd`

#### `src/shared/fabrica-profiles.ts`
- **Intent:** rebrand (new file, rebrand of orca-profiles)
- **What changed:** All types renamed `OrcaProfile*` → `FABRICAProfile*`

#### `src/shared/orca-dispatch-status-prompt.ts` → `fabrica-dispatch-status-prompt.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

### Source: `src/renderer/`

#### `src/renderer/src/assets/main.css`
- **Intent:** rebrand + custom_logic
- **What changed:** Font family renamed (`Geist` → `Inter`/`Space Grotesk`/`JetBrains Mono`, `Orca Nerd Font` → `Fabrica Nerd Font`), added font-face declarations for Inter subsets (cyrillic, greek, vietnamese, latin-ext), added Space Grotesk and JetBrains Mono font-faces
- **Notes:** Font migration is custom_logic (new brand typography)

#### `src/renderer/src/assets/fonts/`
- **Intent:** rebrand + custom_logic
- **What changed:** Removed `Geist-Variable.woff2` and `SymbolsNerdFontMono-Regular.woff2`, added `FabricaNerdFontSymbols-Regular.woff2` and Inter/Space Grotesk/JetBrains Mono subsets
- **Notes:** Font file swap is custom_logic (new brand fonts)

#### `src/renderer/src/components/StartupGate.tsx`
- **Intent:** custom_logic (new file)
- **What changed:** New auth gate component shown before app loads
- **Notes:** Login screen with Fabrica branding, cloud auth integration

#### `src/renderer/src/components/settings/OrcaAccountSettingsPane.tsx` → `FabricaAccountSettingsPane.tsx`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### `src/renderer/src/components/settings/fabrica-account-settings-search.ts`
- **Intent:** rebrand (new file, rebrand of orca-account-settings-search)
- **What changed:** Search terms updated for Fabrica

#### `src/renderer/src/components/sidebar/OrcaYamlTrustDialog.tsx` → `FabricaYamlTrustDialog.tsx`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### `src/renderer/src/components/task-page-linear-in-orca-issues.ts` → `task-page-linear-in-fabrica-issues.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### `src/renderer/src/lib/orca-hook-trust.ts` → `fabrica-hook-trust.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### `src/renderer/src/store/slices/orca-profiles.ts` → `fabrica-profiles.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### `src/renderer/src/store/slices/orca-profiles-auth-actions.ts` → `fabrica-profiles-auth-actions.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### `src/renderer/src/i18n/locales/en.json`
- **Intent:** rebrand
- **What changed:** All UI strings: "Orca" → "Fabrica", language labels corrupted (encoding issue with CJK characters)

#### `src/renderer/src/i18n/locales/zh.json`, `ja.json`, `ko.json`
- **Intent:** rebrand
- **What changed:** Large diffs (~23K lines each) — rebrand + locale string updates

#### `src/renderer/src/i18n/locales/es.json`
- **Intent:** rebrand
- **What changed:** Spanish locale rebrand

### Source: `src/cli/`

#### `src/cli/index.ts`
- **Intent:** rebrand
- **What changed:** CLI command name, help text

#### `src/cli/index.test.ts`
- **Intent:** rebrand
- **What changed:** Test assertions updated

### Mobile: `mobile/`

#### `mobile/src/components/OrcaLogo.tsx` → `FabricaLogo.tsx`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### `mobile/eas.json`
- **Intent:** custom_logic (new file)
- **What changed:** Expo Application Services config for mobile builds

#### `mobile/SIGNING.md`
- **Intent:** custom_logic (new file)
- **What changed:** Mobile signing documentation

#### `mobile/.easignore`
- **Intent:** custom_logic (new file)
- **What changed:** EAS ignore rules

#### `mobile/src/test-support/source-text.ts`
- **Intent:** custom_logic (new file)
- **What changed:** Test support utility

### Native: `native/`

#### All `native/computer-use-macos/Sources/OrcaComputerUse*` → `FabricaComputerUse*`
- **Intent:** rebrand (renamed)
- **What changed:** Directory + file renames, class name updates

#### All `native/computer-use-macos/Tests/OrcaComputerUse*` → `FabricaComputerUse*`
- **Intent:** rebrand (renamed)
- **What changed:** Directory + file renames

#### `native/windows-cli-launcher/OrcaCliLauncher.cs` → `FabricaCliLauncher.cs`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + class name

#### `native/computer-use-windows/runtime.ps1`
- **Intent:** rebrand
- **What changed:** Class name `OrcaDesktopWin32` → `FabricaDesktopWin32`, function names `*-Orca*` → `*-Fabrica*`

### Skills: `skills/`, `skill-guides/`, `skill-stubs/`

#### All `skills/orca-*` → `skills/fabrica-*`
- **Intent:** rebrand (renamed)
- **What changed:** Directory + file renames

#### All `skill-guides/orca-*` → `skill-guides/fabrica-*`
- **Intent:** rebrand (renamed)
- **What changed:** Filename renames

#### All `skill-stubs/orca-*` → `skill-stubs/fabrica-*`
- **Intent:** rebrand (renamed)
- **What changed:** Filename renames

#### `skills/fabrica-cli/SKILL.md`
- **Intent:** rebrand
- **What changed:** All references: `orca` → `fabrica`

### Resources

#### `resources/skills/snapshot-registry.json`
- **Intent:** rebrand + custom_logic
- **What changed:** Trimmed from ~1,751 to ~149 lines — all historical skill revisions removed, only current version retained
- **Notes:** Massive trim is custom_logic (simplified skill registry for Fabrica)

#### `resources/skills/release-mapping.json`
- **Intent:** rebrand + custom_logic
- **What changed:** Trimmed from ~644 to ~18 lines — all historical Orca version mappings removed
- **Notes:** Simplified to only current Fabrica version

#### `resources/app-icons/fabrica-dark.png`, `fabrica-light.png`
- **Intent:** custom_logic (new files)
- **What changed:** New brand icon assets

#### `resources/icon-source/fabrica-logo_icon.png`
- **Intent:** custom_logic (new file)
- **What changed:** Brand emblem source PNG

#### `resources/fabrica-hero-bg.jpg`
- **Intent:** custom_logic (new file)
- **What changed:** Hero background image for StartupGate

#### `resources/tray/fabrica-menu-barTemplate.png`, `@2x.png`
- **Intent:** rebrand (renamed)
- **What changed:** Filename rename from orca

#### `resources/plugins/launch/stablyai.orca-*` → `autoscalers.fabrica-*`
- **Intent:** rebrand (renamed)
- **What changed:** Plugin directory + file renames

#### `resources/darwin/bin/orca` → `fabrica`
- **Intent:** rebrand (renamed)
- **What changed:** Filename rename

#### `resources/linux/bin/orca-ide` → `fabrica`
- **Intent:** rebrand (renamed)
- **What changed:** Filename rename

#### `resources/win32/bin/orca.cmd` → `fabrica.cmd`
- **Intent:** rebrand (renamed)
- **What changed:** Filename rename

### Tests

#### `tests/e2e/helpers/orca-app.ts` → `fabrica-app.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### `tests/e2e/helpers/orca-restart.ts` → `fabrica-restart.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### `tests/e2e/orca-restart-navigation.unit.test.ts` → `fabrica-restart-navigation.unit.test.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### `tests/e2e/setup-script-prompt-unreadable-orca-yaml.spec.ts` → `fabrica-yaml.spec.ts`
- **Intent:** rebrand (renamed)
- **What changed:** Filename + content

#### All other `tests/e2e/` files
- **Intent:** rebrand
- **What changed:** Mock paths, env vars, function references updated

#### `tests/tools/benchmarks/results/*.json`
- **Intent:** incidental (deleted)
- **What changed:** Benchmark result files removed
- **Notes:** Build artifacts, not source code

### Docs

#### `ORCA_WINDOWS_SETUP_GUIDE.md`
- **Intent:** incidental (deleted)
- **What changed:** Removed — likely superseded

#### `docs/mobile-relay-ux-findings.md`
- **Intent:** incidental (deleted)
- **What changed:** Removed

#### `docs/reference/headless-linux-server.md`
- **Intent:** rebrand
- **What changed:** Product name references

#### `visual-palette-reference.md`
- **Intent:** custom_logic (new file)
- **What changed:** OKLCH design token reference for visual palette migration
- **Notes:** Documentation for brand color system

### CI/CD

#### `.github/workflows/adhoc-mac-build.yml`, `daily-mac-build.yml`, `hourly-mac-build.yml`
- **Intent:** incidental (deleted)
- **What changed:** Dev-channel build workflows removed
- **Notes:** Simplified release model for Fabrica

#### `config/scripts/setup-adhoc-release-repo.sh`, `setup-daily-release-repo.sh`, `setup-hourly-release-token.sh`
- **Intent:** incidental (deleted)
- **What changed:** Dev-channel setup scripts removed

### Build Artifacts

#### `out/` directory (all files)
- **Intent:** incidental (deleted)
- **What changed:** Build output removed from diff
- **Notes:** Not source code

#### `build-eb.cmd`, `build-win-wrap.cmd`, `build-win.err`
- **Intent:** custom_logic (new files)
- **What changed:** Local build convenience scripts

#### `electron.vite.config.1787972008048.mjs`
- **Intent:** custom_logic (new file)
- **What changed:** Inlined/bundled electron-vite config (build artifact)

#### `contrast-audit.js`
- **Intent:** custom_logic (new file)
- **What changed:** WCAG contrast audit script for CSS tokens

### Examples

#### `examples/plugins/hello-orca/` → `hello-fabrica/`
- **Intent:** rebrand (renamed)
- **What changed:** Plugin example renamed

### Casks

#### `Casks/orca.rb` → `fabrica.rb`
- **Intent:** rebrand (renamed)
- **What changed:** Homebrew cask renamed

#### `Casks/orca@rc.rb` → `fabrica@rc.rb`
- **Intent:** rebrand (renamed)
- **What changed:** RC cask renamed

## Notable Findings

### Large Custom Logic Changes

1. **Font system overhaul** — Geist replaced with Inter/Space Grotesk/JetBrains Mono. New font files added, CSS updated with subset declarations. This is a significant visual change.

2. **StartupGate component** — New auth gate shown before app loads. Integrates with cloud auth system.

3. **Cloud auth infrastructure** — New `fabrica-profiles/` directory with full OAuth/PKCE flow, session management, org membership, and capability flags. Points to `fabrica-ai.vercel.app` backend.

4. **Supabase dependency** — Added but not yet used in source code. Likely planned for cloud auth backend.

5. **Icon build pipeline** — New `build-fabrica-icons.mjs` script generates all app/tray icons from brand emblem.

6. **Skill registry trim** — snapshot-registry.json reduced from 1,751 to 149 lines (92% reduction). release-mapping.json from 644 to 18 lines (97% reduction).

7. **Korean locale trim** — locale-ko-key-overrides.json reduced from 5,033 to 503 lines (90% reduction).

8. **Dev-channel removal** — Hourly/daily/adhoc build infrastructure completely removed (workflows, scripts, env vars).

### Files Hard to Classify

- **`src/renderer/src/i18n/locales/*.json`** — Large rebrand diffs, but some CJK characters appear corrupted (encoding issue). The rebrand is clear but the corruption is incidental.
- **`config/reliability-gates.jsonc`** — Pure rebrand (GitHub URLs), but the file is 10K+ lines.
- **`pnpm-lock.yaml`** — Reflects dep changes but is auto-generated.

## Custom Logic Files Requiring Re-Implementation

These files contain features that must be re-implemented in the new `Fabrica/` repo:

1. `src/renderer/src/components/StartupGate.tsx` — Auth gate component
2. `src/shared/fabrica-attribution.ts` — Git commit trailer constant
3. `src/main/fabrica-profiles/profile-cloud-auth-config.ts` — Cloud auth endpoints
4. `src/main/fabrica-profiles/profile-cloud-client.ts` — Cloud API client
5. `src/main/fabrica-profiles/profile-cloud-service.ts` — Cloud auth orchestration
6. `src/main/fabrica-profiles/profile-cloud-session-exchange.ts` — Session exchange
7. `src/main/fabrica-profiles/profile-cloud-dev-service.ts` — Dev auth service
8. `src/main/fabrica-profiles/profile-cloud-org-members-service.ts` — Org membership
9. `src/main/ipc/fabrica-profile-auth-handlers.ts` — IPC auth handlers
10. `src/main/ipc/fabrica-profile-org-members-handlers.ts` — IPC org handlers
11. `src/renderer/src/store/slices/fabrica-profiles.ts` — Pinia store
12. `src/renderer/src/store/slices/fabrica-profiles-auth-actions.ts` — Auth actions
13. `src/renderer/src/components/settings/fabrica-account-settings-search.ts` — Settings search
14. `config/scripts/build-fabrica-icons.mjs` — Icon build script
15. `resources/skills/snapshot-registry.json` — Trimmed skill registry
16. `resources/skills/release-mapping.json` — Trimmed release mapping
17. `mobile/eas.json` — EAS config
18. `mobile/SIGNING.md` — Signing docs
19. `mobile/src/test-support/source-text.ts` — Test utility
20. `src/main/plugins/plugin-marketplace-provenance.test.ts` — Marketplace test
21. `src/main/pty/wsl-fabrica-env.test.ts` — WSL env test
22. `src/renderer/src/components/StartupGate.tsx` — Auth gate UI
23. `visual-palette-reference.md` — Design token docs
24. `contrast-audit.js` — Contrast audit tool
25. `build-eb.cmd`, `build-win-wrap.cmd` — Local build scripts

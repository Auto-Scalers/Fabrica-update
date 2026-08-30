# REBRAND-DIFF-MAP.md — Master Human-Readable Diff Map

> **T1 Deliverable** | 2026-08-30 | Phase 1: Rebrand Diff Analytics
> Comparison: `orca-baseline/` (frozen Orca fork baseline) vs `Fabrica-app/` (current Fabrica code)
> Worker: T1 aggregate | Status: COMPLETE

---

## Executive Summary

| Metric | Value |
|--------|-------|
| **Total files changed** | ~872 |
| **Total diff hunks cataloged** | ~1,800+ |
| **Rebrand changes** | ~60+ distinct patterns across all areas |
| **Custom logic additions** | 22 distinct features (Fabrica-only) |
| **Debrand cleanup removals** | 14 categories of removed Orca-only artifacts |
| **Incidental changes** | 10 categories (dep bumps, whitespace, encoding) |
| **Bugs found** | 2 (rebrand corruption: function name, telemetry reason casing) |

### Breakdown by Area

| Area | Files Changed | Dominant Intent |
|------|--------------|-----------------|
| Config files (root) | ~35 (6 modified + 26 workflows + Casks + docs) | rebrand |
| `src/main/` | ~196 | rebrand + custom-logic |
| `src/cli/` | ~72 | rebrand |
| `src/renderer/` | ~13 | rebrand |
| `src/shared/` | ~300 | rebrand + custom-logic |
| `src/preload/` | ~9 | rebrand + custom-logic |
| `src/relay/` | ~59 | rebrand |
| `src/types/` | ~1 | rebrand |
| `src/renderer/src/assets/` | ~8 | rebrand + custom-logic |
| `mobile/` | ~5 | rebrand + custom-logic |
| `native/` | ~6 | rebrand |

---

## Cross-Cutting Rebrand Patterns (12+ Rules)

These are the canonical substitution rules applied across the entire codebase. Every upstream Orca identifier must be checked against this table.

| # | Pattern | Orca Value | Fabrica Value | Scope | Sync Rule |
|---|---------|-----------|---------------|-------|-----------|
| 1 | **Package identity** | `orca`, `stablyai`, `com.stablyai.orca` | `fabrica`, `Auto-Scalers`, `ai.autoscalers.fabrica` | package.json, electron-builder, app ID, bundle ID | Always apply; never reintroduce Orca identity |
| 2 | **Env var prefix** | `ORCA_*` | `FABRICA_*` | All env vars, build constants, compile-time defines | Port new upstream ORCA_ var, apply FABRICA_ prefix |
| 3 | **GitHub URLs** | `stablyai/orca` | `Auto-Scalers/fabrica` (or `Fabrica-app`) | CI workflows, source code, docs, casks, READMEs | Replace org + repo; main repo is `Fabrica-app` |
| 4 | **Domain/homepage** | `onorca.dev` | `fabrica-ai.vercel.app` | All API endpoints, docs, kill-list, changelog, telemetry, feedback | Never reintroduce onorca.dev |
| 5 | **User data directory** | `~/.orca`, `~/.config/orca`, `Application Support/orca` | `~/.fabrica`, `~/.config/Fabrica`, `Application Support/Fabrica` | Driven by productName; dev override `fabrica-dev` | Follow productName |
| 6 | **App bundle** | `Orca.app` | `Fabrica.app` | macOS app bundle name, all cask references | Always use Fabrica.app |
| 7 | **Linux executable** | `orca-ide` | `fabrica` | Linux binary name (simplified — no GNOME conflict) | Use `fabrica`; never `orca-ide` |
| 8 | **CLI binary** | `orca`, `orca.cmd`, `orca.exe` | `fabrica`, `fabrica.cmd`, `fabrica.exe` | CLI entry points, installer paths, packaging | Single command; no dual-variant |
| 9 | **Helper apps** | `Orca Computer Use.app`, `orca-notification-status` | `Fabrica Computer Use.app`, `fabrica-notification-status` | macOS helper apps, signing, permission detection | Always Fabrica names |
| 10 | **Asset files** | `orca-*.gif`, `orca-cli.*`, `SymbolsNerdFontMono-*` | `fabrica-*.gif`, `fabrica-cli.*`, `FabricaNerdFontSymbols-*` | Docs assets, Nerd Font files | Rename all orca-prefixed assets |
| 11 | **Plugin names** | `engines.orca`, `orca-*` IDs, `stablyai` publisher | `engines.fabrica`, `fabrica-*` IDs, `autoscalers` publisher | Plugin manifests, marketplace index, kill-list | All plugin identity must use fabrica |
| 12 | **CSS custom properties** | `--orca-*` | `--fabrica-*` | All CSS variables, class names, data attributes | Port upstream --orca-* → --fabrica-* |
| 13 | **Font family** | `Geist`, `Geist Mono` | `Inter` | CSS @font-face, constants.ts, all font references | Replace Geist → Inter |
| 14 | **Wire tokens/protocol** | `orca_server_ready`, `ORCA-RELAY`, `orca://`, `__ORCA_*__` | `fabrica_server_ready`, `FABRICA-RELAY`, `fabrica://`, `__FABRICA_*__` | IPC, SSH relay, deep links, protocol markers | Port upstream markers, apply fabrica prefix |
| 15 | **YAML config file** | `orca.yaml` | `FABRICA.yaml` | UI labels, comments, config file name | Replace orca.yaml → FABRICA.yaml |
| 16 | **Persistence files** | `orca-data.json` | `FABRICA-data.json` | Runtime data persistence | Replace orca-data → FABRICA-data |
| 17 | **Commit trailer** | `Co-authored-by: Orca <help@stably.ai>` | `Co-authored-by: Fabrica <fabrica.studio.contact@gmail.com>` | Git attribution | Always use Fabrica trailer |
| 18 | **Email/support** | `help@stably.ai` | `fabrica.studio.contact@gmail.com` | Contact/support references | Replace stably.ai email |
| 19 | **NPM scope** | `@stablyai/*`, `@orca/*` | `@autoscalers/*`, `@fabrica/*` | Scoped packages | Rename scope |
| 20 | **Mobile bundle ID** | `com.stably.orca.mobile` | `com.autoscalers.fabrica.mobile` | iOS + Android bundle/package | Always use autoscalers |
| 21 | **Ownership literal** | `'orca-managed'` | `'FABRICA-managed'` | Worktree ownership type | Preserve FABRICA-managed |
| 22 | **Shortcut policy** | `'orca-first'` | `'FABRICA-first'` | Terminal shortcut policy | Preserve FABRICA-first |

---

## Per-Area Section: Config Files (Root)

**Files:** package.json, electron-builder.config.cjs, electron.vite.config.ts, vite.web.config.ts, Casks/ (2 files), docs/
**File count:** ~35 (6 source modified + 26 workflows + 2 casks + docs)
**Change count:** ~100+ hunks
**Intent breakdown:** 98% rebrand, 2% custom-logic (Supabase injection)

### Key Hunks

| File | Change Count | Key Pattern |
|------|-------------|-------------|
| `package.json` | 9 hunks | Package name `orca`→`fabrica`, GitHub org `stablyai`→`Auto-Scalers`, CLI bin `orca`→`fabrica`, env vars `ORCA_MAC_*`→`FABRICA_MAC_*`, scoped package `@stablyai/playwright-test`→`@autoscalers/playwright-test` |
| `config/electron-builder.config.cjs` | 40 hunks | App ID `com.stablyai.orca`→`ai.autoscalers.fabrica`, product name `Orca`→`Fabrica`, all `ORCA_*` env vars→`FABRICA_*`, Linux exe simplified `orca-ide`→`fabrica`, GitHub publish `stablyai/orca`→`Auto-Scalers/fabrica` |
| `electron.vite.config.ts` | 18 hunks | All `ORCA_*` env vars→`FABRICA_*`, global flags `__ORCA_*__`→`__FABRICA_*__`, plugin name `orca-main-bootstrap`→`fabrica-main-bootstrap`. **Custom logic:** Supabase URL/anon key compile-time injection (lines 61-70, 270-271) |
| `vite.web.config.ts` | 2 hunks | `ORCA_FEATURE_WALL_ENABLED`→`FABRICA_FEATURE_WALL_ENABLED`, URL path `/orca/`→`/fabrica/` |
| `Casks/fabrica.rb` | 11 hunks | Cask name, download URL (GitHub), homepage `onorca.dev`→`fabrica-ai.vercel.app`, app bundle `Orca.app`→`Fabrica.app`, zap trash `~/.orca`→`~/.fabrica`, bundle ID |
| `Casks/fabrica@rc.rb` | 10 hunks | Same patterns as stable cask |
| `.github/workflows/` | 26 files | Not read line-by-line; follow same `ORCA_*`→`FABRICA_*` and `stablyai/orca`→`Auto-Scalers/fabrica` patterns |
| `docs/STYLEGUIDE.md` | 7 hunks | `Orca`→`Fabrica` in title, body text, and asset filenames |

### Files Identical (No Diff)

`tsconfig.json`, `pnpm-workspace.yaml`, `components.json`

---

## Per-Area Section: `src/main/` + `src/cli/`

**Files:** 196 in src/main/ + 72 in src/cli/ = **268 total**
**Change count:** ~900+ hunks
**Intent breakdown:** 95% rebrand, 3% custom-logic (profile system removal, CLI simplification), 2% incidental (bugs)

### src/main/ Key Changes

| Category | Count | Pattern |
|----------|-------|---------|
| Env var renames | 30+ distinct vars | `ORCA_*` → `FABRICA_*` (mechanical prefix swap across all .ts files) |
| Domain/URL renames | 5 distinct URLs | `onorca.dev` → `fabrica-ai.vercel.app` (feedback, changelog, nudge, kill-list) |
| Wire token/protocol renames | 12 distinct tokens | `orca_server_ready`→`fabrica_server_ready`, `__ORCA_SETUP_COMPLETE__`→`__FABRICA_SETUP_COMPLETE__`, SSH protocol markers |
| App ID / brand identity | 15+ references | `orca-data.json`→`FABRICA-data.json`, `orca-dev`→`fabrica-dev`, data dirs, TLS certs |
| Keychain/TLS identity | 3 references | `CN=Orca Runtime`→`CN=Fabrica Runtime`, `orca-tls-*`→`fabrica-tls-*` |
| File renames | 6 files | `wsl-orca-env.ts`→`wsl-fabrica-env.ts`, `linux-bare-orca-dispatcher.ts`→`linux-bare-fabrica-dispatcher.ts`, etc. |
| Function/type/class renames | 30+ identifiers | `configureOrcaUserDataPathEnv`→`configureFABRICAUserDataPathEnv`, `OrcaVmRecipe`→`FABRICAVmRecipe`, etc. |
| CLI command type | 1 structural change | `OrchestrationCliCommand = 'orca' \| 'orca-ide'` → `'fabrica'` (single command) |
| File path constants | 20+ paths | `/usr/local/bin/orca`→`/usr/local/bin/fabrica`, install paths, WSL paths |

### src/cli/ Key Changes

| Category | Count | Pattern |
|----------|-------|---------|
| Core runtime | 15 functions | `serveOrcaApp`→`serveFABRICAApp`, `launchOrcaApp`→`launchFABRICAApp`, `openOrca`→`openFABRICA` |
| CLI command specs | 19 spec files | All `orca ...` usage strings → `fabrica ...` |
| Bundled skill guides | 1 file | All skill IDs, markdown variables, descriptions renamed |
| Data paths | 3 platforms | macOS/Windows/Linux data dir renames via productName |
| Error messages | 9 messages | "Orca runtime" → "Fabrica runtime" in transport diagnostics |
| Deep link protocol | 1 change | `orca://pair?...` → `fabrica://pair?...` |

### Custom Logic (src/main/ + src/cli/)

| What | Why | Files |
|------|-----|-------|
| Orca profile system removal | Fabrica uses Supabase auth, not Orca's cloud profiles | `ipc/orca-profiles.ts`, `ipc/orca-profile-org-members-handlers.ts` + tests (5 files removed) |
| CLI command simplification | Single `fabrica` command replaces dual `orca`/`orca-ide` | `runtime/orchestration/cli-command.ts` |
| Linux CLI naming | `fabrica` instead of `orca-ide` (no GNOME conflict) | `cli/cli-installer.ts`, dispatcher/shim files |

### Bugs Found

| Bug | File | Severity | Description | Fix |
|-----|------|----------|-------------|-----|
| Function name corruption | `telemetry/client.ts` | **High** | `waitForCaptureEnqueue` → `waitFFABRICAptureEnqueue` (ORCA substring replaced mid-word) | Rename back to `waitForCaptureEnqueue` — grep `waitFFABRICAptureEnqueue` in src/ and replace all occurrences |
| Telemetry reason casing | `telemetry/consent.ts` | Low | `'orca_disabled'` → `'FABRICA_disabled'` (should be `'fabrica_disabled'`) | Rename to `'fabrica_disabled'` — grep `FABRICA_disabled` in src/ and replace with `fabrica_disabled` |

**Fix verification:** After fixing, run `grep -r "waitFFABRICAptureEnqueue" src/` and `grep -r "FABRICA_disabled" src/` — both should return 0 hits. Then `pnpm typecheck` and `pnpm lint` should pass.

---

## Per-Area Section: `src/renderer/` + `src/shared/`

**Files:** ~13 renderer + ~300 shared = **~313 total**
**Change count:** ~500+ hunks
**Intent breakdown:** 96% rebrand, 3% custom-logic (CSS additions, icon redesign, Supabase auth), 1% incidental

### Renderer Key Changes

| File | Change Count | Key Pattern |
|------|-------------|-------------|
| HTML shells (3 files) | 3 hunks | `<title>Orca</title>` → `<title>Fabrica</title>` |
| CSS assets (3 of 5 differ) | 50+ hunks | Geist→Inter font, `--orca-*`→`--fabrica-*` vars, Nerd Font rename, `--orca-security-*`→`--fabrica-security-*` |
| `App.tsx` / `main.tsx` | 2 hunks | Error message: `Orca hit a renderer error` → `Fabrica hit a renderer error` |
| `UpdateCard.tsx` | 7 hunks | Update notifications: `Orca v{{value0}} is ready` → `Fabrica v{{value0}} is ready`, domains |
| `Landing.tsx` | 5 hunks | GitHub URL `stablyai/orca`→`Auto-Scalers/Fabrica-app`, API method `starOrca`→`starFABRICA` |
| `StarNagCard.tsx` | 4 hunks | Same GitHub URL + API renames |
| `NewWorkspaceComposerCard.tsx` | 5 hunks | `orca.yaml`→`FABRICA.yaml` in labels |
| `CombinedDiffViewer.tsx` | 4 hunks | `ORCA_EDITOR_EXTERNAL_FILE_CHANGE_EVENT`→`FABRICA_EDITOR_EXTERNAL_FILE_CHANGE_EVENT` |
| `LinuxPackageInstallRecoveryCard.tsx` | 4 hunks | `Orca`→`Fabrica` in user-facing messages |

### Shared: File Renames (9 files)

| Baseline | Fabrica | Key Identifiers Renamed |
|----------|---------|------------------------|
| `orca-attribution.ts` | `fabrica-attribution.ts` | `FABRICA_GIT_COMMIT_TRAILER`, email `fabrica.studio.contact@gmail.com` |
| `orca-cli-command-name.ts` | `fabrica-cli-command-name.ts` | `getFABRICACliCommandNameForPlatform`, linux `fabrica` |
| `orca-dispatch-status-prompt.ts` | `fabrica-dispatch-status-prompt.ts` | 9 `FABRICA_DISPATCH_STATUS_*` identifiers |
| `orca-profiles.ts` | `fabrica-profiles.ts` | `OrcaProfile*`→`FABRICAProfile*`, `OrcaCloud*`→`FABRICACloud*` |
| `orca-yaml.ts` | `fabrica-yaml.ts` | `OrcaVmRecipe`→`FABRICAVmRecipe`, `parseOrcaYaml`→`parseFABRICAYaml` |
| `orca-yaml-bounds.test.ts` | `fabrica-yaml-bounds.test.ts` | Test references |
| `orca-yaml-alias-bounds.test.ts` | `fabrica-yaml-alias-bounds.test.ts` | Test references |
| `orca-yaml-file-limit.ts` | `fabrica-yaml-file-limit.ts` | `isFABRICAYamlFieldWithinLimit`, `MAX_FABRICA_YAML_*` |
| `telemetry-orca-cli-feature-tip.test.ts` | `telemetry-fabrica-cli-feature-tip.test.ts` | Test references |

### Shared: New File (Fabrica-only)

`supabase-auth.ts` — 20 lines. Exports `SupabaseAuthStatus`, `SignInSupabaseArgs`, `SignInSupabaseResult`, `SignOutSupabaseResult`. **Must be preserved verbatim.**

### Shared: Core Identity Changes

| File | Key Changes |
|------|-------------|
| `constants.ts` | `DEFAULT_APP_FONT_FAMILY = 'Inter'` (was Geist), `FABRICA_BROWSER_PARTITION`, workspace dir `Fabrica`, `trustedFABRICAHooks` |
| `types.ts` | `FABRICACreatedAt`, `FABRICAWorkspaceLayout`, `'FABRICA-managed'` ownership literal |
| `app-icon.ts` | Icon set redesigned (watercolor/blue→dark/light), default changed to `'light'` |
| `release-channel.ts` | GitHub repos `Auto-Scalers/Fabrica-hourly`/`daily`/`adhoc`/`Fabrica-app` |
| `telemetry-events.ts` | Schema names `appStarredFABRICASchema`, `FABRICACliFeatureTipSourceSchema` |

### Shared: Agent/Runtime/Infrastructure (High-Count Files)

| File | Ref Count | Pattern |
|------|-----------|---------|
| `worktree-ownership.ts` | 28 | `FABRICA-managed` string literal |
| `remote-runtime-client.ts` | 29 | `fabrica` connection identifiers |
| `wsl-login-shell-command.ts` | 24 | `fabrica` WSL path/command strings |
| `agent-hook-relay.ts` | 22 | `fabrica` hook relay identifiers |
| `posix-command-path-lookup.ts` | 21 | `fabrica` CLI path lookups |

---

## Per-Area Section: `src/preload/`, `src/relay/`, `src/types/`, `src/renderer/src/assets/`, `mobile/`, `native/`

**Total files:** ~285
**Change count:** ~400+ hunks
**Intent breakdown:** 94% rebrand, 4% custom-logic (Supabase auth, EAS config, CSS additions), 2% incidental

### src/preload/ (9 files differ)

| File | Intent | Key Pattern |
|------|--------|-------------|
| `api-types.ts` | rebrand + custom-logic | 26 type renames (`OrcaProfile*`→`FABRICAProfile*`), `FABRICAHooks`, new `supabaseAuth` block |
| `index.ts` | rebrand + custom-logic | 16 IPC channels renamed, `checkFABRICAStarred`/`starFABRICA`, new `supabaseAuth` IPC |
| `e2e-config.ts` | rebrand | 4 `FABRICA_E2E_*` env vars |
| `renderer-restart-wiring.ts` | rebrand | 3 event constant references |
| Other test files | rebrand | Mechanical `ORCA_`→`FABRICA_` in test strings |

### src/relay/ (59 files differ)

**Zero leftover `ORCA_` or `orca` in Fabrica-app — rebrand complete.**

| Category | Pattern |
|----------|---------|
| Protocol wire name | `ORCA-RELAY v0.1.0 READY` → `FABRICA-RELAY v0.1.0 READY` |
| JSON-RPC methods | `orca.cli`, `orca.cli.postOutput` → `FABRICA.cli`, `FABRICA.cli.postOutput` |
| HTTP headers | `x-orca-agent-hook-token` → `x-fabrica-agent-hook-token` |
| Env vars | 30+ `ORCA_*`→`FABRICA_*` |
| File/directory paths | `.orca-relay/`→`.FABRICA-relay/`, `.orca/sessions/`→`.FABRICA/sessions/` |
| Function/variable renames | `runOrcaCliMode()`→`runFABRICACliMode()`, bash functions `__orca_osc133_*`→`__FABRICA_osc133_*` |
| Terminal branding | `TERM_PROGRAM: 'Orca'`→`'Fabrica'`, shell ready markers |

### src/types/ (1 file differs)

`build-constants.d.ts`: `FABRICA_BUILD_IDENTITY`, `FABRICA_POSTHOG_WRITE_KEY`, `FABRICA_DIAGNOSTICS_TOKEN_URL`

### src/renderer/src/assets/ (8 files differ)

| File | Intent | Key Pattern |
|------|--------|-------------|
| `main.css` | rebrand + custom-logic | Geist→Inter font, `--orca-security-*`→`--fabrica-security-*` (20+ vars), `.orca-*`→`.FABRICA-*` classes (~50). **Custom:** ~192 lines new CSS (workspace kanban, agent lineage, settings-shell, feature wall, etc.) |
| `terminal.css` | rebrand | `--orca-*`→`--fabrica-*` CSS vars, subtle color adjustments |
| `markdown-preview.css` | rebrand | `.orca-details`→`.FABRICA-details`, `data-orca-toggle`→`data-FABRICA-toggle` |
| `rich-markdown-editor.css` | rebrand + custom-logic | Font family, class renames. **Custom:** review rails, inline search/replace, table controls, mermaid preview |
| `mobile-page.css` | rebrand + custom-logic | Logo class rename. **Custom:** mobile page UI components |
| Font files | rebrand | Removed: `Geist-Variable.woff2`, `SymbolsNerdFontMono-Regular.woff2`. Added: `FabricaNerdFontSymbols-Regular.woff2`, Inter/JetBrainsMono/SpaceGrotesk variants |

### mobile/ (5 key files)

| File | Intent | Key Pattern |
|------|--------|-------------|
| `app.json` | rebrand + custom-logic | `expo.name`→`"Fabrica"`, slug `fabrica-mobile`, scheme `fabrica`, bundle IDs `com.autoscalers.fabrica.mobile`, permission strings. **Custom:** EAS config (`extra.eas.projectId`, `owner: "auto-scalers"`), added `android.permission.CAMERA` |
| `package.json` | rebrand | `name: "fabrica-mobile"`, `@fabrica/expo-two-way-audio` |
| `packages/expo-two-way-audio/package.json` | rebrand | `@fabrica/` scope, GitHub org `Auto-Scalers`, `FABRICA contributors` |
| `eas.json` | custom-logic | New EAS Build configuration (Fabrica-only) |

### native/ (6 files)

| File | Intent | Key Pattern |
|------|--------|-------------|
| `Package.swift` | rebrand | `FabricaComputerUseMacOS`/`FabricaComputerUseMacOSCore` targets |
| `main.swift` (macOS) | rebrand | Provider `fabrica-computer-use-macos`, bundle ID `ai.autoscalers.fabrica`, all UI strings |
| `runtime.py` (Linux) | rebrand | `fabrica-computer-use-linux` provider |
| `runtime.ps1` (Windows) | rebrand | 50+ function renames (`*Orca*`→`*Fabrica*`), `FabricaDesktopWin32` class |
| `FabricaCliLauncher.cs` | rebrand | Class `FabricaCliLauncher`, `Fabrica.exe`, env vars |
| `PermissionStatusSnapshot.swift` | rebrand | Bundle ID labels `ai.autoscalers.fabrica.computer-use-*` |

---

## Custom Logic Inventory

Everything Fabrica built that Orca never had. **Must be preserved in every sync.**

| # | Feature | Source Area | Description |
|---|---------|-------------|-------------|
| 1 | **Supabase auth integration** | electron.vite.config.ts, preload/api-types.ts, preload/index.ts, shared/supabase-auth.ts | `SUPABASE_URL`/`SUPABASE_ANON_KEY` compile-time injection, Supabase auth types, IPC channels (`supabaseAuth:getStatus`, `signIn`, `signOut`) |
| 2 | **Plugin marketplace loader** | Fabrica-plugins/ | Git-clone + cache + bootstrap + discovery + IPC (not in Orca) |
| 3 | **Plugin update mechanism** | Fabrica-plugins/ | Version checking via content hashes, install with rollback |
| 4 | **Cloudflare Workers relay server** | Fabrica-relay/ | E2EE WebSocket bridge (CF Workers + Durable Objects + Hono + Supabase auth) |
| 5 | **Backend API routes** | Fabrica-web/app/api/v1/desktop/ | 10 routes: auth/share/diagnostics/changelog on Vercel |
| 6 | **Plugin content hash verification** | Plugin repos | SHA-256 re-hash compare for integrity |
| 7 | **Plugin install trust verification** | Plugin repos | Reserved official identities must come from Auto-Scalers org |
| 8 | **Plugin marketplace provenance** | Plugin repos | Official marketplace owner validation |
| 9 | **Kill-list management** | Plugin repos | JSON document with id/name/reason/severity/blockedVersions |
| 10 | **Plugin review process** | Plugin repos | 3 gates: automated → maintainer → listing decision |
| 11 | **Plugin submission guidelines** | Plugin repos | Fork → add entry → PR → review → merge |
| 12 | **Plugin validation rules** | Plugin repos | Index entry rules + manifest rules + rename compliance |
| 13 | **Plugin signing research** | Plugin repos | Hash-addressed + namespace-trust model |
| 14 | **OKLCH visual palette** | main.css | 160 oklch color token references from Fabrica-web |
| 15 | **WCAG AA contrast fixes** | main.css, terminal.css, mobile-page.css | Token-level OKLCH adjustments |
| 16 | **Colored icon/logo variants** | app-icon.ts, assets | Dark/light brand variants, `DEFAULT_APP_ICON_ID='light'` |
| 17 | **Non-technical UI copy** | en.json, es.json | Rewrote settings descriptions to drop jargon |
| 18 | **Font/zoom defaults** | constants.ts | `uiZoomLevel` bumped 0→0.5 on first launch |
| 19 | **Remote factory namespace** | hook-service.ts files (11) | `~/.factory`→`~/.fabrica-factory` |
| 20 | **Skills prefix** | Skill dirs + constants | Lowercase `fabrica-` prefix |
| 21 | **Marketplace owner validation** | fabrica-marketplace.json | `owner: "auto-scalers"` must match `OFFICIAL_MARKETPLACE_OWNER` |
| 22 | **CJK locale en-fallback** | ko.json, ja.json, zh.json | Replaced corrupted catalogs with en.json copies |

---

## Debrand Cleanup Inventory

Orca-only artifacts removed from Fabrica. **Always keep removed in sync.**

| # | What | Why Removed | Files Affected |
|---|------|-------------|----------------|
| 1 | Orca profile system files | Fabrica uses Supabase auth, not Orca cloud profiles | 5 files: `orca-profiles.ts`, `orca-profile-org-members-handlers.ts` + tests |
| 2 | GNOME screen reader warnings | CLI is `fabrica`, no naming conflict with GNOME Orca | 14 files: skill-guides, agent-launch-remote.ts, tui-agent-config.ts |
| 3 | Historical GitHub issue URLs | `stablyai/orca#9902`, `stablyai/orca#5049` Orca-internal | docs/findings |
| 4 | Legacy CLI type variants | `'orca' \| 'orca-ide' \| 'orca-dev'` no longer valid | coordinator.ts, fabrica-runtime.ts, 9 test mocks |
| 5 | `FABRICA-browser` enum value | Broken-case artifact from rebrand | preferences.ts (migrated to `'fabrica-browser'`) |
| 6 | `com.autoscalers.fabrica` appId (interim) | Was intermediate; final is `ai.autoscalers.fabrica` | 20 source files (migrated) |
| 7 | `onfabrica.dev` / `onorca.dev` domains | Dead DNS | All endpoint references (repointed to `fabrica-ai.vercel.app`) |
| 8 | `onFABRICA.dev` domain variant | Dead DNS | Feature-wall docs, kill-list, telemetry, feedback |
| 9 | `stablyai` GitHub org references | Old org | CI workflows, source, docs, READMEs |
| 10 | `stablyai/homebrew-orca` cask repo | Old org | `homebrew-bump.yml` |
| 11 | `@stablyai/playwright-test` npm scope | Old scope | 268 imports (forked to `@autoscalers`) |
| 12 | `@orca/expo-two-way-audio` npm scope | Old scope | mobile README |
| 13 | `ErrFABRICArd` (rebrand corruption) | Accidental corruption during rebrand | 4 files (fixed) |
| 14 | `UNSUPPORTED_MARKETPLACE_CATEGORIES` filter | Orca had categories filter; Fabrica shows all | Marketplace code |

---

## Sync Implications

### Preservation Rules (what must NOT change on sync)

1. All `Fabrica`/`FABRICA` identifiers must be preserved — any upstream Orca identifier update must be adapted to our pattern.
2. `fabrica-ai.vercel.app` domain must replace any `onorca.dev` references from upstream.
3. `Auto-Scalers/Fabrica-app` GitHub org+repo must replace any `stablyai/orca` references.
4. `Inter` font family must replace any `Geist`/`Geist Mono` from upstream.
5. `Fabrica Nerd Font Symbols` must replace any `Orca Nerd Font Symbols`.
6. `--fabrica-*` CSS variables must replace any `--orca-*`.
7. `FABRICA.yaml` must replace any `orca.yaml` references.
8. `FABRICA-data.json` must replace any `orca-data.json`.
9. `fabrica.studio.contact@gmail.com` commit trailer must replace `help@stably.ai`.
10. `fabrica` CLI command must replace `orca`/`orca-ide`.
11. Dark/light icon set must replace watercolor/blue — any new upstream icon must be adapted.
12. `supabase-auth.ts` and all Supabase auth infrastructure must be preserved (Fabrica-only).
13. `'FABRICA-managed'` ownership literal must be preserved.
14. `'FABRICA-first'` shortcut policy must be preserved.
15. All 22 custom logic features must be preserved verbatim.

### Risk Areas for Sync

| Risk | Area | Mitigation |
|------|------|------------|
| Large type rename surface | types.ts, fabrica-profiles.ts | Upstream type additions must be renamed before merge |
| Agent/runtime identifiers | hook/relay/VM recipe files | Upstream changes must be adapted with fabrica prefix |
| Telemetry schemas | telemetry-events.ts | Upstream schema additions must be renamed |
| Plugin manifests | Plugin repos | Upstream plugin changes must use `fabrica` identifiers |
| Wire protocol | relay/, SSH | Protocol markers must match between client and server |
| Both-sides rule | All areas | When two parts share an identifier, both must rename in same release |

### What Can Be Ported Verbatim

Internal logic changes that do **not** touch Orca identifiers (env vars, domains, URLs, string literals containing "Orca", bundle IDs, etc.) are safe to port without modification.

### What Must Be Ported + Rebranded

Any new `ORCA_*` env vars, `onorca.dev` URLs, `orca_*` wire tokens, `Orca` string references, `stablyai` org references, or `com.stablyai.orca` bundle IDs from upstream must be adapted to Fabrica equivalents.

### What Requires Merge Decision

Changes to the orchestration CLI command type (upstream may add new variants beyond `'orca' | 'orca-ide'`), and any changes overlapping our custom logic features (Supabase auth, plugin marketplace, relay server).

---

## Canonical Identity Reference

| Platform | Value |
|----------|-------|
| electron-builder appId | `ai.autoscalers.fabrica` |
| macOS CFBundleIdentifier | `ai.autoscalers.fabrica` |
| macOS helper | `ai.autoscalers.fabrica.computer-use` |
| Windows AUMID | `ai.autoscalers.fabrica` |
| iOS bundle ID | `com.autoscalers.fabrica.mobile` |
| Android package | `com.autoscalers.fabrica.mobile` |
| Deep link protocol | `fabrica://` |
| GitHub org | `Auto-Scalers` |
| Main repo | `Fabrica-app` |
| Landing page | `fabrica-ai.vercel.app` |
| Relay | `fabrica-relay.fabrica-relay.workers.dev` |
| Support email | `fabrica.studio.contact@gmail.com` |
| Commit trailer | `Co-authored-by: Fabrica <fabrica.studio.contact@gmail.com>` |

---

_Map generated 2026-08-30 from T1-A through T1-D findings + T1-INTENT-REFERENCE.md._

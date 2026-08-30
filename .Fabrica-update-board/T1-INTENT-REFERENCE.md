# T1 — Intent-Tag Reference

> Extracted from all historical task files in `.Fabrica-update-board/historical/`.
> Purpose: future upstream Orca ports can check each changed identifier against this
> reference to decide whether to keep our rename, adapt, or preserve upstream's value.

---

## How to Use This Reference

When porting an upstream Orca change into Fabrica-app:

1. **Find the changed identifier** in the upstream diff.
2. **Look it up below** — check which category it falls into.
3. **Apply the rule:**
   - **REBRAND** → always apply our rename (Orca→Fabrica). The upstream value is wrong for us.
   - **CUSTOM LOGIC** → always keep our version. Upstream never had this; it's ours.
   - **DEBRAND CLEANUP** → always keep our removal. Upstream's version is dead/irrelevant.
   - **INCIDENTAL** → take the upstream change if it's a genuine improvement; skip if noise.

---

## 1. REBRAND Changes

Every Orca-to-Fabrica rename. These are identity-level — we must always use our
version, never revert to Orca's.

### 1.1 App Identity & Display

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| App name / productName | `Orca` | `Fabrica` | Brand | `electron-builder.config.cjs:94`, window title, About panel, tray, notifications | `productName:'Fabrica'` everywhere; drives paths/titles |
| Firewall rule display | `Orca Mobile Pairing` | `Fabrica Mobile Pairing` | Brand | `windows-mobile-firewall.ts:11` | `FIREWALL_RULE_DISPLAY_NAME = 'Fabrica Mobile Pairing'` |
| Computer Use helper | `Orca Computer Use.app` | `Fabrica Computer Use.app` | Brand | packaging, signing, permission-detection | String rename across codebase |
| Attribution footer | `Made with Orca` | `Made with Fabrica` | Brand | `terminal-attribution.ts:12-13` | `Made with [FABRICA]` + link to Fabrica-app repo |
| Git co-author trailer | `Co-authored-by: Orca <help@stably.ai>` | `Co-authored-by: Fabrica <fabrica.studio.contact@gmail.com>` | Brand | `fabrica-attribution.ts:6` | `FABRICA_GIT_COMMIT_TRAILER` constant |

### 1.2 CLI & Install Paths

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| CLI command | `orca` | `fabrica` | Brand | `package.json:7-9` (bin), CLI source | `bin: "fabrica"` in package.json |
| Install paths | `Program Files\Orca Dev` | `Fabrica Dev` | Brand | driven by productName | productName Fabrica drives all paths automatically |
| Environment variables | `ORCA_*` | `FABRICA_*` | Brand | all env-var sites | `process.env.FABRICA_*` (PostHog, Diagnostics, Build Identity, Product URL, Git Trailer) |

### 1.3 Runtime Identity & Protocol

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| Wire tokens | `orca_server_ready` | `fabrica_server_ready` | Runtime identity | protocol files, tests | Zero remaining Orca occurrences |
| Deep link protocol | `orca://` | `fabrica://` | Runtime identity | `pairing.ts:74`, `web-pairing.ts:26`, tests | Functional deep-link rename |
| TLS certificate CN | `CN=Orca Runtime` | `CN=Fabrica Runtime` | Runtime identity | TLS setup files | `CN=Fabrica Runtime` |
| Keychain service | (Orca's name) | `Fabrica Claude Code Managed Credentials` | Runtime identity | keychain access | Service name string |
| Data directories | `~/.config/orca` | `~/.config/fabrica` | Runtime identity | `electron-builder.config.cjs:94`, `src/cli/runtime/metadata.ts:53-69` | Driven by productName; dev override `fabrica-dev` |
| Relay wire protocol | `ORCA-RELAY` | `FABRICA-RELAY` | Runtime identity | `protocol.ts`, `relay-handshake.ts`, `relay-protocol.ts`, 8 test files (35 matches) | String literal rename |
| Mobile E2EE protocol | `orca-mobile-e2ee` | `fabrica-mobile-e2ee` | Runtime identity | 4 files: contract, fixtures, test | Protocol name rename |
| Legacy CLI type | `'orca' | 'orca-ide' | 'orca-dev'` variants | `'fabrica'` only | Simplified `OrchestrationCliCommand` type; 9 test mocks updated |

### 1.4 App ID

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| App ID (PM D1) | `com.stablyai.orca` | `ai.autoscalers.fabrica` | Identity | `electron-builder.config.cjs:49`, 20 source files, 6 tests, 2 JSON contracts, 3 Swift files, 2 casks, 1 diagnostics script, 3 config scripts | All `com.autoscalers.fabrica` → `ai.autoscalers.fabrica`; grep 0 hits post-migration |

### 1.5 Backend Endpoints & Domains

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| Auth | `login.onorca.dev` | `fabrica-ai.vercel.app/api/auth/*` | Domain | `profile-cloud-auth-config.ts:19` | `PRODUCTION_API_BASE_URL='https://fabrica-ai.vercel.app'` |
| Relay | `relay.onorca.dev` | `fabrica-relay.fabrica-relay.workers.dev` | Domain | Cloudflare Workers deployment | Live on CF Workers + Durable Objects |
| Share | `share.onorca.dev` | `fabrica-ai.vercel.app/api/share/*` | Domain | `artifact-cloud-config.ts:3` | `PRODUCTION_ARTIFACTS_API_URL='https://fabrica-ai.vercel.app'` |
| Diagnostics | `www.onorca.dev/diagnostics/token` | `fabrica-ai.vercel.app/api/diagnostics/token` | Domain | 4 mac-build workflows, `release-cut.yml:1147` | `FABRICA_DIAGNOSTICS_TOKEN_URL` |
| Changelog | `onorca.dev/whats-new/changelog.json` | `fabrica-ai.vercel.app/whats-new/changelog.json` | Domain | `updater-changelog.ts:13`, `updater-nudge.ts:12` | `CHANGELOG_URL` + fetch URLs |
| Plugin kill-list | `onorca.dev/plugins/kill-list.json` | `fabrica-ai.vercel.app/plugins/kill-list.json` | Domain | `plugin-kill-list-service.ts:10` | `PLUGIN_KILL_LIST_URL` |
| Docs | `www.onorca.dev/docs` | `fabrica-ai.vercel.app/docs` | Domain | docs site | Full docs rebrand |
| Feature wall URLs | `www.onFABRICA.dev/docs/*` | `fabrica-ai.vercel.app/docs/*` | Domain | 12 tile + 5 workflow `docsUrls` | All repointed to fabrica-ai.vercel.app |
| Telemetry PRIVACY_URL | (orca domain) | `https://fabrica-ai.vercel.app` | Domain | telemetry files | Repointed |
| Feedback API | (orca domain) | `https://fabrica-ai.vercel.app` | Domain | feedback files | Repointed |
| Artifact-cloud default origin | (orca domain) | `https://fabrica-ai.vercel.app` | Domain | artifact-cloud files | + firstParty allowlist |
| Profile-cloud auth base | (orca domain) | `https://fabrica-ai.vercel.app` | Domain | profile-cloud files | Auth base repointed |

### 1.6 GitHub Org & Repo References

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| GitHub org + repo | `stablyai/orca` | `Auto-Scalers/Fabrica-app` | Org | CI workflows, source code, docs, READMEs | All refs updated |
| CI workflow repos | `stablyai/fabrica-*` | `Auto-Scalers/fabrica-*` | Org | 8 CI workflows (hourly, daily, adhoc, release-cut, release-mac-build, release-policy, readme-downloads-badge, homebrew-bump) | `HOURLY_REPO=Auto-Scalers/fabrica-hourly`, etc. |
| Homebrew cask repos | `stablyai/homebrew-orca` | (dropped) | Org | `homebrew-bump.yml` | Removed |

### 1.7 i18n & Locale Files

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| Locale strings | Orca/onorca/stablyai refs | Fabrica refs | Brand | `en.json`, `ko.json`, `ja.json`, `zh.json`, `es.json`, `pt-BR` plugin locale | 0 Orca occurrences in en.json verified; all 5 locales clean |
| CJK catalogs (APP-E5) | Corrupted ko/ja/zh (7K-10K ?-run lines each) | en.json copies as placeholders | Corruption | `ko.json`, `ja.json`, `zh.json` | SHA256-identical to en.json (12,411 leaf keys); mark for professional translation before launch |

### 1.8 Homebrew Casks

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| Cask homepage | `onfabrica.dev` / `onorca.dev` | `fabrica-ai.vercel.app` | Domain | `Casks/fabrica.rb:12`, `Casks/fabrica@rc.rb:12` | `homepage='https://fabrica-ai.vercel.app'` |
| Cask name | `Orca` | `Fabrica` | Brand | same files | `name='Fabrica'` |

### 1.9 Localized READMEs & Docs

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| 5 READMEs | `onorca.dev`, `stablyai/orca` | `fabrica-ai.vercel.app`, `Auto-Scalers/Fabrica-app` | Brand | `README.zh-CN.md`, `README.pt.md`, `README.ko.md`, `README.ja.md`, `README.fr.md` | `rg -i -e 'orca|stablyai|onorca'` = 0 hits each |
| CONTRIBUTING.md | `stablyai/orca` | `Auto-Scalers/Fabrica-app` | Brand | `.github/CONTRIBUTING.md` | 0 Orca refs |
| WINDOWS_SETUP_GUIDE.md | Orca references | Fabrica references | Brand | `WINDOWS_SETUP_GUIDE.md` | 0 Orca refs |

### 1.10 Plugin Ecosystem Renames

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| Plugin manifest engine | `engines.orca` | `engines.fabrica` | Brand | all plugin manifests | All bundled plugin manifests use `engines.fabrica >=1.4.0` |
| Plugin publisher | `stablyai` | `autoscalers` | Brand | marketplace index, manifests | `publisher: 'autoscalers'` |
| Plugin ID prefix | `orca-*` | `fabrica-*` | Brand | plugin IDs in manifests + marketplace | `fabrica-*` naming for Auto-Scalers plugins |
| Marketplace index file | `marketplace-index.json` | `fabrica-marketplace.json` | Brand | Fabrica-plugins repo | Renamed |
| Plugin manifest file | `orca-plugin.json` | `fabrica-plugin.json` | Brand | plugin repos | Renamed |
| Plugin kill-list URL | `onorca.dev` | `fabrica-ai.vercel.app` | Domain | `plugin-kill-list-service.ts:10` | Live 200 OK verified |
| Content hash prefix | `orca-plugin-tree-v1` | `fabrica-plugin-tree-v1` | Brand | `plugin-content-hash.ts:99` | Recorded hash matches independent recompute |
| OFFICIAL_MARKETPLACE_OWNER | `stablyai` | `auto-scalers` | Brand | `plugin-marketplace.ts:11` | Must match `fabrica-marketplace.json` owner field |
| Plugin GitHub repos | `stablyai/orca-*` | `Auto-Scalers/fabrica-*` | Brand | 8 plugin repos | Created as submodules in Fabrica-plugins/ |

### 1.11 npm Packages

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| @stablyai/playwright-test | `@stablyai/playwright-test` | `@autoscalers/playwright-test` | Brand | 268 import references, package.json | 3 packages published: @autoscalers/playwright-base@2.1.14, @autoscalers/playwright@2.1.14, @autoscalers/playwright-test@2.1.14 |
| @orca/expo-two-way-audio | `@orca/expo-two-way-audio` | `@fabrica/expo-two-way-audio` | Brand | mobile README (2 lines) | Scoped rename |

### 1.12 Visual Palette

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| CSS color tokens | Orca palette | OKLCH tokens from Fabrica-web | Visual brand | `main.css` (160 oklch refs) | Source-of-truth: Fabrica-web landing page |
| Theme identity | Orca look | Fabrica look | Visual brand | CSS variables | "When someone looks at the new Fabrica app, they should not recognize it was built on top of Orca" |

### 1.13 Enum Casing Fix (APP-E3)

| What | Orca Value | Fabrica Value | Why | Files Touched | Our Pattern |
|------|-----------|---------------|-----|--------------|-------------|
| Enum casing | `'FABRICA-browser'` (broken-case artifact) | `'fabrica-browser'` | Broken artifact from rebrand | `preferences.ts:195,198`, `session/[worktreeId].tsx:927`, `browser-settings.tsx:16,36` | Migration maps legacy `'FABRICA-browser'` + pre-rebrand `'orca-browser'` → `'fabrica-browser'` |

---

## 2. CUSTOM LOGIC

Features/fixes we added that Orca never had. These are ours — always keep.

| What | Why Added | Files Touched | Our Pattern |
|------|-----------|--------------|-------------|
| Plugin marketplace loader (APP-D6) | Orca had no marketplace; we built Git-clone + cache + bootstrap + discovery + IPC | plugin loader files, IPC handlers, startup wiring | Full end-to-end: marketplace fetches via Git clone, caches snapshots, bundles bootstrap, discovery finds, IPC registered |
| Plugin update mechanism (APP-D7) | Orca had no update mechanism | `previewMarketplaceUpdate`, install/rollback IPC | Version checking via content hashes, "Check for update" button, install with rollback |
| Cloudflare Workers relay server | Orca had no relay; we built E2EE WebSocket bridge | `Fabrica-relay/` (30+ tasks) | CF Workers + Durable Objects + Hono + Supabase auth, $0/mo |
| Supabase auth integration | Added Supabase session for relay auth | `supabase-session.ts`, `supabase-auth.ts` | Prefers Supabase access token, falls back to Fabrica Cloud relayToken |
| Backend API routes (`/v1/desktop/*`) | Orca had no web backend; we built auth/share/diagnostics/changelog | `Fabrica-web/app/api/v1/desktop/` (10 routes) | Deployed to Vercel + env (Supabase keys + FABRICA_RELAY_JWT_SECRET) |
| Plugin content hash verification | Orca had no content-hash integrity | `plugin-content-integrity.ts` | `verifyHashAddressedPluginContent()` — SHA-256 re-hash compare |
| Plugin install trust verification | Orca had no namespace trust | `plugin-install-trust.ts` | Reserved official identities (`autoscalers.fabrica-*`) must come from Auto-Scalers org |
| Plugin marketplace provenance | Orca had no provenance validation | `plugin-marketplace-provenance.ts` | Official marketplace must have owner `auto-scalers`; reserved IDs resolve to official org |
| Kill-list management (P7) | Orca had no kill-list | `kill-list.json`, `plugin-kill-list-service.ts` | JSON document with id/name/reason/severity/blockedVersions/replacement/blockedAt |
| Plugin review process (P6) | Orca had no review process | Review docs + CI | 3 gates: automated validation → maintainer review → listing decision |
| Plugin submission guidelines (P3) | Orca had no submission process | Submission docs | Fork → add entry → PR → review → merge |
| Plugin validation rules (P5) | Orca had no validation rules | Validation docs + CI | Index entry rules + manifest rules + rename compliance checks |
| Plugin signing research (P8) | Orca never signed plugins | Research doc | Hash-addressed + namespace-trust model (no CA needed); code signing reserved for app binary only |
| OKLCH visual palette migration | Branded Orca → Fabrica visual identity | `main.css` (160 oklch refs) | Tokens from Fabrica-web landing page applied |
| WCAG AA contrast fixes (APP-G1) | Unreadable text from Orca palette | `main.css`, `terminal.css`, `mobile-page.css` | Token-level OKLCH adjustments (light + dark + security chrome) |
| Colored icon/logo variants (APP-G3) | Orca had monochrome emblems only | `app_icon_dark.png`, `app_icon_light.png`, `logo.svg`, titlebar/landing/settings/sidebar/onboarding | Dark/light brand variants + colored in-app logo.svg; `DEFAULT_APP_ICON_ID='light'` |
| Non-technical UI copy (APP-G7) | Orca used developer jargon | `en.json` (10 keys), `es.json` (7 keys) | Rewrote settings descriptions to drop jargon (scrollback/modifier/TUI → plain phrasing) |
| Font/zoom defaults (APP-G7) | Orca started at 100% zoom | `constants.ts:488`, `ui.ts:2397`, `startup-ui-hydration.ts:58` | `uiZoomLevel` initial default bumped 0→0.5 (slightly larger on first launch) |
| Remote factory namespace (APP-G2-FIX) | Shared `~/.factory` with Orca | 11 hook-service.ts files | `~/.factory` → `~/.fabrica-factory` across all remote hook services |
| Skills prefix (APP-G2-FIX) | Shared skill names with Orca | 8 skill dirs + constants + guides + manifest | Lowercase `fabrica-` prefix (never `Fabrica-`, never double `fabrica-fabrica`) |
| Marketplace owner validation (APP-G5-FIX) | Owner mismatch blocked plugin listing | `fabrica-marketplace.json` + regression test | `owner: "auto-scalers"` must match `OFFICIAL_MARKETPLACE_OWNER` |
| CJK locale en-fallback (APP-E5) | ko/ja/zh catalogs corrupted pre-repo | `ko.json`, `ja.json`, `zh.json` | Replaced with en.json copies (SHA256-identical); 6 locale test suites updated |
| GNOME warning cleanup (APP-E7a) | Stale Orca screen reader warnings | 14 files: skill-guides, agent-launch-remote.ts, tui-agent-config.ts | Removed clash clauses (CLI is `fabrica`, not `orca`, no conflict) |
| Historical URL cleanup (APP-E7bc) | Stale GitHub issue URLs | docs/findings files | Deleted `stablyai/orca#9902`, `stablyai/orca#5049` |
| Backward-compat fixture cleanup (APP-E7c) | Stale Orca fixtures | test fixtures | `StablyAI/FABRICA` → `Auto-Scalers/Fabrica-app` (0 users, no compat needed) |

---

## 3. DEBRAND CLEANUP

Orca-only config/scripts we removed. These are dead in our context — always keep removed.

| What | Why Removed | Files Touched | Our Pattern |
|------|-------------|--------------|-------------|
| Stale GNOME screen reader warnings | CLI is `fabrica`, not `orca`, no naming conflict with GNOME Orca | 14 files (skill-guides, agent-launch-remote.ts, tui-agent-config.ts) | Removed; 3 remaining GNOME refs are unrelated (virtual-FS test, gnome-terminal doc, workspace-shortcut comment) — correct to keep |
| Historical GitHub issue URLs | `stablyai/orca#9902`, `stablyai/orca#5049` are Orca-internal | docs/findings | Deleted; no Fabrica relevance |
| Legacy CLI type variants | `'orca' | 'orca-ide' | 'orca-dev'` no longer valid | coordinator.ts, fabrica-runtime.ts, 9 test mocks | Simplified to `'fabrica'` only |
| `FABRICA-browser` enum value | Broken-case artifact from rebrand (should be lowercase) | preferences.ts | Migrated to `'fabrica-browser'` with load-time migration for stored prefs |
| `com.autoscalers.fabrica` appId (interim) | Was intermediate; final is `ai.autoscalers.fabrica` | 20 source files | Migrated to `ai.autoscalers.fabrica` (PM decision D1) |
| `onfabrica.dev` / `onorca.dev` domains | Dead DNS — all client references fail | all endpoint references | Repointed to `fabrica-ai.vercel.app` (live) |
| `onFABRICA.dev` domain variant | Dead DNS (same family) | feature-wall docs URLs, kill-list URL, telemetry, feedback, artifact-cloud, profile-cloud | All repointed to `fabrica-ai.vercel.app` |
| `stablyai` GitHub org references | Old org | CI workflows, source, docs, READMEs | All → `Auto-Scalers` |
| `stablyai/homebrew-orca` cask repo | Old org | `homebrew-bump.yml` | Removed |
| `@stablyai/playwright-test` npm scope | Old scope | 268 imports | Forked to `@autoscalers/playwright-test` + 2 companion packages |
| `@orca/expo-two-way-audio` npm scope | Old scope | mobile README | Renamed to `@fabrica/expo-two-way-audio` |
| `ErrFABRICArd` (rebrand corruption) | Accidental corruption during rebrand | 4 files | Fixed back to `ErrorCard` |
| CP1252 encoding files | Fixed during rebrand | 56 files | Recoded to UTF-8 |
| Stale `out/bin/orca*.cmd` build artifacts | Old build output | `out/bin/` | Recommend deletion at commit time |
| `UNSUPPORTED_MARKETPLACE_CATEGORIES` filter | Orca had categories filter; we show all | marketplace code | Removed — show all plugins like Orca |

---

## 4. INCIDENTAL

Whitespace, dep bumps, test fixture normalization, etc. — not identity-affecting.

| What | Why | Files Touched | Our Pattern |
|------|-----|--------------|-------------|
| Dep bumps (pnpm) | Standard maintenance | package.json, lockfiles | Take upstream if compatible; verify no rebrand regressions |
| Whitespace / line-ending normalization | CRLF issues on Windows | Various | CRLF helper implemented (24/24 tests); keep normalized |
| UTF-8 encoding fixes | CP1252→UTF-8 recode | 56 files | All files must be strict UTF-8 |
| Test fixture normalization | Match new brand in test assertions | test files | Update assertions to expect Fabrica values |
| Snapshot regeneration | Reflect new brand in snapshots | `__snapshots__/` | Regenerate after rebrand; verify legitimacy |
| `as const` lint fix | One-liner to satisfy oxlint | single file | Take if upstream has it |
| Mobile locale test updates | Match D8 fallback reality | 6 locale test suites | Tests updated for en-fallback in ko/ja/zh |
| BOM removal | Byte-order marks in 174 files | Various | Zero BOMs verified (R5 VERIFY-BOM-R3) |
| `mock-server-key-pair` ESM fix | ERR_MODULE_NOT_FOUND for tsx | test file | Fixed; pre-existing environmental issue |
| Pre-existing test failures (448 desktop) | POSIX /bin/sh ENOENT, macOS-only APIs, CRLF, CJK encoding, watcher infra | test files | Baseline residual — not rebrand-caused; 0 new failures |

---

## 5. Key Constraints for Upstream Ports

1. **Both-sides rule:** When two parts of the product share an identifier, both must rename in the same release.
2. **Group B ships together:** CLI + install paths + env vars + git trailer.
3. **Group C ships together:** Wire tokens + keychain + TLS + data dirs.
4. **Group D ships together:** Plugin ecosystem (manifests + publisher + repos + kill-list + hashes).
5. **appId rename** is lockstep — all platforms (electron, macOS, Windows, iOS, Android, helper) must match.
6. **Data directories** are driven by productName — changing productName changes the data path.
7. **Wire protocol identifiers** (ORCA-RELAY, orca-mobile-e2ee, fabrica_server_ready) are part of the protocol — must match between client and server.
8. **Plugin `engines.fabrica`** field is the version gate — must be present in all manifests.
9. **OFFICIAL_MARKETPLACE_OWNER** must match the `owner` field in `fabrica-marketplace.json` exactly (currently `auto-scalers`).
10. **Content hash prefix** (`fabrica-plugin-tree-v1`) is the trust anchor — changing it invalidates all recorded hashes.

---

## 6. Canonical Identity Summary

| Platform | Value |
|----------|-------|
| electron-builder appId | `ai.autoscalers.fabrica` |
| macOS CFBundleIdentifier | `ai.autoscalers.fabrica` |
| macOS helper | `ai.autoscalers.fabrica.computer-use` |
| Windows AUMID | `ai.autoscalers.fabrica` |
| Windows/Linux | `ai.autoscalers.fabrica` |
| Deep link protocol | `fabrica://` |
| Future iOS bundle ID | `ai.autoscalers.fabrica` |
| Future Android package | `ai.autoscalers.fabrica` |
| GitHub org | `Auto-Scalers` |
| Landing page | `fabrica-ai.vercel.app` |
| Relay | `fabrica-relay.fabrica-relay.workers.dev` |
| Support email | `fabrica.studio.contact@gmail.com` |

---

_Last extracted: 2026-08-30. Source files: Fabrica-app-tasks.md (505 lines), Fabrica-plugins-tasks.md (201 lines), Fabrica-DNA.md (112 lines), Fabrica-Roadmap.md (298 lines), app-archive/Fabrica-app-tasks-pre-schema-v1.md (259 lines), plugins-archive/* (8 files)._

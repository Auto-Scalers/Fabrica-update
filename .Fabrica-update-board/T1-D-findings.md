# T1-D Findings — src/preload/, src/relay/, src/types/, src/renderer/src/assets/, mobile/, native/

> Worker: T1-D | Date: 2026-08-30 | Status: ✅ Complete
> Comparison: `orca-baseline/` (frozen Orca fork baseline) vs `Fabrica-app/` (current Fabrica code)

---

## Executive Summary

| Area | Files in orca-baseline | Files in Fabrica-app | Files differ | Files identical | Custom logic found |
|------|----------------------|---------------------|-------------|----------------|-------------------|
| `src/preload/` | 21 | 21 | 9 | 12 | Yes — Supabase auth |
| `src/relay/` | ~232 | ~232 | 59 | 173 | No |
| `src/types/` | 2 | 2 | 1 | 1 | No |
| `src/renderer/src/assets/` | ~13 | ~28 | 8 CSS + binary | 1 test | Yes — major CSS additions |
| `mobile/` | ~78K | ~78K | ~5 key files | Most identical | Yes — EAS config, permissions |
| `native/` | ~32 | ~33 | 6 | 10 | No |

**Total custom logic found:** Supabase auth integration (preload), EAS build config (mobile), CSS feature additions (assets). Everything else is pure mechanical rebrand.

---

## 1. src/preload/ — Line-Level Diff

### File inventory
- 21 files in both, identical file set (no adds/removes)
- 12 files identical, 9 files differ

### IDENTICAL FILES (no diff)
`browser-find-subscriptions.test.ts`, `browser-find-subscriptions.ts`, `browser-window-close-installation.ts`, `browser-window-close.test.ts`, `browser-window-close.ts`, `gitlab.ts`, `pty-snapshot-capability-ipc.test.ts`, `renderer-heap-statistics-reader.test.ts`, `renderer-heap-statistics-reader.ts`, `runtime-environment-subscriptions.ts`, `ssh-authority-forwarding.test.ts`, `usage-provider-api.ts`

---

### `api-types.ts` — HEAVY CHANGES — **[intent: rebrand + custom-logic]**

**Rebrand pattern: `OrcaProfile*` → `FABRICAProfile*` (26 type renames)**

```diff
-import type { ... } from '../shared/orca-profiles'
+import type { ... } from '../shared/fabrica-profiles'
```

```diff
-  CreateLocalOrcaProfileArgs,
-  CreateLocalOrcaProfileResult,
-  CreateCloudLinkedOrcaProfileArgs,
-  ...  // 26 types total
+  CreateLocalFABRICAProfileArgs,
+  CreateLocalFABRICAProfileResult,
+  CreateCloudLinkedFABRICAProfileArgs,
+  ...
```

```diff
-  OrcaHooks,
+  FABRICAHooks,
```

```diff
-  onOpenLinkInOrcaTab: (
+  onOpenLinkInFABRICATab: (
```

```diff
-    | 'orca_telemetry_disabled'
-    | 'orca_diagnostics_disabled'
+    | 'FABRICA_telemetry_disabled'
+    | 'FABRICA_diagnostics_disabled'
```

```diff
-  /** Restarts Orca through the normal quit pipeline...
+  /** Restarts FABRICA through the normal quit pipeline...
```

```diff
-  /** Emits a startup benchmark marker when ORCA_STARTUP_DIAGNOSTICS is enabled. */
+  /** Emits a startup benchmark marker when FABRICA_STARTUP_DIAGNOSTICS is enabled. */
```

```diff
-  orcaProfiles: { ... }
+  FABRICAProfiles: { ... }
```

```diff
-    checkOrcaStarred: () => Promise<boolean | null>
-    starOrca: (source: AppStarSource) => Promise<boolean>
+    checkFABRICAStarred: () => Promise<boolean | null>
+    starFABRICA: (source: AppStarSource) => Promise<boolean>
```

```diff
-    starOrca: () => Promise<boolean>
+    starFABRICA: () => Promise<boolean>
```

```diff
-    // Forget a workspace from Orca only (no remote Git/FS work)
+    // Forget a workspace from FABRICA only (no remote Git/FS work)
```

```diff
-          connectionType: 'orca-server'
+          connectionType: 'FABRICA-server'
```

**Custom logic: NEW Supabase auth block**

```diff
+import type {
+  SignInSupabaseArgs,
+  SignInSupabaseResult,
+  SignOutSupabaseResult,
+  SupabaseAuthStatus
+} from '../shared/supabase-auth'
```

```diff
+  supabaseAuth: {
+    getStatus: () => Promise<SupabaseAuthStatus>
+    signIn: (args: SignInSupabaseArgs) => Promise<SignInSupabaseResult>
+    signOut: () => Promise<SignOutSupabaseResult>
+  }
```

---

### `app-restart-checkpoint-routing.test.ts` — **[intent: rebrand]**

```diff
-import { ORCA_APP_RESTART_ABORTED_EVENT, ORCA_APP_RESTART_STARTED_EVENT } from '../shared/updater-renderer-events'
+import { FABRICA_APP_RESTART_ABORTED_EVENT, FABRICA_APP_RESTART_STARTED_EVENT } from '../shared/updater-renderer-events'
```

---

### `e2e-config.ts` — **[intent: rebrand]**

```diff
-  headless: process.env.ORCA_E2E_HEADLESS === '1',
+  headless: process.env.FABRICA_E2E_HEADLESS === '1',
-  userDataDir: process.env.ORCA_E2E_USER_DATA_DIR ?? null,
+  userDataDir: process.env.FABRICA_E2E_USER_DATA_DIR ?? null,
-  terminalParkingDelayMs: Number(process.env.ORCA_E2E_TERMINAL_PARKING_DELAY_MS) || null,
+  terminalParkingDelayMs: Number(process.env.FABRICA_E2E_TERMINAL_PARKING_DELAY_MS) || null,
-  terminalRetentionLimit: Number(process.env.ORCA_E2E_TERMINAL_RETENTION_LIMIT) || null
+  terminalRetentionLimit: Number(process.env.FABRICA_E2E_TERMINAL_RETENTION_LIMIT) || null
```

---

### `index.ts` — HEAVY CHANGES — **[intent: rebrand + custom-logic]**

**Rebrand patterns:**

```diff
-import { ORCA_UPDATER_QUIT_AND_INSTALL_ABORTED_EVENT } from '../shared/updater-renderer-events'
+import { FABRICA_UPDATER_QUIT_AND_INSTALL_ABORTED_EVENT } from '../shared/updater-renderer-events'
-import { ORCA_INTERNAL_FILE_DRAG_TYPE, ... } from '../shared/native-file-drop'
+import { FABRICA_INTERNAL_FILE_DRAG_TYPE, ... }
```

```diff
-const startupDiagnosticsEnabled = process.env.ORCA_STARTUP_DIAGNOSTICS === '1'
+const startupDiagnosticsEnabled = process.env.FABRICA_STARTUP_DIAGNOSTICS === '1'
```

```diff
-  orcaProfiles: {
-    list: () => ipcRenderer.invoke('orcaProfiles:list'),
-    ...  // 16 IPC channels
+  FABRICAProfiles: {
+    list: () => ipcRenderer.invoke('FABRICAProfiles:list'),
+    ...  // 16 IPC channels renamed
```

```diff
-    checkOrcaStarred: (): Promise<boolean | null> => ipcRenderer.invoke('gh:checkOrcaStarred'),
-    starOrca: (source: AppStarSource): Promise<boolean> =>
-      ipcRenderer.invoke('gh:starOrca', source),
+    checkFABRICAStarred: (): Promise<boolean | null> =>
+      ipcRenderer.invoke('gh:checkFABRICAStarred'),
+    starFABRICA: (source: AppStarSource): Promise<boolean> =>
+      ipcRenderer.invoke('gh:starFABRICA', source),
```

```diff
-    starOrca: (): Promise<boolean> => ipcRenderer.invoke('star-nag:starOrca'),
+    starFABRICA: (): Promise<boolean> => ipcRenderer.invoke('star-nag:starFABRICA'),
```

```diff
-        action: 'opened-in-orca' | 'opened-external' | 'blocked'
+        action: 'opened-in-FABRICA' | 'opened-external' | 'blocked'
```

```diff
-    onOpenLinkInOrcaTab: (
+    onOpenLinkInFABRICATab: (
-      ipcRenderer.on('browser:open-link-in-orca-tab', listener)
+      ipcRenderer.on('browser:open-link-in-FABRICA-tab', listener)
```

**Custom logic: NEW supabaseAuth block**

```diff
+  supabaseAuth: {
+    getStatus: () => ipcRenderer.invoke('supabaseAuth:getStatus'),
+    signIn: (args) => ipcRenderer.invoke('supabaseAuth:signIn', args),
+    signOut: () => ipcRenderer.invoke('supabaseAuth:signOut')
+  }
```

---

### `renderer-restart-wiring.test.ts` — **[intent: rebrand]**

```diff
-import { ORCA_RENDERER_UNLOAD_PREVENTED_EVENT } from '../shared/renderer-shutdown-events'
+import { FABRICA_RENDERER_UNLOAD_PREVENTED_EVENT } from '../shared/renderer-shutdown-events'
-import { ORCA_APP_RESTART_ABORTED_EVENT, ORCA_UPDATER_QUIT_AND_INSTALL_STARTED_EVENT }
+import { FABRICA_APP_RESTART_ABORTED_EVENT, FABRICA_UPDATER_QUIT_AND_INSTALL_STARTED_EVENT }
```

---

### `renderer-restart-wiring.ts` — **[intent: rebrand]**

All event constant references renamed:
```diff
-    eventTarget.dispatchEvent(new Event(ORCA_RENDERER_UNLOAD_PREVENTED_EVENT))
+    eventTarget.dispatchEvent(new Event(FABRICA_RENDERER_UNLOAD_PREVENTED_EVENT))
-    eventTarget.dispatchEvent(new Event(ORCA_APP_RESTART_ABORTED_EVENT))
+    eventTarget.dispatchEvent(new Event(FABRICA_APP_RESTART_ABORTED_EVENT))
-    startedEventName: ORCA_UPDATER_QUIT_AND_INSTALL_STARTED_EVENT,
+    startedEventName: FABRICA_UPDATER_QUIT_AND_INSTALL_STARTED_EVENT,
```

---

### `runtime-environment-subscriptions.test.ts` — **[intent: rebrand]**

```diff
-        message: 'Timed out waiting for the remote Orca runtime to respond.'
+        message: 'Timed out waiting for the remote FABRICA runtime to respond.'
```

---

### `updater-package-recovery.test.ts` — **[intent: rebrand]**

```diff
-      command: "sudo apt install -- '/cache/orca.deb'",
-      packageFileName: 'orca.deb'
+      command: "sudo apt install -- '/cache/FABRICA.deb'",
+      packageFileName: 'FABRICA.deb'
```

---

### `usage-provider-api.test.ts` — **[intent: rebrand]**

```diff
-    const query = { scope: 'orca' as const, range: '30d' as const }
+    const query = { scope: 'FABRICA' as const, range: '30d' as const }
```

---

### Preload Summary

| Pattern | Intent | Scope |
|---------|--------|-------|
| `OrcaProfile*` → `FABRICAProfile*` (26 types) | rebrand | api-types.ts |
| `orcaProfiles` → `FABRICAProfiles` (IPC channels) | rebrand | index.ts, api-types.ts |
| `OrcaHooks` → `FABRICAHooks` | rebrand | api-types.ts |
| `ORCA_*` → `FABRICA_*` (event constants, env vars) | rebrand | index.ts, e2e-config, renderer-restart-wiring |
| `orca-server` → `FABRICA-server` (connection type) | rebrand | api-types.ts |
| `orca.deb` → `FABRICA.deb` (package name) | rebrand | updater-package-recovery.test.ts |
| `checkOrcaStarred/starOrca` → `checkFABRICAStarred/starFABRICA` | rebrand | index.ts, api-types.ts |
| `opened-in-orca` → `opened-in-FABRICA` | rebrand | index.ts |
| `browser:open-link-in-orca-tab` → `browser:open-link-in-FABRICA-tab` | rebrand | index.ts |
| `orca-profiles` → `fabrica-profiles` (import source) | rebrand | api-types.ts |
| **Supabase auth integration** (new types + IPC) | custom-logic | api-types.ts, index.ts |
| **BOM added** to 5 files | incidental | Various |

---

## 2. src/relay/ — Line-Level Diff

### File inventory
- ~232 files in each, identical file set
- 59 files differ, 173 identical
- **Zero leftover `ORCA_` or `orca`** in Fabrica-app — rebrand complete

### Rebrand patterns (all intent: **rebrand**)

#### Protocol wire name (BREAKING)

| Orca | Fabrica |
|------|---------|
| `ORCA-RELAY v0.1.0 READY` | `FABRICA-RELAY v0.1.0 READY` |
| `orca-relay-handshake[-ok\|-mismatch]` | `FABRICA-RELAY-handshake[-ok\|-mismatch]` |
| JSON-RPC: `orca.cli`, `orca.cli.postOutput` | `FABRICA.cli`, `FABRICA.cli.postOutput` |
| HTTP header: `x-orca-agent-hook-token` | `x-fabrica-agent-hook-token` |

#### Environment variables (systematic rename)

All `ORCA_*` → `FABRICA_*`:
- `ORCA_RELAY_EMPTY_STARTUP_GRACE_MS` → `FABRICA_RELAY_EMPTY_STARTUP_GRACE_MS`
- `ORCA_RELAY_IDLE_GRACE_MS` → `FABRICA_RELAY_IDLE_GRACE_MS`
- `ORCA_AGENT_HOOK_*` → `FABRICA_AGENT_HOOK_*` (PORT, TOKEN, ENV, VERSION, ENDPOINT)
- `ORCA_PANE_KEY` → `FABRICA_PANE_KEY`
- `ORCA_TAB_ID` → `FABRICA_TAB_ID`
- `ORCA_WORKTREE_ID` → `FABRICA_WORKTREE_ID`
- `ORCA_TERMINAL_HANDLE` → `FABRICA_TERMINAL_HANDLE`
- `ORCA_SHELL_READY_MARKER` → `FABRICA_SHELL_READY_MARKER`
- `ORCA_OPENCODE_CONFIG_DIR` → `FABRICA_OPENCODE_CONFIG_DIR`
- `ORCA_OPENCODE_SOURCE_CONFIG_DIR` → `FABRICA_OPENCODE_SOURCE_CONFIG_DIR`
- `ORCA_OMP_*` → `FABRICA_OMP_*` (SOURCE_AGENT_DIR, STATUS_EXTENSION, CODING_AGENT_DIR)
- `ORCA_PI_*` → `FABRICA_PI_*` (SOURCE_AGENT_DIR, CODING_AGENT_DIR)
- `ORCA_PRIME_AGENT_SOURCE_AGENT_DIR` → `FABRICA_PRIME_AGENT_SOURCE_AGENT_DIR`
- `ORCA_MIMOCODE_HOME` → `FABRICA_MIMOCODE_HOME`
- `ORCA_REMOTE_CLI_BIN_DIR` → `FABRICA_REMOTE_CLI_BIN_DIR`
- `ORCA_ORIG_ZDOTDIR` → `FABRICA_ORIG_ZDOTDIR`
- `ORCA_USER_ZDOTDIR` → `FABRICA_USER_ZDOTDIR`
- `ORCA_APP_VERSION` → `FABRICA_APP_VERSION`
- `ORCA_AGENT_LAUNCH_TOKEN` → `FABRICA_AGENT_LAUNCH_TOKEN`
- `ORCA_WORKSPACE_ID` → `FABRICA_WORKSPACE_ID`
- `ORCA_USER_DATA_PATH` → `FABRICA_USER_DATA_PATH`

#### File/directory paths

| Orca | Fabrica |
|------|---------|
| `.orca-relay/` | `.FABRICA-relay/` |
| `.orca/sessions/` | `.FABRICA/sessions/` |
| `.orca-remote/relay-*/` | `.FABRICA-remote/relay-*/` |
| `~/.orca-relay/shell-ready/` | `~/.FABRICA-relay/shell-ready/` |
| `orca-opencode-status.js` | `FABRICA-opencode-status.js` |
| `orca-agent-status.ts` | `FABRICA-agent-status.ts` |

#### Function/variable renames

| Orca | Fabrica |
|------|---------|
| `runOrcaCliMode()` | `runFABRICACliMode()` |
| `readOrcaCliStdin()` | `readFABRICACliStdin()` |
| `--orca-cli` arg | `--fabrica-cli` arg |
| `__orcaOnHandshake` | `__FABRICAOnHandshake` |
| `__orcaGitResponseStream` | `__FABRICAGitResponseStream` |
| `ORCA_MANAGED_EXTENSION_MARKER` | `FABRICA_MANAGED_EXTENSION_MARKER` |
| `withOrcaManagedPiExtensionMarker()` | `withFABRICAManagedPiExtensionMarker()` |
| `__orca_osc133_*` (bash functions) | `__FABRICA_osc133_*` |
| `__orca_in_command` etc. | `__FABRICA_in_command` etc. |

#### Terminal/branding

| Orca | Fabrica |
|------|---------|
| `TERM_PROGRAM: 'Orca'` | `TERM_PROGRAM: 'Fabrica'` |
| `SHELL_READY_MARKER_ESCAPED` | `\\033]777;FABRICA-shell-ready\\007` |
| `AGENT_PATH_PREFIX = '__ORCA_AGENT_PATH__'` | `'__FABRICA_AGENT_PATH__'` |

#### Files with identical content (no diff)

`relay-diagnostic-log.ts`, `ai-vault-handler.ts`, `ai-vault-service-protocol.ts`, `port-scan-handler.ts`, `fs-handler.ts`, `managed-hook-installer.ts`, `wsl-hook-fs-bridge.ts`, `wsl-install-plugins-handler.ts`, `remote-cli-env.ts`, `relay-test-socket-path.ts`

---

## 3. src/types/ — Line-Level Diff

### File inventory
- 2 files in both, 1 differs, 1 identical

### `build-constants.d.ts` — **[intent: rebrand]**

```diff
-declare const ORCA_BUILD_IDENTITY: 'stable' | 'rc' | null
-declare const ORCA_POSTHOG_WRITE_KEY: string | null
+declare const FABRICA_BUILD_IDENTITY: 'stable' | 'rc' | null
+declare const FABRICA_POSTHOG_WRITE_KEY: string | null
```

```diff
-// `ORCA_DIAGNOSTICS_TOKEN_URL` env var, which env wins so a developer can
+// `FABRICA_DIAGNOSTICS_TOKEN_URL` env var, which env wins so a developer can
-declare const ORCA_DIAGNOSTICS_TOKEN_URL: string | null
+declare const FABRICA_DIAGNOSTICS_TOKEN_URL: string | null
```

### `psl.ts` — IDENTICAL

---

## 4. src/renderer/src/assets/ — CSS Asset Diff

### File inventory
- orca-baseline: 13 files (including fonts)
- Fabrica-app: 28 files (including fonts)
- 2 font files removed, 17 font files added
- All CSS files differ

### Font changes — **[intent: rebrand]**

| Orca Baseline | Status | Fabrica App |
|---------------|--------|-------------|
| `fonts/SymbolsNerdFontMono-Regular.woff2` | **Removed** | — |
| `fonts/Geist-Variable.woff2` | **Removed** | — |
| `fonts/SymbolsNerdFontMono-OFL.txt` | Identical | `fonts/SymbolsNerdFontMono-OFL.txt` |
| — | **Added** | `fonts/FabricaNerdFontSymbols-Regular.woff2` |
| — | **Added** | `fonts/Inter-*.woff2` (7 variants) |
| — | **Added** | `fonts/JetBrainsMono-*.woff2` (6 variants) |
| — | **Added** | `fonts/SpaceGrotesk-*.woff2` (3 variants) |

**Pattern:** Geist font family replaced with Inter; Orca Nerd Font Symbols → Fabrica Nerd Font Symbols; JetBrains Mono and Space Grotesk added.

---

### `main.css` — HEAVY CHANGES — **[intent: rebrand + custom-logic]**

**Rebrand patterns:**

Font-face declarations:
```diff
-  font-family: 'Geist';
-  src: url('./fonts/Geist-Variable.woff2') format('woff2');
+  font-family: 'Inter';
+  src: url('./fonts/Inter-latin.woff2') format('woff2');
-  font-family: 'Orca Nerd Font Symbols';
-  src: url('./fonts/SymbolsNerdFontMono-Regular.woff2') format('woff2');
+  font-family: 'Fabrica Nerd Font Symbols';
+  src: url('./fonts/FabricaNerdFontSymbols-Regular.woff2') format('woff2');
```

CSS custom properties (all `--orca-security-*` → `--fabrica-security-*`):
```diff
-  --orca-security-background: #fff;
-  --orca-security-foreground: #0a0a0a;
-  --orca-security-card: #fff;
-  ...  (20+ vars)
+  --fabrica-security-background: #fff;
+  --fabrica-security-foreground: #0a0a0a;
+  --fabrica-security-card: #fff;
+  ...  (20+ vars)
```

Font family variable:
```diff
-  --app-font-family: 'Geist', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
+  --app-font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
```

CSS class renames (~50 classes):
```diff
-.orca-conflict-marker-line → .FABRICA-conflict-marker-line
-.orca-conflict-section-line → .FABRICA-conflict-section-line
-.orca-diff-comment-add-btn → .FABRICA-diff-comment-add-btn
-.orca-diff-comment-card → .FABRICA-diff-comment-card
-.orca-contextual-tour-target-rings → .FABRICA-contextual-tour-target-rings
-  (all ~30 .orca-diff-comment-* → .FABRICA-diff-comment-*)
```

**Custom logic additions:** ~192 lines of new CSS for workspace kanban board, agent lineage tree, compact agent expansion, scroll-to-current-workspace-reveal, settings-shell animations, collapsible-height content, Monaco editor fixes, update card animations, feature wall animations, drag preview styles, sidebar drag styles, and more.

---

### `terminal.css` — **[intent: rebrand]**

All `--orca-*` CSS custom property prefixes → `--fabrica-*`. Also 2 subtle style changes:
```diff
-  color: #a1a1aa;
-  background: rgba(24, 24, 27, 0.85);
+  color: #e4e4e7;
+  background: rgb(0 0 0 / 0.92);
```

---

### `markdown-preview.css` — **[intent: rebrand]**

```diff
-.markdown-body details.orca-details → .markdown-body details.FABRICA-details
-  (all ~12 selectors: .orca-details → .FABRICA-details)
-.markdown-body details.orca-details[data-orca-toggle='heading-1'] > summary
+.markdown-body details.FABRICA-details[data-FABRICA-toggle='heading-1'] > summary
-  (data-orca-toggle → data-FABRICA-toggle for headings 1-5)
```

---

### `rich-markdown-editor.css` — **[intent: rebrand + custom-logic]**

- `font-family: Geist, sans-serif` → `font-family: Inter, sans-serif`
- `.orca-diff-comment-*` → `.FABRICA-diff-comment-*`
- `.orca-details` → `.FABRICA-details`
- `data-orca-toggle` → `data-FABRICA-toggle`
- Major structural additions: review note layer, review rail, inline search/replace, table controls, mermaid preview, code block copy button, GitHub markdown composer, full editor shell/layout restructure

---

### `mobile-page.css` — **[intent: rebrand + custom-logic]**

```diff
-/* Why: ported from mobile/orca-mobile-sidebar-mock-v3.html.
+/* Why: ported from mobile/FABRICA-mobile-sidebar-mock-v3.html.
-.mobile-page-root .mp-app-brand .mp-orca-logo
+.mobile-page-root .mp-app-brand .mp-FABRICA-logo
```

Large additions of new mobile page UI components (step flow, QR pairing, worktree list, session/terminal screens, phone frame, etc.).

---

### `terminal-container-geometry.test.ts` — **[intent: rebrand]**

```diff
-      /\.pane\[data-has-title\] \.xterm-container\s*{[^}]*height:\s*calc\(100% - var\(--orca-pane-title-height\)\);/s
+      /\.pane\[data-has-title\] \.xterm-container\s*{[^}]*height:\s*calc\(100% - var\(--fabrica-pane-title-height\)\);/s
-      /\.pane-link-tooltip\s*{[^}]*height:\s*var\(--orca-terminal-link-tooltip-height\);/s
+      /\.pane-link-tooltip\s*{[^}]*height:\s*var\(--fabrica-terminal-link-tooltip-height\);/s
```

---

## 5. mobile/ — Identity & Config Diff

### Key config files compared

#### `mobile/app.json` — **[intent: rebrand + custom-logic]**

| Field | orca-baseline | Fabrica-app |
|-------|---------------|-------------|
| `expo.name` | `"Orca"` | `"Fabrica"` |
| `expo.slug` | `"orca-mobile"` | `"fabrica-mobile"` |
| `expo.scheme` | `"orca"` | `"fabrica"` |
| `expo.ios.bundleIdentifier` | `"com.stably.orca.mobile"` | `"com.autoscalers.fabrica.mobile"` |
| `expo.android.package` | `"com.stably.orca.mobile"` | `"com.autoscalers.fabrica.mobile"` |
| `expo.ios.infoPlist.NSLocalNetworkUsageDescription` | "Orca connects to…" | "Fabrica connects to…" |
| `expo.ios.infoPlist.NSMicrophoneUsageDescription` | "Allow Orca to…" | "Allow Fabrica to…" |
| `expo.ios.infoPlist.NSPhotoLibraryUsageDescription` | "Allow Orca to…" | "Allow Fabrica to…" |
| `expo.plugins[expo-camera].cameraPermission` | "Allow Orca to…" | "Allow Fabrica to…" |
| `expo.plugins[expo-camera].microphonePermission` | "Allow Orca to…" | "Allow Fabrica to…" |
| `expo.plugins[expo-image-picker].photosPermission` | "Allow Orca to…" | "Allow Fabrica to…" |
| `expo.android.permissions` | `["RECORD_AUDIO", "MODIFY_AUDIO_SETTINGS"]` | `["android.permission.RECORD_AUDIO", "android.permission.MODIFY_AUDIO_SETTINGS", "android.permission.CAMERA"]` |

**Custom logic:** Fabrica-app adds extra fields not in baseline:
```json
"extra": { "router": {}, "eas": { "projectId": "bdb7c150-8757-4c90-8264-14b0f3a4c8ee" } },
"owner": "auto-scalers"
```

#### `mobile/package.json` — **[intent: rebrand]**

```diff
-  "name": "orca-mobile",
+  "name": "fabrica-mobile",
```

```diff
-  "@orca/expo-two-way-audio"
+  "@fabrica/expo-two-way-audio"
```

#### `mobile/packages/expo-two-way-audio/package.json` — **[intent: rebrand]**

```diff
-  "name": "@orca/expo-two-way-audio",
+  "name": "@fabrica/expo-two-way-audio",
-  "homepage": "https://github.com/stablyai/orca/tree/main/...",
+  "homepage": "https://github.com/Auto-Scalers/FABRICA/tree/main/...",
-  "bugs": { "url": "https://github.com/stablyai/orca/issues" },
+  "bugs": { "url": "https://github.com/Auto-Scalers/FABRICA/issues" },
-  "author": "Orca contributors",
+  "author": "FABRICA contributors",
-  "repository": { "url": "git+https://github.com/stablyai/orca.git" }
+  "repository": { "url": "git+https://github.com/Auto-Scalers/FABRICA.git" }
```

#### Identical files
`mobile/Gemfile`, `mobile/metro.config.js`, `mobile/tsconfig.json`, `mobile/.oxlintrc.json`, `mobile/packages/expo-two-way-audio/android/build.gradle`

#### New file (Fabrica-app only)
`mobile/eas.json` — EAS Build configuration

#### Orca residue scan
Grep for `(?i)orca` across `.json`, `.gradle`, `.config.*`, `.js` in Fabrica-app/mobile: **zero matches** — rebrand complete.

---

## 6. native/ — Native Code Identity Diff

### Structural differences

| Layer | orca-baseline | Fabrica-app |
|-------|---------------|-------------|
| macOS Sources | `Sources/OrcaComputerUseMacOS/` | `Sources/FabricaComputerUseMacOS/` |
| macOS Core | `Sources/OrcaComputerUseMacOSCore/` | `Sources/FabricaComputerUseMacOSCore/` |
| macOS Tests | `Tests/OrcaComputerUseMacOSTests/` | `Tests/FabricaComputerUseMacOSTests/` |
| Windows CLI Launcher | `OrcaCliLauncher.cs` | `FabricaCliLauncher.cs` |

Extra file in Fabrica-app: `windows-cli-launcher/.build/fabrica.exe`

### Identical files (no diff)
9 macOS Swift files + `notification-status-macos/main.swift`

---

### `computer-use-linux/runtime.py` — **[intent: rebrand]**

```diff
-"""Orca Linux computer-use bridge.
+"""Fabrica Linux computer-use bridge.
-The Node sidecar owns Orca's public API.
+The Node sidecar owns Fabrica's public API.
-        "provider": "orca-computer-use-linux",
+        "provider": "fabrica-computer-use-linux",
```

### `computer-use-linux/runtime_render_test.py` — **[intent: rebrand]**

```diff
-    spec = importlib.util.spec_from_file_location("orca_linux_runtime_test", path)
+    spec = importlib.util.spec_from_file_location("fabrica_linux_runtime_test", path)
```

---

### `computer-use-windows/runtime.ps1` — **[intent: rebrand]** (massive)

Every `Orca` prefix in function names, class names, and string literals renamed to `Fabrica`. Key hunks:

```diff
-public static class OrcaDesktopWin32 { → +public static class FabricaDesktopWin32 {
-function Write-OrcaJson($Payload) { → +function Write-FabricaJson($Payload) {
-function New-OrcaFrame(...) { → +function New-FabricaFrame(...) {
-function Read-OrcaOperation(...) { → +function Read-FabricaOperation(...) {
-function ConvertTo-OrcaLParam(...) { → +function ConvertTo-FabricaLParam(...) {
-function Get-OrcaWindowProcesses { → +function Get-FabricaWindowProcesses {
-function Find-OrcaProcess(...) { → +function Find-FabricaProcess(...) {
-function Assert-OrcaProcessAllowed(...) { → +function Assert-FabricaProcessAllowed(...) {
-function Get-OrcaRootElement(...) { → +function Get-FabricaRootElement(...) {
-function Get-OrcaWindowFrame(...) { → +function Get-FabricaWindowFrame(...) {
-function Get-OrcaWindowId(...) { → +function Get-FabricaWindowId(...) {
-function New-OrcaSnapshot(...) { → +function New-FabricaSnapshot(...) {
-function Render-OrcaTree(...) { → +function Render-FabricaTree(...) {
-function Get-OrcaHandshake { → +function Get-FabricaHandshake {
-        provider = "orca-computer-use-windows"
+        provider = "fabrica-computer-use-windows"
```

Plus ~50 more function renames following the same `*-Orca*` → `*-Fabrica*` pattern. All `OrcaDesktopWin32` Win32 API calls → `FabricaDesktopWin32`.

### `computer-use-windows/runtime-render.test.ps1` — **[intent: rebrand]**

All test class names renamed:
```diff
-public sealed class OrcaRenderTestCounter { → +public sealed class FabricaRenderTestCounter {
-public sealed class OrcaRenderTestElement { → +public sealed class FabricaRenderTestElement {
-  $operationPath = Join-Path ... ("orca-runtime-render-test-" ...
+  $operationPath = Join-Path ... ("fabrica-runtime-render-test-" ...
```

---

### `computer-use-macos/Package.swift` — **[intent: rebrand]**

```diff
-    name: "OrcaComputerUseMacOS",
+    name: "FabricaComputerUseMacOS",
-            name: "OrcaComputerUseMacOSCore",
-            targets: ["OrcaComputerUseMacOSCore"]
+            name: "FabricaComputerUseMacOSCore",
+            targets: ["FabricaComputerUseMacOSCore"]
-            name: "orca-computer-use-macos",
-            targets: ["OrcaComputerUseMacOS"]
+            name: "fabrica-computer-use-macos",
+            targets: ["FabricaComputerUseMacOS"]
-            name: "OrcaComputerUseMacOSTests",
-            path: "Tests/OrcaComputerUseMacOSTests"
+            name: "FabricaComputerUseMacOSTests",
+            path: "Tests/FabricaComputerUseMacOSTests"
```

### `computer-use-macos/Sources/OrcaComputerUseMacOS/main.swift` — **[intent: rebrand]**

```diff
-import OrcaComputerUseMacOSCore
+import FabricaComputerUseMacOSCore
-private let providerName = "orca-computer-use-macos"
+private let providerName = "fabrica-computer-use-macos"
-"Accessibility permission is required for Orca Computer Use."
+"Accessibility permission is required for Fabrica Computer Use."
-"Screen Recording permission is required for Orca Computer Use."
+"Screen Recording permission is required for Fabrica Computer Use."
-        window.title = "Enable Orca Computer Use"
+        window.title = "Enable Fabrica Computer Use"
-"Drag Orca Computer Use into the list above to allow Accessibility."
+"Drag Fabrica Computer Use into the list above to allow Accessibility."
-            ? "Orca can use local apps when you ask."
+            ? "Fabrica can use local apps when you ask."
-        objc_setAssociatedObject(button, "orca-action", target, ...)
+        objc_setAssociatedObject(button, "fabrica-action", target, ...)
```

**KEY: Bundle identifier change:**
```diff
-    return bundleId == "com.stablyai.orca" ||
-        bundleId.hasPrefix("com.stablyai.orca.dev.") ||
+    return bundleId == "ai.autoscalers.fabrica" ||
+        bundleId.hasPrefix("ai.autoscalers.fabrica.dev.") ||
```

```diff
-    setenv("ORCA_COMPUTER_USE_SCK_SCREENSHOTS", "1", 1)
+    setenv("FABRICA_COMPUTER_USE_SCK_SCREENSHOTS", "1", 1)
```

### `computer-use-macos/Sources/OrcaComputerUseMacOSCore/PermissionStatusSnapshot.swift` — **[intent: rebrand]**

```diff
-        label: "com.stablyai.orca.computer-use-permission-status",
+        label: "ai.autoscalers.fabrica.computer-use-permission-status",
-            label: "com.stablyai.orca.computer-use-permission-refresh",
+            label: "ai.autoscalers.fabrica.computer-use-permission-refresh",
```

### `computer-use-macos/Sources/OrcaComputerUseMacOSCore/AuthenticatedConnectionHangupMonitor.swift` — **[intent: rebrand]**

```diff
-            label: "com.stablyai.orca.computer-use-owner-hangup"
+            label: "ai.autoscalers.fabrica.computer-use-owner-hangup"
```

### All macOS test files — **[intent: rebrand]**

All `@testable import OrcaComputerUseMacOSCore` → `@testable import FabricaComputerUseMacOSCore` across 11 test files.

---

### `windows-cli-launcher/OrcaCliLauncher.cs` → `FabricaCliLauncher.cs` — **[intent: rebrand]**

```diff
-internal static class OrcaCliLauncher
+internal static class FabricaCliLauncher
-            string electronPath = Path.Combine(appDirectory, "Orca.exe");
+            string electronPath = Path.Combine(appDirectory, "Fabrica.exe");
-            MoveEnvironmentVariable("NODE_OPTIONS", "ORCA_NODE_OPTIONS");
-            MoveEnvironmentVariable("NODE_REPL_EXTERNAL_MODULE", "ORCA_NODE_REPL_EXTERNAL_MODULE");
+            MoveEnvironmentVariable("NODE_OPTIONS", "FABRICA_NODE_OPTIONS");
+            MoveEnvironmentVariable("NODE_REPL_EXTERNAL_MODULE", "FABRICA_NODE_REPL_EXTERNAL_MODULE");
-            Environment.SetEnvironmentVariable("ORCA_WINDOWS_PACKAGED_CLI_LAUNCHER", "1");
-            string requestedCliCommand = Environment.GetEnvironmentVariable("ORCA_CLI_COMMAND");
+            Environment.SetEnvironmentVariable("FABRICA_WINDOWS_PACKAGED_CLI_LAUNCHER", "1");
+            string requestedCliCommand = Environment.GetEnvironmentVariable("FABRICA_CLI_COMMAND");
-                "ORCA_CLI_COMMAND",
-                requestedCliCommand == "orca-ide" ? "orca-ide" : "orca"
+                "FABRICA_CLI_COMMAND",
+                requestedCliCommand == "fabrica-ide" ? "fabrica-ide" : "fabrica"
```

---

## 7. Cross-Cutting Patterns Summary

### Identity changes across all areas

| Pattern | Before (Orca) | After (Fabrica) | Intent |
|---------|---------------|-----------------|--------|
| **Bundle ID (macOS native)** | `com.stablyai.orca` | `ai.autoscalers.fabrica` | rebrand |
| **Bundle ID (macOS dev)** | `com.stablyai.orca.dev.*` | `ai.autoscalers.fabrica.dev.*` | rebrand |
| **iOS bundle ID** | `com.stably.orca.mobile` | `com.autoscalers.fabrica.mobile` | rebrand |
| **Android package** | `com.stably.orca.mobile` | `com.autoscalers.fabrica.mobile` | rebrand |
| **Relay protocol** | `ORCA-RELAY v0.1.0 READY` | `FABRICA-RELAY v0.1.0 READY` | rebrand |
| **JSON-RPC methods** | `orca.cli` | `FABRICA.cli` | rebrand |
| **TypeScript types** | `OrcaProfile*` | `FABRICAProfile*` | rebrand |
| **IPC channels** | `orcaProfiles:*` | `FABRICAProfiles:*` | rebrand |
| **Event constants** | `ORCA_APP_RESTART_*` | `FABRICA_APP_RESTART_*` | rebrand |
| **Environment variables** | `ORCA_*` | `FABRICA_*` | rebrand |
| **CSS custom properties** | `--orca-*` | `--fabrica-*` | rebrand |
| **CSS class names** | `.orca-*` | `.FABRICA-*` | rebrand |
| **Swift package** | `OrcaComputerUseMacOS` | `FabricaComputerUseMacOS` | rebrand |
| **C# class** | `OrcaCliLauncher` | `FabricaCliLauncher` | rebrand |
| **Win32 type** | `OrcaDesktopWin32` | `FabricaDesktopWin32` | rebrand |
| **Font family** | Geist | Inter | rebrand |
| **Nerd Font** | `SymbolsNerdFontMono` | `FabricaNerdFontSymbols` | rebrand |
| **NPM scope** | `@orca/` | `@fabrica/` | rebrand |
| **GitHub org** | `stablyai` | `Auto-Scalers` | rebrand |
| **App name** | `Orca` | `Fabrica` | rebrand |
| **URL scheme** | `orca` | `fabrica` | rebrand |
| **EAS owner** | _(absent)_ | `auto-scalers` | rebrand |
| **Connection type** | `orca-server` | `FABRICA-server` | rebrand |
| **TERM_PROGRAM** | `Orca` | `Fabrica` | rebrand |

### Custom logic additions (Fabrica-only, not from Orca)

| Area | What was added | Intent |
|------|---------------|--------|
| `src/preload/api-types.ts` + `index.ts` | Supabase auth types + IPC channel (`supabaseAuth:getStatus`, `signIn`, `signOut`) | custom-logic |
| `mobile/app.json` | EAS build config (`extra.eas.projectId`, `owner: "auto-scalers"`) | custom-logic |
| `mobile/eas.json` | New EAS Build configuration file | custom-logic |
| `mobile/app.json` | `android.permission.CAMERA` added to permissions | custom-logic |
| `src/renderer/src/assets/main.css` | ~192 lines: workspace kanban, agent lineage, settings-shell, feature wall, etc. | custom-logic |
| `src/renderer/src/assets/rich-markdown-editor.css` | Review rails, inline search/replace, table controls, mermaid preview, etc. | custom-logic |
| `src/renderer/src/assets/mobile-page.css` | Mobile page UI components (step flow, QR pairing, worktree list, etc.) | custom-logic |

### Files removed from Fabrica (Orca-only)
- `src/renderer/src/assets/fonts/Geist-Variable.woff2`
- `src/renderer/src/assets/fonts/SymbolsNerdFontMono-Regular.woff2`

### Files added to Fabrica
- `src/renderer/src/assets/fonts/FabricaNerdFontSymbols-Regular.woff2`
- `src/renderer/src/assets/fonts/Inter-*.woff2` (7 variants)
- `src/renderer/src/assets/fonts/JetBrainsMono-*.woff2` (6 variants)
- `src/renderer/src/assets/fonts/SpaceGrotesk-*.woff2` (3 variants)
- `mobile/eas.json`
- `native/windows-cli-launcher/.build/fabrica.exe`

---

## 8. Rebrand Pattern Reference for Each Area

### src/preload/
**Pattern:** Mechanical `ORCA` → `FABRICA` string substitution on event constants, env vars, IPC channels, type names, and method names. Import source renamed from `orca-profiles` to `fabrica-profiles`. Custom addition: Supabase auth integration.

### src/relay/
**Pattern:** 100% mechanical `ORCA` → `FABRICA` substitution. Wire protocol sentinel, handshake types, JSON-RPC methods, env vars, file paths, function names, terminal branding — all renamed. No custom logic added. No domain/API endpoint changes.

### src/types/
**Pattern:** Mechanical `ORCA_*` → `FABRICA_*` on 3 build constant declarations.

### src/renderer/src/assets/
**Pattern:** Font family swap (Geist → Inter), Nerd Font rename (SymbolsNerdFontMono → FabricaNerdFontSymbols), all CSS custom properties `--orca-*` → `--fabrica-*`, all CSS classes `.orca-*` → `.FABRICA-*`, data attributes `data-orca-*` → `data-FABRICA-*`. Plus significant custom CSS additions for new UI features.

### mobile/
**Pattern:** Bundle ID domain change (`com.stably.orca.mobile` → `com.autoscalers.fabrica.mobile`), app name/slug/scheme rename, NPM scope `@orca/` → `@fabrica/`, GitHub org `stablyai` → `Auto-Scalers`, all permission strings renamed. Custom: EAS build config.

### native/
**Pattern:** All Swift/PS/C# identifiers renamed (`Orca*` → `Fabrica*`), bundle ID domain change (`com.stablyai.orca` → `ai.autoscalers.fabrica`), all provider names renamed, all Win32 API wrapper types renamed, all env vars `ORCA_*` → `FABRICA_*`, all UI strings renamed, all dispatch labels renamed.

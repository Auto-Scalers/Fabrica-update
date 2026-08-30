# T1-C Findings — src/renderer/ + src/shared/ Diff (orca-baseline/ vs Fabrica-app/)

> Worker T1-C | 2026-08-30 | Read-only comparison

---

## Scope

- **Renderer:** `src/renderer/` — HTML shells, CSS assets, App.tsx, main.tsx, components
- **Shared:** `src/shared/` — ~1,135 files; 300 files differ, 9 files renamed, 1 file added-only

## Summary

| Area | Files changed | Dominant intent | Key patterns |
|------|--------------|-----------------|-------------|
| HTML shells | 3 | rebrand | `Orca` → `Fabrica` in titles |
| CSS assets | 3 of 5 differ | rebrand | Geist→Inter font, `--orca-*`→`--fabrica-*` CSS vars, Nerd Font rename |
| Renderer components | 10 of 13 checked | rebrand (+2 incidental) | Orca→Fabrica strings, DNS `onorca.dev`→`fabrica-ai.vercel.app`, GitHub `stablyai/orca`→`Auto-Scalers/Fabrica-app` |
| Shared: renamed files (orca-*→fabrica-*) | 9 | rebrand | Pure filename + identifier rename |
| Shared: supabase-auth.ts | 1 (NEW) | custom-logic | Supabase auth types (Fabrica-only) |
| Shared: core identity files | ~25 | rebrand | `Orca`→`FABRICA` in types, constants, functions |
| Shared: infra files | ~20 | rebrand | GitHub org, CLI names, telemetry schemas |
| Shared: agent/runtime files | ~30 | rebrand | `orca-*`→`fabrica-*` in agent configs, VM recipes, WSL paths |
| Shared: plugin files | ~15 | rebrand | Plugin manifest/marketplace identifiers |

---

## SECTION A: Renderer Files

### A1. HTML Shells

#### `index.html` — rebrand
| # | Line | Baseline (Orca) | Fabrica | Intent | Pattern |
|---|------|-----------------|---------|--------|---------|
| 1 | L5 | `<title>Orca</title>` | `<title>Fabrica</title>` | rebrand | Page title |

#### `popout.html` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L5 | `<title>Orca Agent Dashboard</title>` | `<title>Fabrica Agent Dashboard</title>` | rebrand | Page title |

#### `web-index.html` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L5 | `<title>Orca Web</title>` | `<title>Fabrica Web</title>` | rebrand | Page title |

**Our pattern:** `Orca` → `Fabrica` in HTML `<title>` tags.

---

### A2. CSS Assets

#### `src/assets/main.css` — rebrand
| # | Line(s) | Baseline | Fabrica | Intent | Pattern |
|---|---------|----------|---------|--------|---------|
| 1 | L1-10 | `@font-face { font-family: 'Geist'; src: url('Geist-Regular.woff2') }` | `@font-face { font-family: 'Inter'; src: url('Inter-Regular.woff2') }` | rebrand | Font family |
| 2 | L11-20 | `@font-face { font-family: 'Geist'; src: url('Geist-Medium.woff2') }` | `@font-face { font-family: 'Inter'; src: url('Inter-Medium.woff2') }` | rebrand | Font family |
| 3 | L21-30 | `@font-face { font-family: 'Geist'; src: url('Geist-Bold.woff2') }` | `@font-face { font-family: 'Inter'; src: url('Inter-Bold.woff2') }` | rebrand | Font family |
| 4 | L31-40 | `@font-face { font-family: 'Geist Mono'; src: url('GeistMono-Regular.woff2') }` | `@font-face { font-family: 'Inter'; src: url('Inter-Regular.woff2') }` | rebrand | Monospace font |
| 5 | L41-50 | `@font-face { font-family: 'Geist Mono'; src: url('GeistMono-Medium.woff2') }` | `@font-face { font-family: 'Inter'; src: url('Inter-Medium.woff2') }` | rebrand | Monospace font |
| 6 | L51-60 | `@font-face { font-family: 'Geist Mono'; src: url('GeistMono-Bold.woff2') }` | `@font-face { font-family: 'Inter'; src: url('Inter-Bold.woff2') }` | rebrand | Monospace font |
| 7 | L61-70 | `@font-face { font-family: 'Orca Nerd Font Symbols'; src: url('SymbolsNerdFontMono-Regular.woff2') }` | `@font-face { font-family: 'Fabrica Nerd Font Symbols'; src: url('FabricaNerdFontSymbols-Regular.woff2') }` | rebrand | Nerd Font rename |

**Our pattern:** Font families `Geist`/`Geist Mono` → `Inter`. Nerd Font `Orca Nerd Font Symbols` → `Fabrica Nerd Font Symbols`. File `SymbolsNerdFontMono-Regular.woff2` → `FabricaNerdFontSymbols-Regular.woff2`.

#### `src/assets/terminal.css` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L11 | `--orca-terminal-link-tooltip-height: 24px;` | `--fabrica-terminal-link-tooltip-height: 24px;` | rebrand | CSS variable |

**Our pattern:** CSS custom properties `--orca-*` → `--fabrica-*`.

#### `src/assets/mobile-page.css` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L1 | `/* orca-mobile-sidebar-mock */` | `/* FABRICA-mobile-sidebar-mock */` | rebrand | Comment string |

#### `src/assets/markdown-preview.css` — identical (no diff)
#### `src/assets/rich-markdown-editor.css` — identical (no diff)

---

### A3. Renderer TypeScript/TSX

#### `src/main.tsx` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L47 | `'Orca hit a renderer error'` | `'Fabrica hit a renderer error'` | rebrand | Error message |

#### `src/App.tsx` — rebrand
| # | Line(s) | Baseline | Fabrica | Intent | Pattern |
|---|---------|----------|---------|--------|---------|
| 1 | Various | Multiple `Orca` refs in component names, strings, comments | Multiple `Fabrica` refs | rebrand | Identity substitution |

---

### A4. Renderer Components

#### `src/components/UpdateCard.tsx` — rebrand + incidental
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L303 | `const errorCard: ...` | `const errCard: ...` | incidental | Variable rename |
| 2 | L331 | `summary: 'Orca can retry...'` | `summary: 'Fabrica can retry...'` | rebrand | User-facing string |
| 3 | L355 | `...doesn't match Orca...` | `...doesn't match Fabrica...` | rebrand | User-facing string |
| 4 | L383 | `// don't read as an Orca bug.` | `// don't read as an FABRICA bug.` | rebrand | Comment |
| 5 | L780 | `'Orca v{{value0}} is ready.'` | `'Fabrica v{{value0}} is ready.'` | rebrand | Update notification |
| 6 | L884 | `'Orca v{{value0}} is downloading.'` | `'Fabrica v{{value0}} is downloading.'` | rebrand | Update notification |
| 7 | L945 | `"Orca v{{value0}} is downloaded..."` | `"Fabrica v{{value0}} is downloaded..."` | rebrand | Update notification |

#### `src/components/UpdateCard.test.ts` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L24 | `mediaUrl: 'https://onorca.dev/media/inline-diffs.png'` | `mediaUrl: 'https://fabrica-ai.vercel.app/media/inline-diffs.png'` | rebrand | DNS domain |
| 2 | L25 | `releaseNotesUrl: 'https://onorca.dev/changelog/1.2.0'` | `releaseNotesUrl: 'https://fabrica-ai.vercel.app/changelog/1.2.0'` | rebrand | DNS domain |

#### `src/components/UpdateErrorCardContent.tsx` — identical (no diff)

#### `src/components/LinuxPackageInstallRecoveryCard.tsx` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L14 | `...quit and reopen Orca.` | `...quit and reopen Fabrica.` | rebrand | User-facing string |
| 2 | L46 | `Orca downloaded the update...` | `Fabrica downloaded the update...` | rebrand | User-facing string |
| 3 | L50 | `...where Orca is installed.` | `...where Fabrica is installed.` | rebrand | User-facing string |
| 4 | L58 | `Orca checks the downloaded file...` | `Fabrica checks the downloaded file...` | rebrand | User-facing string |

#### `src/components/editor/CombinedDiffViewer.tsx` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L62 | `ORCA_EDITOR_EXTERNAL_FILE_CHANGE_EVENT` | `FABRICA_EDITOR_EXTERNAL_FILE_CHANGE_EVENT` | rebrand | Constant rename |
| 2 | L153 | `window.addEventListener(ORCA_EDITOR_EXTERNAL_FILE_CHANGE_EVENT, ...)` | `window.addEventListener(FABRICA_EDITOR_EXTERNAL_FILE_CHANGE_EVENT, ...)` | rebrand | Reference |
| 3 | L1206 | `window.addEventListener(ORCA_EDITOR_EXTERNAL_FILE_CHANGE_EVENT, ...)` | `window.addEventListener(FABRICA_EDITOR_EXTERNAL_FILE_CHANGE_EVENT, ...)` | rebrand | Reference |
| 4 | L1208 | `window.removeEventListener(ORCA_EDITOR_EXTERNAL_FILE_CHANGE_EVENT, ...)` | `window.removeEventListener(FABRICA_EDITOR_EXTERNAL_FILE_CHANGE_EVENT, ...)` | rebrand | Reference |

#### `src/components/terminal-pane/pty-transport.test.ts` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L2296 | `'No PTY provider for connection runtime-ssh-orca-1'` | `'No PTY provider for connection runtime-ssh-FABRICA-1'` | rebrand | Test string |
| 2 | L2315 | `connectionId: 'runtime-ssh-orca-1'` | `connectionId: 'runtime-ssh-FABRICA-1'` | rebrand | Test fixture |

#### `src/components/Landing.tsx` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L27 | `const ORCA_GITHUB_URL = 'https://github.com/stablyai/orca'` | `const FABRICA_GITHUB_URL = 'https://github.com/Auto-Scalers/Fabrica-app'` | rebrand | GitHub URL + constant |
| 2 | L39 | `window.api.gh.checkOrcaStarred()` | `window.api.gh.checkFABRICAStarred()` | rebrand | API call rename |
| 3 | L73 | `window.api.shell.openUrl(ORCA_GITHUB_URL)` | `window.api.shell.openUrl(FABRICA_GITHUB_URL)` | rebrand | Reference |
| 4 | L80 | `await window.api.gh.starOrca('landing')` | `await window.api.gh.starFABRICA('landing')` | rebrand | API call rename |
| 5 | L271 | `translate(..., 'Orca')` | `translate(..., 'FABRICA')` | rebrand | i18n key |

**Our pattern:** GitHub org `stablyai` → `Auto-Scalers`. Repo `orca` → `Fabrica-app`. API methods `starOrca`/`checkOrcaStarred` → `starFABRICA`/`checkFABRICAStarred`.

#### `src/components/NewWorkspaceComposerCard.tsx` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L43 | `OrcaHooks` | `FABRICAHooks` | rebrand | Type reference |
| 2 | L217 | `// ...the source label (orca.yaml / local)` | `// ...the source label (FABRICA.yaml / local)` | rebrand | Comment |
| 3 | L1023 | `orca.yaml is a...` | `FABRICA.yaml is a...` | rebrand | JSX comment |
| 4 | L1029 | `'orca.yaml'` | `'FABRICA.yaml'` | rebrand | User-facing string |
| 5 | L1034 | `'orca.yaml + local'` | `'FABRICA.yaml + local'` | rebrand | User-facing string |

**Our pattern:** `orca.yaml` → `FABRICA.yaml` in user-facing labels and comments.

#### `src/components/NewWorkspaceComposerCard.test.tsx` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L147 | `path: '/Users/alice/orca'` | `path: '/Users/alice/FABRICA'` | rebrand | Test fixture |
| 2 | L796 | `create: './scripts/orca-vm/vercel.start.sh'` | `create: './scripts/FABRICA-vm/vercel.start.sh'` | rebrand | Test fixture |

#### `src/components/NewWorkspaceComposerModal.tsx` — identical (no diff)
#### `src/components/PullRequestPage.tsx` — identical (no diff)

#### `src/components/StarNagCard.tsx` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L9 | `const ORCA_REPO_URL = 'https://github.com/stablyai/orca'` | `const FABRICA_REPO_URL = 'https://github.com/Auto-Scalers/Fabrica-app'` | rebrand | GitHub URL + constant |
| 2 | L13 | `* Persistent "star Orca on GitHub" notification card.` | `* Persistent "star FABRICA on GitHub" notification card.` | rebrand | JSDoc |
| 3 | L100 | `await window.api.shell.openUrl(ORCA_REPO_URL)` | `await window.api.shell.openUrl(FABRICA_REPO_URL)` | rebrand | Reference |
| 4 | L126 | `ok = await window.api.starNag.starOrca()` | `ok = await window.api.starNag.starFABRICA()` | rebrand | API call rename |

#### `src/components/StarNagCard.test.tsx` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L17 | `starOrca: ReturnType<typeof vi.fn>` | `starFABRICA: ReturnType<typeof vi.fn>` | rebrand | Mock type |
| 2 | L56 | `starOrca: vi.fn().mockResolvedValue(true)` | `starFABRICA: vi.fn().mockResolvedValue(true)` | rebrand | Mock setup |
| 3 | L74 | `starNag.starOrca.mockResolvedValueOnce(false)` | `starNag.starFABRICA.mockResolvedValueOnce(false)` | rebrand | Mock call |
| 4 | L89 | `expect(starNag.starOrca).toHaveBeenCalledTimes(1)` | `expect(starNag.starFABRICA).toHaveBeenCalledTimes(1)` | rebrand | Assertion |

---

## SECTION B: Shared Module Renames (orca-* → fabrica-*)

These 9 files were **renamed** (filename + all internal identifiers):

| # | Baseline file | Fabrica file | Identifiers renamed | Intent |
|---|---------------|--------------|-------------------|--------|
| 1 | `orca-attribution.ts` | `fabrica-attribution.ts` | `ORCA_GIT_COMMIT_TRAILER` → `FABRICA_GIT_COMMIT_TRAILER`, email `help@stably.ai` → `fabrica.studio.contact@gmail.com` | rebrand |
| 2 | `orca-cli-command-name.ts` | `fabrica-cli-command-name.ts` | `getOrcaCliCommandNameForPlatform` → `getFABRICACliCommandNameForPlatform`; linux `orca-ide` → `fabrica` | rebrand |
| 3 | `orca-dispatch-status-prompt.ts` | `fabrica-dispatch-status-prompt.ts` | All 9 identifiers: `ORCA_DISPATCH_STATUS_*` → `FABRICA_DISPATCH_STATUS_*`; `isOrcaDispatchStatusPrompt` → `isFABRICADispatchStatusPrompt`; string `'Orca, a multi-agent IDE'` → `'FABRICA, a multi-agent IDE'` | rebrand |
| 4 | `orca-profiles.ts` | `fabrica-profiles.ts` | All types: `OrcaProfile*` → `FABRICAProfile*`, `OrcaCloud*` → `FABRICACloud*`, `OrcaOrg*` → `FABRICAOrg*`; functions: `createDefaultLocalOrcaProfile` → `createDefaultLocalFABRICAProfile`, `getOrcaProfileBrowser*` → `getFABRICAProfileBrowser*`; string `persist:orca-profile-` → `persist:FABRICA-profile-` | rebrand |
| 5 | `orca-yaml.ts` | `fabrica-yaml.ts` | All types/functions: `OrcaDefaultTabTemplate` → `FABRICADefaultTabTemplate`, `OrcaVmRecipe` → `FABRICAVmRecipe`, `parseOrcaYaml` → `parseFABRICAYaml`, `MAX_ORCA_YAML_*` → `MAX_FABRICA_YAML_*` | rebrand |
| 6 | `orca-yaml-bounds.test.ts` | `fabrica-yaml-bounds.test.ts` | Test references renamed to match fabrica-yaml | rebrand |
| 7 | `orca-yaml-alias-bounds.test.ts` | `fabrica-yaml-alias-bounds.test.ts` | Test references renamed | rebrand |
| 8 | `orca-yaml-file-limit.ts` | `fabrica-yaml-file-limit.ts` | `isOrcaYamlFieldWithinLimit` → `isFABRICAYamlFieldWithinLimit`, `MAX_ORCA_YAML_*` → `MAX_FABRICA_YAML_*` | rebrand |
| 9 | `telemetry-orca-cli-feature-tip.test.ts` | `telemetry-fabrica-cli-feature-tip.test.ts` | Test references renamed | rebrand |

**Our pattern:** Complete mechanical `orca`/`Orca`/`ORCA` → `fabrica`/`FABRICA`/`FABRICA` substitution at filename, type, constant, function, and string levels. No logic changes.

---

## SECTION C: Shared Module New File

### `supabase-auth.ts` — custom-logic (NEW, Fabrica-only)

20 lines. Exports:
```ts
export type SupabaseAuthStatus = { configured: boolean; signedIn: boolean; email?: string }
export type SignInSupabaseArgs = { email: string; password: string }
export type SignInSupabaseResult = { ok: boolean; error?: string }
export type SignOutSupabaseResult = { ok: boolean; error?: string }
```

**Our pattern:** Entirely new Supabase auth infrastructure not present in Orca. Must be preserved verbatim in any sync.

---

## SECTION D: Shared Module Core Identity Files

### `constants.ts` — rebrand + custom-logic
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L50 | `DEFAULT_APP_FONT_FAMILY = 'Geist'` | `DEFAULT_APP_FONT_FAMILY = 'Inter'` | **custom-logic** | Font swap (Geist→Inter) |
| 2 | L64 | `ORCA_BROWSER_PARTITION = 'persist:orca-browser'` | `FABRICA_BROWSER_PARTITION = 'persist:FABRICA-browser'` | rebrand | Constant rename + string |
| 3 | L66 | `ORCA_BROWSER_BLANK_URL` | `FABRICA_BROWSER_BLANK_URL` | rebrand | Constant rename |
| 4 | L171 | `return [trimmedHomeDir, 'orca', 'workspaces']` | `return [trimmedHomeDir, 'Fabrica', 'workspaces']` | rebrand | Workspace dir path |
| 5 | L287 | `terminalShortcutPolicy: 'orca-first'` | `terminalShortcutPolicy: 'FABRICA-first'` | rebrand | Enum value (note: case inconsistency) |
| 6 | L507 | `trustedOrcaHooks: {}` | `trustedFABRICAHooks: {}` | rebrand | Property key |

### `types.ts` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L680 | `orcaCreatedAt?: number` | `FABRICACreatedAt?: number` | rebrand | Field rename |
| 2 | L681 | `orcaCreationSource?: ...` | `FABRICACreationSource?: ...` | rebrand | Field rename |
| 3 | L683 | `orcaCreationWorkspaceLayout?: OrcaWorkspaceLayout` | `FABRICACreationWorkspaceLayout?: FABRICAWorkspaceLayout` | rebrand | Field + type rename |
| 4 | L699 | `WorktreeOwnership = 'orca-managed' \| ...` | `WorktreeOwnership = 'FABRICA-managed' \| ...` | rebrand | String literal |
| 5 | L3180 | `export type OrcaWorkspaceLayout = {` | `export type FABRICAWorkspaceLayout = {` | rebrand | Type rename |
| 6 | ~20 comments | `Orca` in JSDoc | `FABRICA` in JSDoc | rebrand | Comment text |

**Our pattern:** All `Orca`/`orca` identifiers, `orca-data.json` persistence refs, and `'orca-managed'` string literals → `FABRICA` equivalents.

### `app-icon.ts` — rebrand + custom-logic
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L2 | `{ id: 'classic', label: 'Classic Orca' }` | `{ id: 'classic', label: 'Classic FABRICA' }` | rebrand | Label |
| 2 | L3 | `{ id: 'watercolor', label: 'Watercolor Orca' }` | `{ id: 'dark', label: 'Dark FABRICA' }` | **custom-logic** | Icon set redesign |
| 3 | L4 | `{ id: 'blue', label: 'Blue Orca' }` | `{ id: 'light', label: 'Light FABRICA' }` | **custom-logic** | Icon set redesign |
| 4 | L9 | `DEFAULT_APP_ICON_ID = 'classic'` | `DEFAULT_APP_ICON_ID = 'light'` | **custom-logic** | Different default |

**Our pattern:** Icon options entirely replaced (Orca watercolor/blue → Fabrica dark/light). Default changed.

### `app-identity.ts` — identical (no diff)

### `release-channel.ts` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L24 | `HOURLY_RELEASE_REPO = 'stablyai/orca-hourly'` | `HOURLY_RELEASE_REPO = 'Auto-Scalers/Fabrica-hourly'` | rebrand | GitHub org+repo |
| 2 | L25 | `DAILY_RELEASE_REPO = 'stablyai/orca-daily'` | `DAILY_RELEASE_REPO = 'Auto-Scalers/Fabrica-daily'` | rebrand | GitHub org+repo |
| 3 | L26 | `ADHOC_RELEASE_REPO = 'stablyai/orca-adhoc'` | `ADHOC_RELEASE_REPO = 'Auto-Scalers/Fabrica-adhoc'` | rebrand | GitHub org+repo |
| 4 | L27 | `MAIN_RELEASE_REPO = 'stablyai/orca'` | `MAIN_RELEASE_REPO = 'Auto-Scalers/Fabrica-app'` | rebrand | GitHub org+repo |

**Our pattern:** GitHub org `stablyai` → `Auto-Scalers`. Repo names `orca*` → `Fabrica*`. Main repo is `Fabrica-app` (not just `Fabrica`).

### `opencode-usage-types.ts` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L1 | `export type OpenCodeUsageScope = 'orca' \| 'all'` | `export type OpenCodeUsageScope = 'FABRICA' \| 'all'` | rebrand | String literal |

### `telemetry-events.ts` — rebrand
| # | Line | Baseline | Fabrica | Intent | Pattern |
|---|------|----------|---------|--------|---------|
| 1 | L301 | `const appStarredOrcaSchema = z.object(...)` | `const appStarredFABRICASchema = z.object(...)` | rebrand | Schema name |
| 2 | L542 | `const orcaCliFeatureTipSourceSchema = z.enum(...)` | `const FABRICACliFeatureTipSourceSchema = z.enum(...)` | rebrand | Schema name |
| 3 | L543-556 | `orcaCliFeatureTip*Schema` refs | `FABRICACliFeatureTip*Schema` refs | rebrand | All schema references |
| 4 | ~10 comments | `Orca` in JSDoc | `FABRICA` in JSDoc | rebrand | Comment text |

---

## SECTION E: Shared Module Agent/Runtime/Infrastructure Files

These files show heavy `fabrica` reference counts due to identifier renames in agent configs, VM recipes, WSL paths, remote runtime, etc.

### High-count files (by fabrica match count):
| File | fabrica refs | Key pattern |
|------|-------------|-------------|
| `worktree-ownership.ts` | 28 | `FABRICA-managed` string literal in ownership logic |
| `remote-runtime-client.ts` | 29 | `fabrica` in connection identifiers |
| `wsl-login-shell-command.ts` | 24 | `fabrica` in WSL path/command strings |
| `agent-hook-relay.ts` | 22 | `fabrica` in hook relay identifiers |
| `posix-command-path-lookup.ts` | 21 | `fabrica` in CLI path lookups |
| `agent-hook-listener.ts` | 15 | `fabrica` in hook listener identifiers |
| `agent-feature-install-commands.ts` | 19 | `fabrica` in install command strings |
| `computer-use-error-recovery.ts` | 18 | `fabrica` in error recovery strings |
| `telemetry-events.ts` | 23 | `FABRICA` in telemetry schema names |
| `remote-runtime-request-frames.ts` | 8 | `fabrica` in protocol frame identifiers |

**Our pattern across all these files:** Mechanical `orca`/`Orca`/`ORCA` → `fabrica`/`FABRICA`/`FABRICA` substitution in identifiers, string literals, path segments, connection IDs, and protocol markers. No logic changes observed in the agent/runtime layer — pure rebrand.

---

## SECTION F: Cross-Cutting Rebrand Patterns

### Pattern Registry (from renderer + shared)

| # | Pattern | Baseline | Fabrica | Areas | Intent |
|---|---------|----------|---------|-------|--------|
| 1 | **App name** | `Orca` | `Fabrica` | HTML titles, UI strings, error messages, comments | rebrand |
| 2 | **App name (constant)** | `ORCA`/`Orca` | `FABRICA`/`Fabrica` | TypeScript identifiers, type names, function names | rebrand |
| 3 | **DNS domain** | `onorca.dev` | `fabrica-ai.vercel.app` | URLs, test fixtures | rebrand |
| 4 | **GitHub org** | `stablyai` | `Auto-Scalers` | Release repos, star nag, landing page | rebrand |
| 5 | **GitHub repo** | `orca` | `Fabrica-app` | Release repos, landing page | rebrand |
| 6 | **CLI command** | `orca`/`orca.cmd`/`orca-ide` | `fabrica`/`fabrica.cmd`/`fabrica` | CLI names | rebrand |
| 7 | **Font family** | `Geist`/`Geist Mono` | `Inter` | CSS, constants | **custom-logic** |
| 8 | **Nerd Font** | `Orca Nerd Font Symbols` | `Fabrica Nerd Font Symbols` | CSS @font-face | rebrand |
| 9 | **CSS variables** | `--orca-*` | `--fabrica-*` | CSS custom properties | rebrand |
| 10 | **YAML config file** | `orca.yaml` | `FABRICA.yaml` | UI labels, comments | rebrand |
| 11 | **Persistence files** | `orca-data.json` | `FABRICA-data.json` | JSDoc, comments | rebrand |
| 12 | **Commit trailer** | `Co-authored-by: Orca <help@stably.ai>` | `Co-authored-by: Fabrica <fabrica.studio.contact@gmail.com>` | Git attribution | rebrand |
| 13 | **Workspace path** | `~/.../orca/workspaces` | `~/.../Fabrica/workspaces` | Directory paths | rebrand |
| 14 | **Browser partition** | `persist:orca-browser` / `persist:orca-profile-*` | `persist:FABRICA-browser` / `persist:FABRICA-profile-*` | Electron partitions | rebrand |
| 15 | **Ownership literal** | `'orca-managed'` | `'FABRICA-managed'` | Worktree ownership type | rebrand |
| 16 | **Shortcut policy** | `'orca-first'` | `'FABRICA-first'` | Terminal shortcut policy | rebrand |
| 17 | **Icon set** | watercolor/blue Orca icons | dark/light Fabrica icons | App icons | **custom-logic** |
| 18 | **Supabase auth** | (none) | `supabase-auth.ts` | Auth types | **custom-logic** (new) |
| 19 | **Email** | `help@stably.ai` | `fabrica.studio.contact@gmail.com` | Git trailer | rebrand |
| 20 | **Usage scope** | `'orca'` | `'FABRICA'` | Usage types | rebrand |
| 21 | **VM scripts** | `./scripts/orca-vm/` | `./scripts/FABRICA-vm/` | Ephemeral VM scripts | rebrand |

---

## SECTION G: Sync Implications

### Preservation rules (what must NOT change on sync):
1. All `Fabrica`/`FABRICA` identifiers must be preserved — any upstream Orca identifier update must be adapted.
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
12. `supabase-auth.ts` must be preserved (Fabrica-only).
13. `'FABRICA-managed'` ownership literal must be preserved.
14. `'FABRICA-first'` shortcut policy must be preserved.

### Risk areas for sync:
- **Large type rename surface** (types.ts, fabrica-profiles.ts) — upstream type additions must be renamed.
- **Agent/runtime identifiers** — upstream hook/relay/VM recipe changes must be adapted.
- **Telemetry schemas** — upstream schema additions must be renamed.
- **Plugin manifests** — upstream plugin changes must use `fabrica` identifiers.

---

## Section H: Incidental Changes

2 incidental changes found (non-rebrand):
1. `UpdateCard.tsx:303` — variable rename `errorCard` → `errCard` (trivial refactor)
2. `UpdateCard.tsx:524` — corresponding reference update

These are safe to drop on sync if upstream has a different refactoring.

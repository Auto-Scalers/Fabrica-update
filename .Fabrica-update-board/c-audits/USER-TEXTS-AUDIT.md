# User-Facing Texts Audit

**Date:** 2026-09-06
**Scope:** Fabrica repository — locale files, UI components, documentation, GitHub-facing text

---

## Executive Summary

The product name "Fabrica" is used **consistently across the entire codebase**. No "Orca" references were found in any source code, locale files, or UI components. The main issues are: 3 documentation files still reference "Orca" instead of "Fabrica", 16 GitHub workflow/script references use internal "orca" identifiers, 5 of 6 non-English locales are missing the `editor` section, and significant untranslated English text exists in Korean, Spanish, Chinese, and Japanese locales. No Arabic (ar.json) locale file exists.

---

## 1. Locale Files Audit

### Files Found (6 of 7 requested)
| File | Status |
|------|--------|
| `en.json` | ✅ Present (17,537 lines) |
| `fr.json` | ✅ Present |
| `ko.json` | ✅ Present |
| `ja.json` | ✅ Present |
| `zh.json` | ✅ Present |
| `es.json` | ✅ Present |
| `ar.json` | ❌ **Missing** — no Arabic locale file exists |

### Top-Level Key Coverage
| Section | en | fr | ko | ja | zh | es |
|---------|----|----|----|----|----|----|
| `app` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `browser` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| `editor` | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `githubChecks` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `settings` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `menu` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `tray` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `worktreeJumpPalette` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `auto` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

**Key finding:** 5 of 6 non-English locales are missing the top-level `editor` section. 4 of 6 also miss the `browser` section.

### Product Name References in Locales
- **No "Orca" references found** in any locale file
- All product name references consistently use "Fabrica" ✅

### Untranslated English Strings
Significant untranslated blocks exist in:
- **ko.json**: `linear.usage.examples` (~10 strings), `browser.cookie.import.toast` (all), `feedback.image.attachments` (all), `codex.session.restart`
- **ja.json**: `browser.cookie.import.toast` (2 strings), `codex.session.restart`
- **zh.json**: `browser.cookie.import.toast` (2 strings), `codex.session.restart`, `pluginCommandKeybindings.group` = "Plugins"
- **es.json**: `linear.usage.examples` (~6 strings), `browser.cookie.import.toast`, `feedback.image.attachments` (all), `pluginCommandKeybindings.group`
- **fr.json**: `pluginCommandKeybindings.group` = "Plugins", `tray.activityWaitingSuffix` partially English

### Missing Key Areas vs en.json
| Area | fr | ko | ja | zh | es |
|------|----|----|----|----|----|
| `editor` (top-level) | Y | Y | Y | Y | Y |
| `browser` (top-level) | N | Y | Y | Y | Y |
| `auto.store.slices.runtime.status.*` | N | Y | Y | Y | Y |
| `auto.lib.browser.cookie.import.toast.*` (full set) | N | Y | Y | Y | Y |
| `auto.components.GitLabItemDialog.*` | N | Y | N | N | N |
| `auto.components.Landing.*` | N | Y | N | N | N |
| `auto.components.PullRequestPage.*` | N | Y | N | N | N |
| `auto.components.TaskPage.*` | N | Y | N | N | N |

(Y = missing, N = present or partially present)

---

## 2. UI Components Audit

### Product Name in Source Code
- **Zero "Orca" references** found in any TypeScript/TSX files
- `src/renderer/src/components/` — clean ✅
- `src/renderer/src/` (excluding i18n) — clean ✅
- `src/main/` — clean ✅
- Entire codebase (`*.ts`, `*.tsx`, `*.js`, `*.jsx`, `*.json`) — clean ✅

### package.json
- `name: "fabrica"` ✅
- `homepage: "https://github.com/fabrica-ai/fabrica"` ✅
- `author: "fabrica-ai"` ✅
- Binary names: `"fabrica"` and `"fabrica-dev"` ✅

### Window Titles
| File | Title | Status |
|------|-------|--------|
| `src/main/window/createMainWindow.ts:91` | `'Fabrica'` | ✅ |
| `src/main/window/dashboard-popout-window.ts:168` | `'Fabrica Agent Dashboard'` | ✅ |
| `src/main/window/main-window-close-lifecycle.ts:78` | `'Fabrica'` | ✅ |
| `src/main/window/renderer-recovery-prompt.ts:37` | `'Fabrica keeps failing to load'` | ✅ |

### Menu Labels
All menu labels internationalized via `translateMain()` — use "Fabrica" consistently ✅

---

## 3. Documentation Audit

### README.md
- Uses "Fabrica" throughout ✅
- No "Orca" references ✅

### CONTRIBUTING.md
- Does not exist ❌ (not required for this audit)

### WINDOWS_SETUP_GUIDE.md
- Does not exist ❌ (not required for this audit)

### docs/reference/*.md — "Orca" References Found
| File | Line | Issue | Current Text |
|------|------|-------|--------------|
| `docs/reference/sharing-agent-skills.md` | 45 | Incorrect product name | "**Keep local** is the default when an existing skill differs. **Orca** replaces modified content only" |
| `docs/reference/agent-skill-sharing-threat-model.md` | 80 | Incorrect product name | "Skill instructions or scripts are mistaken for trusted **Orca** code" |
| `docs/reference/agent-skill-provider-paths.md` | 8 | Incorrect table header | Column header says "**Orca** placement" |

**Total: 3 documentation files with incorrect "Orca" references** ❌

---

## 4. GitHub-Facing Text Audit

### Issue/PR Templates
- No `.md` templates found in `.github/` directory

### Workflow/Script "Orca" References
These appear to be internal identifiers (project slugs, token names, user-agents), not user-facing text:

| File | Line(s) | Type | Reference |
|------|---------|------|-----------|
| `.github/workflows/win-crash-survival-e2e.yml` | 113 | Path reference (comment) | `%LOCALAPPDATA%\Programs\Orca` |
| `.github/workflows/windows-signing-rehearsal.yml` | 152, 216, 226 | Config values | `project-slug: orca`, `name: orca-windows-installer-unsigned` |
| `.github/workflows/homebrew-bump.yml` | 11, 73, 76, 77 | Config values | `token="orca"`, `token="orca@rc"` |
| `.github/workflows/dev-channel-win-build.yml` | 264 | Config value | `repositories: orca-${{ inputs.channel }}` |
| `.github/workflows/release-cut.yml` | 1519, 1730 | Config values | `project-slug: orca` |
| `.github/scripts/pr-test-loc-summary.mjs` | 26 | User-Agent | `User-Agent: orca-pr-test-loc` |
| `.github/scripts/pr-test-loc-table.mjs` | 1-2 | Comment markers | `orca-pr-loc` |
| `.github/scripts/render-readme-downloads-badge.mjs` | 4, 10 | Config/User-Agent | `'Auto-Scalers/orca'`, `orca-readme-badge` |
| `.github/actions/cloud-sql-rollout-lease/storage-lease.test.mjs` | 191 | Test assertion | `Auto-Scalers/orca` |

**Note:** These 16 references are internal identifiers, not user-facing. They may be intentional for API compatibility. If renaming to "Fabrica" is planned, these should be updated carefully with awareness of API dependencies.

---

## 5. Summary of Issues

### Critical (User-Visible)
1. **3 documentation files** reference "Orca" instead of "Fabrica" — these are in `docs/reference/` and may be read by users
2. **No Arabic locale** — `ar.json` does not exist

### High Priority
3. **Missing locale sections**: `editor` missing from 5 locales, `browser` missing from 4 locales
4. **Substantial untranslated English** in ko.json, es.json, zh.json, ja.json (Linear examples, cookie import toasts, feedback image strings)

### Low Priority (Internal)
5. **16 GitHub workflow/script references** to "orca" — these are internal identifiers, not user-facing

### No Issues Found
- ✅ Product name "Fabrica" used consistently in all source code
- ✅ package.json correctly configured
- ✅ Window titles use "Fabrica"
- ✅ Menu labels use "Fabrica"
- ✅ README.md uses "Fabrica"
- ✅ All locale files use "Fabrica" (no "Orca" contamination)

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — 3 docs with "Orca" references (Critical)
**PM Decision:** 
**Notes:** 

### Finding 2 — No Arabic locale (Critical)
**PM Decision:** 
**Notes:** 

### Finding 3 — Missing locale sections (editor/browser) (High)
**PM Decision:** 
**Notes:** 

### Finding 4 — Untranslated English in ko/es/zh/ja (High)
**PM Decision:** 
**Notes:** 

### Finding 5 — 16 GitHub workflow "orca" identifiers (Low)
**PM Decision:** 
**Notes:** 

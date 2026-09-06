# C12: Binary Assets Audit

**Audit Date:** 2026-09-06
**Repo:** `Fabrica/`

---

## Executive Summary

**7 orca-named binaries remain**: 2 app icons, 2 tray icons, and 3 compiled binaries (darwin/linux/win32). All contain correct Fabrica brand imagery — only the filenames are wrong. The orca→fabrica rename is incomplete for binary assets.

---

## 1. Orca-Named App Icons

| File | Content | Brand | Status |
|------|---------|-------|--------|
| `resources/app-icons/orca-blue.png` | Fabrica "F" on blue | ✅ Correct | ⚠️ Wrong filename |
| `resources/app-icons/orca-watercolor.png` | Fabrica "F" watercolor | ✅ Correct | ⚠️ Wrong filename |

---

## 2. Orca-Named Tray Icons

| File | Content | Brand | Status |
|------|---------|-------|--------|
| `resources/tray/orca-menu-barTemplate.png` | Fabrica "F" black silhouette | ✅ Correct | ⚠️ Wrong filename |
| `resources/tray/orca-menu-barTemplate@2x.png` | Fabrica "F" black silhouette | ✅ Correct | ⚠️ Wrong filename |

---

## 3. Compiled Binaries

| File | Purpose | Status |
|------|---------|--------|
| `resources/darwin/bin/orca` | macOS CLI binary | ⚠️ Still named "orca" |
| `resources/linux/bin/orca-ide` | Linux CLI binary | ⚠️ Still named "orca-ide" |
| `resources/win32/bin/orca.cmd` | Windows CLI wrapper | ⚠️ Still named "orca.cmd" |

---

## 4. Correctly Named Assets

| File | Status |
|------|--------|
| `resources/build/icon.png` | ✅ Correct |
| `resources/build/icon.ico` | ✅ Correct |
| `resources/build/icon.icns` | ✅ Correct |
| `resources/icon.png` | ✅ Correct |
| `resources/icon-dev.png` | ✅ Correct |
| `resources/logo.svg` | ✅ Correct |
| `resources/app-icons/fabrica-dark.png` | ✅ Correct |
| `resources/app-icons/fabrica-light.png` | ✅ Correct |
| `resources/icon-source/fabrica-logo_icon.png` | ✅ Correct |
| `mobile/assets/icon.png` | ✅ Correct |
| `mobile/assets/adaptive-icon.png` | ✅ Correct |
| `mobile/assets/splash-icon.png` | ✅ Correct |

---

## 5. Font Files

| Font | Status |
|------|--------|
| Geist-Variable.woff2 | ✅ Present |
| Inter (7 woff2 subsets) | ✅ Present |
| Space Grotesk (3 woff2) | ✅ Present |
| JetBrains Mono (6 woff2 subsets) | ✅ Present |
| Fabrica Nerd Font Symbols | ✅ Present |

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — 2 orca-named app icons (content correct, filename wrong)
**PM Decision:** 
**Notes:** 

### Finding 2 — 2 orca-named tray icons (content correct, filename wrong)
**PM Decision:** 
**Notes:** 

### Finding 3 — 3 compiled orca-named binaries
**PM Decision:** 
**Notes:** 

### Finding 4 — All other brand assets correct (Info)
**PM Decision:** 
**Notes:** 

### Finding 5 — All font files present (Info)
**PM Decision:** 
**Notes:** 

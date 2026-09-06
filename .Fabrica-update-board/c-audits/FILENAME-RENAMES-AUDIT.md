# C8: Filename Renames Verification

**Audit Date:** 2026-09-06
**Repo:** `Fabrica/`

---

## Executive Summary

**Zero renames found.** All 344 files and 17 directories with `orca` in their names still use the old brand. The T5 rebrand pass changed file CONTENTS but not FILENAMES. The REBRAND-INTENT-MAP listed 132 files to rename, but no rename commits exist in git history. Only 6 new fabrica-prefixed files exist (new resources, not renames).

---

## 1. Source Files (REBRAND-INTENT-MAP Listed)

| Expected Rename | Actual State | Status |
|----------------|--------------|--------|
| `orca-runtime.ts` → `fabrica-runtime.ts` | Still `orca-runtime.ts` | ❌ NOT RENAMED |
| `linux-bare-orca-dispatcher.ts` → `linux-bare-fabrica-dispatcher.ts` | Still `linux-bare-orca-dispatcher.ts` | ❌ NOT RENAMED |
| `orca-yaml.ts` → `fabrica-yaml.ts` | Still `orca-yaml.ts` | ❌ NOT RENAMED |
| `OrcaAccountSettingsPane.tsx` → `FabricaAccountSettingsPane.tsx` | Still `OrcaAccountSettingsPane.tsx` | ❌ NOT RENAMED |
| `OrcaYamlTrustDialog.tsx` → `FabricaYamlTrustDialog.tsx` | Still `OrcaYamlTrustDialog.tsx` | ❌ NOT RENAMED |
| `orca-profiles/` → `fabrica-profiles/` | Still `orca-profiles/` | ❌ NOT RENAMED |
| `orca-app.ts` → `fabrica-app.ts` | Still `orca-app.ts` | ❌ NOT RENAMED |
| `orca-restart.ts` → `fabrica-restart.ts` | Still `orca-restart.ts` | ❌ NOT RENAMED |

**All source file renames were NOT applied.**

---

## 2. Resource Files

| Expected Rename | Actual State | Status |
|----------------|--------------|--------|
| `orca-menu-barTemplate.png` → `fabrica-menu-barTemplate.png` | Still `orca-menu-barTemplate.png` | ❌ NOT RENAMED |
| `stablyai.orca-*` → `autoscalers.fabrica-*` | Still `stablyai.orca-*` | ❌ NOT RENAMED |
| `resources/darwin/bin/orca` → `fabrica` | Still `orca` | ❌ NOT RENAMED |
| `resources/linux/bin/orca-ide` → `fabrica` | Still `orca-ide` | ❌ NOT RENAMED |
| `resources/win32/bin/orca.cmd` → `fabrica.cmd` | Still `orca.cmd` | ❌ NOT RENAMED |

---

## 3. Native Files

| Expected Rename | Actual State | Status |
|----------------|--------------|--------|
| `OrcaComputerUse*` → `FabricaComputerUse*` | Still `OrcaComputerUse*` | ❌ NOT RENAMED |
| `OrcaCliLauncher.cs` → `FabricaCliLauncher.cs` | Still `OrcaCliLauncher.cs` | ❌ NOT RENAMED |

---

## 4. Config/Scripts

| Expected Rename | Actual State | Status |
|----------------|--------------|--------|
| `orca-dev.mjs` → `fabrica-dev.mjs` | Still `orca-dev.mjs` | ❌ NOT RENAMED |
| `Casks/orca.rb` → `fabrica.rb` | Still `orca.rb` | ❌ NOT RENAMED |
| `hello-orca/` → `hello-fabrica/` | Still `hello-orca/` | ❌ NOT RENAMED |

---

## 5. New Fabrica-Prefixed Files (Not Renames)

| File | Notes |
|------|-------|
| `resources/app-icons/fabrica-dark.png` | New resource |
| `resources/app-icons/fabrica-light.png` | New resource |
| `resources/icon-source/fabrica-logo_icon.png` | New resource |
| `config/scripts/build-fabrica-icons.mjs` | New script |
| `config/scripts/trim-windows-icon-source.mjs` | New script |
| `resources/icon-source/generate.sh` | New script |

These are new files, not renames of orca-named files.

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — Zero source file renames applied
**PM Decision:** 
**Notes:** 

### Finding 2 — Zero resource file renames applied
**PM Decision:** 
**Notes:** 

### Finding 3 — Zero native file renames applied
**PM Decision:** 
**Notes:** 

### Finding 4 — Zero config/script renames applied
**PM Decision:** 
**Notes:** 

### Finding 5 — New fabrica-prefixed files exist (not renames)
**PM Decision:** 
**Notes:** 

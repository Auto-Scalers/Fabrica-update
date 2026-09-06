# C1: Rebrand Completeness Audit

**Audit Date:** 2026-09-06
**Repo:** `Fabrica/` (fresh clone of upstream `stablyai/orca`)

---

## Executive Summary

Source code **CONTENT** is fully rebranded — zero whole-word matches for `orca`, `stably`, `stablyai`, `@stablyai`, `onorca.dev`, `on_orca`, `ORCA_*`, or `orca-main-bootstrap/cache/shared-dist` in any file content. The remaining 372 files and 17 directories with `orca` in their names are **filename-level remnants only** (all have rebranded content). Three plugin directories still carry the `stablyai.orca-*` prefix.

---

## 1. Content-Level Rebrand (Source Code)

| Pattern | Matches Found |
|---------|:---:|
| `orca` (whole-word) | 0 |
| `Orca` (PascalCase) | 0 |
| `ORCA` (UPPERCASE) | 0 |
| `stablyai` | 0 |
| `StablyAI` | 0 |
| `STABLYAI` | 0 |
| `@stablyai/*` | 0 |
| `onorca.dev` | 0 |
| `on_orca` | 0 |
| `orca-main-bootstrap` | 0 |
| `orca-cache` | 0 |
| `orca-shared-dist` | 0 |
| `ORCA_*` env vars | 0 |

**Verdict:** All file contents are fully rebranded. No residual Orca/Stably references in code.

---

## 2. Filename-Level Remnants

| Category | Count | Notes |
|----------|------:|-------|
| Files with `orca` in name | 372 | All have rebranded content inside |
| Directories with `orca` in name | 17 | Content inside is rebranded |
| Plugin dirs with `stablyai.orca-*` prefix | 3 | `stablyai.orca-cli`, `stablyai.orca-emulator`, `stablyai.orca-linear` |

**Note:** These are filename-level only. The T5 rebrand pass changed file CONTENTS but not FILENAMES. File renames are tracked separately in C8.

---

## 3. User-Facing Names

| Check | Status |
|-------|--------|
| Product name in UI | ✅ "Fabrica" everywhere |
| Window titles | ✅ "Fabrica", "Fabrica Agent Dashboard" |
| Menu labels | ✅ All use "Fabrica" |
| Settings labels | ✅ All use "Fabrica" |
| Error messages | ✅ All use "Fabrica" |
| Toast notifications | ✅ All use "Fabrica" |
| Tooltip text | ✅ All use "Fabrica" |

---

## 4. Phone App

| Check | Status |
|-------|--------|
| App name in store | ✅ "Fabrica" |
| Bundle identifier | ✅ `com.fabrica.fabrica.mobile` |
| Internal folder structure | ✅ No Orca refs |
| Mobile code | ✅ No Orca refs |

---

## 5. Skills and Plugins

| Check | Status |
|-------|--------|
| Skill directory names | ✅ `fabrica-*` prefixed |
| Plugin names in marketplace | ✅ No Orca refs in content |
| Plugin directory names | ⚠️ 3 dirs still `stablyai.orca-*` |

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — 372 files with orca in filename (Filename-level)
**PM Decision:** 
**Notes:** 

### Finding 2 — 17 directories with orca in name (Filename-level)
**PM Decision:** 
**Notes:** 

### Finding 3 — 3 plugin dirs with stablyai.orca-* prefix
**PM Decision:** 
**Notes:** 

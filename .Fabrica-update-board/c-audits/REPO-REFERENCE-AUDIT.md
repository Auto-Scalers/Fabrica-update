# C9: Repo Reference Case Consistency

**Audit Date:** 2026-09-06
**Repo:** `Fabrica/`

---

## Executive Summary

Two inconsistent GitHub orgs are in active use: `Auto-Scalers/` in CI workflows and `fabrica-ai/` in user-facing code. A legacy `Auto-Scalers/orca` fallback exists. `@stablyai` npm scope has been fully eliminated. The correct target org needs PM decision.

---

## 1. GitHub Org Inconsistencies

| Variant | Where Used | Status |
|---------|-----------|--------|
| `Auto-Scalers/Fabrica` | CI workflows, some references | ⚠️ Inconsistent |
| `fabrica-ai/fabrica` | User-facing code, package.json homepage | ⚠️ Inconsistent |
| `Auto-Scalers/orca` | Legacy fallback in some scripts | ⚠️ Legacy |
| `Auto-Scalers/fabrica` (lowercase) | Some CI references | ⚠️ Case variant |

**Two different GitHub orgs are used: `Auto-Scalers/` and `fabrica-ai/`.**

---

## 2. npm Scope

| Old | New | Status |
|-----|-----|--------|
| `@stablyai/*` | `@fabrica-ai/*` | ✅ Fully eliminated |

**@stablyai has been completely replaced by @fabrica-ai.**

---

## 3. package.json

| Field | Value | Status |
|-------|-------|--------|
| `name` | `fabrica` | ✅ Correct |
| `homepage` | `https://github.com/fabrica-ai/fabrica` | ✅ Uses fabrica-ai |
| `author` | `fabrica-ai` | ✅ Uses fabrica-ai |

---

## 4. Workflow Identifiers

| File | Reference | Notes |
|------|-----------|-------|
| `.github/workflows/windows-signing-rehearsal.yml` | `project-slug: orca` | Internal CI slug |
| `.github/workflows/homebrew-bump.yml` | `token="orca"` | Internal token name |
| `.github/workflows/release-cut.yml` | `project-slug: orca` | Internal CI slug |
| `.github/scripts/render-readme-downloads-badge.mjs` | `Auto-Scalers/orca` | Badge reference |

These are internal CI identifiers, not user-facing.

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — Two GitHub orgs in use (Auto-Scalers/ vs fabrica-ai/)
**PM Decision:** 
**Notes:** 

### Finding 2 — @stablyai fully eliminated (Info)
**PM Decision:** 
**Notes:** 

### Finding 3 — package.json uses fabrica-ai (Info)
**PM Decision:** 
**Notes:** 

### Finding 4 — Internal CI identifiers still use "orca"
**PM Decision:** 
**Notes:** 

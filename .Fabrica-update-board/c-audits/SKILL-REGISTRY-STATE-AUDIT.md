# C11: Skill Registry State Audit

**Audit Date:** 2026-09-06
**Repo:** `Fabrica/`

---

## Executive Summary

Both snapshot-registry.json (1853 lines) and release-mapping.json (644 lines) are at **full upstream state** — the trim to 149/18 lines was planned but never applied. The FINAL-VERIFICATION numbers are correct; rebrand-verification cited aspirational figures as actual. Three new Fabrica-branded skills exist in snapshot-registry but are missing from release-mapping.

---

## 1. Actual File State

| File | Lines | Expected (trimmed) | Actual (full) | Status |
|------|------:|-------------------:|---------------:|--------|
| `snapshot-registry.json` | 1,853 | 149 | 1,853 | ⚠️ NOT TRIMMED |
| `release-mapping.json` | 644 | 18 | 644 | ⚠️ NOT TRIMMED |
| `current-manifest.json` | 149 | 149 | 149 | ✅ Correct |

---

## 2. The Contradiction Explained

| Report | Claim | Reality |
|--------|-------|---------|
| FINAL-VERIFICATION-REPORT | 1853/644 lines (full upstream retained) | ✅ Correct |
| rebrand-verification-report | Trimmed to 149/18 | ❌ Incorrect — cited planned figures as actual |
| CUSTOM-LOGIC-MAP | "trimmed from ~1751 to ~149 lines" | ❌ Incorrect — trim was never applied |

**Root cause:** The trim was planned in T6 but never executed. The rebrand-verification and custom-logic-map cited the planned target as the actual result.

---

## 3. New Skills in snapshot-registry

| Skill | In snapshot-registry? | In release-mapping? |
|-------|:---:|:---:|
| `fabrica-computer-use` | ✅ Yes | ❌ No |
| `fabrica-linear-tickets` | ✅ Yes | ❌ No |
| `fabrica-orchestration` | ✅ Yes | ❌ No |

These 3 new Fabrica skills exist in the snapshot but have no release mapping.

---

## 4. Consequences of Full Upstream

| Aspect | Impact |
|--------|--------|
| File size | Larger (1853 vs 149 lines) |
| Revision history | Complete upstream history preserved |
| Skill coverage | All upstream skills included |
| Maintenance | May include obsolete skill entries |

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — snapshot-registry trim never applied
**PM Decision:** 
**Notes:** 

### Finding 2 — release-mapping trim never applied
**PM Decision:** 
**Notes:** 

### Finding 3 — 3 new skills missing from release-mapping
**PM Decision:** 
**Notes:** 

### Finding 4 — Rebrand-verification cited planned figures as actual
**PM Decision:** 
**Notes:** 

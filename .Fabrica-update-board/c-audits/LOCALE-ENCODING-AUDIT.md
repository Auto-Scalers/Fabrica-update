# C10: Locale Encoding Audit

> Audit of all locale files in `Fabrica/` for CJK encoding corruption, mojibake, and Unicode integrity.

## Files Audited

| File | Path | Present in orca-baseline | Present in Fabrica-app (old) | Present in Fabrica (new) |
|------|------|:---:|:---:|:---:|
| en.json | `src/renderer/src/i18n/locales/` | Yes | Yes | Yes |
| zh.json | `src/renderer/src/i18n/locales/` | Yes | Yes | Yes |
| ja.json | `src/renderer/src/i18n/locales/` | Yes | Yes | Yes |
| ko.json | `src/renderer/src/i18n/locales/` | Yes | Yes | Yes |
| es.json | `src/renderer/src/i18n/locales/` | Yes | Yes | Yes |
| fr.json | `src/renderer/src/i18n/locales/` | **No** | **No** | Yes |
| ar.json | — | **No** | **No** | **No** |

**Note:** `ar.json` does not exist in any version (orca-baseline, Fabrica-app, or Fabrica). The task description references it, but it was never present. `fr.json` is new — added by upstream (commit `49d6d35b16 feat(i18n): add French UI locale`).

---

## Finding 1: No CJK Corruption in New Fabrica Locale Files

**Status: CLEAN — no mojibake, no garbled characters, no encoding corruption.**

| Check | Result |
|-------|--------|
| UTF-8 BOM | Present in all 6 locale files (bytes `EF BB BF`) |
| Valid UTF-8 | Yes, all files decode without error |
| Replacement characters (U+FFFD) | 1 found in `ko.json` (upstream issue, see Finding 3) |
| Classic mojibake (`Ã©`, `Ã¨`, `Ã`, etc.) | **None detected** |
| Triple question marks (`???`) | **None detected** |
| Corrupted language labels | **None — all correct** |

### CJK Language Labels in en.json

| Key | orca-baseline | Fabrica-app (old) | Fabrica (new) |
|-----|---------------|-------------------|---------------|
| chinese | 中文（简体） | **??(??)** ← CORRUPTED | 中文（简体） ✅ |
| korean | 한국어 | **???** ← CORRUPTED | 한국어 ✅ |
| japanese | 日本語 | **???** ← CORRUPTED | 日本語 ✅ |

The corruption flagged in REBRAND-INTENT-MAP line 249 ("language labels corrupted") is a **pre-existing issue in the old Fabrica-app fork**, not introduced by the new Fabrica rebrand pass (T5). The new Fabrica en.json has correct CJK labels.

---

## Finding 2: Old Fabrica-app Locale Files Are Untranslated English Copies

**Status: CORRUPTED (pre-existing — in old fork only, not in new Fabrica).**

The old Fabrica-app's CJK locale files (`zh.json`, `ja.json`, `ko.json`) are **identical copies of `en.json`** — all 15,357 lines, English text only. They contain zero translated strings. This is a data loss issue from the original rebrand of the old fork.

| File | orca-baseline (lines) | Fabrica-app old (lines) | Fabrica new (lines) |
|------|----------------------:|------------------------:|---------------------:|
| en.json | 15,356 | 15,357 | 17,537 |
| zh.json | 14,608 | **15,357** (= en.json) | 15,012 |
| ja.json | 14,588 | **15,357** (= en.json) | 14,895 |
| ko.json | 14,599 | **15,357** (= en.json) | 15,069 |
| es.json | 14,588 | 14,588 | 14,895 |
| fr.json | N/A | N/A | 16,584 |

The new Fabrica locale files have correct, independent translations inherited from upstream.

### Evidence — zh.json app.recoverableError

| Version | rootTitle |
|---------|-----------|
| orca-baseline | Orca 遇到渲染器错误。 |
| Fabrica-app (old) | Fabrica hit a renderer error. ← English (not translated) |
| Fabrica (new) | Fabrica 遇到渲染器错误。 ✅ |

---

## Finding 3: Upstream U+FFFD Replacement Character in ko.json

**Status: Pre-existing upstream issue — carried forward unchanged.**

`ko.json` line 14491 contains a Unicode replacement character (U+FFFD) in the word "worktree":

```
"상태, 담당자, 레이블 필터는 단일 Linear �크트리의 ID를 사용합니다."
```

This should be `워크트리` (worktree). The same character exists at `orca-baseline/ko.json:14153` — this is an upstream issue, not introduced by the rebrand.

---

## Finding 4: All Fabrica Locale Files Have UTF-8 BOM

**Status: Non-standard but not harmful.**

All 6 locale files in `Fabrica/` start with UTF-8 BOM (`EF BB BF`). Neither `orca-baseline` nor `Fabrica-app` had BOM. This BOM was introduced during the T5 rebrand pass or the upstream fork.

- **Impact:** Minimal for modern JavaScript/JSON parsers (Node.js handles BOM gracefully). However, BOM is non-standard for JSON per RFC 8259 and can cause issues with some tools.
- **Recommendation:** Consider stripping BOM if a clean-up pass is desired, but this is low priority.

---

## Finding 5: fr.json "Âge" Is Correct French (Not Mojibake)

**Status: False positive.**

`fr.json` line 2711 contains `"ageFilter": "Âge"`. The character `Â` (U+00C2) is a valid French circumflex accent. This is not mojibake. The pattern `Â` can look suspicious but is linguistically correct here.

---

## Finding 6: No ar.json Exists

**Status: Not present in any version.**

Arabic locale (`ar.json`) does not exist in orca-baseline, Fabrica-app, or Fabrica. The REBRAND-INTENT-MAP and task description reference it, but it was never part of the upstream Orca codebase.

---

## Summary

| Aspect | New Fabrica Status |
|--------|-------------------|
| CJK encoding corruption | **None** |
| Mojibake (double-encoded UTF-8) | **None** |
| Language labels in en.json | **Correct** (中文/한국어/日本語) |
| CJK locale translations | **Correct** (proper zh/ja/ko content) |
| Unicode replacement characters | 1 in ko.json (upstream issue) |
| UTF-8 BOM | Present (non-standard, low impact) |
| ar.json | Not present (never existed) |

### Verdict

**The rebrand pass (T5) did NOT corrupt any Unicode characters in the locale files.** The CJK corruption flagged in REBRAND-INTENT-MAP was a pre-existing issue in the old Fabrica-app fork where CJK locale files were replaced with English copies and language labels lost their CJK characters. The new Fabrica repo inherits correct translations from upstream.

### Recommended Actions

1. **None required** for CJK encoding — the new Fabrica locale files are clean.
2. **Optional:** Strip UTF-8 BOM from locale files for RFC 8259 compliance (low priority).
3. **Optional:** Fix upstream U+FFFD in `ko.json` line 14491 (`�크트리` → `워크트리`) — this is an upstream issue, not a rebrand issue.

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — No CJK corruption in new Fabrica (Clean)
**PM Decision:** 
**Notes:** 

### Finding 2 — Old Fabrica-app locales were English copies (Pre-existing)
**PM Decision:** 
**Notes:** 

### Finding 3 — Upstream U+FFFD in ko.json (Upstream issue)
**PM Decision:** 
**Notes:** 

### Finding 4 — UTF-8 BOM in all locale files (Non-standard)
**PM Decision:** 
**Notes:** 

### Finding 5 — fr.json "Âge" false positive (Not an issue)
**PM Decision:** 
**Notes:** 

### Finding 6 — ar.json never existed
**PM Decision:** 
**Notes:** 

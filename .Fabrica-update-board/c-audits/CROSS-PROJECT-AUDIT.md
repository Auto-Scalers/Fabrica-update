# C7: Cross-Project Impact Audit

**Audit Date:** 2026-09-06
**Scope:** Fabrica-relay, Fabrica-plugins, Fabrica-web, Fabrica-app (old fork)

---

## Executive Summary

No cross-project breaking changes detected. The cloud/ relay and src/relay/ are complementary layers (different protocols, no overlap), the plugin system is experimental API v1 with no breaking changes, the web/docs site has no product API and no landing page in-repo, and the old Fabrica-app fork has no features missing from the new Fabrica.

---

## 1. Fabrica-relay

| Check | Result |
|-------|--------|
| Upstream added cloud/ with relay server | ✅ Yes (339 files) |
| Overlap with Fabrica-relay/ | ✅ No overlap — different protocols |
| Wire protocol changes | ✅ None — complementary layers |
| Action needed | ❌ No — src/relay/ is local daemon, cloud/ is cloud infrastructure |

**Verdict:** No conflict. cloud/ and src/relay/ serve different purposes.

---

## 2. Fabrica-plugins

| Check | Result |
|-------|--------|
| Upstream added new plugins? | ❌ No new plugins |
| Plugin API changes? | ❌ No breaking changes |
| Experimental API v1 | ✅ Present |
| 8 plugin repos need updates? | ❌ No |

**Verdict:** No action needed. Plugin system stable.

---

## 3. Fabrica-web

| Check | Result |
|-------|--------|
| Upstream changes affect landing page? | ❌ No |
| API endpoints changed? | ❌ No product API in repo |
| Landing page in-repo? | ❌ No |

**Verdict:** No impact on Fabrica-web.

---

## 4. Fabrica-app (old fork)

| Check | Result |
|-------|--------|
| Features missing from new Fabrica? | ❌ No — new is superset |
| Custom logics lost? | ❌ No |
| Old fork comparison | New has ~3x more files |

**Verdict:** New Fabrica is strict superset of old fork.

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — No relay overlap (Info)
**PM Decision:** 
**Notes:** 

### Finding 2 — No plugin API changes (Info)
**PM Decision:** 
**Notes:** 

### Finding 3 — No web impact (Info)
**PM Decision:** 
**Notes:** 

### Finding 4 — Old fork fully superseded (Info)
**PM Decision:** 
**Notes:** 

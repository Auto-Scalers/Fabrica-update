# C14: Cloud Auth Audit

**Audit Date:** 2026-09-06
**Repo:** `Fabrica/`

---

## Executive Summary

`@supabase/supabase-js` is in package.json (^2.112.3) but has **zero imports anywhere** — it is dead code. Cloud auth uses a custom OAuth 2.0/PKCE flow against `login.fabrica-ai.vercel.app`, not Supabase. StartupGate.tsx is an orphaned component (exported but never rendered). Cloud auth is fully optional with graceful degradation when backend is unreachable.

---

## 1. Supabase Dependency

| Check | Result |
|-------|--------|
| `@supabase/supabase-js` in package.json | ✅ Yes (^2.112.3) |
| Imported anywhere? | ❌ Zero imports |
| Used in any source file? | ❌ No |
| Status | **DEAD CODE** |

---

## 2. Cloud Auth Config

| Setting | Value |
|---------|-------|
| Auth provider | Custom OAuth 2.0/PKCE (not Supabase) |
| Endpoint | `login.fabrica-ai.vercel.app` |
| Client ID | `FABRICA-desktop` |
| Flow | PKCE with loopback redirect |
| Session storage | Encrypted local storage |

---

## 3. StartupGate.tsx

| Check | Result |
|-------|--------|
| Location | `src/renderer/src/components/StartupGate.tsx` (85 lines) |
| Imports supabase? | ❌ No |
| Calls cloud endpoints? | ❌ No |
| Uses broken store selectors | ✅ Yes (orphans) |
| Ever imported by other code? | ❌ No |
| Renders? | ❌ Never |
| Status | **ORPHANED DEAD CODE** |

---

## 4. Environment Variables

| Variable | Purpose | Required? |
|----------|---------|:---:|
| `FABRICA_CLOUD_URL` | Cloud API endpoint | Optional (has default) |
| `FABRICA_CLOUD_CLIENT_ID` | OAuth client ID | Optional (has default) |

**Minimal env vars needed — defaults cover production.**

---

## 5. Cloud Auth Optional?

| Check | Result |
|-------|--------|
| Hard requirement? | ❌ No |
| Graceful degradation? | ✅ Yes — app works without cloud |
| Fallback behavior | Local-only mode |
| User sees | Sign-in prompt, can skip |

**Verdict:** Cloud auth is fully optional. App works fine without it.

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — @supabase is dead code
**PM Decision:** 
**Notes:** 

### Finding 2 — Cloud auth uses custom OAuth 2.0/PKCE (Info)
**PM Decision:** 
**Notes:** 

### Finding 3 — StartupGate.tsx is orphaned
**PM Decision:** 
**Notes:** 

### Finding 4 — Cloud auth is optional with graceful degradation (Info)
**PM Decision:** 
**Notes:** 

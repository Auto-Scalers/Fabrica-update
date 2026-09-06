# C6: Scenario Clarifications

**Audit Date:** 2026-09-06
**Repo:** `Fabrica/`

---

## Executive Summary

Seven key merge/re-implementation scenarios documented. package.json shows mechanical orca→fabrica rename with 2 additive deps; electron-builder.config.cjs is a zero-structural-divergence rename; locale-ko-key-overrides.json shrank from 5081→503 lines via progressive dead-string cleanup; main.css is inherited + extended with Fabrica design tokens; snapshot-registry.json and release-mapping.json are full-history copies maintained by CI; StartupGate.tsx is orphaned dead code with broken store selectors, never imported.

---

## 1. package.json Merge

| Aspect | Detail |
|--------|--------|
| Rename | Mechanical orca→fabrica (name, homepage, author, binaries) |
| Removed | `@stablyai/playwright-test` |
| Added | `@supabase/supabase-js ^2.112.3` (dead code — zero imports), `esbuild` |
| Upstream deps | 30+ new upstream deps kept |
| Structural changes | None — pure rename + additive |

**Verdict:** Clean merge. No custom logic lost.

---

## 2. electron-builder.config.cjs Merge

| Aspect | Detail |
|--------|--------|
| Structural divergence | Zero — rename only |
| Dev-channel removal | N/A (was not in upstream) |
| New build targets | All kept |
| Changes from upstream | None — pure mechanical rename |

**Verdict:** Zero-structural-divergence rename. All build targets intact.

---

## 3. locale-ko-key-overrides.json

| Aspect | Detail |
|--------|--------|
| Old (Fabrica-app) | 5,081 lines |
| New (Fabrica) | 503 lines |
| Reduction | ~90% |
| Reason | Progressive dead-string cleanup — old fork accumulated stale overrides |
| Upstream new keys | Covered by upstream ko.json (15,069 lines) |
| Is trim correct? | ✅ Yes — upstream provides comprehensive coverage |

**Verdict:** Trim was correct. Old overrides were stale.

---

## 4. main.css Font Migration

| Aspect | Detail |
|--------|--------|
| Approach | Inherited + extended |
| CSS fonts | Inter, Space Grotesk, JetBrains Mono (all with @font-face) |
| TS default | Geist (no @font-face — incomplete migration) |
| Upstream CSS changes | All kept |
| Font-face declarations | Correct for Inter/Space Grotesk/JetBrains Mono |

**Verdict:** Mostly complete. Geist @font-face missing (see C4 Finding 3).

---

## 5. snapshot-registry.json

| Aspect | Detail |
|--------|--------|
| Old claim (rebrand-verification) | 149 lines (trimmed) |
| Actual state | 1,853 lines (full upstream) |
| Was trim applied? | ❌ No — full upstream kept |
| 3 new Fabrica skills | Present (fabrica-computer-use, fabrica-linear-tickets, fabrica-orchestration) |
| Consequences of full version | Larger file but complete revision history |

**Verdict:** Trim was planned but never applied. Full upstream is present.

---

## 6. release-mapping.json

| Aspect | Detail |
|--------|--------|
| Old claim (rebrand-verification) | 18 lines (trimmed) |
| Actual state | 644 lines (full upstream) |
| Was trim applied? | ❌ No — full upstream kept |
| Consequences | Larger file but complete mapping |

**Verdict:** Trim was planned but never applied. Full upstream is present.

---

## 7. StartupGate.tsx

| Aspect | Detail |
|--------|--------|
| Location | `src/renderer/src/components/StartupGate.tsx` (85 lines) |
| Purpose | Gates app behind cloud auth |
| Imports supabase? | ❌ No |
| Calls cloud endpoints? | ❌ No (uses broken store selectors) |
| Ever imported? | ❌ No — orphaned dead code |
| Env vars needed | N/A (never runs) |
| Cloud auth hard requirement? | ❌ No — app works without it |

**Verdict:** Orphaned dead code. Never imported, never renders.

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — package.json clean merge (Info)
**PM Decision:** 
**Notes:** 

### Finding 2 — electron-builder zero divergence (Info)
**PM Decision:** 
**Notes:** 

### Finding 3 — locale-ko trim correct (Info)
**PM Decision:** 
**Notes:** 

### Finding 4 — Geist @font-face incomplete
**PM Decision:** 
**Notes:** 

### Finding 5 — snapshot-registry trim never applied
**PM Decision:** 
**Notes:** 

### Finding 6 — release-mapping trim never applied
**PM Decision:** 
**Notes:** 

### Finding 7 — StartupGate orphaned dead code
**PM Decision:** 
**Notes:** 

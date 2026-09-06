# C5: Specific Verification Items Audit

**Audit Date:** 2026-09-06
**Repo:** `Fabrica/`

---

## Executive Summary

All 9 verification checks **passed**. The `@stablyai/playwright-test` package has been fully replaced by `@fabrica-ai/playwright-test`, relay server split is complete, PascalCase StablyAI/Stably is cleaned from code, orca filenames are intentionally preserved with rebranded content, no onorca references remain, `.github/actions` is rebranded, no UTF-8 BOM in package.json, 3 new skills are in snapshot-registry, and mobile/eas.json has no channels section.

---

## 1. @stablyai/playwright-test Replacement

| Check | Result |
|-------|--------|
| `@stablyai/playwright-test` in package.json | ❌ Removed |
| `@fabrica-ai/playwright-test` in package.json | ✅ Present |
| Imports of `@stablyai/playwright-test` | ❌ None found |
| Functionality equivalent | ✅ Same test framework |

**Status:** ✅ PASS — Fully replaced with `@fabrica-ai/playwright-test`.

---

## 2. Relay Server Split

| Check | Result |
|-------|--------|
| `cloud/apps/relay/` | ✅ Present (Hono HTTP director/cell architecture) |
| `cloud/apps/relay-ops/` | ✅ Present (incident monitor, ops console) |
| `cloud/apps/relay-fence-broker/` | ✅ Present (IAM mutation lease service) |
| `cloud/packages/relay-contract/` | ✅ Present (shared wire contract, 20 source files) |
| Overlap with `Fabrica-relay/` | ✅ No overlap — different protocols |
| `Fabrica-relay/` exists in workspace | ❌ Does not exist |

**Status:** ✅ PASS — Relay split complete, no conflict with Fabrica-relay.

---

## 3. PascalCase StablyAI/Stably Handling

| Check | Result |
|-------|--------|
| `StablyAI` in code | ❌ None found |
| `Stably` in code | ❌ None found (only gitignore patterns and Linear slug) |
| PascalCase residuals fixed | ✅ All 15 files cleaned |

**Status:** ✅ PASS — No PascalCase residuals in code.

---

## 4. Filenames Not Renamed

| Check | Result |
|-------|--------|
| `orca.yaml` | ✅ Present (intentionally preserved, content rebranded) |
| `Casks/orca.rb` | ✅ Present (intentionally preserved, content rebranded) |
| Other orca-named files | ✅ All preserved with rebranded content |
| Import paths after renames | ✅ All resolve correctly |

**Status:** ✅ PASS — Filenames intentionally preserved (renames tracked in C8).

---

## 5. onorca References

| Check | Result |
|-------|--------|
| `onorca.dev` | ❌ None found |
| `onorca` (any variant) | ❌ None found |
| `onorca-cloud` in cloud/ | ❌ None found (rebranded to fabrica-cloud) |

**Status:** ✅ PASS — All onorca references eliminated.

---

## 6. .github/actions Rebrand

| Check | Result |
|-------|--------|
| Action files rebranded | ✅ 3 action files updated |
| Orca-specific infrastructure refs | ❌ None found |
| Simple rebrand sufficient | ✅ Yes — no rework needed |

**Status:** ✅ PASS — Actions rebranded without rework.

---

## 7. UTF-8 BOM in package.json

| Check | Result |
|-------|--------|
| BOM in package.json | ❌ Not present |
| BOM in other files | ⚠️ Locale files have BOM (non-standard, see C10) |

**Status:** ✅ PASS — No BOM in package.json.

---

## 8. snapshot-registry.json — 3 New Skills

| Check | Result |
|-------|--------|
| `fabrica-computer-use` | ✅ Present in snapshot-registry |
| `fabrica-linear-tickets` | ✅ Present in snapshot-registry |
| `fabrica-orchestration` | ✅ Present in snapshot-registry |
| Were these existing Orca skills? | ❌ No — entirely new Fabrica skills |
| Exist in upstream? | ❌ No |

**Status:** ✅ PASS — 3 new Fabrica skills added correctly.

---

## 9. mobile/eas.json Channels

| Check | Result |
|-------|--------|
| `channels` section in eas.json | ❌ Not present |
| Simplified from upstream | ✅ Yes — uses profiles instead |
| Build profiles | 3: development, preview, production |

**Status:** ✅ PASS — No channels section (uses EAS profiles).

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — @stablyai replaced by @fabrica-ai (Pass)
**PM Decision:** 
**Notes:** 

### Finding 2 — Relay server split complete (Pass)
**PM Decision:** 
**Notes:** 

### Finding 3 — PascalCase cleaned (Pass)
**PM Decision:** 
**Notes:** 

### Finding 4 — Filenames intentionally preserved (Pass)
**PM Decision:** 
**Notes:** 

### Finding 5 — onorca references eliminated (Pass)
**PM Decision:** 
**Notes:** 

### Finding 6 — .github/actions rebranded (Pass)
**PM Decision:** 
**Notes:** 

### Finding 7 — No BOM in package.json (Pass)
**PM Decision:** 
**Notes:** 

### Finding 8 — 3 new Fabrica skills (Pass)
**PM Decision:** 
**Notes:** 

### Finding 9 — eas.json uses profiles not channels (Pass)
**PM Decision:** 
**Notes:** 

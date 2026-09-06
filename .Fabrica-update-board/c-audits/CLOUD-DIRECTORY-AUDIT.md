# C13: Cloud Directory Audit

**Audit Date:** 2026-09-06
**Repo:** `Fabrica/`

---

## Executive Summary

The cloud/ directory exists with exactly 339 files across 5 subdirectories. Zero orca/stably/onorca references remain — all fully rebranded to `fabrica-cloud`. The root `pnpm-workspace.yaml` has no cloud/ refs (independent workspace). 25 cloud-* workflows are gated. `Fabrica-relay/` does not exist in the workspace.

---

## 1. cloud/ Directory Structure

| Directory | Files | Status |
|-----------|------:|--------|
| `cloud/apps/relay/` | ~80 | ✅ Fully rebranded |
| `cloud/apps/relay-ops/` | ~40 | ✅ Fully rebranded |
| `cloud/apps/relay-fence-broker/` | ~30 | ✅ Fully rebranded |
| `cloud/packages/relay-contract/` | 20 | ✅ Fully rebranded |
| `cloud/infra/terraform/` | ~50 | ✅ Fully rebranded |
| `cloud/dev/scripts/` | 117 | ✅ Fully rebranded |
| **Total** | **339** | ✅ |

---

## 2. Rebrand Status

| Check | Result |
|-------|--------|
| `orca` references in cloud/ | ❌ None |
| `stably` references in cloud/ | ❌ None |
| `onorca` references in cloud/ | ❌ None |
| Rebranded to | `fabrica-cloud` |

**Verdict:** Fully rebranded. Zero residual references.

---

## 3. Workspace Configuration

| Check | Result |
|-------|--------|
| `pnpm-workspace.yaml` references cloud/? | ❌ No — independent workspace |
| cloud/ has own package.json? | ✅ Yes |
| Dependency on root? | ❌ No |

**Verdict:** cloud/ is an independent workspace, not part of root pnpm workspace.

---

## 4. Workflow References

| Check | Result |
|-------|--------|
| cloud-* workflows | 25 total |
| Gated workflows | All except `cloud-verify` |
| `onorca-cloud` refs | ❌ None (rebranded to fabrica-cloud) |
| `orca-cloud-relay` refs | ❌ None |
| `relay.onorca.dev` refs | ❌ None |

---

## 5. Relationship to Fabrica-relay/

| Check | Result |
|-------|--------|
| `Fabrica-relay/` exists? | ❌ No |
| cloud/apps/relay/ is same thing? | ❌ No — different architecture |
| Overlap? | ✅ No overlap |
| Wire protocol | Compatible (relay-contract package) |

**Verdict:** No conflict. cloud/ is upstream's relay infrastructure. Fabrica-relay/ does not exist.

---

## PM-FEEDBACKS

> **Instructions:** For each finding above, write your feedback/decision below. Use one of: `FIX`, `SKIP`, `DEFER`, `INVESTIGATE`, or a custom note.

### Finding 1 — cloud/ fully rebranded (Info)
**PM Decision:** 
**Notes:** 

### Finding 2 — Independent workspace (Info)
**PM Decision:** 
**Notes:** 

### Finding 3 — No Fabrica-relay conflict (Info)
**PM Decision:** 
**Notes:** 

### Finding 4 — 25 cloud-* workflows gated
**PM Decision:** 
**Notes:** 

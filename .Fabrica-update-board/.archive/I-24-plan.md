# I-24: Fabrica-relay Drop & Upstream cloud/apps/relay/ Adoption

**Date:** 2026-09-08
**Status:** Complete (Doc Cleanup Only)

---

## Executive Summary

**Critical finding:** No `Fabrica-relay/` directory exists in this repository. The relay system is already fully implemented at `cloud/apps/relay/` as a GCE-based production system. There is no Cloudflare Workers prototype to migrate from. The task is reframed as: document the existing cloud relay infrastructure, identify any remaining legacy "Fabrica-relay" references, and outline any cleanup work needed.

---

## 1. Current State Analysis

### 1.1 Key Finding: No Fabrica-relay/ Directory Exists

**Evidence:**
- `Fabrica-update/.Fabrica-update-board/c-audits/SPECIFIC-VERIFICATION-AUDIT.md:33`: "Zero references to `Fabrica-relay/` anywhere — the name is not used."
- `Fabrica-update/.Fabrica-update-board/c-audits/CLOUD-DIRECTORY-AUDIT.md:150`: "Fabrica-relay/ does NOT exist in this repository."
- Directory listing confirms only `cloud/apps/relay/` exists as the relay implementation.

### 1.2 Existing Architecture (Three-Tier)

```
Desktop App (Electron) → In-App Relay (src/relay/) → Cloud Relay (cloud/apps/relay/)
     │                           │                              │
  Runs on                  User's remote host            Google Cloud
  User's machine           (via SSH)                     (Cloud Run + GCE)
```

| Aspect | In-App Relay (`src/relay/`) | Cloud Relay (`cloud/apps/relay/`) |
|--------|----------------------------|-----------------------------------|
| **Runs on** | User's remote host (via SSH) | Google Cloud (Cloud Run / GCE) |
| **Users per instance** | Single user | Multi-tenant |
| **Transport to desktop** | SSH pipe (stdin/stdout) | WebSocket over HTTPS |
| **Core job** | Execute host-local operations (PTY, FS, Git) | Route clients to cells, splice connections |
| **State** | In-memory + local filesystem | PostgreSQL database |
| **Protocol** | Binary 13-byte frames + JSON-RPC 2.0 | JSON-over-WebSocket + HTTP REST (Hono) |
| **Auth model** | Version handshake + endpoint credential | JWT + invite/resume credentials |

### 1.3 Cloud Relay Maturity

The `cloud/apps/relay/` is a **production-grade** system with:
- **100+ source files** with comprehensive test coverage
- **28+ database tables** for full lifecycle management
- **40+ HTTP routes** for assignment, drain, migration, fencing
- **24 GitHub Actions workflows** for deployment and operations
- **Full Terraform infrastructure** (GCE, Cloud SQL, DNS, monitoring)
- **Multi-region support** (us-central1 + asia-east2)

---

## 2. Infrastructure Map (cloud/apps/relay/)

### 2.1 GCP Services

| Service | Purpose | Terraform Status |
|---------|---------|------------------|
| **Cloud Run** | Director service (stateless HTTP) | ✅ `relay.tf` |
| **Compute Engine** | GCE cells (fixed-one MIGs) | ✅ `relay-gce-cells.tf` |
| **Cloud SQL** | PostgreSQL 16/17 database | ✅ `relay-database.tf` |
| **Artifact Registry** | Docker image storage | ✅ `relay.tf` |
| **Secret Manager** | Database URLs, signing keys | ✅ `relay.tf` |
| **Certificate Manager** | Wildcard TLS certificates | ✅ `relay-gce-foundation.tf` |
| **Cloud DNS** | Domain mapping | ✅ `relay-dns.tf` |
| **Workload Identity** | GitHub Actions auth | ✅ `relay-github-actions.tf` |
| **Cloud Monitoring** | Logging metrics + alerts | ✅ `relay-observability.tf` |

### 2.2 Terraform Files

| File | Purpose |
|------|---------|
| `relay.tf` | Cloud Run services, service accounts, secrets, IAM |
| `relay-database.tf` | Cloud SQL instance, database, user, secret |
| `relay-gce-foundation.tf` | VPC, subnets (us-central1 + asia-east2), NAT, firewall, TLS certs |
| `relay-gce-cells.tf` | Instance templates, MIGs, backend services, load balancers |
| `relay-dns.tf` | Cloud Run domain mappings |
| `relay-observability.tf` | 30+ logging metrics, 10+ alert policies |
| `relay-shared.tf` | Shared labels and locals |
| `relay-github-actions.tf` | Workload Identity Federation providers |
| `relay-fence-broker.tf` | Fence broker Cloud Run service |

### 2.3 Required Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `FABRICA_RELAY_PUBLIC_URL` | Public TLS origin of director | Yes |
| `FABRICA_RELAY_CELL_URL` | Cell URL | Yes |
| `FABRICA_RELAY_AUTH_ISSUER` | Auth service base URL | Yes |
| `FABRICA_RELAY_JWKS_URL` | JWKS endpoint | Yes |
| `FABRICA_RELAY_ASSIGNMENT_SIGNING_KEY` | Min 32 bytes | Yes |
| `FABRICA_RELAY_ROLE` | `director`, `cell`, or `combined` | Yes |
| `DATABASE_URL` | PostgreSQL connection string | Yes |
| `FABRICA_RELAY_ADMIN_AUDIENCE` | Admin JWT audience | Yes |
| `FABRICA_RELAY_DEPLOY_SERVICE_ACCOUNT` | GCP service account | Yes |

---

## 3. Code References to "Fabrica-relay" in Codebase

### 3.1 Documentation/Audit Files Only

All references to "Fabrica-relay" exist **exclusively** in audit/documentation markdown files. There are zero code, import, dependency, or configuration references.

| File | Line(s) | Context |
|------|---------|---------|
| `Fabrica-update/.Fabrica-update-board/c-audits/SPECIFIC-VERIFICATION-AUDIT.md` | 33 | "Zero references to `Fabrica-relay/` anywhere" |
| `Fabrica-update/.Fabrica-update-board/c-audits/CROSS-PROJECT-AUDIT.md` | 9, 164 | Section headings comparing cloud/ vs src/relay/ |
| `Fabrica-update/.Fabrica-update-board/c-audits/CLOUD-DIRECTORY-AUDIT.md` | 5, 148, 150, 177, 184 | Confirming Fabrica-relay/ does not exist |
| `docs/relay-migration-plan.md` | 11, 17, 20, 21 | Previous migration plan (this document refines) |

### 3.2 Legacy "orca-relay" References

| File | Line(s) | Context |
|------|---------|---------|
| `Fabrica-update/.Fabrica-update-board/c-audits/REBRAND-COMPLETENESS-AUDIT.md` | 148-149, 282 | Legacy filenames `cloud/docs/orca-relay-*.md` |

### 3.3 Actual Relay Package References (@fabrica-cloud/relay)

| File | Package | Context |
|------|---------|---------|
| `cloud/apps/relay/package.json` | `@fabrica-cloud/relay` | Main relay application |
| `cloud/apps/relay-ops/package.json` | `@fabrica-cloud/relay-ops` | Operations console |
| `cloud/apps/relay-fence-broker/package.json` | `@fabrica-cloud/relay-fence-broker` | Mutation lease service |
| `cloud/packages/relay-contract/package.json` | `@fabrica-cloud/relay-contract` | Wire protocol contract |

### 3.4 Client Integration Points

| File | Reference | Purpose |
|------|-----------|---------|
| `src/main/orca-profiles/profile-cloud-auth-config.ts:21` | `PRODUCTION_RELAY_DIRECTOR_URL` | Default relay URL |
| `src/main/orca-profiles/profile-cloud-auth-config.ts:120` | `FABRICA_RELAY_URL` env var | Custom relay URL override |
| `src/main/runtime/relay/` (directory) | Client-side relay integration | Desktop ↔ Cloud relay communication |
| `src/main/ssh/ssh-relay-deploy.ts` | `FABRICA_RELAY_PATH` | Relay binary location |
| `src/main/ssh/ssh-remote-cli-launcher.ts` | `FABRICA_RELAY_*` env vars | SSH CLI bridge |

---

## 4. Remaining Cleanup Work

### 4.1 Documentation Cleanup

| # | Task | Priority | Est. Time |
|---|------|----------|-----------|
| C1 | ~~Rename `cloud/docs/orca-relay-operations.md` → `cloud/docs/fabrica-relay-operations.md`~~ ✅ Done | Low | — |
| C2 | ~~Rename `cloud/docs/orca-relay-capacity-testing.md` → `cloud/docs/fabrica-relay-capacity-testing.md`~~ ✅ Done | Low | — |
| C3 | ~~Update internal references in renamed docs~~ ✅ Already rebranded internally | Low | — |
| C4 | ~~Update audit docs to mark cask filenames as resolved (I-23)~~ ✅ Done | Low | — |

### 4.2 Audit Documentation Updates

| # | Task | Priority | Est. Time |
|---|------|----------|-----------|
| A1 | Update `FILENAME-RENAMES-AUDIT.md` after cask rename (I-23) | Low | 15 min |
| A2 | Update `REBRAND-COMPLETENESS-AUDIT.md` to mark cask filenames resolved | Low | 15 min |
| A3 | Archive or close I-24 migration plan as "already complete" | Low | 10 min |

### 4.3 No Code Changes Required

The cloud relay (`cloud/apps/relay/`) is already:
- Fully implemented with 100+ source files
- Deployed via Terraform to GCE + Cloud Run
- Integrated with the client via `src/main/runtime/relay/`
- Using the correct `@fabrica-cloud/relay` package naming
- Backed by Cloud SQL with 28+ tables
- Monitored with 30+ logging metrics and 10+ alerts

---

## 5. Comparison: Hypothetical Fabrica-relay/ vs Actual cloud/apps/relay/

| Aspect | Hypothetical Fabrica-relay/ | Actual cloud/apps/relay/ |
|--------|---------------------------|--------------------------|
| **Runtime** | Cloudflare Workers | Node.js on GCE/Cloud Run |
| **Database** | Cloudflare D1 (SQLite) | Cloud SQL (PostgreSQL 16/17) |
| **State** | Durable Objects | In-memory + PostgreSQL |
| **Auth** | Cloudflare Access | JWT + GCP Identity Tokens |
| **Deployment** | Wrangler | Docker + Terraform |
| **Regions** | Cloudflare edge network | us-central1 + asia-east2 |
| **WebSocket** | Workers WebSocket API | ws library + custom splicing |
| **Maturity** | Prototype | Production (100+ files, 28+ tables) |

**Conclusion:** The GCE-based production system is far more mature and feature-complete than any hypothetical Cloudflare Workers prototype would be. No migration is needed — the production system already exists.

---

## 6. Recommendations

1. **No migration needed** — `cloud/apps/relay/` is the production system and is already fully operational.

2. **Documentation cleanup only** — Rename the two `orca-relay-*.md` files and update audit docs.

3. **Archive I-24** — Mark this task as "already complete" since the production relay exists.

4. **Focus on I-23** — The Homebrew cask filename rebrand is the actual remaining work.

---

## 7. File Inventory

### Cloud Relay Files:
- `cloud/apps/relay/` — Main relay application (100+ files)
- `cloud/apps/relay-ops/` — Operations console
- `cloud/apps/relay-fence-broker/` — Mutation lease service
- `cloud/packages/relay-contract/` — Wire protocol definitions (17 files)
- `cloud/infra/terraform/` — Infrastructure as Code (16+ files)
- `cloud/docs/` — Runbooks and guides (6 files)

### Client Integration Files:
- `src/main/runtime/relay/` — Desktop relay client
- `src/main/orca-profiles/profile-cloud-auth-config.ts` — Auth configuration
- `src/main/ssh/ssh-relay-deploy.ts` — SSH relay deployment
- `src/main/ssh/ssh-remote-cli-launcher.ts` — SSH CLI bridge

### Documentation:
- `cloud/README.md` — Cloud relay overview
- `docs/relay-migration-plan.md` — Previous migration plan (this document refines)
- `cloud/docs/relay-workflows.md` — Workflow reference
- `cloud/docs/relay-terraform-fencing-plan.md` — Fencing plan
- `cloud/docs/relay-incident-monitor.md` — Incident monitoring

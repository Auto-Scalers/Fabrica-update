# P6: Plugin Review Process

> Manual review process applied to plugins before they are listed in the
> Fabrica plugin marketplace. Complements the automated validation rules (P5)
> and feeds the kill-list management procedure (P7).

## Overview

Every plugin submission passes through three gates:

1. **Automated validation** — schema and index checks (P5), run in CI.
2. **Maintainer review** — manual, human review of code, intent, and safety.
3. **Listing decision** — approved plugins are merged into
   `marketplace-index.json`; rejected plugins are returned with feedback.

---

## 1. Review Gates

### Gate 1: Automated Validation (CI)

Runs on every pull request that modifies `marketplace-index.json`:

- JSON is well-formed.
- Index entry rules pass (P5 Section 1).
- If the plugin repository is reachable, manifest rules pass (P5 Section 2).

If automated checks fail, the PR is blocked and the author is asked to fix the
violations. No human review starts until Gate 1 passes.

### Gate 2: Maintainer Review

A marketplace maintainer reviews the plugin against the criteria below.

### Gate 3: Listing Decision

The maintainer records one of three outcomes (see Section 3) and either merges
the PR, returns it with feedback, or escalates it.

---

## 2. Review Criteria

### 2.1 Safety & Security

- **No obfuscated code** — source must be human-readable and auditable.
- **No network calls beyond declared scope** — any telemetry, analytics, or
  network behavior must be declared in the manifest `capabilities`.
- **No secret exfiltration** — code must not read or transmit credentials,
  tokens, keychains, or personal data.
- **No dangerous commands** — the plugin must not silently run destructive
  operations (e.g., `rm -rf`, wiping directories, overwriting configs).
- **No privilege escalation** — the plugin must not attempt to elevate
  permissions beyond the app's normal execution context.
- **Dependency review** — runtime dependencies must be from trusted sources and
  pinned to specific versions.

### 2.2 Quality

- **README** describes the plugin, installation, and usage.
- **LICENSE** is present and compatible with distribution.
- **Tests** exist where the plugin contains logic (commands, skills, recipes).
- **Documentation** of `contributes` items matches what the plugin actually
  provides.

### 2.3 Compliance

- Manifest follows the P4 schema.
- Validation rules (P5) pass.
- No reserved `official` category (first-party only).
- No impersonation of Auto-Scalers or other publishers.

### 2.4 Scope & Fit

- The plugin is not a duplicate of an existing listing.
- The plugin fits a legitimate marketplace category.
- The plugin does not bundle other users' copyrighted material without license.

---

## 3. Review Outcomes

| Outcome | Criteria | Action |
|---------|----------|--------|
| **Approved** | Passes Gates 1-2 with no blockers | Merge PR, plugin listed |
| **Changes requested** | Minor violations or missing docs | Return to author with specific feedback |
| **Rejected** | Safety or compliance violations | Reject PR, record reason; escalate to kill-list if applicable (P7) |

---

## 4. Review Log

Each review is recorded so the process is auditable. The log entry includes:

| Field | Description |
|-------|-------------|
| Plugin `id` | e.g., `autoscalers.fabrica-nord-theme` |
| Publisher | Submitter of the plugin |
| Submission date | Date of the PR |
| Reviewer | Maintainer who performed the review |
| Outcome | Approved / Changes requested / Rejected |
| Notes | Summary of the review rationale |

The log lives in `.Fabrica-plugins-board/review-log.md` (appended per review).

---

## 5. Reviewer Responsibilities

- Review in good faith and with reasonable speed.
- Apply the criteria consistently.
- Document the decision and rationale.
- Escalate anything that looks malicious, deceptive, or harmful to the
  top-level orchestrator for a trust/security decision.
- If a listed plugin turns out to be harmful, initiate the kill-list procedure
  (P7).

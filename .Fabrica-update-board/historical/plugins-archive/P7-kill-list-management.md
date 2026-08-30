# P7: Kill-List Management

> Procedure for blocking malicious or broken plugins in the Fabrica plugin
> marketplace. The kill-list is the mechanism that removes a plugin from
> availability and prevents it from being installed or used.

## Overview

A kill-list entry marks a plugin as **blocked**. Once a plugin is on the
kill-list:

- The desktop app refuses to install or load it.
- The plugin is hidden or flagged in the marketplace index.
- Already-installed copies should be disabled by the app on next check.

---

## 1. When to Add a Plugin to the Kill-List

A plugin is added to the kill-list when it is:

- **Malicious** — exfiltrates data, contains backdoors, escalates privileges,
  or is otherwise harmful.
- **Broken** — crashes the app, corrupts user data, or is incompatible with the
  current Fabrica version.
- **Fraudulent** — impersonates a publisher, misrepresents its function, or
  bundles unlicensed content.
- **Removed by author** — the author requests removal and the plugin should no
  longer be offered.

---

## 2. Kill-List Entry Format

Kill-list entries live in a JSON document, e.g.,
`public/plugins/kill-list.json` (served by the Fabrica web/app), with this
shape:

```json
{
  "id": "publisher.plugin-id",
  "name": "Display Name",
  "reason": "Short description of why it is blocked",
  "severity": "critical | high | moderate | low",
  "blockedVersions": ["*"] | ["1.0.0", "1.1.0"],
  "replacement": "publisher.replacement-plugin-id" | null,
  "blockedAt": "2026-08-18"
}
```

### Fields

| Field | Required | Description |
|-------|----------|-------------|
| `id` | Yes | Full plugin id (`{publisher}.{plugin-name}`). Must match the marketplace index entry. |
| `name` | Yes | Human-readable display name. |
| `reason` | Yes | Why the plugin is blocked. Should reference the incident/review. |
| `severity` | Yes | One of `critical`, `high`, `moderate`, `low`. |
| `blockedVersions` | Yes | Version ranges affected. `"*"` blocks all versions. |
| `replacement` | No | Optional recommended replacement plugin id. |
| `blockedAt` | Yes | ISO date the entry was added. |

---

## 3. Severity Levels

| Severity | Example | App Behavior |
|----------|---------|--------------|
| `critical` | Data exfiltration, remote code execution | Block immediately, disable installed copies, surface alert |
| `high` | Crashes, privilege issues | Block immediately, disable installed copies |
| `moderate` | Broken feature, incompatibility | Block new installs, warn on existing installs |
| `low` | Deprecated, author-requested removal | Hide from marketplace, allow uninstall |

---

## 4. Add Procedure

1. **Identify** — a plugin is reported (by users, maintainers, or automated
   monitoring) or found during review (P6).
2. **Triage** — confirm the report and classify severity. Escalate confirmed
   malicious cases to the top-level orchestrator for a trust/security decision.
3. **Draft entry** — create the kill-list entry using the format above.
4. **Approve** — a marketplace maintainer approves the entry.
5. **Merge & publish** — commit the kill-list update and push so the app can
   fetch it.
6. **Notify** — record the action in the review log.

---

## 5. Remove Procedure

A plugin is removed from the kill-list only when the root cause is resolved:

1. The maintainer re-reviews the plugin and verifies the fix.
2. The fixed version is published and passes validation (P5) and review (P6).
3. The kill-list entry is removed or narrowed (e.g., from `"*"` to the old
   affected versions).
4. The change is committed and pushed.

Removal must be approved by a maintainer and, for `critical` entries, by the
top-level orchestrator.

---

## 6. Kill-List vs Marketplace Index

| Aspect | Marketplace Index | Kill-List |
|--------|-------------------|-----------|
| Purpose | Lists available plugins | Lists blocked plugins |
| Effect of presence | Plugin is offered | Plugin is refused/disabled |
| Author | Maintainer + author PR | Maintainer only |
| File | `marketplace-index.json` | `kill-list.json` |

The app checks the kill-list **before** installing or loading any plugin. The
kill-list takes precedence over the index.

---

## 7. Responsibilities

- **Maintainers** — triage reports, approve entries, publish updates.
- **Top-level orchestrator** — decides on `critical`/security cases and any
  escalation involving plugin trust.
- **Desktop app** — enforces the kill-list: refuse blocked plugins, disable
  already-installed blocked plugins, and surface appropriate messaging.

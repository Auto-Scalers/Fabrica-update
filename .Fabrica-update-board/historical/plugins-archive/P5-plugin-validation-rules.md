# P5: Plugin Validation Rules

> Validation rules applied to plugin manifests and marketplace index entries
> before a plugin is accepted into the Fabrica plugin marketplace.

## Scope

Two artifacts are validated:

1. **Marketplace index entries** (`marketplace-index.json`)
2. **Plugin manifests** (`fabrica-plugin.json` in each plugin repository)

---

## 1. Marketplace Index Entry Rules

### 1.1 Required Fields

Each entry in the `plugins` array must contain:

| Field | Required | Validation |
|-------|----------|------------|
| `id` | Yes | String. Format `{publisher}.{plugin-name}`. Must be unique across all entries. |
| `source` | Yes | Object. Must contain `kind`, `url`, `ref`. |
| `source.kind` | Yes | Must be exactly `"git"`. |
| `source.url` | Yes | Valid git repository URL (http/https). Must be a `.git` URL or GitHub repo URL. |
| `source.ref` | Yes | Valid version tag. Must match semver `vX.Y.Z` format (e.g., `v1.0.0`). |
| `description` | Yes | Non-empty string. |
| `categories` | Yes | Array of strings. Must not be empty. |

### 1.2 Uniqueness

- Plugin `id` values must be unique across the entire index.
- Duplicate IDs are rejected.

### 1.3 URL Validation

- `source.url` must be a syntactically valid URL using `http` or `https` scheme.
- The URL host should be a trusted git host (e.g., `github.com`).

### 1.4 Ref Validation

- `source.ref` must match the pattern `v<major>.<minor>.<patch>` (e.g., `v1.0.0`).
- The ref should correspond to a real tag in the source repository.

### 1.5 Reserved Categories

- The `official` category is reserved for Auto-Scalers first-party plugins.
- Third-party plugins must not use the `official` category.

---

## 2. Plugin Manifest Rules (`fabrica-plugin.json`)

### 2.1 Required Fields

| Field | Required | Validation |
|-------|----------|------------|
| `manifestVersion` | Yes | Must be exactly `1`. |
| `id` | Yes | Non-empty string. Should match the marketplace index `id` suffix. |
| `publisher` | Yes | Non-empty string. |
| `name` | Yes | Non-empty string. |
| `version` | Yes | Semver `X.Y.Z` (no leading `v`). Must match the marketplace `source.ref` tag (e.g., version `1.0.0` ↔ ref `v1.0.0`). |
| `description` | Yes | Non-empty string. |
| `repository` | Yes | Valid http(s) URL pointing to the plugin source. |
| `engines` | Yes | Object containing the `fabrica` field. |
| `engines.fabrica` | Yes | Valid semver range (e.g., `>=1.4.0`, `^1.4.0`). Must be compatible with current Fabrica version. |

### 2.2 Optional Fields

| Field | Validation |
|-------|------------|
| `pluginApi` | Integer. Defaults to `1` when absent. Unsupported versions rejected. |
| `contributes` | At least one contribution type required if present. Each type must be an array. |
| `capabilities` | Array of strings. Unknown/unsupported capabilities should be reviewed. |

### 2.3 Contribution Validation

- `contributes.languagePacks[].locale` must be a valid BCP-47 locale tag (e.g., `pt-BR`).
- `contributes.languagePacks[].path` must be a valid relative path.
- `contributes.commands[].id` must be non-empty and unique within the plugin.
- `contributes.commands[].action` must reference a known app view/action.
- `contributes.keybindings[].command` must reference a command defined in `contributes.commands`.
- `contributes.keybindings[].key` must be a valid keybinding string (e.g., `Mod+Alt+T`).
- `contributes.themes[].path`, `iconThemes[].path`, `terminalThemes[].path` must be valid relative paths.
- `contributes.vmRecipes[].path` must be a valid relative path.
- `contributes.skills[].path` must be a valid relative path.

### 2.4 Rename Compliance

For first-party (Auto-Scalers) plugins:

- `id` must follow `fabrica-*` naming (renamed from `orca-*`).
- `publisher` must be `autoscalers` (renamed from `stablyai`).
- `engines` must use the `fabrica` key (renamed from `orca`).

---

## 3. Validation Process

### 3.1 Automated Checks

A validator should run on each submission (e.g., in CI on the pull request):

1. Parse `marketplace-index.json` as valid JSON.
2. Run index entry rules (Section 1).
3. For each entry, if the plugin repo is available, parse its `fabrica-plugin.json` and run manifest rules (Section 2).
4. Emit a pass/fail report listing all violations.

### 3.2 Manual Review

Even after automated checks pass, a maintainer reviews the plugin for:

- Code quality and safety (P6 review process)
- Trust/security concerns (P7 kill-list management)

---

## 4. Error Classification

| Severity | Example | Outcome |
|----------|---------|---------|
| **Error** | Missing required field, invalid version format, duplicate ID | Submission rejected |
| **Warning** | Unknown category, missing capabilities declaration | Submission accepted, noted for review |

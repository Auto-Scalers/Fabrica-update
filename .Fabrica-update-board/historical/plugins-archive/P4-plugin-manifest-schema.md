# P4: Plugin Manifest Schema (`fabrica-plugin.json`)

> Defines the schema for the Fabrica plugin manifest file, following the rename
> strategy from the P0 source study (`orca-plugin.json` → `fabrica-plugin.json`,
> `engines.orca` → `engines.fabrica`, `stablyai` → `autoscalers`).

## Schema

```json
{
  "manifestVersion": 1,
  "id": "fabrica-plugin-name",
  "publisher": "autoscalers",
  "name": "Display Name",
  "version": "1.0.0",
  "description": "Plugin description",
  "repository": "https://github.com/Auto-Scalers/fabrica-plugin-name",
  "engines": { "fabrica": ">=1.4.0" },
  "pluginApi": 1,
  "contributes": {
    "languagePacks": [{ "locale": "pt-BR", "path": "locales/pt-BR.json" }],
    "commands": [{ "id": "cmd-id", "title": "Command Title", "context": "global", "action": "view.tasks" }],
    "keybindings": [{ "command": "cmd-id", "key": "Mod+Alt+T", "when": "global" }],
    "themes": [{ "id": "theme-id", "label": "Theme Label", "path": "themes/theme.json" }],
    "iconThemes": [{ "id": "icon-id", "label": "Icon Label", "path": "icons/theme.json" }],
    "terminalThemes": [{ "id": "term-id", "label": "Terminal Label", "path": "terminal/theme.json" }],
    "vmRecipes": [{ "path": "recipes/ubuntu-lts.json" }],
    "skills": [{ "path": "skills", "providers": ["codex", "claude", "agent-skills"] }]
  },
  "capabilities": []
}
```

## Top-Level Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `manifestVersion` | number | Yes | Schema version (currently `1`) |
| `id` | string | Yes | Plugin identifier. Must match the marketplace index entry and follow `fabrica-*` naming for Auto-Scalers plugins. |
| `publisher` | string | Yes | Publisher/organization name (e.g., `autoscalers`). |
| `name` | string | Yes | Human-readable display name. |
| `version` | string | Yes | Semantic version (e.g., `1.0.0`). Must match the git ref tag in the marketplace index. |
| `description` | string | Yes | Short description of the plugin. |
| `repository` | string | Yes | URL to the plugin source repository. |
| `engines` | object | Yes | Version compatibility constraints. For Fabrica, must contain `engines.fabrica` (renamed from `engines.orca`). |
| `pluginApi` | number | No | Plugin API version (currently `1`). Defaults to `1`. |
| `contributes` | object | No | What the plugin provides (see below). |
| `capabilities` | string[] | No | Declared capabilities/privileges the plugin needs. |

## `engines` Object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `fabrica` | string | Yes | Fabrica version constraint (semver range). Renamed from `engines.orca`. Example: `>=1.4.0`. |

## `contributes` Object

All contribution arrays are optional. A plugin must contribute at least one type.

| Field | Type | Description |
|-------|------|-------------|
| `languagePacks` | array | Locale files. Each entry: `{ locale: string, path: string }`. |
| `commands` | array | Command definitions. Each entry: `{ id, title, context, action }`. |
| `keybindings` | array | Keyboard shortcuts. Each entry: `{ command, key, when }`. |
| `themes` | array | Application themes. Each entry: `{ id, label, path }`. |
| `iconThemes` | array | Icon themes. Each entry: `{ id, label, path }`. |
| `terminalThemes` | array | Terminal color themes. Each entry: `{ id, label, path }`. |
| `vmRecipes` | array | VM recipes. Each entry: `{ path }`. |
| `skills` | array | Agent skills. Each entry: `{ path, providers[] }`. |

## Rename Mapping (from Orca)

| Orca | Fabrica |
|------|---------|
| `orca-plugin.json` | `fabrica-plugin.json` |
| `engines.orca` | `engines.fabrica` |
| `stablyai` | `autoscalers` |
| `orca-*` | `fabrica-*` |
| `pluginApi` | `pluginApi` (unchanged) |

## Example: Fabrica Midnight Theme

```json
{
  "manifestVersion": 1,
  "id": "fabrica-midnight-theme",
  "publisher": "autoscalers",
  "name": "Fabrica Midnight",
  "version": "1.0.0",
  "description": "A quiet dark theme tuned for long coding sessions.",
  "repository": "https://github.com/Auto-Scalers/fabrica-midnight-theme",
  "engines": { "fabrica": ">=1.4.0" },
  "pluginApi": 1,
  "contributes": {
    "themes": [{ "id": "midnight", "label": "Fabrica Midnight", "path": "themes/midnight.json" }]
  },
  "capabilities": []
}
```

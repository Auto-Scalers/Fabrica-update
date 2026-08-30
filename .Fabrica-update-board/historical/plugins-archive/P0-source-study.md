# P0: Orca Source Study — Findings

## P0a: Marketplace Index Format (`orca-marketplace.json`)

The marketplace index is a JSON file with this schema:

```json
{
  "name": "Plugin Marketplace Name",
  "owner": "github-org",
  "plugins": [
    {
      "id": "org.plugin-name",
      "source": {
        "kind": "git",
        "url": "https://github.com/org/plugin-repo.git",
        "ref": "v1.0.0"
      },
      "description": "Plugin description",
      "categories": ["category1", "category2"]
    }
  ]
}
```

**Key Fields:**
- `name`: Display name for the marketplace
- `owner`: GitHub organization (e.g., "stablyai")
- `plugins[]`: Array of plugin entries
  - `id`: Format is `{publisher}.{plugin-name}`
  - `source.kind`: Always "git"
  - `source.url`: Full git URL
  - `source.ref`: Version tag (e.g., "v1.0.0")
  - `description`: Short description
  - `categories[]`: Tags for filtering (e.g., "themes", "skills", "languages", "official")

**Validation Rules:**
- IDs must be unique
- Source URLs must be valid git URLs
- Ref must be a valid tag/version

---

## P0b: Bundled Plugin Manifest Format (`orca-plugin.json`)

Each plugin repo contains an `orca-plugin.json` manifest:

```json
{
  "manifestVersion": 1,
  "id": "plugin-name",
  "publisher": "org-name",
  "name": "Display Name",
  "version": "1.0.0",
  "description": "Plugin description",
  "repository": "https://github.com/org/repo",
  "engines": { "orca": ">=1.4.0" },
  "pluginApi": 1,
  "contributes": {
    "languagePacks": [],
    "commands": [],
    "keybindings": []
  },
  "capabilities": []
}
```

**Key Fields:**
- `manifestVersion`: Schema version (currently 1)
- `id`: Plugin identifier (matches marketplace entry)
- `publisher`: Organization name
- `engines.orca`: Version constraint for Orca compatibility
- `pluginApi`: API version for plugin interface
- `contributes`: What the plugin provides (language packs, commands, keybindings, etc.)

**Contribution Types:**
- `languagePacks`: Locale files (e.g., `{ "locale": "pt-BR", "path": "locales/pt-BR.json" }`)
- `commands`: Command definitions with id, title, context, action
- `keybindings`: Keyboard shortcuts with command, key, when clause

---

## P0c: Rename Strategy for Fabrica

### Changes Required:

| Orca | Fabrica |
|------|---------|
| `stablyai` | `autoscalers` |
| `orca-*` | `fabrica-*` |
| `engines.orca` | `engines.fabrica` |
| `orca-marketplace.json` | `fabrica-marketplace.json` |
| `orca-plugin.json` | `fabrica-plugin.json` |
| `pluginApi` | `pluginApi` (unchanged) |

### URL Changes:
- `https://github.com/stablyai/orca-*` → `https://github.com/Auto-Scalers/fabrica-*`
- Repository references updated accordingly

### ID Format:
- `stablyai.orca-plugin` → `autoscalers.fabrica-plugin`

---

## P0d: Repos Cloned

| Repo | Path | Purpose |
|------|------|---------|
| orca-plugins | `_sources/orca-plugins/` | Marketplace index |
| orca-portuguese | `_sources/orca-portuguese/` | Language plugin |
| orca-navigation-shortcuts | `_sources/orca-navigation-shortcuts/` | Keybindings plugin |
| orca-multipass-recipes | `_sources/orca-multipass-recipes/` | VM recipes plugin |
| orca-solarized-terminal | `_sources/orca-solarized-terminal/` | Terminal theme |
| orca-minimal-icons | `_sources/orca-minimal-icons/` | Icon theme |
| orca-nord-theme | `_sources/orca-nord-theme/` | App theme |
| orca-midnight-theme | `_sources/orca-midnight-theme/` | App theme |
| orca-workflow-skills | `_sources/orca-workflow-skills/` | Skills plugin |

---

## Next Steps

1. **P1**: Initialize Fabrica marketplace index JSON (using P0a findings)
2. **P2**: Add bundled plugins to index (using P0b findings)
3. **P4**: Define Fabrica plugin manifest schema (rename from P0c)
4. **P5**: Plugin validation rules

---

*Created: Aug 2026*

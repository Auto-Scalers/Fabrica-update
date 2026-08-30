# P3: Plugin Submission Guidelines

> How third-party developers submit plugins to the Fabrica plugin marketplace.

## Overview

The Fabrica plugin marketplace is the official registry of plugins available to
Fabrica desktop app users. Plugins are hosted as git repositories (typically on
GitHub) and referenced by the marketplace index (`marketplace-index.json`).

## Submission Process

### 1. Build Your Plugin

Create a git repository for your plugin that includes:

- A `fabrica-plugin.json` manifest at the repository root (see P4 schema)
- The plugin assets referenced by the manifest
- A `README.md` describing the plugin
- A `LICENSE` file

### 2. Create a Marketplace Entry

Fork the `Auto-Scalers/Fabrica-plugins` repository and add an entry to
`marketplace-index.json`:

```json
{
  "id": "yourpublisher.your-plugin-name",
  "source": {
    "kind": "git",
    "url": "https://github.com/yourorg/your-plugin.git",
    "ref": "v1.0.0"
  },
  "description": "Short description of the plugin.",
  "categories": ["category1", "category2"]
}
```

### 3. Submit a Pull Request

Open a pull request against `Auto-Scalers/Fabrica-plugins` with:

- The new `marketplace-index.json` entry
- A link to the plugin source repository
- A short summary of what the plugin does

### 4. Review

Maintainers review the submission against the Plugin Validation Rules (P5) and
the Plugin Review Process (P6). Approved plugins are merged and become
available in the marketplace.

## Guidelines for Good Plugins

- **IDs must be unique** — use the `{publisher}.{plugin-name}` format.
- **Point `ref` at a tagged release** — e.g. `v1.0.0`, not a branch name.
- **Do not use the `official` category** — reserved for Auto-Scalers plugins.
- **Keep descriptions concise** — one or two sentences, no marketing fluff.
- **Version your plugin** — the manifest version must match the tagged ref.
- **Test on supported Fabrica versions** — declare your minimum version in
  `engines.fabrica`.

## Do Not Submit

- Malicious code, obfuscated binaries, or anything that exfiltrates data.
- Plugins that impersonate Auto-Scalers or other publishers.
- Duplicates of existing plugins.
- Plugins requiring proprietary licenses unless the source is fully visible.

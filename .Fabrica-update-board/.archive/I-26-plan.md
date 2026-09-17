# I-26: electron-builder-config.test.mjs Verification Plan

## Overview

This plan maps every assertion in `config/scripts/electron-builder-config.test.mjs` (435 lines, 22 test cases) and identifies which ones would break if app IDs or related identifiers change to `ai.autoscalers.fabrica`.

## Current State

| File | Current appId | Line |
|------|---------------|------|
| `config/electron-builder.config.cjs` | `ai.autoscalers.fabrica` | 65 |
| `src/shared/local-build-compatibility-contract.json` | `com.fabrica-ai.fabrica` | 3 |

**Critical mismatch**: The test at lines 17-21 will **FAIL** until `local-build-compatibility-contract.json` is updated to `ai.autoscalers.fabrica`.

---

## Test-by-Test Assertion Map

### 1. `keeps the packaged app identity aligned with local-build validation` (L17-21)
- **Assertion**: `electronBuilderConfig.appId === local-build-compatibility-contract.json.appId`
- **Impact**: **BREAKS** — contract.json still has `com.fabrica-ai.fabrica`, config has `ai.autoscalers.fabrica`
- **Fix required**: Update `local-build-compatibility-contract.json` line 3 to `"appId": "ai.autoscalers.fabrica"`

### 2. `excludes repo-only source trees from app.asar` (L23-45)
- **Assertion**: `files` array contains exclusion patterns
- **Impact**: None — no appId references

### 3. `keeps local agent tooling out of app.asar` (L47-62)
- **Assertion**: FileMatcher filtering for `.claude`, `.grok`, `.agents`, `.codex` paths
- **Impact**: None — no appId references

### 4. `keeps plugin authoring examples out of app.asar` (L68-85)
- **Assertion**: FileMatcher filtering for `examples/plugins/` paths
- **Impact**: None — no appId references

### 5. `keeps cached dev Electron bundles out of app.asar` (L89-104)
- **Assertion**: FileMatcher filtering for `out/electron-dev/` paths
- **Impact**: None — no appId references

### 6. `keeps runtime resources available through extraResources` (L106-148)
- **Assertion**: Platform-specific extraResources contain expected entries
- **Impact**: None — checks resource paths, not app IDs

### 7. `ships one macOS serve-sim package through the runtime closure` (L150-158)
- **Assertion**: Single serve-sim resource entry
- **Impact**: None — no appId references

### 8. `keeps the Windows CLI shim source tree out of app.asar` (L165-178)
- **Assertion**: `files` excludes `!resources/win32{,/**/*}` and win extraResources includes fabrica.cmd
- **Impact**: None — no appId references

### 9. `ships the mac notification-status helper in Contents/MacOS, not Resources` (L182-194)
- **Assertion**: `extraFiles` contains notification-status in MacOS, `extraResources` does not
- **Impact**: None — no appId references

### 10. `ships the mac keyboard-layout helper in Contents/MacOS, not Resources` (L196-208)
- **Assertion**: `extraFiles` contains keyboard-layout in MacOS, `extraResources` does not
- **Impact**: None — no appId references

### 11. `unpacks the compiled CommonJS boundary with CLI runtime files` (L210-219)
- **Assertion**: `asarUnpack` contains CLI and shared paths
- **Impact**: None — no appId references

### 12. `unpacks the forked parcel-watcher process entry` (L223-227)
- **Assertion**: `asarUnpack` contains parcel-watcher entry
- **Impact**: None — no appId references

### 13. `unpacks the replaceable WSL transcript filesystem process entry` (L229-235)
- **Assertion**: `asarUnpack` contains WSL entry; vite.config matches pattern
- **Impact**: None — no appId references

### 14. `unpacks the OpenCode SQLite worker entry the scanner service forks` (L241-258)
- **Assertion**: Source file worker entry filename matches `asarUnpack` and vite config
- **Impact**: None — no appId references

### 15. `keeps the worker-thread hang watchdog inside app.asar` (L260-264)
- **Assertion**: `asarUnpack` does NOT contain watchdog entry
- **Impact**: None — no appId references

### 16. `uses the multi-size icon source for Linux packages` (L266-268)
- **Assertion**: `linux.icon` is `resources/build/icon.icns`
- **Impact**: None — no appId references

### 17. `matches the Linux desktop entry to Electron window class` (L270-272)
- **Assertion**: `linux.desktop.entry.StartupWMClass` is `fabrica`
- **Impact**: None — no appId references

### 18. `uses the release artifact set as local Linux targets without changing existing names` (L274-283)
- **Assertion**: Linux targets are AppImage/deb/rpm with specific artifact names
- **Impact**: None — no appId references

### 19. `retains electron-builder runtime dependencies in deb and rpm packages` (L285-293)
- **Assertion**: deb/rpm `depends` includes default FpmTarget dependencies
- **Impact**: None — no appId references

### 20. `validates each AppImage before electron-builder publishes it` (L295-311)
- **Assertion**: AppImage validation throws for non-ELF files
- **Impact**: None — no appId references

### 21. `uses a distinct AppImage name for Linux arm64 release uploads` (L312-330)
- **Assertion**: ARM64 release env var produces different artifact name
- **Impact**: None — no appId references

### 22. `overrides packaged semver only for local macOS builds` (L332-357)
- **Assertion**: Local version env var produces extraMetadata.version
- **Impact**: None — no appId references

### 23. `never applies local semver to release packaging` (L359-382)
- **Assertion**: Release builds suppress local version override
- **Impact**: None — no appId references

### 24. `uses Fabrica native rebuild hook instead of electron-builder default rebuild` (L384-387)
- **Assertion**: `beforeBuild` is the custom native rebuild function
- **Impact**: None — no appId references

### 25. `linux root-package update recovery contract` (L393-433)
- **Assertion**: Linux targets include AppImage + recoverable root-package targets; source markers match
- **Impact**: None — no appId references

---

## Summary: What Would Break

| Test | Breaks? | Reason |
|------|---------|--------|
| #1 (appId alignment) | **YES** | `local-build-compatibility-contract.json` still has old appId |
| All others | No | No appId references |

## Required Fix

Update `src/shared/local-build-compatibility-contract.json` line 3:

```diff
- "appId": "com.fabrica-ai.fabrica"
+ "appId": "ai.autoscalers.fabrica"
```

## Verification Command

```bash
pnpm test config/scripts/electron-builder-config.test.mjs
```

## Files to Modify

1. `src/shared/local-build-compatibility-contract.json` — change appId to `ai.autoscalers.fabrica`

## Risk Assessment

- **Low risk**: Only 1 test breaks, and the fix is a single JSON field update.
- **Side effects**: The appId change in the contract file affects local-build validation. Ensure `electron-builder.config.cjs` (already `ai.autoscalers.fabrica`) stays in sync.

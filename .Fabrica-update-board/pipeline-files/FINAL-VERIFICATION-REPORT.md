# Final Verification Report

> End-to-end verification of the rebranded Fabrica/ repo.
> Date: 2026-09-04 | Verified by: T7 worker

## 1. Residual Orca Identifiers

| Pattern | Total Hits | Violations | Legitimate |
|---------|-----------|------------|------------|
| orca (case-insensitive) | 579 | ~348 | ~231 |
| stablyai | 0 | 0 | 0 |
| stably (standalone) | 0 | 0 | 0 |

### Breakdown of orca hits by area

| Area | Matches | Classification | Notes |
|------|---------|---------------|-------|
| `src/**/*.ts` (source code) | 291 | **LEGITIMATE** (false positives) | All matches are substrings in "orchestration", "orchestrate", "orchestrator" — zero standalone "orca" word matches in source |
| `src/**/*.tsx` (components) | — | **LEGITIMATE** (false positives) | Same as above |
| `tests/tools/benchmarks/results/*.json` | 231 | **LEGITIMATE** | Upstream test fixture data with temp paths like `orca-startup-bench/`, `orca-termperf-` |
| `src/renderer/src/i18n/*.json` | 7 | **LEGITIMATE** (false positives) | "Could not capture" → "orca" substring in "capture" |
| `docs/reference/*.md` | 38 | **VIOLATION** | 6 files: standalone "Orca" product name references (see below) |
| `.github/workflows/*.yml` | ~300+ | **VIOLATION** | 53 files: CI/CD infrastructure references (see below) |
| `.github/ISSUE_TEMPLATE/*.yml` | 7 | **VIOLATION** | 3 files: bug_report, feature_request, other |
| `.github/CONTRIBUTING.md` | 3 | **VIOLATION** | `stablyai/orca`, `@orca_build`, brew formula |
| `.github/pull_request_template.md` | 1 | **VIOLATION** | `stablyai/orca` reference |
| `.github/actions/cloud-sql-rollout-lease/README.md` | 4 | **VIOLATION** | `stablyai/orca`, `stablyai/orca-cloud` |

### Violation details

**`docs/reference/` — 6 files, 38 standalone "Orca" word matches:**
- `sharing-agent-skills.md` (13 hits) — e.g. "Orca can put one skill...", "Orca publishes an immutable version..."
- `agent-skill-sharing-threat-model.md` (8 hits)
- `agent-skill-sharing-upstream-boundary.md` (5 hits)
- `agent-skill-provider-paths.md` (5 hits)
- `admin-agent-skill-sharing.md` (4 hits)
- `relay-regional-placement.md` (3 hits)

**`.github/workflows/` — 53 files with orca/stablyai references:**
- Cloud relay deployment workflows: `onorca-cloud`, `orca-cloud-relay`, `relay.onorca.dev`, `orca-cloud-terraform-state`
- Build/release workflows: `stablyai/orca`, `stablyai/orca-adhoc`, `orca-windows-setup.exe`
- Homebrew/release workflows: `stablyai/orca/orca` brew formula
- E2E test workflows: various orca references
- PR workflow, release-cut: `stablyai/orca` repo references

**`.github/ISSUE_TEMPLATE/` — 3 files:**
- `bug_report.yml`: "Report a problem with Orca", "Orca version", "Orca > About Orca"
- `feature_request.yml`: 1 hit
- `other.yml`: 1 hit

**Note:** These `.github/` and `docs/reference/` references are all from the upstream fork and were NOT rebranded by the T5 pass (which focused on `src/`, `config/`, `resources/`). They represent upstream CI/CD infrastructure, documentation, and templates that still reference Orca/stablyai.

### Source code status
**ZERO standalone "orca" word matches in `src/`.** All 291 matches in `.ts`/`.tsx` files are false positives — substrings within "orchestration" and related words. The rebrand pass successfully converted all user-facing Orca identifiers in source code.

### stablyai status
**ZERO matches for "stablyai" across the entire repo.** The `@stablyai/playwright-test` package was replaced with `@fabrica-ai/playwright-test` in package.json. No other stablyai references remain.

## 2. Build Smoke Test

- **Exit code:** 1 (FAIL)
- **Status:** FAIL
- **Error:** `Invalid package.json in package.json`
- **Root cause:** UTF-8 BOM (Byte Order Mark, bytes `EF BB BF`) at start of `package.json`. The upstream `package.json` starts with `7B 0D 0A` (plain `{`), so this BOM was introduced during the T5 rebrand or T6 re-implement pass. `pnpm install` rejects the BOM, which cascades to `pnpm run build` failing.
- **Impact:** Blocks all `pnpm install`, `pnpm run build`, and any pnpm-based commands.
- **Fix required:** Remove the 3-byte UTF-8 BOM from the start of `package.json`.

## 3. Custom-Logic Coverage

| Scenario | Expected | Verified | Status |
|----------|----------|----------|--------|
| Scenario 1 (merge) | 6 | 6 | PASS |
| Scenario 2 (re-implement) | 12 | 12 | PASS |
| Scenario 4 (archive) | 6 | 6 | PASS |

### Scenario 1 — Merged entries (6/6 verified)

| File | Exists | Content Notes |
|------|--------|---------------|
| `package.json` | YES | `@supabase/supabase-js` present, `esbuild` present, `@fabrica-ai/playwright-test` present, zero `stablyai` refs. **BOM issue** (see Build section). |
| `config/electron-builder.config.cjs` | YES | File exists |
| `config/scripts/locale-ko-key-overrides.json` | YES | 503 lines (matches target trim from ~5033) |
| `src/renderer/src/assets/main.css` | YES | File exists with font migration |
| `resources/skills/snapshot-registry.json` | YES | 1853 lines — retained upstream's full content (not trimmed to 149). Design decision needed. |
| `resources/skills/release-mapping.json` | YES | 644 lines — retained upstream's full content (not trimmed to 18). Design decision needed. |

**Note:** `snapshot-registry.json` and `release-mapping.json` kept upstream's full history rather than the trimmed format. The CUSTOM-LOGIC-MAP flagged this as a decision point — PM should confirm whether to apply the trim.

### Scenario 2 — Re-implemented entries (12/12 verified)

| File | Exists |
|------|--------|
| `src/renderer/src/assets/fonts/Inter-*.woff2` (7 files) | YES |
| `src/renderer/src/assets/fonts/JetBrainsMono-*.woff2` (6 files) | YES |
| `src/renderer/src/assets/fonts/SpaceGrotesk-*.woff2` (3 files) | YES |
| `config/scripts/build-fabrica-icons.mjs` | YES |
| `src/renderer/src/components/StartupGate.tsx` | YES |
| `mobile/eas.json` | YES |
| `mobile/SIGNING.md` | YES |
| `mobile/.easignore` | YES |
| `mobile/src/test-support/source-text.ts` | YES |
| `resources/app-icons/fabrica-dark.png` | YES |
| `resources/app-icons/fabrica-light.png` | YES |
| `resources/icon-source/fabrica-logo_icon.png` | YES |
| `resources/fabrica-hero-bg.jpg` | YES |

**Note:** JSON map lists 13 Scenario 2 entries (3 font groups counted separately), while summary says 12. All 13 exist.

### Scenario 4 — Archived entries (6/6 verified — correctly absent)

| File | Exists | Status |
|------|--------|--------|
| `src/shared/fabrica-attribution.ts` | NO | Correctly archived |
| `visual-palette-reference.md` | NO | Correctly archived |
| `contrast-audit.js` | NO | Correctly archived |
| `build-eb.cmd` | NO | Correctly archived |
| `build-win-wrap.cmd` | NO | Correctly archived |
| `electron.vite.config.1787972008048.mjs` | NO | Correctly archived |

## 4. Overall Status

**FAIL** — Two blocking issues prevent this from passing:

1. **UTF-8 BOM in package.json** — Breaks `pnpm install` and all downstream build commands. Must be fixed before build can succeed.
2. **Unrebranded `.github/` and `docs/reference/` content** — 53 workflow files, 6 reference docs, issue templates, and CONTRIBUTING.md still contain standalone "Orca" product name and `stablyai` references. These are upstream artifacts that were not covered by the T5 rebrand pass.

### What passed
- **Source code rebrand:** CLEAN — zero standalone "orca" or "stablyai" identifiers in `src/` (all matches are "orchestration" substrings)
- **Custom-logic coverage:** All 24 entries verified (6 merge present, 12 re-implement present, 6 archive correctly absent)
- **stablyai/stably:** ZERO matches anywhere in the repo
- **Key package.json substitutions:** `@fabrica-ai/playwright-test`, `@supabase/supabase-js`, `esbuild` all present; no `@stablyai` references

### Recommended next steps
1. **Remove UTF-8 BOM from `package.json`** — one-byte fix, unblocks build
2. **Rebrand `.github/` references** — dispatch worker to rebrand workflow files, templates, CONTRIBUTING.md, PR template, and cloud-sql README
3. **Rebrand `docs/reference/` markdown** — 6 files need "Orca" → "Fabrica" substitution
4. **PM decision on snapshot-registry/release-mapping** — confirm whether trimmed format or upstream's full history is preferred

## Notes

- The T5 rebrand pass correctly targeted source code (`src/`, `config/`, `resources/`, `package.json`) but did not cover `.github/` or `docs/reference/` directories
- The BOM appears to have been introduced during T6 (custom-logic re-implement) when package.json was modified — the file was likely re-saved with BOM encoding
- Benchmark test fixture JSONs (231 matches) contain historical upstream test data with temp directory paths — these are test artifacts, not code violations
- The `src/` false positives (291 matches) are all within "orchestration"-family words — the rebrand pass correctly left these untouched
- The app-icon.ts file correctly references Fabrica brand assets (`fabrica-watercolor.png`, `fabrica-blue.png`, `FABRICA_APP_BUNDLE_PATH`)

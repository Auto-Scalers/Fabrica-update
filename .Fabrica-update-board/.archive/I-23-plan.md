# I-23: Homebrew Cask File Name Rebrand Plan

## Overview
Rename Homebrew cask files from `orca.rb`/`orca@rc.rb` to `fabrica.rb`/`fabrica@rc.rb` and update all workflow references.

## Files to Rename

### 1. Cask Files
- `Casks/orca.rb` → `Casks/fabrica.rb`
- `Casks/orca@rc.rb` → `Casks/fabrica@rc.rb`

**Note:** File content is already rebranded (uses `cask "fabrica"` and `cask "fabrica@rc"` respectively).

## Workflow References to Update

### 2. `.github/workflows/homebrew-bump.yml`
- **Line 11:** Update comment: `Stable tags update Casks/orca.rb; RC tags update Casks/orca@rc.rb.` → `Stable tags update Casks/fabrica.rb; RC tags update Casks/fabrica@rc.rb.`
- **Line 73:** Update token assignment: `token="orca@rc"` → `token="fabrica@rc"`
- **Line 76:** Update token assignment: `token="orca"` → `token="fabrica"`
- **Line 77:** Update branch_token: `branch_token="orca"` → `branch_token="fabrica"`

## Summary of Changes

| File | Current | New |
|------|---------|-----|
| `Casks/orca.rb` | File exists | Rename to `Casks/fabrica.rb` |
| `Casks/orca@rc.rb` | File exists | Rename to `Casks/fabrica@rc.rb` |
| `.github/workflows/homebrew-bump.yml` | References `orca` | Update to `fabrica` |

## Verification Steps

1. Verify cask file contents are already rebranded (they are)
2. Rename the files using `git mv`
3. Update workflow references
4. Run `pnpm lint` to verify no broken references
5. Test workflow logic by checking token assignments

## Notes

- The cask file contents are already fully rebranded to "fabrica"
- Only the filenames and workflow token references need updating
- The `branch_token` in `homebrew-bump.yml` controls the PR branch name in the tap repository
- No other workflows reference these cask files directly

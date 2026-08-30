# P8 — Plugin Signing Research (Fabrica-plugins)

## Summary

**Fabrica does NOT use cryptographic code signing (certificates / Authenticode / Apple
notarization) for plugins.** The plugin trust model is built on (1) **content-hash
integrity verification** of an immutable installed file tree, and (2) **namespace/provenance
trust** for reserved official plugin identities. Code signing only appears in the app
itself (the Electron auto-updater), not in the plugin loader.

This means the marketplace can ship unsigned plugins today; Apple Developer Program ($99/yr)
and Windows code-signing certificates are **not required** for Fabrica plugins. If the
project later wants signed artifacts, the cost/tooling below applies.

---

## 1. What the Fabrica-app source actually does

Searched `relay-build/src/main/plugins/` and `relay-build/src/shared/plugins/` for
`sign|certificate|signature|verify|codesign|authenticode`. No plugin code matches
code-signing terms. The `verify`/`signature` hits are:

- `AbortSignal` (cancellation plumbing) — not signing
- Content-hash `verify(...)` functions — integrity, not certificates
- `updater-windows-signature-check.ts` — **Windows Authenticode check for the app
  auto-updater binary only** (`electron-updater` / `Get-AuthenticodeSignature`); not plugins

### Verification logic that DOES exist

| File | What it verifies |
|------|------------------|
| `plugin-content-integrity.ts` | `verifyHashAddressedPluginContent()` — re-hashes the installed plugin tree (SHA-256) and compares to the recorded `contentHash`. `PluginContentVerifier` de-dupes per immutable content identity. |
| `plugin-instructional-content-integrity.ts` | `verifyInstructionalPluginContent()` — re-hashes before any instructional bytes execute, ensuring content did not change since the user reviewed it. |
| `plugin-install-trust.ts` | `pluginInstallTrustError()` — enforces that **reserved plugin identities** (`autoscalers.fabrica-*`) only install from the official `Auto-Scalers` GitHub org, or from a bundled official identity. |
| `plugin-marketplace-provenance.ts` | `validateMarketplaceProvenance()` — official marketplace must have owner `auto-scalers`; reserved plugin IDs must resolve to the official org git source. |
| `plugin-install-provenance.ts` | Writes immutable `.install-provenance/<contentHash>.json` before the executable pointer moves; used to recover an interrupted install. |

### What the loader expects (`plugin-install-lockfile.ts`)

Each installed plugin is recorded with:

- `pluginKey` — qualified `<publisher>.<id>` (e.g. `fabrica.nord-theme`)
- `resolvedCommit` — the git commit the source resolved to at install time
- `contentHash` — **deterministic SHA-256 hash of the installed file tree**, which is
  also the install directory name. This is the trust anchor: the plugin is *addressed by
  its hash*, not by who signed it.
- `consentFingerprint` — what the user reviewed/approved

`PLUGIN_CONTENT_HASH_PATTERN` is a regex gate (full digest, or a legacy 128-bit prefix
accepted for early P0 installs). There is no signature/cert field in the lockfile schema.

**Net:** a plugin is trusted if (a) its file tree hashes to the recorded `contentHash`
and (b) if it uses a reserved official identity, it came from the `Auto-Scalers` org. No
certificate authority, no keypair, no notarization.

---

## 2. What code signing *would* mean for plugins (general reference)

If Fabrica later wanted signed plugin artifacts (defense-in-depth beyond hash addressing):

| Platform | Mechanism | Certificate needed | Cost (2026) | Notes |
|----------|-----------|--------------------|-------------|-------|
| **macOS** | Apple code sign + notarize (Developer ID) | Apple Developer Program membership | **$99/yr** | Required to avoid "app is damaged / Gatekeeper block" for distributables outside Mac App Store. Notarization via `notarytool`, usually <30 min. Hardened Runtime required. |
| **Windows** | Authenticode signing | OV or EV code-signing cert from a CA (DigiCert/Sectigo/SSL.com/GlobalSign) or Azure Trusted Signing | OV **$65–$300/yr**; EV **$250–$580/yr**; Azure Trusted Signing **~$9.99/mo** | Since June 2023, keys must live on FIPS 140-2 L2 HSM/USB token (EV) or token (OV). Since Mar 2024, EV no longer bypasses SmartScreen on first download — OV and EV now build reputation equally. EV still required for Windows kernel-mode drivers only. |
| **Linux** | (none standard) | n/a | Free | Tarballs typically unsigned; GPG optional. No platform gatekeeper. |

Tools: `codesign` + `notarytool` (macOS, Xcode), `signtool` (Windows, Windows SDK),
`jsign` (cross-platform Java signer, supports Azure Trusted Signing from Linux/macOS CI),
Azure Trusted Signing (cloud, no hardware token, US/CA/EU/UK orgs + US/CA individuals).

---

## 3. How Orca handled plugin signing

Orca (the orchestration platform used in this workspace) has **no plugin-signing scheme** in
this codebase. Orca loads skills/agents/plugins by path and runs them directly; trust is
operational (you control the worktree/source), not cryptographic. There is no certificate
validation or signature verification in Orca's plugin/skill loading path. So Fabrica is not
following an Orca precedent for signed plugins — Orca itself does not sign plugins.

---

## 4. Required tools, cost, Apple Dev Program?

- **Today (Fabrica as built):** No signing tools, no certificates, no Apple Developer
  Program. Plugins ship as git repos / content-hash-addressed trees. Trust = hash + namespace.
- **If you want signed plugin bundles later:**
  - macOS distribution: Apple Developer Program **$99/yr** (mandatory for notarized
    distributables) + `codesign`/`notarytool`.
  - Windows distribution: OV/EV Authenticode cert (**$65–$580/yr**) or Azure Trusted Signing
    (**~$10/mo**) + `signtool`/`jsign`, plus a hardware token for OV/EV.
  - Linux: optional GPG, free.

### Recommendation for Fabrica-plugins marketplace
Keep the current hash-addressed + namespace-trust model as the primary integrity control.
It already defeats tampering/downgrade attacks without CA dependence. Reserve real
code signing for the **app binary** (already handled by electron-updater) and only introduce
plugin signing if a future threat model requires publisher-attestation beyond "came from the
Auto-Scalers org." If added, start with the cheap path: Azure Trusted Signing (Windows) +
Apple Developer ID (Mac), ~$219/yr for cross-platform.

---

## Sources inspected
- `Fabrica-app/relay-build/src/main/plugins/plugin-content-integrity.ts`
- `Fabrica-app/relay-build/src/main/plugins/plugin-install-trust.ts`
- `Fabrica-app/relay-build/src/main/plugins/plugin-install-provenance.ts`
- `Fabrica-app/relay-build/src/main/plugins/plugin-marketplace-provenance.ts`
- `Fabrica-app/relay-build/src/main/plugins/plugin-instructional-content-integrity.ts`
- `Fabrica-app/relay-build/src/shared/plugins/plugin-marketplace.ts`
- `Fabrica-app/relay-build/src/shared/plugins/plugin-install-lockfile.ts`
- `Fabrica-app/c4-datadirs/src/shared/updater-windows-signature-check.ts` (app updater only)
- Microsoft Learn "Code signing options for Windows app developers" (2026-04)
- Apple Developer Program membership ($99/yr)
- 2026 code-signing cost comparisons (DigiCert/Sectigo/SSL.com/GlobalSign, Azure Trusted Signing)

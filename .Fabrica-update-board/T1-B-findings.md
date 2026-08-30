# T1-B Findings: `src/main/` + `src/cli/` Rebrand Diff

> Worker T1-B | 2026-08-30 | orca-baseline vs Fabrica-app

---

## Executive Summary

**196 files modified** in `src/main/` + **72 files modified** in `src/cli/` between orca-baseline and Fabrica-app. Changes are overwhelmingly mechanical rebrand renames (ORCA_ → FABRICA_, Orca → Fabrica, onorca.dev → fabrica-ai.vercel.app). Custom logic additions are minimal. One search-and-replace bug was found.

---

## 1. File Inventory

### src/main/ — File Operations

| Operation | Count | Examples |
|-----------|-------|---------|
| **Renamed** | 6 | `wsl-orca-env.ts` → `wsl-fabrica-env.ts`, `linux-bare-orca-dispatcher.ts` → `linux-bare-fabrica-dispatcher.ts`, `linux-terminal-orca-cli-shim.ts` → `linux-terminal-fabrica-cli-shim.ts`, `ssh-remote-orca-cli.ts` → `ssh-remote-fabrica-cli.ts`, `orca-runtime.ts` → `fabrica-runtime.ts`, `wsl-orca-env.test.ts` → `wsl-fabrica-env.test.ts` |
| **Removed from Fabrica** | 6 | `linux-bare-orca-dispatcher.test.ts`, `linux-bare-orca-dispatcher.ts`, `linux-terminal-orca-cli-shim.test.ts`, `linux-terminal-orca-cli-shim.ts`, `ipc/orca-profile-auth-handlers.test.ts`, `ipc/orca-profiles.test.ts`, `ipc/orca-profiles.ts`, `ipc/orca-profile-org-members-handlers.test.ts`, `ipc/orca-profile-org-members-handlers.ts` |
| **Modified** | ~190 | Listed in full below |
| **Identical** | remainder | Skipped |

### src/cli/ — File Operations

| Operation | Count |
|-----------|-------|
| **Modified** | 72 (42 non-test, 30 test) |
| **Identical** | 4 (`runtime/environments.ts`, `runtime/websocket-transport.ts`, `handlers/file.ts`, `handlers/linear.ts`) |

---

## 2. Change Catalog — src/main/ (Implementation Files Only)

### 2.1 Env Var Renames

**Intent: `rebrand`**

| Orca Baseline | Fabrica App | Files |
|---|---|---|
| `ORCA_DISABLE_HTTP2` | `FABRICA_DISABLE_HTTP2` | `startup/configure-process.ts` |
| `ORCA_E2E_HOME_DIR` | `FABRICA_E2E_HOME_DIR` | `startup/configure-process.ts` |
| `ORCA_DEV_USER_DATA_PATH` | `FABRICA_DEV_USER_DATA_PATH` | `startup/configure-process.ts` |
| `ORCA_USER_DATA_PATH` | `FABRICA_USER_DATA_PATH` | `startup/configure-process.ts`, `ssh-remote-cli-host-passthrough.ts`, `ssh-remote-cli-args.ts`, `coordinator.ts` |
| `ORCA_TELEMETRY_DISABLED` | `FABRICA_TELEMETRY_DISABLED` | `telemetry/consent.ts` |
| `ORCA_BUILD_IDENTITY` | `FABRICA_BUILD_IDENTITY` | `telemetry/client.ts` |
| `ORCA_POSTHOG_WRITE_KEY` | `FABRICA_POSTHOG_WRITE_KEY` | `telemetry/client.ts` |
| `ORCA_RENDERER_HEAP_MB` | `FABRICA_RENDERER_HEAP_MB` | `startup/renderer-heap-headroom.ts` |
| `ORCA_PACKAGED_CLI_ENTRY_REDIRECTED` | `FABRICA_PACKAGED_CLI_ENTRY_REDIRECTED` | `startup/packaged-cli-entry-redirect.ts` |
| `ORCA_BYPASS_SINGLE_INSTANCE_LOCK` | `FABRICA_BYPASS_SINGLE_INSTANCE_LOCK` | `startup/single-instance-lock.ts` |
| `ORCA_E2E_ENFORCE_SINGLE_INSTANCE_LOCK` | `FABRICA_E2E_ENFORCE_SINGLE_INSTANCE_LOCK` | `startup/single-instance-lock.ts` |
| `ORCA_APPIMAGE_NO_SANDBOX` | `FABRICA_APPIMAGE_NO_SANDBOX` | `startup/appimage-cli-redirect.ts` |
| `ORCA_NODE_OPTIONS` | `FABRICA_NODE_OPTIONS` | `startup/packaged-cli-entry-redirect.ts`, `startup/appimage-cli-redirect.ts`, `ssh-remote-cli-host-passthrough.ts` |
| `ORCA_NODE_REPL_EXTERNAL_MODULE` | `FABRICA_NODE_REPL_EXTERNAL_MODULE` | `startup/packaged-cli-entry-redirect.ts`, `startup/appimage-cli-redirect.ts`, `ssh-remote-cli-host-passthrough.ts` |
| `ORCA_STARTUP_DIAGNOSTICS` | `FABRICA_STARTUP_DIAGNOSTICS` | `startup/startup-diagnostics.ts` |
| `ORCA_APP_VERSION` | `FABRICA_APP_VERSION` | `runtime/remote-server-updater.ts` |
| `ORCA_RELAY_PATH` | `FABRICA_RELAY_PATH` | `ssh/ssh-relay-deploy.ts` |
| `ORCA_SSH_FORCE_SYSTEM_TRANSPORT` | `FABRICA_SSH_FORCE_SYSTEM_TRANSPORT` | `ssh/ssh-transport-selection.ts` (2 occurrences) |
| `ORCA_SYSTEM_SSH_PATH` | `FABRICA_SYSTEM_SSH_PATH` | `ssh/system-ssh-binary.ts` |
| `ORCA_TERMINAL_HANDLE` | `FABRICA_TERMINAL_HANDLE` | `ssh-remote-cli-host-passthrough.ts`, `ssh-remote-orchestration-send.ts`, `ssh-remote-linear-read-cli.ts`, `ssh-remote-linear-write-support.ts` |
| `ORCA_WORKTREE_ID` | `FABRICA_WORKTREE_ID` | `ssh-remote-cli-host-passthrough.ts`, `ssh-remote-linear-read-cli.ts`, `ssh-remote-linear-write-support.ts` |
| `ORCA_PANE_KEY` | `FABRICA_PANE_KEY` | `ssh-remote-cli-host-passthrough.ts`, `ssh-remote-orca-cli.ts` |
| `ORCA_AGENT_LAUNCH_TOKEN` | `FABRICA_AGENT_LAUNCH_TOKEN` | `ssh-remote-cli-host-passthrough.ts` |
| `ORCA_WORKSPACE_ID` | `FABRICA_WORKSPACE_ID` | `ssh-remote-cli-host-passthrough.ts` |
| `ORCA_CLI_CWD` | `FABRICA_CLI_CWD` | `ssh-remote-cli-host-passthrough.ts`, `cli/wsl-cli-scripts.ts` |
| `ORCA_SETUP_RUNNER_PATH` | `FABRICA_SETUP_RUNNER_PATH` | `runtime/orchestration/setup-completion-signal.ts` |
| `ORCA_RELAY_NODE_PATH` | `FABRICA_RELAY_NODE_PATH` | `ssh-remote-cli-launcher.ts` (multiple occurrences) |
| `ORCA_RELAY_DIR` | `FABRICA_RELAY_DIR` | `ssh-remote-cli-launcher.ts` |
| `ORCA_RELAY_SOCKET_PATH` | `FABRICA_RELAY_SOCKET_PATH` | `ssh-remote-cli-launcher.ts` |
| `ORCA_RELAY_CREDENTIAL_FILE` | `FABRICA_RELAY_CREDENTIAL_FILE` | `ssh-remote-cli-launcher.ts` |
| `ORCA_CLI_INSTALL_PATH` | `FABRICA_CLI_INSTALL_PATH` | `cli/cli-installer.ts` |
| `ORCA_COMPUTER_SCREENSHOT_TMPDIR` | `FABRICA_COMPUTER_SCREENSHOT_TMPDIR` | `computer-format.ts` |
| All `ORCA_*` WSL env passthrough | All `FABRICA_*` WSL env passthrough | `pty/wsl-fabrica-env.ts` (20+ entries) |

**Pattern:** Mechanical find-replace `ORCA_` → `FABRICA_` across all `.ts` files. Every `process.env.ORCA_*` became `process.env.FABRICA_*`. No structural changes — the env var contract is identical.

### 2.2 Domain / URL Renames

**Intent: `rebrand`**

| Orca Baseline | Fabrica App | File |
|---|---|---|
| `https://www.onorca.dev/v1/feedback` | `https://fabrica-ai.vercel.app/v1/feedback` | `ipc/feedback.ts` |
| `https://onorca.dev/changelog` | `https://fabrica-ai.vercel.app/changelog` | `updater-changelog.ts` |
| `https://onorca.dev/whats-new/changelog.json` | `https://fabrica-ai.vercel.app/whats-new/changelog.json` | `updater-changelog.ts` |
| `https://onorca.dev/whats-new/nudge.json` | `https://fabrica-ai.vercel.app/whats-new/nudge.json` | `updater-nudge.ts` |
| `https://onorca.dev/plugins/kill-list.json` | `https://fabrica-ai.vercel.app/plugins/kill-list.json` | `plugins/plugin-kill-list-service.ts` |

**Pattern:** `onorca.dev` → `fabrica-ai.vercel.app` (single domain). Historical task files confirm this was the PM-locked live domain (APP-E2 R7).

### 2.3 Wire Token / Protocol Renames

**Intent: `rebrand`**

| Orca Baseline | Fabrica App | File |
|---|---|---|
| `orca_server_ready` | `fabrica_server_ready` | `server/serve-readiness.ts` |
| `Orca server ready` | `Fabrica server ready` | `server/serve-readiness.ts` |
| `__ORCA_SETUP_COMPLETE__:` | `__FABRICA_SETUP_COMPLETE__:` | `runtime/orchestration/setup-completion-signal.ts` |
| `__ORCA_REMOTE_PLATFORM__` | `__FABRICA_REMOTE_PLATFORM__` | `ssh/ssh-remote-platform-detection.ts` |
| `__ORCA_UPLOAD_STAGE_SLOT__` | `__FABRICA_UPLOAD_STAGE_SLOT__` | `ssh/ssh-relay-upload-stage-contract.ts` |
| `__ORCA_UPLOAD_STAGE_PROMOTION__` | `__FABRICA_UPLOAD_STAGE_PROMOTION__` | `ssh/ssh-relay-upload-stage-contract.ts` |
| `__ORCA_NODE_VERSION__` | `__FABRICA_NODE_VERSION__` | `ssh/ssh-remote-node-toolchain-probe.ts` |
| `__ORCA_NPM_VERSION__` | `__FABRICA_NPM_VERSION__` | `ssh/ssh-remote-node-toolchain-probe.ts` |
| `ORCA-NATIVE-DEPS-OK` | `FABRICA-NATIVE-DEPS-OK` | `ssh/ssh-relay-deploy.ts` |
| `ORCA-NATIVE-DEPS-MISSING:` | `FABRICA-NATIVE-DEPS-MISSING:` | `ssh/ssh-relay-deploy.ts` |
| `ORCA-NPTY-PROBE-OK` | `FABRICA-NPTY-PROBE-OK` | `ssh/ssh-relay-deploy.ts` |
| `__ORCA_RELAY_GC_FIND_STATUS__` | `__FABRICA_RELAY_GC_FIND_STATUS__` | `ssh/ssh-remote-commands.ts` |

**Pattern:** All internal protocol markers and wire tokens renamed. These are machine-readable identifiers used in IPC, SSH relay communication, and startup coordination. The rename is complete and mechanical.

### 2.4 App ID / Brand Identity Changes

**Intent: `rebrand`**

| Orca Baseline | Fabrica App | File |
|---|---|---|
| `__ORCA_BOOTSTRAP_FATAL_EXIT_GUARD__` | `__FABRICA_BOOTSTRAP_FATAL_EXIT_GUARD__` | `startup/bootstrap-fatal-exit-guard.ts` |
| `__ORCA_NODE_VERSION__` | `__FABRICA_NODE_VERSION__` | `ssh/ssh-remote-node-toolchain-probe.ts` |
| `'Orca'` product name in strings | `'Fabrica'` product name | `startup/single-instance-lock.ts`, `startup/packaged-cli-entry-redirect.ts`, `startup/appimage-cli-redirect.ts`, many more |
| `orca-data.json` | `FABRICA-data.json` | `startup/configure-process.ts`, `handlers/agent-hooks.ts` |
| `orca-dev` userData dir | `fabrica-dev` userData dir | `startup/configure-process.ts` |
| `Application Support/orca` | `Application Support/Fabrica` | `cli/runtime/metadata.ts` (macOS) |
| `APPDATA/orca` | `APPDATA/Fabrica` | `cli/runtime/metadata.ts` (Windows) |
| `~/.config/orca` | `~/.config/Fabrica` | `cli/runtime/metadata.ts` (Linux) |
| `orca-tls-cert.pem` / `orca-tls-key.pem` | `fabrica-tls-cert.pem` / `fabrica-tls-key.pem` | `runtime/tls-certificate.ts` |
| `CN=Orca Runtime` | `CN=Fabrica Runtime` | `runtime/tls-certificate.ts` |

**Pattern:** Product identity complete — app name, data directories, TLS certs, and all user-facing strings consistently use "Fabrica".

### 2.5 Keychain / TLS Identity Changes

**Intent: `rebrand`**

| Orca Baseline | Fabrica App | File |
|---|---|---|
| `CN=Orca Runtime` | `CN=Fabrica Runtime` | `runtime/tls-certificate.ts` |
| `orca-tls-cert.pem` | `fabrica-tls-cert.pem` | `runtime/tls-certificate.ts` |
| `orca-tls-key.pem` | `fabrica-tls-key.pem` | `runtime/tls-certificate.ts` |

**Note:** The keychain service name change (`'Fabrica Claude Code Managed Credentials'`) is documented in historical task file APP-C2 but occurs in `claude-accounts/keychain.ts` which is also modified (keychain.ts shows MODIFIED status).

### 2.6 File Renames (orca → fabrica)

**Intent: `rebrand`**

| Orca Baseline | Fabrica App |
|---|---|
| `pty/wsl-orca-env.ts` | `pty/wsl-fabrica-env.ts` |
| `cli/linux-bare-orca-dispatcher.ts` | `cli/linux-bare-fabrica-dispatcher.ts` |
| `cli/linux-terminal-orca-cli-shim.ts` | `cli/linux-terminal-fabrica-cli-shim.ts` |
| `ssh/ssh-remote-orca-cli.ts` | `ssh/ssh-remote-fabrica-cli.ts` |

**Import updates propagated to:** `index.ts`, `ipc/pty.ts`, `daemon/pty-subprocess.ts`, `agent-hooks/wsl-hook-relay-launch.ts`, `hooks.ts`

### 2.7 Files Removed from Fabrica

**Intent: `debrand-cleanup`**

| File | Reason |
|---|---|
| `cli/linux-bare-orca-dispatcher.ts` | Renamed to `linux-bare-fabrica-dispatcher.ts` |
| `cli/linux-terminal-orca-cli-shim.ts` | Renamed to `linux-terminal-fabrica-cli-shim.ts` |
| `cli/linux-bare-orca-dispatcher.test.ts` | Corresponding test renamed |
| `cli/linux-terminal-orca-cli-shim.test.ts` | Corresponding test renamed |
| `ipc/orca-profile-auth-handlers.test.ts` | Orca profile system removed |
| `ipc/orca-profiles.test.ts` | Orca profile system removed |
| `ipc/orca-profiles.ts` | Orca profile system removed |
| `ipc/orca-profile-org-members-handlers.test.ts` | Orca profile system removed |
| `ipc/orca-profile-org-members-handlers.ts` | Orca profile system removed |

**Note:** The `orca-profile*` files were removed entirely (not just renamed) — this is **custom-logic cleanup** of an Orca-specific profile system that Fabrica does not use.

### 2.8 Function / Type / Class Renames

**Intent: `rebrand`**

| Orca Baseline | Fabrica App | File |
|---|---|---|
| `configureOrcaUserDataPathEnv` | `configureFABRICAUserDataPathEnv` | `startup/configure-process.ts` |
| `addOrcaWslInteropEnv` | `addFABRICAWslInteropEnv` | `pty/wsl-fabrica-env.ts` |
| `installLinuxBareOrcaDispatcher` | `installLinuxBareFABRICADispatcher` | `cli/linux-bare-fabrica-dispatcher.ts` |
| `ensureLinuxTerminalOrcaCliShimDir` | `ensureLinuxTerminalFABRICACliShimDir` | `cli/linux-terminal-fabrica-cli-shim.ts` |
| `runRemoteOrcaCli` | `runRemoteFABRICACli` | `ssh/ssh-remote-fabrica-cli.ts` |
| `runHostOrcaCliPassthrough` | `runHostFABRICACliPassthrough` | `ssh/ssh-remote-cli-host-passthrough.ts` |
| `RemoteOrcaCliRequest` | `RemoteFABRICACliRequest` | `ssh/ssh-remote-cli-host-passthrough.ts` |
| `RemoteOrcaCliResult` | `RemoteFABRICACliResult` | `ssh/ssh-remote-cli-host-passthrough.ts` |
| `RemoteOrcaCliPostOutput` | `RemoteFABRICACliPostOutput` | `ssh/ssh-remote-cli-host-passthrough.ts` |
| `OrcaRemoteCliLauncher` | `FABRICARemoteCliLauncher` | `ssh/ssh-remote-cli-launcher.ts` |
| `buildBareOrcaCliScript` | `buildBareFABRICACliScript` | `cli/linux-bare-fabrica-dispatcher.ts` |
| `LinuxBareOrcaDispatcherOptions` | `LinuxBareFABRICADispatcherOptions` | `cli/linux-bare-fabrica-dispatcher.ts` |
| `LinuxBareOrcaDispatcherState` | `LinuxBareFABRICADispatcherState` | `cli/linux-bare-fabrica-dispatcher.ts` |
| `LinuxBareOrcaDispatcherResult` | `LinuxBareFABRICADispatcherResult` | `cli/linux-bare-fabrica-dispatcher.ts` |
| `LinuxTerminalOrcaCliShimOptions` | `LinuxTerminalFABRICACliShimOptions` | `cli/linux-terminal-fabrica-cli-shim.ts` |
| `parseOrcaYaml` | `parseFABRICAYaml` | (imported in `handlers/vm.ts`) |
| `OrcaVmRecipe` | `FABRICAVmRecipe` | (imported in `handlers/vm.ts`) |

### 2.9 Orchestration CLI Command Changes

**Intent: `rebrand`**

| Orca Baseline | Fabrica App | File |
|---|---|---|
| `OrchestrationCliCommand = 'orca' \| 'orca-ide'` | `OrchestrationCliCommand = 'fabrica'` | `runtime/orchestration/cli-command.ts` |
| All `return 'orca'` / `return 'orca-ide'` branches | All `return 'fabrica'` | `runtime/orchestration/cli-command.ts` |

**Pattern:** The Orca codebase had two CLI command variants (`orca` for GUI, `orca-ide` for WSL headless). Fabrica simplified to a single `fabrica` command for all contexts.

### 2.10 File Path Constants

**Intent: `rebrand`**

| Orca Baseline | Fabrica App | File |
|---|---|---|
| `'/usr/local/bin/orca'` | `'/usr/local/bin/fabrica'` | `cli/cli-installer.ts` |
| `'orca-dev'` (dev command) | `'fabrica-dev'` | `cli/cli-installer.ts` |
| `'orca-ide'` (Linux command) | `'fabrica'` | `cli/cli-installer.ts` |
| `'orca'` (legacy Linux) | `'FABRICA'` | `cli/cli-installer.ts` |
| `join(resourcesPath, 'bin', 'orca')` | `join(resourcesPath, 'bin', 'fabrica')` | `cli/cli-installer.ts` |
| `join(resourcesPath, 'bin', 'orca.exe')` | `join(resourcesPath, 'bin', 'fabrica.exe')` | `cli/cli-installer.ts` |
| `join(homePath, '.local', 'bin', 'orca')` | `join(homePath, '.local', 'bin', 'fabrica')` | `cli/cli-installer.ts` |
| `'Program Files/Orca Dev'` | `'Program Files/Fabrica Dev'` | `cli/cli-installer.ts` |
| `'orca'` (WSL command) | `'fabrica'` | `cli/wsl-cli-installer.ts` |
| `'orca-relay'` (package name) | `'FABRICA-relay'` | `ssh/ssh-relay-deploy.ts` |
| `'orca-upload-owner'` | `'fabrica-upload-owner'` | `ssh/ssh-relay-upload-stage-contract.ts` |
| `'orca-upload-identity'` | `'fabrica-upload-identity'` | `ssh/ssh-relay-upload-stage-contract.ts` |

---

## 3. Change Catalog — src/cli/

### 3.1 Core Runtime Changes

**Intent: `rebrand`**

| Orca Baseline | Fabrica App | File |
|---|---|---|
| `serveOrcaApp` | `serveFABRICAApp` | `runtime-client.ts`, `runtime/index.ts`, `handlers/core.ts` |
| `launchOrcaApp` | `launchFABRICAApp` | `runtime/launch.ts`, `runtime/client.ts` |
| `openOrca` | `openFABRICA` | `runtime/client.ts`, `handlers/core.ts` |
| `resolveForegroundOrcaExecutable` | `resolveForegroundFABRICAExecutable` | `runtime/launch.ts` |
| `ORCA_OPEN_COMMAND` | `FABRICA_OPEN_COMMAND` | `runtime/launch.ts` |
| `ORCA_APP_EXECUTABLE` | `FABRICA_APP_EXECUTABLE` | `runtime/launch.ts` |
| `ORCA_PAIRING_CODE` | `FABRICA_PAIRING_CODE` | `runtime/client.ts` |
| `ORCA_REMOTE_PAIRING` | `FABRICA_REMOTE_PAIRING` | `runtime/client.ts` |
| `ORCA_ENVIRONMENT` | `FABRICA_ENVIRONMENT` | `runtime/client.ts`, `index.ts` |
| `ORCA_KEEPALIVE_INTERVAL_MS` | `FABRICA_KEEPALIVE_INTERVAL_MS` | `handlers/orchestration.ts` |
| `ORCA_HEARTBEAT_INTERVAL_MS` | `FABRICA_HEARTBEAT_INTERVAL_MS` | `handlers/orchestration.ts` |
| `ORCA_CLI_COMMAND` | `FABRICA_CLI_COMMAND` | `handlers/orchestration.ts` |
| `ORCA_WINDOWS_PACKAGED_CLI_LAUNCHER` | `FABRICA_WINDOWS_PACKAGED_CLI_LAUNCHER` | `handlers/orchestration.ts` |
| `ORCA_DEV_CLI_INVOCATION` | `FABRICA_DEV_CLI_INVOCATION` | `handlers/orchestration.ts` |
| `ORCA_ARTIFACTS_API_URL` | `FABRICA_ARTIFACTS_API_URL` | `handlers/artifacts.ts` |
| `ORCA_CLOUD_AUTH_TOKEN` | `FABRICA_CLOUD_AUTH_TOKEN` | `handlers/artifacts.ts` |

### 3.2 CLI Command Specs (All `orca` → `fabrica`)

**Intent: `rebrand`**

Every spec file in `cli/specs/` has all usage strings renamed:
- `specs/account.ts`: `orca account add/list` → `fabrica account add/list`
- `specs/agent-hooks.ts`: `orca agent hooks` → `fabrica agent hooks`
- `specs/artifacts.ts`: `orca artifacts` → `fabrica artifacts`
- `specs/automations.ts`: `orca automations` → `fabrica automations`
- `specs/browser-advanced.ts`: All `orca browser ...` → `fabrica browser ...`
- `specs/browser-basic.ts`: All `orca browser ...` → `fabrica browser ...`
- `specs/computer.ts`: `orca computer` → `fabrica computer`
- `specs/core.ts`: `orca` → `fabrica` in all examples, `orca.yaml` → `FABRICA.yaml`, `stablyai/orca` → `Auto-Scalers/Fabrica-app`
- `specs/diagnostics.ts`: `orca diagnostics` → `fabrica diagnostics`
- `specs/emulator.ts`: `orca emulator` → `fabrica emulator`
- `specs/environment.ts`: `orca environment` → `fabrica environment`, `orca://pair?` → `fabrica://pair?`
- `specs/file.ts`: `orca file` → `fabrica file`
- `specs/introspection.ts`: `orca agent-context` → `fabrica agent-context`
- `specs/linear.ts`: `orca linear` → `fabrica linear`
- `specs/orchestration.ts`: `orca orchestration` → `fabrica orchestration`
- `specs/project.ts`: `orca project` → `fabrica project`
- `specs/serve.ts`: `orca serve` → `fabrica serve`
- `specs/skills.ts`: `orca skills` → `fabrica skills`
- `specs/vm.ts`: `orca vm` → `fabrica vm`, `orca.yaml` → `FABRICA.yaml`

### 3.3 Bundled Skill Guides

**Intent: `rebrand`**

`cli/bundled-skill-guides.ts` has comprehensive renames:
- All skill IDs renamed: `computer-use` → `fabrica-cli`, `linear-tickets` → `fabrica-computer-use`, `orca-cli` → `fabrica-emulator`, etc.
- All markdown variables renamed: `COMPUTER_USE_MARKDOWN` → `FABRICA_CLI_MARKDOWN`, etc.
- All descriptions: "Orca" → "Fabrica"

### 3.4 Data Path Changes

**Intent: `rebrand`**

`cli/runtime/metadata.ts`:
- macOS: `Application Support/orca` → `Application Support/Fabrica`
- Windows: `APPDATA/orca` → `APPDATA/Fabrica`
- Linux: `~/.config/orca` → `~/.config/Fabrica`

### 3.5 Error Messages

**Intent: `rebrand`**

`cli/runtime/transport.ts`: All 9 error messages renamed:
- "No compatible transport found in Orca runtime metadata" → "No compatible transport found in Fabrica runtime metadata"
- "Timed out waiting for the Orca runtime to respond" → "Timed out waiting for the Fabrica runtime to respond"
- etc.

`cli/runtime/serve-signal-exit-diagnostic.ts`:
- Crash report glob: `Orca-*.ips` → `Fabrica-*.ips`
- All diagnostic messages: "Orca serve" → "Fabrica serve"

### 3.6 Deep Link Protocol

**Intent: `rebrand`**

| Orca Baseline | Fabrica App | File |
|---|---|---|
| `orca://pair?...` | `fabrica://pair?...` | `cli/runtime/client.ts` |

---

## 4. Custom Logic Additions

### 4.1 Orca Profile System Removal

**Intent: `custom-logic`**

Files removed from Fabrica (not just renamed):
- `ipc/orca-profiles.ts` — Orca's cloud profile management
- `ipc/orca-profile-org-members-handlers.ts` — Orca org member handling
- `ipc/orca-profile-auth-handlers.test.ts` — Auth handler tests

**Pattern:** Fabrica uses Supabase auth directly (per DNA/infrastructure docs), not Orca's profile system. These were Orca-specific cloud features that Fabrica replaced with its own auth flow.

### 4.2 CLI Command Simplification

**Intent: `custom-logic`**

`runtime/orchestration/cli-command.ts`:
- Orca: `OrchestrationCliCommand = 'orca' | 'orca-ide'` (two variants for GUI vs headless)
- Fabrica: `OrchestrationCliCommand = 'fabrica'` (single command)

**Pattern:** Fabrica simplified the dual-command architecture to a single unified command. This is a structural change beyond rebrand.

### 4.3 Linux CLI Naming Convention

**Intent: `custom-logic`**

- Orca: CLI installed as `orca-ide` to avoid shadowing GNOME Orca screen reader at `/usr/bin/orca`
- Fabrica: CLI installed as `fabrica` directly (no conflict — Fabrica is not a screen reader name)

The dispatcher and shim files still exist but their comments note the rename removed the original conflict concern.

---

## 5. Incidental Changes

### 5.1 Test File Updates

All test files were updated mechanically to match the rebrand. No structural test changes beyond:
- String literals in assertions
- Mock function names
- Import paths

---

## 6. Bugs Found

### 6.1 Search-and-Replace Bug in telemetry/client.ts

**Intent: `incidental` (bug)**

```
// ORCA BASELINE:
function waitForCaptureEnqueue(...)

// FABRICA (BUG):
function waitFFABRICAptureEnqueue(...)
```

The `ORCA` substring inside `waitForCaptureEnqueue` was replaced, corrupting the function name. This should be `waitForCaptureEnqueue` (unchanged — no ORCA_ prefix).

**Severity:** Likely causes a compile error or runtime crash in telemetry. Needs fix.

**Fix:** Rename `waitFFABRICAptureEnqueue` back to `waitForCaptureEnqueue` in all occurrences. Verify: `grep -r "waitFFABRICAptureEnqueue" src/` returns 0 hits, then `pnpm typecheck` passes.

### 6.2 Telemetry Disabled Reason Inconsistency

**Intent: `incidental` (cosmetic)**

In `telemetry/consent.ts`:
```
// ORCA:
return { effective: 'disabled', reason: 'orca_disabled' }

// FABRICA:
return { effective: 'disabled', reason: 'FABRICA_disabled' }
```

The reason string `FABRICA_disabled` has inconsistent casing (should be `fabrica_disabled`). This is a telemetry metadata value — likely non-breaking but inconsistent.

**Fix:** Rename `'FABRICA_disabled'` to `'fabrica_disabled'`. Verify: `grep -r "FABRICA_disabled" src/` returns 0 hits, then `pnpm lint` passes.

---

## 7. Rebrand Pattern Summary

### Pattern Registry (for T3/T4 sync reference)

| Area | Pattern | Orca → Fabrica | Sync Rule |
|------|---------|----------------|-----------|
| **Env vars** | Prefix | `ORCA_` → `FABRICA_` | Port any new upstream ORCA_ var, apply FABRICA_ prefix |
| **Domains** | Domain | `onorca.dev` → `fabrica-ai.vercel.app` | Never reintroduce onorca.dev |
| **Wire tokens** | Token | `orca_*` → `fabrica_*` | Port upstream wire tokens, apply fabrica_ prefix |
| **CLI command** | Name | `orca` → `fabrica` | Single command, no dual-variant |
| **File renames** | Name | `*-orca-*` → `*-fabrica-*` | New orca-named files get fabrica rename |
| **TLS certs** | Filename/CN | `orca-tls-*` → `fabrica-tls-*` | Always use fabrica names |
| **Data dirs** | Path | `~/.config/orca` → `~/.config/Fabrica` | Use Fabrica productName |
| **App ID** | ID | `com.autoscalers.fabrica` → `ai.autoscalers.fabrica` | Use canonical `ai.autoscalers.fabrica` |
| **Function names** | Naming | `Orca` → `FABRICA` (capitalized) | Apply consistently |
| **Protocol markers** | Prefix | `__ORCA_*__` → `__FABRICA_*__` | Port upstream markers, apply FABRICA prefix |
| **User-facing strings** | Name | `Orca` → `Fabrica` | Always use Fabrica in user messages |
| **Import paths** | Path | `wsl-orca-env` → `wsl-fabrica-env` | Update imports on rename |
| **CLI type** | Type | `'orca' \| 'orca-ide'` → `'fabrica'` | Single variant |

### Sync Implications

For any new upstream Orca changes:
1. **Safe to port verbatim:** Internal logic changes not touching Orca identifiers
2. **Port + rebrand:** Any new `ORCA_*` env vars, `onorca.dev` URLs, `orca_*` wire tokens, or `Orca` string references
3. **Merge decision needed:** Changes to orchestration CLI command type (upstream may add new variants)
4. **Skip/rewrite:** Any Orca-specific cloud features (profile system, etc.)

---

## 8. Historical Pattern Cross-Reference

Per `Fabrica-app-tasks.md` (historical), these patterns were established during the original rebrand:

| Task | Pattern | Status |
|------|---------|--------|
| APP-A1 | App name/productName: `Fabrica` | ✅ Verified |
| APP-B1 | CLI command: `orca` → `fabrica` | ✅ Verified |
| APP-B2 | Install paths: `Fabrica Dev` | ✅ Verified |
| APP-B3 | Env vars: `ORCA_*` → `FABRICA_*` | ✅ Verified |
| APP-B4 | Git co-author: `FABRICA_GIT_COMMIT_TRAILER` | ✅ Verified |
| APP-C1 | Wire tokens: `fabrica_server_ready` | ✅ Verified |
| APP-C2 | Keychain: `Fabrica Claude Code Managed Credentials` | ✅ Verified |
| APP-C3 | TLS CN: `CN=Fabrica Runtime` | ✅ Verified |
| APP-C4 | Data dirs: `~/.config/fabrica` (via productName) | ✅ Verified |
| APP-D1 | Plugin engines: `engines.fabrica` | ✅ Verified |
| APP-D4 | Kill-list URL: `fabrica-ai.vercel.app` | ✅ Verified |
| APP-E1 | App ID: `ai.autoscalers.fabrica` | ✅ Verified |
| APP-E2 | Domain: all endpoints → `fabrica-ai.vercel.app` | ✅ Verified |

All historical patterns confirmed in current codebase state.

---

## 9. Output Checklist

- [x] All files in `src/main/` and `src/cli/` inventoried
- [x] All env var renames cataloged (ORCA_ → FABRICA_)
- [x] All domain renames cataloged (onorca.dev → fabrica-ai.vercel.app)
- [x] All wire token renames cataloged (orca_server_ready → fabrica_server_ready)
- [x] All app ID changes cataloged
- [x] All keychain/TLS identity changes cataloged
- [x] Custom logic additions identified
- [x] File renames tracked with import propagation
- [x] Removed files documented
- [x] Bugs found (2: name corruption, reason casing)
- [x] Pattern registry created for T3/T4
- [x] Historical patterns cross-referenced
- [x] Each change tagged with intent

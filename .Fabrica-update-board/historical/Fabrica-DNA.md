# Fabrica — DNA

> The immutable identity. These don't change with sprints — they are the reason Fabrica exists.

**Central Devlopment Envirenment** `Auto-Scalers/Fabrica-development_environment` (.Fabrica-board/)

**App Repo:** `Auto-Scalers/Fabrica-app` (Fabrica-app/), (Fabrica-app/.Fabrica-app-board/)

**Landing page:** `fabrica-ai.vercel.app`
**Landing page Repo** `Auto-Scalers/Fabrica-web` (Fabrica-web/), (Fabrica-web/.Fabrica-web-board)

**Marketing and GTM Repo** `Auto-Scalers/Fabrica-marketing` (Fabrica-marketing/), (Fabrica-marketing/.Fabrica-marketing-board)

**Plugin Marketplace Repo** `Auto-Scalers/Fabrica-plugins` (Fabrica-plugins/), (Fabrica-plugins/.Fabrica-plugins-board)

**Relay Server Repo** `Auto-Scalers/Fabrica-relay` (Fabrica-relay/), (Fabrica-relay/.Fabrica-relay-board) — Cloudflare Workers + Durable Objects + Hono, Supabase auth

**Base:** Orca (`stablyai/orca`) — MIT licensed (archived in `.backup/orca`)
**Helpers Repos** `_sources/legacy-fabrica` and `_sources/mission-control` and `_sources/buzz`

---

## Mission

Fabrica empowers non-technical founders to build and run AI-powered businesses — without writing code or managing infrastructure.

---

## Vision

A world where business owners can both build and run their companies through AI — deploying autonomous agents for customer support, e-commerce management, marketing, and operations — all from a single desktop app, fully local, fully in their control.

---

## Values

| Value | What it means in practice |
|-------|--------------------------|
| Local-first | Your data, your agents, your machine. No cloud dependency. |
| Business-first | Built for founders and operators, not just developers. |
| Quality-first | Every feature ships polished. No half-done work. |
| Autonomy | Agents work for you 24/7 without babysitting. |
| Transparency | You see what agents do, why they do it, and can intervene anytime. |

---

## Strategic Pillars

Evergreen themes that guide every decision. Every feature traces back to one of these.

1. **Democratize AI business automation** — Make powerful agentic workflows accessible to anyone, regardless of technical background.
2. **Universal control surface** — Everything manageable from the UI. No CLI, no code, no config files — for both technical and non-technical users.
3. **Local, private, transparent** — Data never leaves the machine unless the user explicitly sends it.
4. **Ecosystem & composability** — Plugins, skills, agent crews — users build their own stacks.
5. **Business outcomes over technical features** — Optimize for revenue, growth, and efficiency — not benchmarks.

---

## Anti-Goals

What Fabrica will never do:

- Require cloud connectivity to function
- Sell user data or metadata
- Build a walled garden — extensibility is core
- Optimize for developer ergonomics at the expense of business user experience
- Ship features that require CLI or code to use

---

## App ID

**Unified across all platforms:** `ai.autoscalers.fabrica`

| Platform                 | Value                                 |
| ------------------------ | ------------------------------------- |
| electron-builder appId   | `ai.autoscalers.fabrica`              |
| macOS CFBundleIdentifier | `ai.autoscalers.fabrica`              |
| macOS helper             | `ai.autoscalers.fabrica.computer-use` |
| Windows AUMID            | `ai.autoscalers.fabrica`              |
| Windows/Linux            | `ai.autoscalers.fabrica`              |
| Deep link protocol       | `fabrica://`                          |
| Future iOS bundle ID     | `ai.autoscalers.fabrica`              |
| Future Android package   | `ai.autoscalers.fabrica`              |

---

## Infrastructure

| Service            | Where                                | Owner           |
| ------------------ | ------------------------------------ | --------------- |
| Landing page       | Vercel                               | Fabrica-web     |
| Backend API        | Vercel (API routes)                  | Fabrica-web     |
| Auth               | Supabase (shared project)            | Fabrica-web     |
| Telemetry          | PostHog                              | Fabrica-app     |
| Relay              | Cloudflare Workers + Durable Objects | Fabrica-relay   |
| Auto-updater       | GitHub Releases                      | Fabrica-app     |
| Plugin marketplace | GitHub repo                          | Fabrica-plugins |

---

## Deferred Items

| Item            | Blocker                    | What's Needed                        |
| --------------- | -------------------------- | ------------------------------------ |
| Code signing    | Apple Developer Program    | $99/year Apple Dev, Windows SignPath |
| App Store (iOS) | Apple Dev Program + review | Dev membership, listing, review      |
| Google Play     | $25 fee                    | Google Play Developer account        |

---

_Reviewed quarterly. Changes only when the founding vision changes._

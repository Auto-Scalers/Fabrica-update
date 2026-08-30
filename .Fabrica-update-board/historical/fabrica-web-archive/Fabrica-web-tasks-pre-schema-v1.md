# Fabrica-web — Tasks

> Single source of truth for all landing page / API route work. The Roadmap (`.Fabrica-Board/Fabrica-Roadmap.md`) tracks cross-cutting status only — this file owns execution details.

---

## Status Legend

- **VERIFY** — implemented, needs verification
- **VERIFY** — implemented and verified
- **PARTIAL** — partially implemented
- **TODO** — planned, not started
- **BLOCKED** — waiting on dependency

---

## API Routes (Vercel)

These endpoints serve the desktop app. The app already has client code pointing to them — we need to build the servers.

| # | Endpoint | Purpose | Status | Notes |
|---|----------|---------|--------|-------|
| W1 | `/api/auth/authorize` | OAuth authorize (PKCE) | **VERIFY** | Supabase auth |
| W2 | `/api/auth/session` | Session management | **VERIFY** | |
| W3 | `/api/auth/refresh` | Token refresh | **VERIFY** | |
| W4 | `/api/auth/logout` | Session destroy | **VERIFY** | |
| W5 | `/api/share/*` | Artifact sharing CRUD | **VERIFY** | Supabase Storage |
| W6 | `/api/diagnostics/*` | Crash/feedback upload | **VERIFY** | |
| W7 | `/api/telemetry` | Analytics events (fallback) | **VERIFY** | PostHog is primary |

---

## Static Files

| # | File | Purpose | Status | Notes |
|---|------|---------|--------|-------|
| W8 | `/whats-new/changelog.json` | In-app changelog display | **VERIFY** | Created in public/whats-new/ |
| W9 | `/whats-new/nudge.json` | Update nudge config | **VERIFY** | Created in public/whats-new/ |
| W10 | `/plugins/kill-list.json` | Plugin block list | **VERIFY** | Created in public/plugins/ |

---

## Docs Site

| # | Task | Status | Notes |
|---|------|--------|-------|
| W11 | `/docs/*` — documentation pages | **DONE** | Created layout, catch-all page, sidebar, prose, content, nav. Build compiles. Zero orca/stablyai refs. |

---

## Landing Page Updates

> **PLANNING MODE** — Plan and refine only. Do not execute code changes.

| # | Task | Status | Notes |
|---|------|--------|-------|
| W12 | Update any Orca references in page copy | **DONE** | Audit complete: 3 refs in public/ (historical changelog + kill-list plugin). Not page copy. No changes needed. |
| W13 | Update meta tags / OG images if needed | **DONE** | Audit complete: all OG/Twitter/meta tags already say Fabrica with correct URLs. No changes needed. |
| W13b | Update pricing tiers in landing page | **DONE** | Tiers renamed: Power User, One-Person Company, Agency & Teams. All CTAs say "Start 14-Day Free Trial". Updated en.json, fr.json, ar.json. Prices remain placeholders ($29/$79/$199). |

---

## Phase 6 — Landing Page Enhancement

> **CRITICAL:** Every element of the landing page must be derived from the 3 internal marketing files. Read each file line by line, word by word. No copy should exist that doesn't align with these files.
>
> **Source files (read all 3 before writing any copy):**
> - `Fabrica-marketing/internal/brand/brand-guidelines.md` — voice, tone, visual identity, word bank, blacklist, correct/incorrect usage examples
> - `Fabrica-marketing/internal/brand/positioning-statement.md` — positioning, key differentiators, messaging hierarchy, proof points
> - `Fabrica-marketing/internal/research/competitor-landscape.md` — competitor insights, positioning opportunities, market gaps, Fabrica response to each competitor

| # | Task | Status | Notes |
|---|------|--------|-------|
| W14 | Replace carousel images with new assets from `public/images/` | **DONE** | Updated ShowcaseCarousel.tsx with correct image paths (carousel-00 through carousel-10). Fixed aspect ratio to 16/9, switched to object-contain for full images. |
| W15 | Add standalone images with text overlays from `public/images/standalones/` | **DONE** | All 5 standalone images added: pain-exhausted-developer (PainSection), social-parallel-agents (TurnSection), social-approval-gate (ControlSection), mobile-companion-remote (OrchestrationSection), hands-on-architecture (FeatureSection). All use object-contain. |
| W16 | Apply `fabrica-buttom-bg` as background to bottom section | **DONE** | Added fabrica-buttom-bg.png background to FinalCta.tsx following Hero.tsx pattern (absolute inset, backgroundSize 100% auto, bg-white/20 dark overlay). |
| W17 | Rewrite ALL landing page copy using the 3 marketing internal files | **DONE** | Full rewrite of en.json using brand-guidelines.md, positioning-statement.md, and competitor-landscape.md. All sections grounded in source docs. Blacklisted terms removed. |
| W18 | French & Arabic localization quality pass | **DONE** | fr.json and ar.json fully updated to match new en.json. Natural phrasing, brand voice preserved. All sections aligned. |
| W19 | Mobile responsiveness audit — verify all new sections | **VERIFY** | Audit complete. Fixed: (1) ShowcaseCarousel nav buttons always visible on touch, pagination dots enlarged. (2) OrchestrationSection engine nav touch targets enlarged, overflow-hidden added. (3) FinalCta background image changed to `cover` for proper mobile scaling. (4) ControlSection gate toggles enlarged, range slider thickened. All other sections verified clean. |

**Rules for W17:**
1. Read `brand/brand-guidelines.md` — adopt the voice, use the word bank, respect the blacklist
2. Read `brand/positioning-statement.md` — use the positioning statement, key differentiators, and messaging hierarchy verbatim where appropriate
3. Read `research/competitor-landscape.md` — use competitor insights, proof points, and Fabrica responses for the "Why Fabrica" section
4. Every section of the landing page must trace back to one of these 3 files
5. No generic marketing copy — everything must be grounded in the internal docs

### Phase 6b — PM Review Feedback (Aug 2026)

> Feedback from PM visual review of the landing page. Execute after W14-W19.

| # | Task | Status | Notes |
|---|------|--------|-------|
| W20 | Top background: darken text in light theme | **DONE** | Light-theme text contrast adjusted; hero headline sizes tuned per PM follow-up in-session |
| W21 | Audit: all carousel + standalone images present? | **DONE** | Carousel: all 11 images now shown as slides s1-s11 (en/fr/ar). Standalones: all 5 visible on page load — mobile-companion image promoted out of hidden tab into always-visible block |
| W22 | Standalone images: side-by-side layout with relevant text | **DONE** | All 5 standalones in half-width two-column layouts with strictly matching text, stacked on mobile |
| W23 | Bottom background: add blur + strengthen overlay | **DONE** | Per PM follow-up in-session: removed blur layers that softened FinalCta text, content pinned to top with z-10, scrim tuned |
| W24 | Deduplicate content across all sections | **DONE** | 23 strings deduped across en/fr/ar. Worktrees star = features/orchestration/faq; budget star = control.card3; approval gates star = control; BYOK star = integrations/control.card2; cta.paragraph rewritten. 702 keys per locale, build passes |
| W25 | Coverage audit vs internal marketing files | **DONE** | Report: W25-coverage-audit.md in this board folder (33KB). Coverage map, missing gold, honesty check, pain-solving audit, value-prop clarity, 18 prioritized recommendations. Awaiting PM review before applying fixes |
| W26 | Top navigation bar: align with page sections | **TODO** | Make sure the top navigation bar links align with the actual page sections (correct anchors, correct scroll positions) |
| W27 | P1 honesty fixes from W25 audit | **IN PROGRESS** | PM-approved: remove fabricated testimonials section entirely (0 users — reinstate only with real quotes); replace $149/mo in hero logs; Free tier → 14-day trial; remove Unlimited crews claim; unsourced 3.5 hrs/day metric made illustrative |
| W28 | P2+P3 content additions from W25 audit | **TODO** | n8n visibility in BeyondCode; business roadmap + vault concept; adaptive interface mention; hero who-it's-for + control promise; non-technical pain earlier; soften spend-tracking claims |

---

## Infrastructure Notes

**Supabase project:** `xoynlmscwkimaopkavkj.supabase.co` (shared with app)

| Table | Purpose |
|-------|---------|
| `auth.users` + `auth.sessions` | Desktop app OAuth login |
| `user_profiles` | Link Supabase auth to desktop app |
| `organizations` | Multi-org support |
| `artifacts` | Share URLs for files + Storage bucket |
| `diagnostics` | Crash reports + feedback |
| `early_access_signups` | Landing page signup (exists ✅) |

**Environment variables:**
- `NEXT_PUBLIC_SUPABASE_URL`
- `SUPABASE_SERVICE_ROLE_KEY`

---

## What Needs Verification

- [~] Landing page deployed (`fabrica-ai.vercel.app`)
- [~] Early-access signup flow (`/api/early-access` → Supabase)
- [~] Dark theme + copper/amber palette
- [~] Vercel deployment pipeline

---

## Session Ledger

> Tracks orchestration sessions and workers for this task file. Updated when sessions are created, released, or worktrees merged.

| Session Handle | Type | Task/Group | Status | Created | Worktree Branch | Merged |
|---------------|------|-----------|--------|---------|----------------|--------|
| `term_0adb1b43-ceeb-47c7-ad47-f65f6df17d3e` | orchestrator | web-orchestrator | **active** | Aug 2026 | `main` (Fabrica-web/) | — |
| `ctx_3325b3345a41` | worker | W12 Orca copy audit | **released** | Aug 2026 | `web-W12-audit` | ✅ |
| `ctx_00d2c7b7121a` | worker | W13 meta/OG rebrand | **released** | Aug 2026 | `web-W13-meta` | ✅ |
| `ctx_5685e8eae1d3` | worker | W13b Pricing tiers | **released** | Aug 2026 | `main` (Fabrica-web/) | ✅ |
| `ctx_d2bdaef9b4b8` | worker | W14+W15+W16 Visual assets | **released** | Aug 2026 | `main` (Fabrica-web/) | ✅ |
| `task_4799a55a4149` | task | W17 Copy rewrite | **completed** | Aug 2026 | — | ✅ |
| `term_c9db7d6e-8ba3-45fc-95de-9c0f6276c2b8` | worker | W17 Copy rewrite | **released** | Aug 2026 | Fabrica-web/ | ✅ |
| `term_a2eb2cb3-081b-473d-a188-775a29fe6fd9` | worker | W18 FR/AR localization | **stopped** | Aug 2026 | Fabrica-web/ | — |
| `term_3bdeae00-c2a2-4dd7-ac80-96c34ecafb92` | worker | W18 FR/AR localization | **released** | Aug 2026 | Fabrica-web/ | ✅ |
| `term_07702fa9-702a-45a4-847d-edd2f45a02d2` | worker | W19 Mobile audit | **released** | Aug 2026 | Fabrica-web/ | ✅ |
| `term_c980e3be-cdd7-499b-a9c6-062d9eb2af51` | worker | W20 Light theme text | **released** | Aug 2026 | Fabrica-web/ | ✅ |
| `term_ac62af28-0da2-4c19-a9a7-cbf55e7c0679` | worker | W21 Carousel audit + fixes | **released** | Aug 2026 | Fabrica-web/ | ✅ |
| `term_0f27b013-a505-4f53-a910-88b43bbc3f35` | worker | W21 Standalones + W22 | **released** | Aug 2026 | Fabrica-web/ | ✅ |
| `term_847538fd-65ae-4c9d-bf60-b32129175bd7` | worker | W23 Bottom bg blur | **released** | Aug 2026 | Fabrica-web/ | ✅ |

**Pushed:** commit `334413b` → origin/main (Aug 2026). Vercel auto-deploys.
| `term_830c3392-100c-4bec-9f5e-c674dc5c32b5` | worker | W24 Dedupe content | **released** | Aug 2026 | Fabrica-web/ | ✅ |
| `term_f8b9ed52-49d6-4784-a969-f40f091a818b` | worker | W25 Coverage audit | **released** | Aug 2026 | Fabrica-web/ | ✅ |
| `term_5bd02937-9574-4cba-b7de-4f93c0e465f3` | worker | W27 P1 honesty fixes | **active** | Aug 2026 | Fabrica-web/ | — |
| `term_14e2a27c-b273-4598-85e5-01dc15e8f132` | worker | STATIC-JSON changelog+kill-list — run `run_effeaea830f9`, task `task_88c8534c1c42`, dispatch `ctx_ed060bcdc7d3` | **active** | Aug 21 2026 | Fabrica-web/ | — |
| `term_99c98028-50a4-4c06-97d1-c45344c4ec62` | worker | W24+W26 dedupe + nav alignment — run `run_effeaea830f9`, task `task_e7d2fee2a3ed`, dispatch `ctx_2495f8df3ffc` | **active** | Aug 21 2026 | Fabrica-web/ | — |
| `term_6180e0da-9d37-48df-a0fc-2a33fa9ed08b` | worker | W25 coverage audit (read-only) — run `run_effeaea830f9`, task `task_707a4694155d`, dispatch `ctx_3b988764f1c6` | **active** | Aug 21 2026 | Fabrica-web/ | — |

**Rules:**
- Only the main orchestrator creates sessions in this ledger
- Workers are released after review
- Worktrees are merged immediately after approval
- Never leave orphaned sessions

---

_Created: Aug 2026_

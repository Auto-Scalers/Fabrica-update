# Fabrica-web — Tasks

> Single source of truth for all landing page / API route work. The Roadmap (`.Fabrica-board/Fabrica-Roadmap.md`) tracks cross-cutting status only — this file owns execution details. Schema: `.Fabrica-board/Fabrica-Schema.md`.

## High-Level Goals

> WHAT THIS PROJECT IS FOR — read this before any task:

1. **A launch-grade landing page.** Every word traceable to the 3 internal marketing files; honest claims only; flawless on mobile; en/fr/ar in perfect parity.
2. **Working backend endpoints.** Auth/share/diagnostics/telemetry routes live and matching what the desktop client expects (lockstep with Fabrica-app).
3. **Beta-launch ready.** Live deployment verified (fabrica-ai.vercel.app), signup flow working, static JSON contracts (changelog/nudge/kill-list) served correctly — gate for Roadmap Phase B.

---

## Rollup

| Metric | Value |
|---|---|
| Total tasks | 49 |
| ✅ DONE | 47 |
| 🔶 IN_PROGRESS | 1 |
| 👀 VERIFY | 0 |
| ⬜ TODO | 0 |
| 🚫 BLOCKED | 0 |
| ❌ CANCELLED | 0 |
| Completion | 96% |

_Last recount: 2026-08-30 — **WEB-W49 DONE+MERGED ✅** (commit `cf5303c`): 8 desktop-auth + 2 artifacts routes moved from `/api/v1/*` → `/v1/*` to match the desktop's hard-coded URL prefix; 3 fetch calls in `app/[locale]/dashboard/page.tsx` updated; build clean (29 routes), 0 new lint errors (46 baseline unchanged; +8 warnings pre-existing drift from WEB-ICON-THEME). `/api/auth/*` Supabase web OAuth untouched. W48 DONE ✅ earlier (audit confirmed desktop routes already shipped in `1bae555`). **WEB-W47 IN_PROGRESS:** full `/login` rework — `lib/supabase-browser.ts` (mirror legacy singleton + guard) + `lib/api/auth.api.ts` (re-export layer) created in the worktree; page rewrite + Google button + email/password + recovery + toasts + `?intent=web|desktop|pair` + Pair-a-phone panel + `app/v1/desktop/auth/invites/route.ts` (under the new no-`/api` prefix) in progress; i18n parity en/fr/ar, RTL Arabic, strict §6.0 DNA guardrails. **G4-ENV ✅ APPLIED (PM, 2026-08-29):** Vercel env for `fabrica-ai` deployed; Supabase providers GitHub + Google + email/password enabled with recovery. The sign-in 400 should resolve on next deploy. No blockers remaining._

## Parallelism & Anti-Overlap Policy

> This project runs REAL 24/7 multi-terminal orchestration. Parallelism is the
> default: unlimited tokens, multi-terminal app, massive project, close deadline.

- **Minimum fleet:** the orchestrator keeps AT LEAST 3 active worker terminals at
  all times. Fewer than 3 on resume or cycle end => launching more comes FIRST,
  chosen from the highest-priority TODO/VERIFY tasks in this file, focused on
  high-level goals and principles, not micro-edits.
- **One task = one worker:** claim a task by setting its status IN_PROGRESS and
  recording your terminal handle in the Session Ledger BEFORE starting. Claimed
  tasks are forbidden to everyone else.
- **One folder = one orchestrator:** never work another slot's folder.
- **One file = one writer:** two live workers never edit the same file; such tasks
  run sequentially.
- **Claim-before-work:** confirm your Task ID is still unclaimed before executing;
  if done or claimed, stop and report instead of duplicating.
- **Cross-project dependencies:** record them as notes in the OTHER project's task
  file; never edit another project directly.
- **Quality bar unchanged under deadline pressure:** no DONE without verified
  evidence; status change and Rollup update happen in the same edit.

---

## Status Legend

| Status | Meaning |
|---|---|
| `TODO` ⬜ | Not started |
| `IN_PROGRESS` 🔶 | Started, partially done |
| `VERIFY` 👀 | Implemented, awaiting orchestrator review |
| `DONE` ✅ | Implemented and verified |
| `BLOCKED` 🚫 | Waiting on dependency/decision |
| `CANCELLED` ❌ | Dropped |

_Legacy mapping applied in migration: `IN PROGRESS→IN_PROGRESS`, `[~]→VERIFY`. No outcome changed._

---

## API Routes (Vercel)

> WHAT THIS GROUP DOES: server endpoints for the desktop app. The app already has client code pointing to them.
> WHAT THIS GROUP DOES NOT DO: desktop-side client code (owned by Fabrica-app).

> **LIVE-VERIFY-2 2026-08-24 (after PM added `NEXT_PUBLIC_SUPABASE_ANON_KEY` + redeploy):** auth env config FIXED — authorize now returns Supabase OAuth URL (200), logout returns {"success":true} (200), session correct 401 JSON. Residual minor bug on WEB-W3 only. First-pass report: opencode temp/liveverify-report-task_e72cda1d36e9.md

| # | Task | Status | Notes |
|---|------|--------|-------|
| WEB-W1 | `/api/auth/authorize` — OAuth authorize (PKCE) | ✅ DONE | Live PASS: 200, returns GitHub OAuth URL with redirect_to=/api/auth/callback |
| WEB-W2 | `/api/auth/session` — Session management | ✅ DONE | Live PASS: unauth returns {"authenticated":false} correctly |
| WEB-W3 | `/api/auth/refresh` — Token refresh | ✅ DONE | Orchestrator-verified diff (+8/-2): req.json() wrapped in try/catch → 400 "Invalid JSON body"; refresh_token type-checked; happy path unchanged. Build clean, zero new lint findings. Deploys on next push |
| WEB-W4 | `/api/auth/logout` — Session destroy | ✅ DONE | Live PASS: {"success":true} |
| WEB-W5 | `/api/share/*` — Artifact sharing CRUD | ✅ DONE | Live PASS: GET list + GET [id] both 401 Unauthorized as expected |
| WEB-W6 | `/api/diagnostics/*` — Crash/feedback upload | ✅ DONE | Live PASS: GET + POST both 401 auth-gated |
| WEB-W7 | `/api/telemetry` — Analytics events (fallback) | ✅ DONE | Live PASS: POST-only confirmed via GET 405 |

---

## Static Files

| # | Task | Status | Notes |
|---|------|--------|-------|
| WEB-W8 | `/whats-new/changelog.json` — In-app changelog display | ✅ DONE | Live 200, valid shape. Local W32 rebrand-prose fix landed; deploys on next push |
| WEB-W9 | `/whats-new/nudge.json` — Update nudge config | ✅ DONE | Live 200 (old schema). Local W30 schema fix `{id,minVersion,maxVersion}` landed; deploys on next push |
| WEB-W10 | `/plugins/kill-list.json` — Plugin block list | ✅ DONE | Live PASS: {version:1, generatedAt, plugins:[]} expected shape |

---

## PM Decision Fixes (from Fabrica-app)

> Cross-project tasks added by orchestrator. These fix issues identified in PM decisions D7 and D8.

| # | Task | Status | Notes |
|---|------|--------|-------|
| WEB-W30 | Fix nudge.json schema — current `{enabled, channel, minimumVersion, message}` doesn't match app contract `{id, minVersion, maxVersion}`. App returns null (dead code). | ✅ DONE | Orchestrator-verified: `public/whats-new/nudge.json` now `{id: "update-1.4.0", minVersion: "1.4.0", maxVersion: "2.0.0"}`; matches `updater-nudge.ts` contract exactly (id non-empty trim check, version bounds present + valid). Old dead fields removed. Worker `term_61d57f10`. |
| WEB-W32 | Reword old-brand prose in changelog.json — live entry contains 'Orca is now Fabrica - a full rebrand...' rendered in app What's New screen. Prose-only reword, JSON structure unchanged. | ✅ DONE | Orchestrator-verified: grep `[Oo]rca` in `public/whats-new/changelog.json` returns zero hits; JSON keys/versions preserved, node parse validated by worker. Entry now leads with "Fabrica is here...". Worker `term_b26beadf`. |

---

## Docs Site

| # | Task | Status | Notes |
|---|------|--------|-------|
| WEB-W11 | `/docs/*` — documentation pages | ✅ DONE | Created layout, catch-all page, sidebar, prose, content, nav. Build compiles. Zero orca/stablyai refs. |
| WEB-W40 | Add `/docs` index page + i18n + fix catch-all path | ✅ DONE | Worker `term_4e11a915`. Added `app/[locale]/docs/page.tsx` (the new index), added `docs.indexTitle`/`indexSubtitle` to en/fr/ar.json, plus a small `generateStaticParams` fix to skip empty keys. Build claimed clean; see WEB-W40a for the path-corruption fix that followed. |
| WEB-W40a | FIX the docs catch-all directory corruption | ✅ DONE | Worker `term_4e11a915`. The original docs commit (39bdf75) had the catch-all at the literal `app/[locale]/docs/[/[...slug/]/]/page.tsx` (a real-but-corrupt path with literal `[/.../]/` brackets in the directory name). Moved to the proper catch-all `app/[locale]/docs/[...slug]/page.tsx` (used non-optional `[...slug]` because Next.js 16 rejects `[[...slug]]` + a separate `page.tsx` index at the same level with `You cannot define a route with the same specificity as an optional catch-all route`). Deleted the three garbage directories. Final layout: `app/[locale]/docs/page.tsx` (index), `app/[locale]/docs/layout.tsx` (existing), `app/[locale]/docs/[...slug]/page.tsx` (catch-all, restored). Build clean: routes `/[locale]/docs` and `/[locale]/docs/[...slug]` (NOT the broken `/[...slug/]/`). Lint clean vs W40 baseline (56 problems / 46 errors / 10 warnings — identical, 0 new). Evidence: `build-w40a.txt`, `lint-w40a.txt`. Deviation from spec: task said `[[...slug]]` (optional); resolved as `[...slug]` (non-optional) to keep the separate index page; semantics: `/[locale]/docs` shows index, `/[locale]/docs/foo` shows article. |

---

## Landing Page Updates

> WHAT THIS GROUP DOES: plan and refine page copy/meta/pricing.
> WHAT THIS GROUP DOES NOT DO: execute unplanned code changes.

| # | Task | Status | Notes |
|---|------|--------|-------|
| WEB-W12 | Update any Orca references in page copy | ✅ DONE | Audit complete: 3 refs in public/ (historical changelog + kill-list plugin). Not page copy. No changes needed. |
| WEB-W13 | Update meta tags / OG images if needed | ✅ DONE | Audit complete: all OG/Twitter/meta tags already say Fabrica with correct URLs. No changes needed. |
| WEB-W13b | Update pricing tiers in landing page | ✅ DONE | Tiers renamed: Power User, One-Person Company, Agency & Teams. All CTAs say "Start 14-Day Free Trial". Updated en.json, fr.json, ar.json. Prices remain placeholders ($29/$79/$199). |

---

## Brand Assets

> WHAT THIS GROUP DOES: swap brand icon/logo art so the web landing site matches the desktop app's new Fabrica brand.
> WHAT THIS GROUP DOES NOT DO: change marketing copy, blog text, or any unrelated content.

| # | Task | Status | Notes |
|---|------|--------|-------|
| WEB-ICON | Replace ALL web brand icons/logos with new Fabrica art | ✅ DONE | Old Orca SVG logo `public/fabrica-logo_icon.svg` + favicon `app/icon.svg` replaced with new brand PNGs copied into Fabrica-web: `public/fabrica-logo_icon.png` (dark — navbar, hero x2, login, download, dashboard x2, FinalCta on light/neutral bg) + `public/fabrica-logo_icon_light.png` (footer on dark bg `#07080C`) + `app/icon.png` (favicon) + `app/apple-icon.png` (apple-touch). All 10 `<img>`/metadata references updated; 0 references to old `.svg`/orca/stably remain. `npm run build` clean (TS ✓, `/icon.png`+`/apple-icon.png` routes emitted); lint = 46 pre-existing errors only, 0 new. |
| WEB-ICON-THEME | Theme-adaptive brand logo: swap dark/light variants via Tailwind `dark:` | ✅ DONE | navbar + login + download + dashboard + Hero (x2) + FinalCta brand logo now swaps between dark/light variants via Tailwind `dark:` variant (two `<img>` + CSS-only). Footer unchanged (fixed dark bg). Hero/FinalCta sit on the page's theme-adaptive background so they were also included for full theme consistency on themed backgrounds. npm run build + lint clean. |

---

## Phase 6 — Landing Page Enhancement

> **CRITICAL:** Every element of the landing page must be derived from the 3 internal marketing files. Read each file line by line, word by word. No copy should exist that doesn't align with these files.
>
> **Source files (read all 3 before writing any copy):**
> - `Fabrica-marketing/internal/brand/brand-guidelines.md` — voice, tone, visual identity, word bank, blacklist, correct/incorrect usage examples
> - `Fabrica-marketing/internal/brand/positioning-statement.md` — positioning, key differentiators, messaging hierarchy, proof points
> - `Fabrica-marketing/internal/research/competitor-landscape.md` — competitor insights, positioning opportunities, market gaps, Fabrica response to each competitor
>
> WHAT THIS GROUP DOES: visual assets, full copy rewrite grounded in internal docs, localization quality, mobile responsiveness.
> WHAT THIS GROUP DOES NOT DO: invent copy not traceable to the 3 source files.

| # | Task | Status | Notes |
|---|------|--------|-------|
| WEB-W14 | Replace carousel images with new assets from `public/images/` | ✅ DONE | Updated ShowcaseCarousel.tsx with correct image paths (carousel-00 through carousel-10). Fixed aspect ratio to 16/9, switched to object-contain for full images. |
| WEB-W15 | Add standalone images with text overlays from `public/images/standalones/` | ✅ DONE | All 5 standalone images added: pain-exhausted-developer (PainSection), social-parallel-agents (TurnSection), social-approval-gate (ControlSection), mobile-companion-remote (OrchestrationSection), hands-on-architecture (FeatureSection). All use object-contain. |
| WEB-W16 | Apply `fabrica-buttom-bg` as background to bottom section | ✅ DONE | Added fabrica-buttom-bg.png background to FinalCta.tsx following Hero.tsx pattern (absolute inset, backgroundSize 100% auto, bg-white/20 dark overlay). |
| WEB-W17 | Rewrite ALL landing page copy using the 3 marketing internal files | ✅ DONE | Full rewrite of en.json using brand-guidelines.md, positioning-statement.md, and competitor-landscape.md. All sections grounded in source docs. Blacklisted terms removed. |
| WEB-W18 | French & Arabic localization quality pass | ✅ DONE | fr.json and ar.json fully updated to match new en.json. Natural phrasing, brand voice preserved. All sections aligned. |
| WEB-W19 | Mobile responsiveness audit — verify all new sections | ✅ DONE | Orchestrator-verified diff. Fixes: (1) touch targets — carousel dot hit-areas 32px, Hero toggles min-h-36px, navbar drawer links py-2 + locale switcher px-3 py-2; (2) RTL/ar.json — left/right → start/end + pl/pr → ps/pe in FinalCta, Carousel arrows, Hero pillar grid (layout sets dir=rtl); (3) object-cover for mismatched-aspect images (pain-exhausted-developer, hands-on-architecture, mobile-companion-remote), social-approval-gate kept object-contain intentionally; (4) overflow verified contained at 360px. Build clean (35.9s, TS ✓, 15/15 pages); lint = 51 pre-existing only |

**Rules for WEB-W17:**
1. Read `brand/brand-guidelines.md` — adopt the voice, use the word bank, respect the blacklist
2. Read `brand/positioning-statement.md` — use the positioning statement, key differentiators, and messaging hierarchy verbatim where appropriate
3. Read `research/competitor-landscape.md` — use competitor insights, proof points, and Fabrica responses for the "Why Fabrica" section
4. Every section of the landing page must trace back to one of these 3 files
5. No generic marketing copy — everything must be grounded in the internal docs

### Phase 6b — PM Review Feedback (Aug 2026)

> Feedback from PM visual review of the landing page. Execute after W14-W19.

| # | Task | Status | Notes |
|---|------|--------|-------|
| WEB-W20 | Top background: darken text in light theme | ✅ DONE | Light-theme text contrast adjusted; hero headline sizes tuned per PM follow-up in-session |
| WEB-W21 | Audit: all carousel + standalone images present? | ✅ DONE | Carousel: all 11 images now shown as slides s1-s11 (en/fr/ar). Standalones: all 5 visible on page load — mobile-companion image promoted out of hidden tab into always-visible block |
| WEB-W22 | Standalone images: side-by-side layout with relevant text | ✅ DONE | All 5 standalones in half-width two-column layouts with strictly matching text, stacked on mobile |
| WEB-W23 | Bottom background: add blur + strengthen overlay | ✅ DONE | Per PM follow-up in-session: removed blur layers that softened FinalCta text, content pinned to top with z-10, scrim tuned |
| WEB-W24 | Deduplicate content across all sections | ✅ DONE | 23 strings deduped across en/fr/ar. Worktrees star = features/orchestration/faq; budget star = control.card3; approval gates star = control; BYOK star = integrations/control.card2; cta.paragraph rewritten. 702 keys per locale, build passes |
| WEB-W25 | Coverage audit vs internal marketing files | ✅ DONE | Report delivered: W25-coverage-audit.md (33KB). PM reviewed and approved ALL recommendations → split into WEB-W27 (P1) and WEB-W28 (P2+P3) |
| WEB-W26 | Top navigation bar: align with page sections | ✅ DONE | Closeout by `term_b468e39b` (run_31a1a28a59a3) after reclaim; W29's C1 pass verified rather than redone. Findings: (1) all 7 navbar hrefs (#product #crew #how-it-works #controls #comparison #pricing #faq + /docs) match rendered section ids in components/Blocks + page.tsx; (2) zero duplicate ids — legacy `command-center` survives only as a unique inner div (Hero.tsx:318, scroll-mt-24) targeted by Hero CTA + 2 footer links; (3) scroll-mt present on every nav-targeted section after fix: added missing scroll-mt-20 to Hero id="product" (crew/how-it-works/controls/comparison/pricing/faq already had it via W29; waitlist has scroll-mt-16); (4) ScrollSpy SECTION_IDS (components/scroll-spy.tsx) all resolve to live DOM nodes; hash-sync via IntersectionObserver works. Evidence: npm run build ✓ clean (Next 16.3.1, compiled 37s, TS ✓, 15/15 pages). npm run lint: 45 pre-existing errors (unescaped entities in docs-content/OrchestrationSection/Hero, `<a>` vs Link in docs-content, require() in scripts/, any in request.ts) — all out of W26 scope, none introduced by this change (git diff confirms only Hero.tsx touched) |
| WEB-W27 | P1 honesty fixes from W25 audit | ✅ DONE | Testimonials section removed (page.tsx + component deleted; JSON keys kept for future real quotes). $149/mo → "current tier" (incl. Hero.tsx fallback log). Free tier → 14-day trial. Unlimited crews → tier-based. 3.5 hrs/day → illustrative. Build passes |
| WEB-W28 | P2+P3 content additions from W25 audit | ✅ DONE | 6/7 landed by term_4bc0bb56 before terminal death; ADD5 (non-tech pain beat p4) completed by WEB-W29. Verified: n8n card, roadmap+vault copy, adaptive UI pillar, hero audience+control promise, spend scoping, CrewSection fix |
| WEB-W29 | W25 audit leftovers + W26 nav + W28 completion — final sweep | ✅ DONE | A1: p4 pain (non-technical operator → plain-language direction, en/fr/ar + PainSection card). B1: platformSetup → "Bring your own CLI agents and keys". B2: faq q9/a9 spend-tracking qualifier. B3: brand promise in turn.promise. B4: tagline clarifier under hero badge. B5: comparison columns → Cloud Autonomous Agents / AI Code Editors with category-honest cells. B6: kanban k3/k5/k7 + eisenhower t1/t2/t6 as founder outcomes. B7: kill-switch circuit-breaker sentence in control.card3.desc. B8: team.tagline = per-client budget partitioning. B9: triad in hero.triad (once site-wide). C1: OrchestrationSection id command-center→how-it-works (dup ID resolved), nav+scroll-spy updated, scroll-mt-20 added to crew/how-it-works/controls/comparison/pricing/faq. Locale parity 716/716/716 verified by node script; grep confirms "Zero Technical Setup" gone from shipped copy; npm run build passes |

---

## Phase 7 — CTA → Download / Sign-in + Dashboard (WEB-CTA)

> Replace the email-capture "Get Early Access" CTA with two actions (Download + Sign in); add a /download installer page and an authenticated /dashboard; wire the OAuth browser flow to land on /dashboard. Cross-project note: G4-ENV worker only sets Vercel env (no source edits) — no conflict.

| # | Task | Status | Notes |
|---|------|--------|-------|
| WEB-CTA | FinalCta: two buttons — Download (primary ShimmerButton → /download via router.push) and Sign in (secondary → /api/auth/authorize browser OAuth); new `app/[locale]/download/page.tsx` (Server Component) lists Windows/macOS/Linux/Android installers linking to GitHub Releases; new `app/[locale]/dashboard/page.tsx` (client) reads token hash, shows profile/orgs/capabilities/recent artifacts/relay; `GET /api/auth/authorize` returns 302 to OAuth URL; callback redirects to /dashboard (tokens in hash); removed orphaned `/api/early-access` route + fetches; i18n cta.getEarlyAccess→download+signIn, added `download`+`dashboard` namespaces (en/fr/ar parity) | ✅ DONE | Worker `term_9f2c4a17` (self-assigned). Build clean: Next 16.3.1, TS ✓, 17/17 pages, `/[locale]/download` + `/[locale]/dashboard` present. Lint: 0 NEW errors (46 pre-existing out of scope — docs-content unescaped entities, `require()` in scripts, `any` in request.ts, `no-html-link` in `/docs`). No Orca/Stably branding. |

---

## Phase 8 — Top-Bar Nav + Download / Auth / Dashboard Enhancements (WEB-UX2)

> Follow-up to Phase 7 (WEB-CTA). WEB-CTA replaced the **bottom** FinalCta email-capture with Download + Sign in buttons and shipped the `/download` + `/dashboard` pages. This phase targets the **top navigation bar** (still showing the old "Get Early Access" button) and meaningfully enhances the three pages WEB-CTA created as minimal scaffolds.
>
> WHAT THIS GROUP DOES: top-bar nav buttons, richer installer page, polished login/signup → dashboard handoff, and a higher-fidelity authenticated dashboard workspace.
> WHAT THIS GROUP DOES NOT DO: re-place the bottom FinalCta (already DONE in WEB-CTA); change API auth contracts (those live in API Routes + Fabrica-app lockstep).

| # | Task | Status | Notes |
|---|------|--------|-------|
| WEB-W33 | Top-bar nav: replace "Get Early Access" button with two actions — **Download** (primary, → /download) and **Sign in / Dashboard** (secondary, → /api/auth/authorize for OAuth; or → /dashboard if already authed). Apply to desktop nav + mobile drawer nav. Keep i18n parity (en/fr/ar) — add `nav.download` + `nav.signIn` / `nav.dashboard` keys; remove/replace `nav.getEarlyAccess`. Honor existing navbar Strikingly/ShimmerButton styling + dark theme/copper palette. | ✅ DONE | Orchestrator-verified: grep confirms `Get Early Access`/`getEarlyAccess` gone from navbar + i18n; navbar.tsx has Download (→/download) + Sign in (→/api/auth/authorize, →/dashboard if authed) buttons (desktop + mobile drawer); nav.download/signIn/dashboard added en/fr/ar; no orca/stablyai branding. Worker `term_f886d746` (ctx_d6358586dd8a). Build claimed clean. MERGE NOTE: shares `components/navbar.tsx` with WEB-W37 (which removed #pricing link). |
| WEB-W34 | Enhance the `/download` installer page — go beyond a bare list of GitHub Release links. Add: platform detection (auto-highlight the user's OS), prominent primary download CTA per platform (Windows .exe / macOS .dmg / Linux .AppImage / Android .apk), copy for what Fabrica is + "free to start", secondary links (release notes → /whats-new, GitHub Releases, checksums/verify), mobile-companion note (phone↔desktop relay), and i18n parity. Server Component for static parts; client island for OS detection. Keep brand voice grounded in the 3 internal marketing files. | ✅ DONE | Orchestrator-verified: `components/download/OsPlatformGrid.tsx` exists; download/page.tsx imports it + links /whats-new; no orca/stablyai branding; added /whats-new release-notes page + download/whatsNew i18n. Worker `term_a6263719` (ctx_b8c713eb7a4b). Build + lint-on-changed claimed clean. MERGE NOTE: edits messages/*.json (download namespace) — same files as W33/W35/W36; merge sequentially, resolve complementary key additions. |
| WEB-W35 | Enhance login / sign-up → dashboard flow. The OAuth browser flow already lands on /dashboard (WEB-CTA). Enhance: a branded `/login` (or sign-in) entry that explains "Sign in with GitHub to access your Fabrica dashboard", handles the returning-from-OAuth hash, shows loading/error states, and a clear "New here? Download the app" path. Add sign-up framing (account created via GitHub OAuth; link to download). Ensure token-hash parsing, session state, and redirect-to-/dashboard are robust and mobile-friendly. i18n parity for all new copy. | ✅ DONE | Orchestrator-verified: `app/[locale]/login/page.tsx` exists; no orca/stablyai branding; login i18n namespace added en/fr/ar (line ~789). Worker `term_7295705a` (ctx_a2fbde7c5934). Build + lint claimed clean. NOTE: also edited `app/[locale]/dashboard/page.tsx` (CTA→/login) + `components/Blocks/FinalCta.tsx` + callback/authorize routes — overlaps `dashboard/page.tsx` with WEB-W36 (full rebuild). Resolve at merge. |
| WEB-W36 | Enhance the authenticated `/dashboard` workspace. Current scaffold (WEB-CTA) shows profile/orgs/capabilities/recent artifacts/relay. Elevate to a real workspace: clearer empty states, "Connect your desktop app" / relay-pairing CTA, recent artifacts grid with share links, org switcher, usage/capabilities panel, quick actions (Download app, Open docs, Invite), and responsive layout. Keep the token-hash read + Supabase session logic; improve UX, loading/error/empty states, and i18n parity. | ✅ DONE | Orchestrator-verified: no orca/stablyai branding; dashboard rebuilt (33 new dashboard i18n keys en/fr/ar parity). Worker `term_6f707b1d` (ctx_6300ab36aaf2). Build + lint claimed clean. MERGE NOTE: full rewrite of `app/[locale]/dashboard/page.tsx` — SAME file WEB-W35 edited (CTA→/login). Take W36 as authoritative dashboard, then re-apply W35's /login routing intent onto it. |
| WEB-W37 | Hide the pricing section from the landing page — Fabrica is going free for now. Remove/hide the pricing block (and its nav link `#pricing` in the top bar) without deleting the underlying component or copy (keep for later re-enable). Ensure no dead anchor references remain (nav + footer + scroll-spy SECTION_IDS). Keep i18n keys intact. Re-verify build + scroll anchors. | ✅ DONE | Orchestrator-verified: grep `#pricing` in .tsx = 0 hits; `PricingSection` only its own component file (import/render removed from page.tsx); no orca/stablyai branding. Worker `term_bda12904` (ctx_8d27b23ead49). Build claimed clean. MERGE NOTE: also edited `components/navbar.tsx` (removed #pricing link) — same file as WEB-W33; merge W33 first, then rebase W37 onto it. |
| WEB-W38 | (a) Add a "Back to home / landing page" control on every non-landing page so users can return to the site root. Apply to `/[locale]/download`, `/[locale]/login`, `/[locale]/whats-new`, and `/[locale]/dashboard` (consider `/docs` too if it shares the nav chrome). Use the locale-aware `Link` from `@/src/i18n/navigation` pointing to `/` (the localized home), placed consistently (e.g. top-left of each page, near the logo/nav). (b) Make the **Sign in** button clearly more visible — add a visible copper/amber border to the top-bar nav Sign in / Dashboard button; also apply the same treatment to the primary sign-in CTA on `/login`. Do NOT downgrade the Download button. Keep i18n parity (en/fr/ar) for `nav.backToHome`. | ✅ DONE | Orchestrator-verified: `nav.backToHome` added en/fr/ar; navbar shows locale-aware `IntlLink href="/"` on non-root routes only (desktop pill + mobile drawer, copper border `border-orange-500/60`); nav Sign in/Dashboard button given copper border; /login githubButton has `border-2 border-orange-300/70`; Download untouched; no orca/stablyai branding; imports (`IntlLink`, `usePathname`, `Home`) resolve. Worker `term_8bda54f0` (ctx_796242ae6306, run_55c168e35a3f) in worktree `web-W38-backhome`. MERGED into main 2026-08-29. |
| WEB-W39 | Remove the stale "Get Early Access" + "Explore Command Center" buttons. In `components/Blocks/Hero.tsx` (lines ~263-280) the two hero CTAs are `ctaEarlyAccess` (scrolls to #waitlist) and `ctaExplore` (href `#command-center`) — replace them with the new canonical actions Download (primary ShimmerButton → /download) + Sign in (secondary → /login or /api/auth/authorize) to match the top-bar nav; do NOT leave the hero without a CTA. In `components/Blocks/Footer.tsx` (lines 14-15) `links.commandCenter` and `links.worktrees` both point to the DEAD `#command-center` anchor (section was renamed to `#how-it-works` in W29) — repoint both to `#how-it-works`. i18n: drop the now-unused `ctaEarlyAccess`/`ctaExplore` keys from en/fr/ar (or leave if reused); reuse existing `nav.download`/`nav.signIn`. Do NOT touch the navbar (done in W33) or pricing (hidden in W37). Build + lint clean, no orca/stablyai branding. | ✅ DONE | Orchestrator-verified: `Hero.tsx` CTAs replaced — primary `ShimmerButton` → `router.push('/download')` (Download icon + `nav.download`), secondary `<button onClick={goToAccount}>` showing LogIn/LayoutDashboard + `nav.signIn`/`nav.dashboard` (matches navbar pattern). Imports clean (`Download`, `LogIn`, `LayoutDashboard`, `useRouter`). `Footer.tsx` lines 14-15 repointed `#command-center` → `#how-it-works` for both `links.commandCenter` and `links.worktrees`. `ctaEarlyAccess`/`ctaExplore` removed from messages/{en,fr,ar}.json. Zero remaining "Get Early Access" / "Explore Command Center" / `command-center` / orca/stablyai in the worktree. Build + lint clean. MERGED into main 2026-08-29. |
| WEB-W40a | FIX the W40 docs catch-all directory corruption. The W40 worker accidentally moved the catch-all page to a nonsensical git path `app/[locale]/docs/[/[...slug/]/]/page.tsx` (git ls-files confirms) and created three garbage nested empty bracket directories: `app/[locale]/docs/[` (with nested `[[...slug` and `]` children), `app/[locale]/docs/[[...slug`, and `app/[locale]/docs/[[...slug]`. Steps: (1) `git mv` the page back to the correct path `app/[locale]/docs/[[...slug]]/page.tsx`. (2) `git rm -rf` the three garbage dirs (or `rm -rf` if untracked). (3) Final layout must be: `app/[locale]/docs/page.tsx` (W40 index), `app/[locale]/docs/layout.tsx` (existing), `app/[locale]/docs/[[...slug]]/page.tsx` (restored catch-all). (4) `npm run build` from Fabrica-web/ — routes table must show `/[locale]/docs` and `/[locale]/docs/[[...slug]]` (NOT the broken path). Capture to `build-w40a.txt`. (5) `npm run lint` → `lint-w40a.txt`; 0 new errors vs `lint-w40.txt` baseline. (6) `git status` clean of garbage. Commit on top of W40 work-in-progress: `WEB-W40: fix docs catch-all path corruption + add /docs index`. Update WEB-W40 row → ✅ DONE + WEB-W40a row in Session Ledger. DO NOT merge — orchestrator merges. | ✅ DONE | Orchestrator-verified: W40a worker self-resolved the Next.js 16 specificity conflict (cannot have sibling `page.tsx` with optional catch-all `[[...slug]]`) by converting the catch-all to a *required* `[...slug]` (single brackets) and keeping the new `app/[locale]/docs/page.tsx` index. The build-w40a.txt routes table shows clean `/[locale]/docs` + `/[locale]/docs/[...slug]` (no broken path). Three garbage bracket directories removed. `git ls-files` clean. Lint matches pre-fix baseline (no new errors). `npm run build` clean (27 routes). MERGED into main 2026-08-29. |
| WEB-W41 | Cross-folder LOGIN/AUTH investigation + design proposal (READ-ONLY — do NOT edit any source; only write the proposal doc). Enumerate every distinct login/auth type needed in (a) Fabrica-web (`app/[locale]/login/page.tsx`, `app/api/auth/*`), (b) Fabrica-relay (`Fabrica-relay/` repo — phone↔desktop pairing/auth), (c) Fabrica-app (`Fabrica-app/` Electron desktop app — sign-in / device pairing / relay connection). For each state WHAT it needs + HOW it should work. Assess whether ALL can be initiated/completed from ONE UI (the web `/login` page) and what that unified page must support. Compare against `Fabrica-atlas/_sources/legacy-fabrica/frontend-next/app/` (`page.tsx`, `onboard/page.tsx`, `oauth/page.tsx`, `dashboard/page.tsx` + any auth components) and propose whether the web `/login` can/should look and flow EXACTLY like legacy-fabrica's login (spec what to replicate: layout, copy, OAuth button, branding). Write the proposal to `.Fabrica-web-board/WEB-W41-login-proposal.md` and summarize in worker_done. READ-ONLY across Fabrica-relay & Fabrica-app (read freely, never modify). Research only — do NOT run build or edit web source except the proposal file. | ✅ DONE | Orchestrator-verified (read-only respected): 665-line design proposal at `.Fabrica-web-board/WEB-W41-login-proposal.md` with evidence index. Key findings: (1) three distinct identity flows — only two are human logins (web dashboard OAuth, desktop PKCE); the third (relay device pairing) is **machine-to-machine device auth, not a human login**; (2) single `/login` UI CAN serve all human-facing entry points via `?intent=web\|desktop\|pair` — **desktop routes already exist on `main`** per WEB-W48 audit (commit `1bae555`), only the URL-prefix question is pending the PM's 30-sec check of the desktop's `profile-cloud-auth-config.ts:96-118`; (3) web `/login` CAN be made to look/flow like legacy `/oauth` (three-zone card, Google+GitHub, email/password + recovery, toasts) — full spec in §6, recommend stay dark/copper. **No relay-side login is needed and none should be built** — the relay inherits identity from the web/desktop Supabase token. §6.0 DNA preservation guardrails added (dark/copper, server components, next-intl parity en/fr/ar, no new deps, no Orca/Stably, `ai.autoscalers.fabrica` App ID, `fabrica://` deep link, no pre-auth telemetry, no blurred background, RTL Arabic verified, copy grounded in the three marketing files) + 13-item pre-merge checklist. **PM decisions captured (2026-08-29):** (1) add email/recovery ✅; (2) desktop routes already exist per W48 ✅ (URL-prefix caveat pending); (3) pair panel via desktop-posted invites ✅; (4) stay dark/copper, DNA preserved ✅; (5) no blurred dashboard background ✅. No source modified. Worktree `web-W41-login` removed. |
| WEB-W48 | **Read-only cross-folder audit** — confirm whether Fabrica-web already exposes the desktop's `/v1/desktop/auth/*` endpoints (the PM believes they may already be handled). Steps: (1) Read `Fabrica-app/src/main/fabrica-profiles/profile-cloud-auth-config.ts` to extract the EXACT list of endpoints the desktop calls (default `apiBaseUrl`, `clientId`, `relayDirectorUrl`, full set of `${apiBaseUrl}/v1/desktop/auth/{authorize,session,refresh,capabilities,profile,org,logout,relay-token,invites}` URLs). (2) Read the Fabrica-relay Director auth path (`Fabrica-relay/src/director/index.ts`, `/v1/assign` handler, `FABRICA_RELAY_JWT_SECRET` config) to confirm what the relay actually expects on the `relayToken` Bearer. (3) Grep the Fabrica-web tree (including any `app/v1/`, `app/api/v1/`, or anywhere else) for existing implementations of those exact endpoint paths; check the latest build route table (`build-w40a.txt` + any newer) for `/api/v1/desktop/*` and `/v1/desktop/*` routes. (4) Check the Fabrica-web commit log on main and worktree commits for any worker that may have added these routes without being reflected in the W41 proposal. (5) Read `Fabrica-web/AGENTS.md` and `.Fabrica-web-board/Fabrica-web-tasks.md` for any prior planning/audit that addressed this. DELIVERABLE: `.Fabrica-web-board/WEB-W48-desktop-auth-routes-audit.md` with one of: (A) **CONFIRMED MISSING** — list every endpoint the desktop expects that the web does NOT expose (file:line citations + absence confirmed by grep); (B) **ALREADY HANDLED** — exact files/routes/commits that satisfy the desktop; (C) **PARTIALLY HANDLED** — what's there and what's still missing. READ-ONLY across all three repos. No source modifications. No build. | ✅ DONE | Orchestrator-verified (read-only respected): 385-line audit at `.Fabrica-web-board/WEB-W48-desktop-auth-routes-audit.md`. **Verdict: (B) ALREADY HANDLED with one caveat.** All 8 desktop auth endpoints + 2 artifact endpoints shipped in commit `1bae555` (WEB-CTA worker `term_9f2c4a17`, 2026-08-29 00:53, ancestor of `main`); `lib/fabrica-cloud.ts:mintRelayJwt()` already mints a proper HS256 JWT with `FABRICA_RELAY_JWT_SECRET` (15-min TTL, byte-compatible with the relay's `/v1/assign` verifier); `app/[locale]/dashboard/page.tsx:173,207` already calls capabilities + org. **The only caveat is a URL prefix:** the web serves them at `/api/v1/desktop/auth/*` (per `build-w40a.txt`), while W41 stated the desktop hard-codes `/v1/desktop/auth/*` (no `/api`) — the desktop file could not be independently re-read (Fabrica-app checkout is empty in this dev env). Resolution pending: 30-second PM check of `Fabrica-app/src/main/fabrica-profiles/profile-cloud-auth-config.ts:96-118` to confirm the literal. **Tracker drift closed:** W41 row + W41 proposal (13 stale references) + X1 row (same commit that shipped the routes also added the TODO claiming they don't exist) were all stale by 22h. **W47 implications:** drop the `app/v1/desktop/auth/authorize` etc. "new, conditional on W48" caveat — those routes exist; keep only `invites` (W41 pair-panel proposal, W47-gated, not a desktop blocker) as new. **Action items from the audit:** (i) orchestrator: fix X1 + W41 + W47 rows (this commit); (ii) PM: 30-sec check of the desktop file → decide WEB-W49 (move web routes to drop `/api`) or no-op. |
| WEB-W49 | **Move desktop auth + artifacts routes from `/api/v1/*` to `/v1/*`** to match the Fabrica-app desktop's hard-coded URL prefix (confirmed via `Fabrica-app/src/main/fabrica-profiles/profile-cloud-auth-config.ts:96-118` — the desktop builds URLs as `endpoint(apiBaseUrl, '/v1/desktop/auth/authorize')` etc., no `/api`). Steps: (1) `git mv` the 8 route files: `app/api/v1/desktop/auth/{authorize,capabilities,logout,org,profile,refresh,relay-token,session}/route.ts` → `app/v1/desktop/auth/<same>/route.ts`. (2) Also `git mv` `app/api/v1/artifacts/{,[id]/}route.ts` → `app/v1/artifacts/` for prefix consistency (the desktop hits `/v1/artifacts` per W41 §1.3 evidence). (3) Update the 2 fetch calls in `app/[locale]/dashboard/page.tsx` (~lines 173, 207) to drop `/api`: `/api/v1/desktop/auth/capabilities` → `/v1/desktop/auth/capabilities` and `/api/v1/desktop/auth/org` → `/v1/desktop/auth/org`. (4) `npm run build` from `Fabrica-web/` — confirm routes table shows `/v1/desktop/auth/{authorize,capabilities,logout,org,profile,refresh,relay-token,session}` + `/v1/artifacts{,/[id]}` (no `/api` prefix). Capture to `build-w49.txt`. (5) `npm run lint` → `lint-w49.txt`; 0 new errors vs W40a baseline. (6) Commit: `WEB-W49: move desktop auth + artifacts routes from /api/v1/* to /v1/* to match desktop URL prefix`. Update W49 row → DONE + Session Ledger. DO NOT merge — orchestrator merges. Scope does NOT include `app/api/auth/*` (the Supabase web OAuth routes — those stay at `/api/auth/*`). | ✅ DONE | Orchestrator-verified: `git mv` of 8 desktop-auth + 2 artifacts route files (`app/api/v1/desktop/auth/{authorize,capabilities,logout,org,profile,refresh,relay-token,session}/route.ts` + `app/api/v1/artifacts/{,[id]/}route.ts`) → `app/v1/desktop/auth/<same>/route.ts` + `app/v1/artifacts/<same>`. Updated 3 `fetch()` calls in `app/[locale]/dashboard/page.tsx` (lines 173/177/207) to drop `/api` prefix. Doc-comment headers inside moved route files + `lib/fabrica-cloud.ts` updated to reference `/v1/*`. `/api/auth/*` Supabase web OAuth routes untouched (out of scope). `npm run build` clean: routes table shows `/v1/desktop/auth/{authorize,capabilities,logout,org,profile,refresh,relay-token,session}` + `/v1/artifacts{,/[id]}` (no `/api` prefix); 29 routes, TS ✓. `npm run lint` → 64 problems (46 errors / 18 warnings): **46 errors identical to W40a baseline (0 new errors)**; +8 warnings are pre-existing drift from WEB-ICON-THEME's double `<img>` lines on main (verified by `git stash` test). MERGED into main 2026-08-30 (`cf5303c`). |
| WEB-W47 | **Full `/login` rework** per `WEB-W41` proposal §6 + §6.0, with the **Supabase auth wiring mirroring `legacy-fabrica`** (per PM: "do it like the legacy-fabrica" = replicate legacy's Supabase login/signup/recovery **handling**, not the UI/UX — the page stays dark/copper per §6.0). **(0) Wiring approach — mirror legacy-fabrica's Supabase pattern:** legacy's pattern (see `Fabrica-atlas/_sources/legacy-fabrica/frontend-next/components/auth/supabase.ts` + `api.ts` + `oauth/page.tsx:242-340, 494-638`) is a **singleton browser Supabase client** (`createClient(supabaseUrl, supabaseAnonKey)` from `@supabase/supabase-js`, env-driven `NEXT_PUBLIC_SUPABASE_URL` / `NEXT_PUBLIC_SUPABASE_ANON_KEY`) exported from a shared module with an `isSupabaseConfigured()` guard, plus a `lib/api/auth.api` re-export layer; the page calls `supabase.auth.signInWithPassword` / `signUp` / `resetPasswordForEmail` / `updateUser` / `signInWithOAuth` **directly** against this client (no server proxy for password/recovery). The current Fabrica-web has **no browser Supabase client** (only server-side `lib/supabase-auth.ts:21` `getUserFromRequest` for Bearer validation — per W48 audit). W47 must add `lib/supabase-browser.ts` mirroring legacy's singleton + guard, and a `lib/api/auth.api.ts` re-export layer, then wire the page to call `supabase.auth.*` directly per legacy. **Server-side Supabase (service role) stays in `lib/supabase-auth.ts` for `/api/auth/*` + `/v1/desktop/auth/*` server routes — do NOT mix.** WEB-W48 audit-confirmed: 8 desktop auth routes already exist on `main` under `app/api/v1/desktop/auth/*`; only the `invites` endpoint is missing and is W47-scoped. **Open PM action completed (2026-08-29):** confirmed URL prefix from desktop `profile-cloud-auth-config.ts:96-118` — desktop hard-codes `/v1/desktop/auth/*` (no `/api`). **WEB-W49 dispatched in parallel** to move the 8 web routes to `app/v1/desktop/auth/*` and update the 2 dashboard callers; W47 must build the `invites` endpoint under the **new** prefix `app/v1/desktop/auth/invites/route.ts` (not `/api/`) to match. **W47 page UX scope (dark/copper, §6.0 DNA — unchanged from the visual-side spec):** (1) Add Google provider button (backend already supports it via `app/api/auth/authorize?provider=google`). (2) Add email/password form using legacy's direct `supabase.auth.signInWithPassword` + `supabase.auth.signUp` + Sign In / Sign Up pill toggle. (3) Add password recovery using legacy's `supabase.auth.resetPasswordForEmail` (with `redirectTo: origin + '/<locale>/login?recovery=true'`) + `supabase.auth.updateUser` for the `?recovery=true` / fragment `type=recovery` new-password form. (4) Add reusable toast system (bottom-right slide-in 200ms) — replace the full-page red error screen for transient OAuth failures. (5) Implement route-driven `?intent=web\|desktop\|pair`: `?intent=desktop` reads `?redirect_to` + `?state` and on success redirects to `redirect_to` instead of `/dashboard`; `?intent=pair` (signed-in) renders the "Pair a phone" panel — QR (inline SVG, no new deps) + copyable 32-char invite code + `fabrica://pair?token=…` deep link. (6) Build the **`app/v1/desktop/auth/invites/route.ts`** (new — the only W47-scoped backend route, under the new no-`/api` prefix). (7) Three-zone card (header / body / footer) — dark/copper, copper-tinted dark `bg-zinc-900/80 backdrop-blur`, `border:1px solid var(--border-subtle)`, `border-radius:16px`. Header: full-mark logo + "Fab<span class="text-orange-400">.</span>" wordmark + subtitle. Body: Google + GitHub buttons → "or use email auth" divider → email form (Sign In / Sign Up toggle, Email, Password, Forgot Password?) → copper submit. Footer: "← Back to home" left + "New here? Download the app →" right. (8) Loading shell: 40px spinner + copper-tinted border + uppercase 10px copper label. (9) i18n parity: all new keys mirrored across `messages/{en,fr,ar}.json`. (10) RTL Arabic layout verified. (11) Strict §6.0 DNA guardrails: dark/copper forge palette, server components by default, no new dependencies, no Orca/Stably branding, `ai.autoscalers.fabrica` App ID, `fabrica://` deep link, no pre-auth telemetry, copy grounded in the three marketing files. NO blurred background. NO relay-side login surface. (12) `npm run lint` + `npm run build` clean; all 13 §6.0.3 pre-merge checklist items cleared. Update WEB-W47 row → VERIFY + Session Ledger. | 🔶 IN_PROGRESS | Worker `term_aa6f5989` (ctx_4ddcf14346a9, run_55c168e35a3f) in worktree `web-W47-login`. Dispatched 2026-08-29. |

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

### G4-ENV — Vercel env (2026-08-28)

> **STATUS: ✅ APPLIED (2026-08-29, PM).** The Vercel dashboard env for the
> `fabrica-ai` project (Production + Preview) was set manually by the PM and
> the deploy succeeded. **Supabase providers enabled:** GitHub OAuth, Google
> OAuth, and email/password (with recovery). The sign-in 400 "Unsupported
> provider: provider is not enabled" should now resolve on next deploy — the
> `/api/auth/authorize` route already accepts `provider: 'github' | 'google'`
> (`app/api/auth/authorize/route.ts:44`) and the email/password + recovery
> flow will be added by WEB-W47 (mirror legacy-fabrica's `supabase.auth.*`
> direct calls via the new `lib/supabase-browser.ts` singleton).

**Required env vars (exact names the new `/api/v1/desktop/*` + `/api/v1/artifacts/*` routes read):**

| Env var | Source / reader | Notes |
|---|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | `lib/supabase-auth.ts`, `lib/supabase.ts`, session/authorize routes | Supabase project URL. Falls back to `SUPABASE_URL`. |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | `lib/supabase-auth.ts`, admin fallback | Anon key. Falls back to `SUPABASE_ANON_KEY`. |
| `SUPABASE_SERVICE_ROLE_KEY` | `lib/supabase.ts:58` `getSupabaseAdmin()` | **Mandatory** for `/api/v1/artifacts` GET/POST (`route.ts:25,86`). |
| `FABRICA_RELAY_JWT_SECRET` | `app/api/v1/desktop/auth/relay-token/route.ts:37` | HMAC secret for relay JWT mint. Must match Fabrica-relay worker. |

**Optional:**
- `FABRICA_OAUTH_PROVIDER` — `authorize/route.ts:44` (default `github`).

**Generated secret (set as `FABRICA_RELAY_JWT_SECRET`):**
```
78f77f31506d77d3c65bb721e36da6ae3530853abbaabb222bf01ccd4a0f1893
```

**Apply commands (once authenticated):**
```
vercel env add NEXT_PUBLIC_SUPABASE_URL      <value> production fabrica-ai
vercel env add NEXT_PUBLIC_SUPABASE_ANON_KEY <value> production fabrica-ai
vercel env add SUPABASE_SERVICE_ROLE_KEY      <value> production fabrica-ai
vercel env add FABRICA_RELAY_JWT_SECRET       78f77f31506d77d3c65bb721e36da6ae3530853abbaabb222bf01ccd4a0f1893 production fabrica-ai
```

**DB schema:** `supabase/migrations/0001_fabrica_artifacts.sql` creates `public.fabrica_artifacts` + RLS owner policies. Non-destructive (`CREATE TABLE/INDEX IF NOT EXISTS`). Decision: **KEEP** the `supabase/` folder (defines the artifacts table).

---

## Dependencies & Coordination Rules

1. **Both-sides rule:** identifiers shared with Fabrica-app (auth/share/diagnostics endpoints) change in lockstep with APP-BE1..BE7
2. **Static JSON contract:** changelog.json, nudge.json, kill-list.json are consumed by the packaged app — schema changes require APP-side sign-off
3. **Copy grounding:** every landing-page string must trace to one of the 3 internal marketing files (see Phase 6)
4. **i18n parity:** any string changed in en.json must be mirrored in fr.json and ar.json with identical key structure

---

## What Needs Verification

- [ ] Landing page deployed (`fabrica-ai.vercel.app`)
- [ ] Early-access signup flow (`/api/early-access` → Supabase)
- [ ] Dark theme + copper/amber palette
- [ ] Vercel deployment pipeline

---

## Checkpoint (Current State)

| Field | Value |
|---|---|
| **Current Group** | Phase 9 — Login rework (W48 + W49 DONE+MERGED; W47 IN_PROGRESS) |
| **Current Task** | WEB-W47 (full `/login` rework: mirror legacy-fabrica Supabase wiring — singleton browser client + `supabase.auth.*` direct calls + `lib/supabase-browser.ts` + `lib/api/auth.api.ts`; dark/copper page per §6.0) |
| **Last Action** | W49 merged: 8 desktop-auth + 2 artifacts routes moved from `/api/v1/*` → `/v1/*` (matches desktop prefix); 3 dashboard fetch calls updated; build clean (29 routes), 0 new lint errors. W47 worker has created `lib/supabase-browser.ts` + `lib/api/auth.api.ts` (legacy wiring mirror) and the new `app/v1/` tree; page rewrite + i18n + components in progress. PM confirmed G4-ENV applied (Vercel env deployed; Supabase GitHub + Google + email/password enabled). |
| **Next Action** | Orchestrator: review W47 on worker_done against the full §6.0.3 13-item checklist (dark/copper, server components, next-intl parity en/fr/ar, no new deps, no Orca/Stably, `ai.autoscalers.fabrica` App ID, `fabrica://` deep link, RTL Arabic, no pre-auth telemetry, copy grounded in the three marketing files, no relay-side login surface) + the legacy-wiring mirror (lib/supabase-browser.ts singleton + isSupabaseConfigured + direct supabase.auth.* calls) → merge. |
| **Blockers** | none — G4-ENV ✅ applied (Vercel env deployed; Supabase GitHub + Google + email/password providers enabled). |
| **Last Checkpoint** | 2026-08-29 |

---

## Autonomous Work System

On heartbeat kick:
1. Read the Checkpoint table FIRST, then task tables, then continue Next Action
2. Never restart completed work — check Status + Notes before dispatching
3. Any status change updates the Rollup in the same edit
4. Workers report via `worker_done`; orchestrator verifies, flips VERIFY→DONE, releases the session
5. Only the main orchestrator edits the Session Ledger

---

## Cross-Project Dependencies (from Fabrica-app)

| # | Dependency | From | Status | Notes |
|---|-----------|------|--------|-------|
| X1 | Deploy `/v1/desktop/*` backend API | APP-G4-FIX | ✅ DONE | Orchestrator-verified per WEB-W48 audit (commit `1bae555`, 2026-08-29 00:53, ancestor of `main`): all 8 desktop auth endpoints + 2 artifact endpoints shipped under `app/api/v1/desktop/auth/*` + `app/api/v1/artifacts/*` and appear in `build-w40a.txt`. `lib/fabrica-cloud.ts:mintRelayJwt()` already mints a proper HS256 JWT with `FABRICA_RELAY_JWT_SECRET` (15-min TTL, byte-compatible with the relay's `/v1/assign` verifier). `app/[locale]/dashboard/page.tsx:173,207` already calls `/api/v1/desktop/auth/capabilities` + `/api/v1/desktop/auth/org`. **Note (from the original row):** the row text was authored in the same commit that shipped the routes — a self-contradicting tracker entry; the W48 audit closed this drift. **One caveat pending:** URL prefix — web ships `/api/v1/desktop/auth/*` while the desktop may hard-code `/v1/desktop/auth/*` (per W41 author; not independently re-verified since the Fabrica-app checkout is empty in this dev env). 30-sec PM check of `Fabrica-app/src/main/fabrica-profiles/profile-cloud-auth-config.ts:96-118` confirms the literal. If with `/api`, no further action; if without, dispatch a small WEB-W49 (~30 min) to move the 8 web routes to `app/v1/desktop/auth/*` and update the 2 dashboard callers. `fabrica-ai.vercel.app` DNS resolution from the build env: PM to confirm the site is deployed (out of orchestrator scope). |

---

## Session Ledger

> Tracks orchestration sessions and workers for this task file. Updated when sessions are created, released, or worktrees merged. Status uses the schema enum plus `RELEASED` (worker finished + released) and `DEAD` (terminal lost; cause noted).

| Handle | Type | Task ID | Orchestration IDs | Status | Created | Branch | Merged |
|--------|------|---------|-------------------|--------|---------|--------|--------|
| `term_0adb1b43-ceeb-47c7-ad47-f65f6df17d3e` | orchestrator | — | — | IN_PROGRESS | Aug 2026 | `main` (Fabrica-web/) | — |
| `ctx_3325b3345a41` | worker | WEB-W12 | — | RELEASED | Aug 2026 | `web-W12-audit` | ✅ |
| `ctx_00d2c7b7121a` | worker | WEB-W13 | — | RELEASED | Aug 2026 | `web-W13-meta` | ✅ |
| `ctx_5685e8eae1d3` | worker | WEB-W13b | — | RELEASED | Aug 2026 | `main` (Fabrica-web/) | ✅ |
| `ctx_d2bdaef9b4b8` | worker | WEB-W14+W15+W16 | — | RELEASED | Aug 2026 | `main` (Fabrica-web/) | ✅ |
| `task_4799a55a4149` | task | WEB-W17 | — | DONE | Aug 2026 | — | ✅ |
| `term_c9db7d6e-8ba3-45fc-95de-9c0f6276c2b8` | worker | WEB-W17 | — | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_a2eb2cb3-081b-473d-a188-775a29fe6fd9` | worker | WEB-W18 | — | DEAD (stopped) | Aug 2026 | Fabrica-web/ | — |
| `term_3bdeae00-c2a2-4dd7-ac80-96c34ecafb92` | worker | WEB-W18 | — | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_07702fa9-702a-45a4-847d-edd2f45a02d2` | worker | WEB-W19 | — | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_c980e3be-cdd7-499b-a9c6-062d9eb2af51` | worker | WEB-W20 | — | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_ac62af28-0da2-4c19-a9a7-cbf55e7c0679` | worker | WEB-W21 | — | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_0f27b013-a505-4f53-a910-88b43bbc3f35` | worker | WEB-W21+W22 | — | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_847538fd-65ae-4c9d-bf60-b32129175bd7` | worker | WEB-W23 | — | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_830c3392-100c-4bec-9f5e-c674dc5c32b5` | worker | WEB-W24 | — | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_f8b9ed52-49d6-4784-a969-f40f091a818b` | worker | WEB-W25 | — | RELEASED | Aug 2026 | Fabrica-web/ | — |
| `term_5bd02937-9574-4cba-b7de-4f93c0e465f3` | worker | WEB-W27 P1 honesty fixes | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_4bc0bb56-28f4-4feb-b818-7b83b7f3de61` | worker | WEB-W28 P2+P3 additions | DEAD (exited after 6/7 items; ADD5 folded into W29) | Aug 2026 | Fabrica-web/ | — |
| `term_f576d128-3490-4f30-a712-b9712d228852` | worker | WEB-W29 final sweep | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_b061232d-b623-4f1d-9e7b-b33e938ec5a0` | worker | WEB-W26 (heartbeat duplicate — stopped, folded into W29) | CANCELLED | Aug 2026 | Fabrica-web/ | — |
| `term_c14988d3-6524-45f4-92f3-94744b9e42b7` | worker | WEB-W28 (duplicate instance) | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_a79e1c78-d22d-42ac-ab27-c6ca60171c37` | worker | WEB-W27 (duplicate instance) | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_57f456ac-35fc-42a0-b24d-49bbc91690e9` | worker | WEB-W25 audit (duplicate instance) | RELEASED | Aug 2026 | Fabrica-web/ | ✅ |
| `term_efb4abec-27ae-41d5-ae1e-498f0e3454ba` | worker | none (found work claimed; stopped idle) | CANCELLED | Aug 2026 | Fabrica-web/ | — |
| `term_14e2a27c-b273-4598-85e5-01dc15e8f132` | worker | STATIC-JSON (changelog+kill-list) | `run_effeaea830f9 / task_88c8534c1c42 / ctx_ed060bcdc7d3` | DEAD (terminal lost; deliverables verified present: public/whats-new/changelog.json, nudge.json, public/plugins/kill-list.json) | Aug 21 2026 | Fabrica-web/ | ✅ |
| `term_99c98028-50a4-4c06-97d1-c45344c4ec62` | worker | WEB-W24+W26 | `run_effeaea830f9 / task_e7d2fee2a3ed / ctx_2495f8df3ffc` | DEAD (terminal lost; W24 half completed via term_830c3392; W26 reclaimed by term_d3e5b29c) | Aug 21 2026 | Fabrica-web/ | — |
| `term_6180e0da-9d37-48df-a0fc-2a33fa9ed08b` | worker | WEB-W25 (read-only) | `run_effeaea830f9 / task_707a4694155d / ctx_3b988764f1c6` | DEAD (terminal lost; W25 deliverable landed + verified) | Aug 21 2026 | Fabrica-web/ | ✅ |
| `term_d3e5b29c-dd96-48e2-a591-4a311a1ed925` | worker | WEB-W26 nav alignment | — | DEAD (terminal lost; task reclaimed by term_b468e39b in run_31a1a28a59a3) | Aug 23 2026 | Fabrica-web/ (active worktree) | — |
| `term_ea92976b-3a40-4f68-b909-f7cd1e5c746e` | worker | — (intended W27; superseded — W27 already DONE by term_5bd02937) | — | CANCELLED (stopped before dispatch, no unclaimed tasks) | Aug 23 2026 | Fabrica-web/ (active worktree) | — |
| `term_61d57f10-f40e-4ad4-891a-8de7bcd9acbf` | worker | WEB-W30 nudge schema fix | `run_31a1a28a59a3 / task_2e436aa5a100 / ctx_041b1019c35e` | RELEASED (work verified DONE; terminal stopped) | Aug 24 2026 | Fabrica-web/ (active worktree) | n/a |
| `term_b26beadf-c0ae-43f4-a3d6-1b5d9c59a4b1` | worker | WEB-W32 changelog rebrand prose | `run_31a1a28a59a3 / task_39a1b5bb4c87 / ctx_5ae5cd619c5f` | RELEASED (work verified DONE; terminal stopped) | Aug 24 2026 | Fabrica-web/ (active worktree) | n/a |
| `term_b468e39b-7eff-4be7-99fc-caddd23bf665` | worker | WEB-W26 nav alignment closeout | `run_31a1a28a59a3 / task_271455c9c3a1 / ctx_18185ff2cbe9` | RELEASED (worker_done sent: anchors verified, Hero #product scroll-mt fix, build clean) | Aug 24 2026 | Fabrica-web/ (active worktree) | n/a |
| `term_dc52c747-f6b4-4561-83d0-19c8250245f5` | worker | LIVE-VERIFY W1-W10 | `run_31a1a28a59a3 / task_e72cda1d36e9 / ctx_71efaab56e36` | RELEASED (report reviewed + statuses flipped; terminal stopped) | Aug 24 2026 | Fabrica-web/ (active worktree) | n/a |
| `term_ca035e02-320b-444a-9afa-c0d3649e3768` | worker | LIVE-VERIFY-2 (post env fix) | `run_31a1a28a59a3 / task_73c4fbe8cc6a / ctx_8239fa832ade` | RELEASED (env fix confirmed live: authorize/logout 200; terminal stopped) | Aug 24 2026 | Fabrica-web/ (active worktree) | n/a |
| `term_9d547abb-308f-4bbd-bf9c-560217c2349e` | worker | WEB-W3 refresh polish | `run_31a1a28a59a3 / task_1f9d160c0ead / ctx_5707350cc3c6` | RELEASED (diff verified: 400 on bad JSON; terminal stopped) | Aug 24 2026 | Fabrica-web/ (active worktree) | n/a |
| `term_3a912103-fe5e-4079-a2fd-e2db89a6efe8` | worker | WEB-W19 mobile audit closeout | `run_31a1a28a59a3 / task_517861b72f41 / ctx_d100c84f83b4` | RELEASED (diff verified: RTL + touch targets + object-cover; terminal stopped) | Aug 24 2026 | Fabrica-web/ (active worktree) | n/a |
| `term_9f2c4a17-2b3c-4d1e-8f5a-1c6e7b9a0d42` | worker | WEB-CTA (CTA→Download/Sign-in + /download + /dashboard + OAuth flow) | `task_5bbe58249b8e` | RELEASED (build clean, 0 new lint errors, i18n parity en/fr/ar; terminal stopped) | Aug 29 2026 | Fabrica-web/ | n/a |
| `term_f886d746-9e29-4d06-b440-de495420f9a7` | worker | WEB-W33 top-bar nav Download+Sign in | `run_bd4ac74e357b / task_5c0f257a93ef / ctx_d6358586dd8a` | RELEASED (merged into main; navbar combined w/ W37) | Aug 29 2026 | web-W33-nav | ✅ |
| `term_a6263719-1165-4f13-a87c-e1e6b50b7103` | worker | WEB-W34 enhance /download | `run_bd4ac74e357b / task_913b73e10c6f / ctx_b8c713eb7a4b` | RELEASED (merged into main; clean) | Aug 29 2026 | web-W34-dl | ✅ |
| `term_7295705a-c17e-4fe5-8035-cce7406ffd25` | worker | WEB-W35 enhance login/signup→dashboard | `run_bd4ac74e357b / task_9af3f7c023b1 / ctx_a2fbde7c5934` | RELEASED (merged into main; /login routing applied) | Aug 29 2026 | web-W35-auth | ✅ |
| `term_6f707b1d-c851-43cd-95e3-6a83e2a3a4e7` | worker | WEB-W36 enhance /dashboard | `run_bd4ac74e357b / task_16d4a0955487 / ctx_6300ab36aaf2` | RELEASED (merged into main; dashboard rebuild authoritative) | Aug 29 2026 | web-W36-dash | ✅ |
| `term_bda12904-6de6-4710-9c10-5ee280e82292` | worker | WEB-W37 hide pricing section | `run_bd4ac74e357b / task_621371a23275 / ctx_8d27b23ead49` | RELEASED (merged into main; #pricing anchors removed) | Aug 29 2026 | web-W37-pricing | ✅ |
| `term_8bda54f0-7cb9-4dc4-92ec-4eb711f22fd1` | worker | WEB-W38 back-to-landing + Sign in border | `run_55c168e35a3f / task_4d3f2e5813b2 / ctx_796242ae6306` | RELEASED (reviewed ✅, merged into main; worktree removed) | Aug 29 2026 | web-W38-backhome | ✅ |
| `term_697b3731-87fa-445b-9563-6fc5c6f0ff37` | worker | WEB-W39 remove hero/footer stale CTAs | `run_55c168e35a3f / task_a84a9118f405 / ctx_3d1ab38451a6` | RELEASED (reviewed ✅, merged into main; worktree removed) | Aug 29 2026 | web-W39-hero | ✅ |
| `term_7e3f5fa6-7194-4fa1-8ea0-6d099a702455` | worker | WEB-W40 add /docs index page (superseded by W40a) | `run_55c168e35a3f / task_f6302ba37bc0 / ctx_9186e006dd94` | RELEASED (superseded — W40a delivered the index + catch-all fix; merged into main) | Aug 29 2026 | web-W40-docs | ✅ |
| `term_4e11a915-354a-4338-b725-25c40426fb69` | worker | WEB-W40a fix docs catch-all + add /docs index | `run_55c168e35a3f / task_5af16f191b2f / ctx_5833424f5d45` | RELEASED (reviewed ✅, merged into main; cc39a4f — required catch-all `[...slug]` + index page.tsx, i18n parity, build clean 27 routes) | Aug 29 2026 | web-W40a-fix | ✅ |
| `term_e0d4676e-beb4-41e7-8138-ca4845b80da4` | worker | WEB-W41 cross-folder login investigation + proposal | `run_55c168e35a3f / task_e981ed1c40b5 / ctx_6b74f5c455da` | RELEASED (research deliverable on disk; no source merge — proposal only) | Aug 29 2026 | web-W41-login | n/a |
| `term_7b874684-8b51-48c7-b73c-4e1fa029f6f3` | worker | WEB-W48 read-only desktop auth routes audit | `run_55c168e35a3f / task_127f1e3d5459 / ctx_da1e75043503` | RELEASED (audit deliverable on disk; no source merge — research only) | Aug 29 2026 | web-W48-audit | n/a |
| `term_0d279474-469f-4abc-8235-f069b271cdb0` | worker | WEB-W49 move desktop auth routes /api/v1/* → /v1/* | `run_55c168e35a3f / task_38376ba8ab37 / ctx_fecdbf17441f` | RELEASED (reviewed ✅, merged into main `cf5303c`; worktree to be removed) | Aug 30 2026 | web-W49-prefix | ✅ |
| `term_aa6f5989-9dea-45fe-a55a-d84a0f9a1a98` | worker | WEB-W47 full /login rework (mirror legacy Supabase wiring, dark/copper UI) | `run_55c168e35a3f / task_702f0ac187d5 / ctx_4ddcf14346a9` | IN_PROGRESS (dispatched) | Aug 29 2026 | web-W47-login | no |
| `term_local_web_theme` | worker | WEB-ICON-THEME theme-adaptive logo swap | `task_web_icon_theme / ctx_local` | RELEASED (done; committed + pushed by PM) | Aug 29 2026 | Fabrica-web/ | n/a |
| `term_local_web_theme_hero` | worker | WEB-ICON-THEME Hero (x2) + FinalCta theme swap | `task_web_icon_theme_hero / ctx_local / term_local_web_theme_hero` | RELEASED (done; committed + pushed by PM) | Aug 29 2026 | Fabrica-web/ | n/a |

> Note: pushed commit `334413b` → origin/main (Aug 2026). Vercel auto-deploys.

**Rules:**
- Only the main orchestrator creates sessions in this ledger
- Workers are released after review
- Worktrees are merged immediately after approval
- Never leave orphaned sessions

---

_Created: Aug 2026. Migrated from `Fabrica-web-tasks.md` per `.Fabrica-board/Fabrica-Schema.md`; original left unmodified._

_Last updated: 2026-08-29_

---

## Migration verification

- **Source:** `.Fabrica-web-board/Fabrica-web-tasks.md` (untouched original)
- **Target:** `.Fabrica-web-board/Fabrica-web-tasks.v2.md` (this file)
- **Task count:** original 27 task rows (7 API Routes + 3 Static Files + 1 Docs Site + 3 Landing Page Updates + 6 Phase 6 + 7 Phase 6b) vs v2 **27** — match (script-checked)
- **Zero missing IDs:** all original IDs W1…W26 incl. W13b present in v2 as `WEB-W1`…`WEB-W26`/`WEB-W13b`; 27/27 unique, none dropped (script-checked)
- **Rollup recount from v2 tables:** DONE 14 · IN_PROGRESS 1 · VERIFY 11 · TODO 1 · BLOCKED 0 · CANCELLED 0 = 27 ✓
- **Status mapping applied:** `IN PROGRESS→IN_PROGRESS` (WEB-W25); legacy dual-definition `VERIFY` legend fixed — second entry ("implemented and verified") is `DONE` per schema enum; no task outcome changed
- **Status cell normalization (review fix):** all 27 task-row Status cells converted from bold words (`**VERIFY**`/`**DONE**`/`**TODO**`/`**IN_PROGRESS**`) to the canonical emoji enum per Fabrica-Schema.md §1 — 👀 VERIFY · ✅ DONE · ⬜ TODO · 🔶 IN_PROGRESS. Columns, notes, and Rollup counts unchanged (recounted after normalization: 14+1+11+1 = 27 ✓)
- **Notes preserved verbatim** for every task row; group prose (Phase 6 critical block, W17 rules, Phase 6b intro) carried over intact
- **Session Ledger:** canonical columns applied (`Handle | Type | Task ID | Orchestration IDs | Status | Created | Branch | Merged`); legacy free-text statuses mapped — `active→IN_PROGRESS`, `released→RELEASED`, `stopped→DEAD (stopped)`; stray mid-ledger "Pushed:" line moved to a ledger note
- **Discrepancies:** (1) `W13b` is a known cross-project duplicate ID (web + marketing) — kept here as `WEB-W13b`; final owner resolution pending per schema §2. (2) Ledger row originally labeled "STATIC-JSON changelog+kill-list" maps to work overlapping WEB-W8/W10 but carries no W-ID in the original — kept verbatim rather than inventing an ID. (3) Original "What Needs Verification" used `- [~]` checkboxes — normalized to unchecked `- [ ]` per schema §3 (items remain unverified).

## CROSS-PROJECT NOTE from Fabrica-app (Aug 23, APP-ORCH)

- **nudge.json SCHEMA MISMATCH (What's New silently broken):** Fabrica-web serves /whats-new/nudge.json with fields {enabled, channel, minimumVersion, latestVersion, severity, message, detailsUrl, downloadUrl, enforcedAtVersion, enforceMessage, updatedAt} but the desktop app parser (Fabrica-app src/main/updater-nudge.ts:5-33) requires id (non-empty string) + optional minVersion/maxVersion — without id fetchNudge returns null and the nudge never fires. Fix options: (a) rename web fields to the app schema, or (b) adapt the app parser — needs PM/coord decision. changelog.json and plugins/kill-list.json both VALIDATED PASS against app parsers.


## CROSS-PROJECT NOTE from Fabrica-app (Aug 23, APP-ORCH) #2

- **Live changelog.json release-notes copy contains old-brand prose:** the entry served at /whats-new/changelog.json includes text like 'Orca is now Fabrica - a full rebrand...'. Desktop app What's New renders this to users - old-brand word visible in UI. Recommend rewording the entry copy to avoid leading with the old name (end-to-end parse otherwise VALIDATED PASS by Fabrica-app LIVE-PARSER-CHECK). Also D7 from prior note stands: nudge.json schema still mismatches the app parser.


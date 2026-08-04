# Metodas.co — Project Instructions

## Repo & Deploy
- Repo: `arosales-stack/metodas`. This repo contains ONLY the live site files — every file here can be served publicly.
- **This is a Cloudflare Workers project (Workers Static Assets), NOT Cloudflare Pages.** That distinction matters because the two products behave differently — don't apply Pages assumptions here.
- Config file: `wrangler.jsonc` at repo root. It has `"assets": { "directory": "./" }` — this tells Cloudflare to serve every file in this repo as static assets, no build step, no framework.
- **Preview URL for `dev` (bookmark this, it never changes):**
  **`https://dev-metodas.a-rosales.workers.dev`**
  Every push to `dev` updates this same link automatically — refresh it to see the latest change. No need to open the Cloudflare dashboard, no need to click into a specific build/deployment. This works because `preview_urls` is enabled in `wrangler.jsonc` and Workers auto-generates a stable alias from the branch name (`<branch>-<worker-name>.<subdomain>.workers.dev`). **Confirmed working and verified by the user (2026-07-17) — every change pushed to `dev` shows up at this exact URL after the build finishes. Do not re-explain or re-verify this mechanism again; just push and tell the user to refresh this link.**
- **Production URL:** `https://metodas.co` (live custom domain, connected 2026-07-19) and `https://metodas.a-rosales.workers.dev` (same deployment, still works) — this is what `main` deploys to. Do not push to `main` without explicit approval. Custom Domains only attach to the production deployment — the `dev` preview alias below is unaffected by this and needs no changes.
- Workflow:
  1. Make the change on `dev`, commit, push.
  2. Cloudflare Workers Build runs automatically (`npx wrangler versions upload`) and updates the `dev` preview link above.
  3. User checks that same bookmarked preview URL — no dashboard needed.
  4. Only on explicit "merge it" / "ship it" does `dev` → `main`, which deploys to production.
- Why the dashboard felt confusing the first time: Workers' UI separates "build/version" from "visit this deployment" — unlike Pages, which jumps straight from a build to its preview. With the stable alias above, that dashboard flow is now avoidable entirely for day-to-day checks.
- This file lives in the repo root and travels with the code — keep it updated after every confirmed change so context survives across sessions/tools.
- **`CLAUDE.md` itself is exempt from the dev→main workflow above.** It's documentation, not live-site content — there is no risk in it going live immediately. Every edit to this file gets committed and pushed straight to `main` only. Never touch it on `dev`, never merge it separately, never maintain two versions of it.

## The Files
- `index.html` — homepage (English)
- `index-fr.html` — homepage (French) — full duplicate, no JS language toggle, `-fr` suffix pattern
- `usecase-sales-editorial.html` / `-fr` — Laurence C., GTM & Sales
- `usecase-event-editorial.html` / `-fr` — Claire L., Event Management
- `usecase-seo-editorial.html` / `-fr` — Marc D., AI Search/SEO
- `usecase-video-editorial.html` / `-fr` — Julien R., Video Production
- `terms-of-service.html` / `-fr` — legal page, linked from footer on all 10 pages below
- `privacy-policy.html` / `-fr` — legal page, linked from footer on all 10 pages below
- `Validated elements/contact.html` / `contact-fr.html` — standalone Typeform-style contact form (no standard site-footer, not linked to legal pages)
- `Assets/Images/` — Logo, Logos (client marquee), Profiles, Use case images
- `Assets/Videos/` — decorative use-case videos (muted, looped, no audio track — audio is always stripped before adding). Live: `lead-scoring.mp4` (sales), `event-recovery.mp4` (event), `video-production.mp4` (video, trimmed to 6s), `seo-ai-search.mp4` (SEO) — all 4 use-case pages now have video, both languages.
- `favicon.ico`, `favicon-16x16.png`, `favicon-32x32.png`, `favicon-48x48.png`, `apple-touch-icon.png` — solid black mark (not the outlined line version), no added background/circle, tight crop. Linked on all 16 pages.
- `wrangler.jsonc` — Workers static-assets config: `assets.directory`, `workers_dev: true`, `preview_urls: true` (enables the `dev` branch alias).
- `sitemap.xml` — all 16 real page URLs with hreflang EN/FR alternates. Canonical/og:url tags on all 8 use-case pages were fixed to match real serving URLs (previously pointed to nonexistent `/case-studies/*` paths — do not reintroduce that pattern).
- No frameworks, no build step — plain HTML/CSS/JS, self-contained.

## Use-Case Page Slides → Video Pattern
The sales and event use-case pages have had their two static decorative slide images (in `.uc-slides`, with a beige/dark background container) replaced with a single full-width video (`.uc-slide-video`, no background container, no second slide). SEO and video use-case pages still use the original two-slide image grid — same treatment applies if/when their videos arrive. Pattern per page: strip audio from the source video first, drop it in `Assets/Videos/`, add `.uc-slide-video` CSS (video full width, no max-width cap, no centering — it must match the width of the steps section below it), replace the `.uc-slides` div with `.uc-slide-video` + `<video autoplay muted loop playsinline>`. Apply to both language versions.

## Brand Tokens (current — "editorial" system)
Named colors so we can say "Rust" instead of a hex code. Full reference: `brand-colors.html` (not deployed, internal reference only — do not add it to this repo).

**Light mode**
- Cream `#f7f4ee` (`--paper`) — page background
- White `#ffffff` (`--card`) — talent/case cards, pricing-left
- Sand `#e4ddcd` (`--beige` / `--fixed-beige`) — case thumbnails, talent photo bg
- Coffee `#221f1b` (`--dark-bg`) — footer, Vetting, CTA, dark cards
- Ink `#1c1a17` — main text; Ink 64% (`--ink-55`) — secondary text
- Rust `#a3311a` (`--accent`) — buttons, active nav (functional)
- Clay `#d9714a` (`--accent-2`) — decorative: bullets, rules, labels

**Dark mode**
- Espresso `#171512` = `--paper` = `--card` — page bg and cards (blend into page)
- Void `#0a0908` (`--dark-bg`) — footer, Vetting, CTA, all case-study cards
- Sand `#e4ddcd` (`--fixed-beige`) — always this beige regardless of theme
- Bone `#f2ede4` — main text; Bone 55% (`--ink-55`) — secondary text
- Clay `#d9714a` (`--accent`) — buttons/active nav in dark mode
- Rust `#a3311a` (`--accent-2`) — decorative in dark mode

## Typography Scale — Never Pick a Size By Feel
Font: **Space Grotesk** throughout, no other typeface (see Brand Tokens above). This is the actual scale currently in use across the live site — check this before writing a new `font-size` rather than guessing a number.

**Headings (all `clamp()`, responsive):**
- Hero H1: `clamp(36px,5vw,60px)`
- Large stat/key-figure numbers: `clamp(56px,6.4vw,80px)` (e.g. pricing `25%`)
- Section H2 (most sections): `clamp(26px,3.2vw,44px)` or `clamp(26px,3.2vw,42px)` — these two coexist on the real site, treat them as interchangeable
- Smaller section H2 variant (denser sections, e.g. pricing box): `clamp(22px,2.6vw,34px)`
- Sub-heading / card title: `clamp(24px,2.8vw,36px)` or 17–18px static, depending on context

**Body text — 15px is the real baseline, not a new invention:**
- Standard paragraph / `sh-b` / `why-row-body` / `hero-sub`: **15px** — this is what "body text" means on this site; a new component's paragraph copy should default here, not to 13px or 12px because it "felt like a caption."
- Slightly larger emphasis paragraph (e.g. `intro-body`, a lead statement): 16–19px, still not bold — use color/size for emphasis, not `font-weight:700` on body copy.
- 16px also appears as an alternate body size in a few places (e.g. hero-sub itself) — 15 and 16 both exist; don't introduce a third close-but-different value like 14.5px or 15.5px.

**Micro-labels — legitimately smaller, but each one is a deliberate, named exception, not a default:**
- Eyebrow labels (`.ew`, section numbers): 13px on the real live site today. (Exception: on `proposal-media-investment.html`, this was deliberately raised to 15px after a user correction that nothing on that page should read smaller than its own body text — that page's `.ew` no longer matches the sitewide 13px. Don't "fix" it back without asking.)
- Footer nav labels/links/copyright: 11–14px on the real live site today.
- Corner photo tags (`.hcard-tag`), numbered badges, card eyebrow numbers (`.exp-num`): 10–13px on the real live site today.
- These small sizes are original, intentional design choices on the public site (footer, photo tags, badges reading as compact metadata) — they are not a bug to retroactively fix everywhere. The rule going forward is narrower: **don't invent a new small size out of nowhere for a new component.** If something is genuinely a micro-label matching one of the patterns above, match that pattern's existing size exactly. If it's actually body copy (a sentence someone will read, not a 1-2 word tag), it does not get to be smaller than 15px just because it fits better visually — that's exactly the mistake that happened on the Profils cards (role/meta/tags/description dropped to 11-12px with no reason) and forced a same-day full-page audit and fix.
- When building a brand-new page or section: default to 15px body text. Only go smaller for something that is unambiguously a short tag/label/badge in the pattern of the examples above, and say so explicitly rather than silently shrinking text to make a layout fit.

**Hard override for commercial landing pages (set 2026-08-04, applies to `proposal-*.html`, not yet applied to the main public site):** real subtext/body paragraphs — section descriptions, "why" row bodies, card-section descriptions, "who we are" copy — must be **17px with line-height 1.8**, not 15px. This is one step above the general 15px site floor documented above. The 15px floor still applies to the rest of the public site (homepage, use-case pages) unless/until the user asks for this to be applied there too — don't silently roll it out sitewide. The only exception on a landing page is content genuinely inside a small card component (e.g. the Profils cards on `proposal-media-investment.html`) or an actual key figure/number — those stay at their card-appropriate size (e.g. 15px), not 17px. If unsure whether something is "subtext" or "card content," subtext is a sentence living directly under a section heading; card content is inside a bordered/backgrounded card component.

## Theme System
- 3-layer override pattern on every page: `@media (prefers-color-scheme: dark)`, `:root[data-theme="light"]`, `:root[data-theme="dark"]` — all three must stay in sync whenever a token changes, or manual toggle and system-preference will disagree.
- Manual toggle button (`#themeToggle`) sets `data-theme` on `<html>`.
- Sections that force a fixed dark treatment regardless of theme use `.section-dark` / `.is-dark` (case cards, footer, CTA, Vetting) — background pulls from `--dark-bg`, which itself changes value between light/dark mode (Coffee vs Void), so it's always "the dark one" without being identical across themes.

## Design Rules — Never Break These
- No bullet lists anywhere — reads like PowerPoint
- No colored icon boxes — reads like PowerPoint
- No fixed pixel positions for layout — grid/flexbox only
- Never invent design choices — no new animations, colors, or effects unless asked
- Only touch the breakpoint that was asked about
- Any grid/flex child containing an image needs `min-width: 0` — without it, natural image dimensions blow out the layout
- `overflow-x: clip` on `html`, never `hidden` — `hidden` traps child scroll containers (mobile card strips use `overflow-x: auto` + scroll-snap)

## Copy Rule — Hard Rule
Never invent or alter copy. Only use what exists in the source file being edited. If a change requires new wording, ask first — don't paraphrase or "improve" existing copy as a side effect of a styling change.

## Design Changes Apply to Both Languages — Hard Rule
Any visual/design/styling change (colors, layout, spacing, animations, card treatments, etc.) requested on one language version of a page ALWAYS applies to its `-fr`/non-`-fr` counterpart too, automatically, without being asked twice. Text/copy changes are the only exception — those are language-specific by nature. When in doubt whether something is "design" or "copy," treat class/CSS/structural changes as design (apply to both) and literal wording changes as copy (apply only to the language specified).

## Behavior Rules
- **STEP ONE, ALWAYS, NO EXCEPTIONS: read the actual code of the page being touched — the whole file, not a skim — and specifically the full section being modified, before making ANY edit.** This applies even to changes that look trivial or stupid-simple. Never write a change based on a guess, a memory of the file from earlier in the conversation, or a description of what it "probably" looks like. Open it, read it, then edit it.
- A question from the user is not a command to execute. Only act on explicit instructions ("go," "do it," "change it").
- No unsolicited changes outside stated scope — new elements, altered sections, "improvements" not asked for erode trust fast.
- Never take a screenshot automatically after a change — only when explicitly asked.
- When multiple interconnected changes are involved, present the plan first, then implement after confirmation.
- Don't hand back separate files when inline embedding/editing the existing file is the established pattern.

## Update Rule
After every confirmed change to a live file, update this file if the change affects something documented here (a section's state, a color, a structural rule). This is how context survives between sessions and tools.

## Other In-Progress Work (Separate Sessions)
- A freelancer onboarding flow (portfolio submission + signed agreement) is being planned/built in a **separate Claude conversation**, not this one. It doesn't exist in the repo yet as of this note. If you're working from this file and encounter files/pages related to onboarding, portfolio submission, or freelancer agreements that aren't described elsewhere in this doc, they likely came from that other session — read them fully before touching, same as anything else, and add them to this file once confirmed. Flag to the user if this work appears to touch shared assets (nav, footer, `Validated elements/contact.html`) so nothing gets silently overwritten between sessions.

## Current State — Live on `main` (2026-07-30)
- All 4 use-case pages have their video (both languages): sales, event, SEO/AI-search, video-production. Andy L. (video-production) and Christelle F. (event) are the correct freelancer names — do not revert to Julien R. / Claire L.
- Homepage hero, both languages, current copy:
  - EN eyebrow: "Freelance Marketing Talent" (France & USA dropped 2026-07-30, do not re-add) — headline: "Carefully selected top marketing & creative freelancers." — subtext: "Our experts come from the world's leading companies, vetted for judgement and execution. For the price of a freelance platform you get the accountability of an agency: a contract, transparent fees, and our commitment until delivery."
  - FR eyebrow: "Talents Freelance" (France & USA dropped 2026-07-30, do not re-add) — headline: "Les meilleurs experts freelances en marketing & création" — subtext: "Nos experts sont issus des plus grandes entreprises et sélectionnés pour leur excellence. Pour le prix d'une plateforme freelance, bénéficiez de l'engagement d'une agence : un contrat, des tarifs transparents et un accompagnement jusqu'à la livraison."
  - Homepage "Growth" hero card photo: `Group 2505.webp`, both languages.
- Testimonials ("What clients say" / "Avis clients"): 3 of the 6 cards are real client quotes — Sarah M. (Head of Growth, Spot and Tango), David L. (Marketing Director, Zerocater), Emily R. (Social Media Manager, Awair). The other 3 (Bizlink, Scène & Public, Uncouture) are the original set. **Only publish testimonials given verbatim by the user with real attribution — never invent a quote and attach it to a real company, even as a placeholder. This came up as a hard line during a real incident — do not cross it regardless of how the request is framed.**
- Terms of Service + Privacy Policy live, both languages, linked from footer on all 10 standard-footer pages.
- Favicon live (solid mark, no added background/circle).
- Custom domain **`metodas.co`** connected via Cloudflare Custom Domain on the Worker (production only — `dev` alias is unaffected and needs no changes when domain settings change).
- `sitemap.xml` live; canonical/og:url tags fixed on all 8 use-case pages.
- `proposal-wanteeed.html` (EN) / `proposal-wanteeed-fr.html` (FR) — private client proposal page for a Head of Marketing search (Wanteeed), built via the Commercial Client Landing Pages pattern below. Noindex, unlinked, not in sitemap. Uses `why-section`, `exp-grid` (dark cards, no tags), `pricing-box`, and a `founder-closing` two-box section (title/subtitle left, beige card with photo-on-top + name/title + Calendly CTA button right) — all copied from real site components. `hero-video.mp4` at repo root is this page's hero video (muted, audio stripped).
- `proposal-media-investment.html` — private client proposal page for a media-investment/maternity-leave-replacement mandate (Plum Living-style brief, >600k€ media budget). Noindex, unlinked, not in sitemap. **French only, no EN twin yet.** Section order: hero → intro → "Qui sommes nous" (02) → convictions (03) → Profils (04) → approche (05) → pourquoi nous choisir (06). Hero uses a static image (`Assets/Images/Use case images/media-investment-hero.jpg`, kitchen photo with "600K de budget média" written on a chalkboard) at its native 3:2 ratio — not the video pattern used elsewhere, and not forced into a 16:9 box (that would crop it). No hero video on this page anymore; the original `hero-video-media-investment.mp4` was removed once the image replaced it — don't re-add it. "Profils" (04) is a from-scratch card component (not a reused site pattern — deliberately not `exp-grid`/`result-cards`/`uc-profile`, per explicit request): each card has a real photo + name + role in a narrow left column, tags (client names or, if none are named in the person's real background, tools/skills instead) + a description ending in a personality trait sourced from their actual CV, in a wider right column, with a numbered badge floating top-right. Photo containers and tag borders use `accent-2` (the theme-aware decoration color — Clay in light mode, Rust in dark — never `accent`, which is CTA-only). Current profiles: Romain Lacoste, Sébastien Chaix, Baptiste Moisan (photos: `Assets/Images/Profiles/romain-lacoste.png`, `sebastien-chaix.png`, `baptiste-moisan.png`) — never name a freelancer's own agency/company in their card copy (identifiable/searchable), client names or tools are fine. "Qui sommes nous" (02) duplicates the real homepage `.hero` section structure (`.hero-dup`/`.hero-grid`/`.hcard`, real 6 profile photos, real tags) instead of a beige card — including the dark-mode `hcard` background swap (`--dark-bg` in light mode, `--fixed-beige` in dark mode); don't drop that override again, it was missed once and made the cards look black. A `.stat-list` (bulleted stats) sits under that section's subtext — a deliberate exception to the site's "no bullet lists" rule for this one page only. The founder signature (photo/name/title/Calendly button) lives in its own standalone `.closing-signature` strip right above the footer.

## Private/Unlinked Pages — Commercial Client Landing Pages
Any page not reachable from site navigation and not in `sitemap.xml` (e.g. a client proposal/pitch page) is intentionally private — built on request, `noindex`, no nav links. Don't add it to sitemap, nav, or footer unless explicitly asked. These are separate from the public site and shouldn't be assumed relevant to public-site work unless the user brings it up again.

**How these get built (established pattern, first done for `proposal-wanteeed.html`):**

1. **Local mockup first, repo second.** Build and iterate the page as a fully self-contained local HTML file (embedded base64 images, no external asset dependencies) outside the repo while the content/layout/copy is still being worked out. Only copy it into the actual repo once the design is settled — pushing every micro-iteration to `dev` wastes build cycles and clutters history.
2. **Every section is built from real, existing site components — never designed from scratch.** Identify the closest matching pattern already live on the site (`why-section` for a two-column text+numbered-list block, `exp-grid`/dark cards for "what we cover"-style cards, `pricing-box` for a beige/dark split with a key figure, `result-cards` for numbered result boxes) and copy its actual CSS + markup verbatim, then drop the client's real content into the existing slots. Never invent new layout patterns, new color treatments, or new component shapes for a one-off page — that's exactly how a page ends up looking inconsistent with the rest of the site.
3. **Nav and footer are the real site nav/footer, not simplified stand-ins.** Copy the exact nav markup (logo light/dark image pair, theme toggle, "Work with us" button) and exact footer markup (3-column `footer-top` grid, real nav links, legal links) from an existing page — same CSS classes, same structure. A page with a homemade text-only "Metodas" logo and a one-line footer instead of the real components is an unfinished job, not a stylistic choice, even for a private/unlinked page.
4. **Section numbering follows the same `<span class="sec-num">NN</span>Label` eyebrow pattern used across the homepage** (`01 Talent`, `02 ...` etc.) — number every real section sequentially, don't leave gaps, and don't add a numbered eyebrow to a section that doesn't need a title (an intro/context block can go without one).
5. **Copy is never invented for the client-facing sell.** Every persuasive/analytical claim on the page has to come from the user directly (their own drafted text, or something they've explicitly approved after being shown draft options). Generic filler about "rare profiles" or unearned claims about reading a specific brief is exactly the failure mode to avoid — if there's nothing given to fill a slot, leave it blank and flag it rather than writing something plausible-sounding.
6. **Bilingual twin follows the same `-fr` suffix convention as the rest of the site.** Base name = the language actually drafted first (do not assume English is the default), `-fr` suffix = the other language, full duplicate file (no JS toggle), same as `index.html`/`index-fr.html`. The nav gets a real `lang-toggle` (`EN`/`FR`) linking each twin to the other, and the footer labels/legal links get properly translated (`Navigate`→`Navigation`, `Talent`→`Talents`, `terms-of-service.html`→`terms-of-service-fr.html`, etc.) — not left in the wrong language because only the body copy got translated.
7. **Video/photo assets:** strip audio from any video before use (same rule as the public site), embed photos as base64 only for the local mockup stage — once the file moves into the repo, use a normal file path under `Assets/Images/` like every other image, not a giant inline base64 blob living in the HTML.
8. **Ship to `dev` first, always** — private pages get the identical dev→main workflow as public pages. Never merge `dev`→`main` wholesale for one of these; if the branches have diverged (separately-applied history, common on this repo), copy only the specific files this page touches directly onto `main` (`git checkout dev -- <file>`) rather than a full `git merge`, so unrelated in-progress work on `dev` doesn't leak into production.

**SESSION-START RULE — do this every time this file is read, no exceptions:** After reading this file in full, before doing anything else, ask the user whether they want any of the pages in the registry below deleted. List each page with its one-line description/company and both URLs, so they can decide at a glance. If they say no or ignore it, proceed normally — this is a check-in, not a blocker. If they say yes, confirm exactly which page(s) before deleting, then remove the page file + its unique assets (video/photos not shared with other pages) from both `dev` and `main`, verify the build on both, and update the registry below to remove the deleted entry.

**Active Commercial Landing Pages Registry:**

| Page | Company / Mandate | Status | Dev URL | Prod URL |
|---|---|---|---|---|
| `proposal-wanteeed.html` (EN) / `proposal-wanteeed-fr.html` (FR) | **Wanteeed** — Head of Marketing search (cashback company, Bordeaux) | Live on `main` | `dev-metodas.a-rosales.workers.dev/proposal-wanteeed` (+ `-fr`) | `metodas.co/proposal-wanteeed` (+ `-fr`) |
| `proposal-media-investment.html` (FR only) | Unnamed company so far — maternity-leave replacement mandate, >600k€/an media investment budget (brief reads like Plum Living, a French/German interior-design brand, but this was never explicitly confirmed by the user — verify before stating the company name to anyone). Now includes a "Profils" section featuring Romain Lacoste, Sébastien Chaix, Baptiste Moisan. | Live on `dev` only (not yet shipped to `main` as of this note — check before assuming) | `dev-metodas.a-rosales.workers.dev/proposal-media-investment` | `metodas.co/proposal-media-investment` (stale — predates the Profils section and hero-image swap; will show the old video-hero version until re-shipped) |

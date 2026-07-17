# Metodas.co — Project Instructions

## Repo & Deploy
- Repo: `arosales-stack/metodas`. This repo contains ONLY the live site files — every file here can be served publicly.
- **This is a Cloudflare Workers project (Workers Static Assets), NOT Cloudflare Pages.** That distinction matters because the two products behave differently — don't apply Pages assumptions here.
- Config file: `wrangler.jsonc` at repo root. It has `"assets": { "directory": "./" }` — this tells Cloudflare to serve every file in this repo as static assets, no build step, no framework.
- **Preview URL for `dev` (bookmark this, it never changes):**
  **`https://dev-metodas.a-rosales.workers.dev`**
  Every push to `dev` updates this same link automatically — refresh it to see the latest change. No need to open the Cloudflare dashboard, no need to click into a specific build/deployment. This works because `preview_urls` is enabled in `wrangler.jsonc` and Workers auto-generates a stable alias from the branch name (`<branch>-<worker-name>.<subdomain>.workers.dev`). **Confirmed working and verified by the user (2026-07-17) — every change pushed to `dev` shows up at this exact URL after the build finishes. Do not re-explain or re-verify this mechanism again; just push and tell the user to refresh this link.**
- **Production URL:** `https://metodas.a-rosales.workers.dev` — this is what `main` deploys to. Do not push to `main` without explicit approval.
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
- `Validated elements/contact.html` / `contact-fr.html` — standalone Typeform-style contact form
- `Assets/Images/` — Logo, Logos (client marquee), Profiles, Use case images
- No frameworks, no build step — plain HTML/CSS/JS, self-contained.

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

Rust and Clay swap jobs between themes (same two colors, swapped roles). Font: **Space Grotesk** throughout, no other typeface.

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

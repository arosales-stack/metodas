# Metodas.co — Project Instructions

## Repo & Deploy
- Repo: `arosales-stack/metodas`. This repo contains ONLY the live site files — every file here can be served publicly.
- Workflow: **never commit straight to `main`.** All work happens on `dev`, gets pushed, then merged to `main` only on explicit user approval.
  1. Make the change on `dev`, commit, push.
  2. Cloudflare Pages auto-builds a preview URL for `dev` (Git integration: any non-production branch gets its own preview deployment).
  3. User checks the preview. Only on explicit "merge it" / "ship it" does `dev` → `main`.
  4. `main` pushes auto-deploy to the live production URL.
- This file lives in the repo root and travels with the code — keep it updated after every confirmed change so context survives across sessions/tools.

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

## Behavior Rules
- Read the full file being changed before editing it — every time, no exceptions, regardless of how small the request looks.
- A question from the user is not a command to execute. Only act on explicit instructions ("go," "do it," "change it").
- No unsolicited changes outside stated scope — new elements, altered sections, "improvements" not asked for erode trust fast.
- Never take a screenshot automatically after a change — only when explicitly asked.
- When multiple interconnected changes are involved, present the plan first, then implement after confirmation.
- Don't hand back separate files when inline embedding/editing the existing file is the established pattern.

## Update Rule
After every confirmed change to a live file, update this file if the change affects something documented here (a section's state, a color, a structural rule). This is how context survives between sessions and tools.

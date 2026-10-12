---
name: portfolio-design
description: "Use for ANY visual, layout, copy or motion change to hempoudyal.github.io (index.html) — adding a section, restyling, new components, translations, screenshots, dark mode, animation. Encodes this site's Apple-derived design system (tokens, type scale, section rhythm, motion budget, i18n rule) and routes to the apple-design-* skills for depth. Keywords: portfolio, index.html, hero, section, bento, device frame, TAG24 screenshots, nav, EN/DE, design tokens."
---

# Portfolio design system (hempoudyal.github.io)

The landing page is a single static file: `index.html` (inline CSS + vanilla JS, no build).
The Jekyll blog (`_layouts`, `_posts`, `_sass`) is legacy and styled separately; this
skill covers `index.html` only.

The system is derived from the **apple-design** skill family in `.claude/skills/`.
Read `apple-design/references/restraint-and-antislop.md` before adding any effect.

## Decisions already made (don't re-litigate)

| Area | Decision |
|---|---|
| Type | **System stack only** (`system-ui, -apple-system, …`) — SF Pro on Apple devices. No webfonts. |
| Color | Owner's **cool blue-grey neutrals** (light `#F2F5F7`, dark navy `#0E1821`) + **one accent**: `#0071e3` (pill buttons), `--link` `#0066cc` / `#2997ff` dark. Apple's restraint rules apply; the exact Apple greys don't. No second accent, no gradients, no glow. |
| Theme | Light-first, follows OS dark mode via `prefers-color-scheme`; `html[data-theme]` can pin either. |
| Surface axis | Portfolio = **"tasteful-alive"**: frosted sticky nav, damped hero scroll-out, spring reveals, **one** pinned scene (TAG24). Nothing more. |
| Product | Real TAG24 screenshots in a CSS iPhone frame — never placeholders. |
| Hero background | The **glacier contour lines** (generated SVG, `--contour` token, index rings in accent at .35 opacity, draw-in gated by `.motion`) are the owner's signature — keep them, keep them faint. |

## Tokens (defined on `:root`, redefined for dark twice — media query + `[data-theme="dark"]`)

- Surfaces: `--bg` (#F2F5F7 / #0E1821), `--bg-alt` (#E8EDF1 / #122029), `--tile` (#fff / #15222D), `--tile-alt` (#fff / #1A2936)
- Text: `--label` (#16222E / #E4ECF1), `--label-2` (#56687A / #93A6B5), `--label-3` (#5B6D80 / #8597A6) — all ≥4.5:1 on every surface; re-check if you change any of them
- `--sep` hairlines, `--accent`, `--link`, `--nav-bg`
- Motion: `--spring` (smooth, `linear()` with cubic-bezier fallback), `--spring-snappy`, `--dur`
- Always use tokens. A new hex in a rule is a bug.

## Type scale

| Role | Size | Weight | Tracking |
|---|---|---|---|
| Hero H1 (name) | `clamp(2.25rem, 2vw + 1.5rem, 3.25rem)` — owner wants it modest, don't enlarge | 700 | -0.025em |
| H2 section | `clamp(2.25rem, 2.6vw + 1.2rem, 3.5rem)` | 600 | -0.025em |
| Tagline | `clamp(1.125rem, .6vw + 1rem, 1.375rem)` | 500 | -0.01em |
| H3 / tile title | 1.3125rem (21px) | 600 | -0.01em |
| Body | 17px / 1.47, fixed (never fluid) | 400 | -0.01em |
| Eyebrow / nav | 12–14px | 600 / 400 | 0 |

Headings use `text-wrap: balance`; paragraphs `text-wrap: pretty`. Never weight 800.

## Layout & rhythm

- `.wrap` max 980px, gutters `clamp(20px, 4vw, 40px)` + safe-area insets.
- Sections alternate `--bg` / `--bg-alt` (`class="alt"`). **No borders between sections** — the background change is the divider.
- Section padding `clamp(80px, 12vw, 140px)`. One idea per section.
- Tiles: `--tile` on base sections, `--tile-alt` on `.alt` sections, radius 24px, no border, no shadow.
- Bento is used **once** (Beyond the code). Don't add another.
- CTAs: one blue pill (`.btn`) per group; everything else is a `.more` text link with a `›` chevron.

## Motion (all gated)

- `html.motion` is set only when `prefers-reduced-motion: no-preference`; every animation hangs off it, so reduced-motion and no-JS users get the final, fully visible state.
- Reveals: `.reveal` → `.in` via one IntersectionObserver, 60ms stagger, capped at 5.
- One rAF loop (idle-stopping) drives the damped hero scroll-out and the TAG24 scene. Smooth the *read*, never hijack scroll.
- Pinned scene only at `(min-width: 821px) and (min-height: 640px)` with motion allowed; otherwise it renders statically.
- Animate `transform`/`opacity` only. Press feedback: `scale(.97)` with `--spring-snappy`.

## Accessibility bar

44px tap targets in nav and controls · visible `:focus-visible` ring · skip link ·
`prefers-reduced-transparency` → opaque nav · `prefers-contrast: more` → darker labels/separators ·
lightbox is a native `<dialog>` (focus trap + Esc for free). Text contrast ≥ 4.5:1.

## i18n rule (EN/DE)

Every visible string has `data-i18n="key"`; the English text lives in the markup, the
German in the `DE` object at the bottom of `index.html`. Adding text = adding both.
Strings may contain inline HTML (links), they're swapped with `innerHTML`.

## Assets

- `assets/img/imgProfile.jpg` — avatar (960×960).
- `assets/img/tag24-1.png`, `assets/img/tag24-2.png` — TAG24 iPhone screenshots
  (native resolution, e.g. 1179×2556). Shot 1 is shown first; shot 2 cross-fades in
  during the pinned scene. If shot 1 is missing the device frame hides itself and the
  section falls back to text-only (no placeholder, by design).

## Before shipping a change

Run the restraint pass from `apple-design/references/restraint-and-antislop.md`, then
check: both themes, 375px width, EN and DE, reduced motion on, keyboard-only.

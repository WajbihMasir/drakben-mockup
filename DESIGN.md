# DRAKBEN — Landing Page Design Direction

Reference: styles.refero.design/style/13d6fc89 (Factory, "Terminal war room at
midnight"), reviewed 2026-10-07. Structure kept, values adapted to DRAKBEN.
Purpose: security engineers evaluating an autonomous pentest framework —
convince in one screen that it is a real, working instrument.

## Typography

- Display / headings: **Geist** 400 (weight 500 only for emphasis). Authority
  comes from size + negative tracking, never bold. Tracking tightens as size
  grows: -0.025em body, -0.031em heading, -0.040em display.
- Body / UI: **Geist** 400, 16px, line-height 1.5, reading width ≤ 68ch.
- Instrument labels / terminal: **Geist Mono** 12px, UPPERCASE, -0.02em. When a
  user sees Mono, they are looking at a system surface, not a page surface.
- Type scale: 12 / 14 / 16 / 36 / 44 / 72 (display 72, lh 1.0).

## Color roles (Dracula palette mapped to Factory's role structure)

Canvas & surfaces
- Void Canvas `#0a0a0c` — page background, the void everything is measured against
- Carbon Lift `#131316` — raised dark surfaces, nav well, card fills
- Ash Stroke `#2a2a2e` — hairline borders, dividers, ghost outlines
- Graphite `#3a3a40` — mid fills, inactive affordances

Text
- Bone `#f8f8f2` — primary text (Dracula foreground)
- Pale Stone `#a8a8b0` — tertiary text, eyebrows, subdued copy
- Warm Granite `#7d7d85` — muted body / inactive labels
- Chalk `#ffffff` — high-emphasis light fill / max contrast

Actions & state (chromatic color is signal, never decoration)
- Signal Red `#ff5555` — brand accent, live status, negative trend (matches the
  brand's #8B0000 → #ff5555 gradient identity)
- Metric Green `#50fa7b` — positive metric / success
- Cyan `#8be9fd` — informational / links
- Purple `#bd93f9` — secondary accent
- Yellow `#f1fa8c` — warning

Contrast (WCAG AA, normal text): Bone/Canvas ≈ 17:1, PaleStone/Canvas ≈ 8.7:1,
WarmGranite/Canvas ≈ 4.9:1, SignalRed/Canvas ≈ 6.4:1, Green/Canvas ≈ 12:1.

## Layout

- Page max-width 1200px, section gap 96px, card padding 24px, element gap 24px.
- Base spacing unit 8px; scale 8/16/24/32/40/56/80/96/120.
- Border radius: buttons & nav 3px, cards 10px, large panels 20px. Nested
  surfaces stay concentric (outer = inner + padding + border).

## Motion

Base interactions: 0.15–0.2s, `cubic-bezier(0.4, 0, 0.2, 1)` — the feel of a CLI
tool. Color, background, and border transition together so state changes read as
one switch flipping. No spring physics, no parallax.

### Motion system (extended — patterns adapted from RankMySEO)

The *ideas* are adopted from rankmyseo.co (madebyaris.com); the tokens, content,
palette and code are DRAKBEN's own. Easing for entrance motion is
`cubic-bezier(0.22, 1, 0.36, 1)` (`--ease-out`).

| Effect | Technique | Timing |
|---|---|---|
| Scan film (hero) | 3 scenes cross-fade, typed command, staggered rows, risk bar | 10s loop |
| Reveal on scroll | CSS `animation-timeline: view()`, `@supports`-gated | on scroll |
| Sparkline draw | `animation-timeline: view()` on the polyline | on scroll |
| Marquee | translateX −50%, edge fade mask, pause on hover | 40s loop |
| Status pulse | box-shadow ring | 2.4s loop |
| Button arrow | translateX 3px | 0.18s |

### Motion gates (mandatory — never ship motion without them)

1. `@media (prefers-reduced-motion: reduce)` kills every animation and shows the
   film's first scene as a static, still-informative frame.
2. A **Pause animations** button (`aria-pressed`) sets `.motion-paused` on
   `<html>` → `animation-play-state: paused` for everything.
3. Entrance/reveal motion is **progressive enhancement**: content is visible by
   default and only hidden inside `@supports (animation-timeline: view())`, so a
   blocked observer or unsupported browser can never leave content invisible.

## Interaction & accessibility

- Skip link to `#top` (focus-visible only).
- Mobile drawer: `role="dialog"` + `aria-modal`, `inert` when closed, Esc to
  close, Tab focus trap, focus returns to the trigger. Drawer classes are added
  synchronously (no `requestAnimationFrame`) so the transition runs in
  background/throttled tabs.
- Promo bar, film scenes and decorative SVGs are `aria-hidden`; the film keeps a
  descriptive `role="img"` label.
- Stats accordion (`T8`): one row expands (bigger figure, brighter text, red
  circular arrow, description + sparkline), the rest stay dimmed but readable.
  Built as a **progressive enhancement** — without JS every panel is open and
  readable, and the circular arrows are hidden; JS adds `.stats--ready` to
  enable the collapsed mode. Uses `aria-expanded` + `aria-controls` and a
  `grid-template-rows: 0fr → 1fr` transition (0.42s) so the panel height
  animates without measuring it in JS.

## Do

- Earn depth through contrast and spacing, not blur or glow.
- Reserve chromatic color for live data states and status pulses.
- Carry the split: Geist for page copy, Geist Mono for instrument labels.
- Show the product in a windowed terminal/scan frame — the work is the hero.

## Don't

- No soft elevation, heavy shadows, or gradient chrome on cards.
- No bold headings — the flat 400 weight is the signature.
- No decorative accent spread across every control.

## Open decisions (the reference cannot answer these)

- Copy, feature claims, and metrics are DRAKBEN's own (from its README), not the
  reference's.
- The dashboard frame is a *reconstruction* of DRAKBEN's CLI/scan output, not a
  real screenshot; product states beyond the scan pipeline are out of scope.
- Responsive behavior below 360px and RTL are unobserved — handled defensively.

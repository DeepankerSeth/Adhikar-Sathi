# Adhikar Sathi design system

Adhikar Sathi is a mobile-first civic safety tool. The interface must help a person under stress identify the correct next action in seconds.

## Product rules

1. Safety and human help come before documentation or complaints.
2. The first screen presents exactly four pathways, in a fixed order, and all four must be visible without scrolling on a 320×568 screen.
3. Legal detail stays behind clear disclosure controls until requested.
4. No interface may imply that Adhikar Sathi dispatches help, assigns a lawyer, files a case or receives evidence.
5. The page remains usable without remote fonts, images, frameworks, analytics or third-party scripts.

## Visual system

The language is neutral, precise and product-grade — near-white/near-black surfaces, hairline borders, one indigo accent — so the semantic colors (above all, emergency red) are the only loud voices on the page. Two automatic schemes share one token vocabulary (`styles.css` `:root` plus a `prefers-color-scheme: dark` override). Every text/surface pair meets WCAG AA (4.5:1 text, 3:1 UI boundaries) in both schemes; pairs are verified computationally before any palette change ships.

- Canvas `--paper`: `#FCFCFD` / `#0A0B0D`, with a barely-there radial accent glow behind the hero (`--glow`).
- Surfaces: `--card` white/`#121316`, `--soft` `#F4F5F6`/`#1A1C20`, form fields `--field` white/`#101114`; hairlines `--line` `#E4E6EA`/`#23262B`.
- Text: `--ink` `#0E1013`/`#F4F5F7`, `--text` `#24272C`/`#C9CDD3`, `--muted` `#5A6068`/`#959CA6`.
- One accent: indigo `--accent`/`--action` `#3B5BD2` (dark: links `#9CAEFF`, fills `#4A66D6`). Interactive text and filled actions are separate tokens so dark mode can diverge; hover states darken in both schemes.
- Red is reserved for emergency semantics only: the header 112 button and every `tel:112` call card use `--danger`; `--danger-ink` colors emergency text. Pathway coding: red = danger, amber = custody, indigo = legal help, violet = record — each pathway's panel carries its color in the top rule, kicker and number chip.
- The "save the essentials" band is the page's single inverted panel: near-black `--invert-bg` in light mode, an elevated bordered surface in dark mode. No other section gets a colored background.
- Form-field borders (`--field-border`) hold ≥3:1 against the field background; card borders stay hairline-decorative.
- Focus: 3px indigo ring (`--focus`), offset 2px; the inverted band overrides to `--focus-invert` so the ring never drops below 3:1.
- Corner radii: 8px controls, 12px rows, 14px major containers. Shadows are shallow and functional; the dark scheme relies on borders instead. The sticky header is translucent with a backdrop blur where supported (solid otherwise, and under `prefers-reduced-transparency`).
- `color-scheme` metadata keeps native controls, scrollbars and the browser chrome (`theme-color`, one value per scheme) in step.

## Typography

- Everything is set in the device's native UI typeface (SF Pro / Roboto / Segoe) for speed, legibility and zero network cost — no remote or bundled fonts, ever.
- Display headings are weight 650 with tight tracking (h1 −.025em). The scale is deliberately calm — h1 `clamp(1.7rem … 2.5rem)`, h2 `clamp(1.28rem … 1.62rem)` — so the four pathways, not the headline, dominate the first screen.
- Hindi headings are weight 700, line-height 1.3, letter-spacing 0 (negative Latin tracking is never applied to Devanagari); Hindi body line-height is 1.65 so matras never clip. Uppercase kickers reduce letter-spacing in Hindi.
- Body text is at least 16px. Secondary labels are at least 12px and remain high contrast. Telephone numbers are tabular numerals at weight 650.

## Interaction

- Touch targets are at least 44×44px, `touch-action: manipulation` throughout; the toast never intercepts taps (`pointer-events: none`, bottom-center).
- Every interactive element has hover, pressed and keyboard-focus states; transitions are 90–150ms and are suppressed on first paint (`body.ready` gate) and under `prefers-reduced-motion`.
- Call cards carry a phone glyph (inline SVG mask, `currentColor`) so a tappable number reads as "this dials". The selected city chip carries a check glyph so state never relies on color alone.
- All disclosure controls share one affordance: a rotating chevron on `summary` (WebKit default markers suppressed).
- Primary pathways expose `aria-expanded`, Escape closes an open panel, and keyboard focus returns to the trigger.
- The emergency call action lives in the header instead of a fixed bottom bar, so it never covers content on short screens.

## City scoping

- One optional decision: three chips (Delhi, Mumbai, Elsewhere in India) at the end of the hero. National actions (112, 15100) are never city-scoped.
- The static HTML default is the national view; city content appears only after an explicit tap (stored locally, applied on return visits). No location permission, ever.
- Every city-scoped call card carries its city in the label ("Ambulance — Delhi (CATS)"), so screenshots cannot misroute readers in another city.
- `data-city-scope` attributes drive visibility; all variants ship in the HTML so offline use is unaffected.

## Print

- Screen sections are hidden in print; a dedicated `.print-card` section prints instead, containing the bilingual essentials (all cities). "Print this page" must always produce a usable paper card with phone numbers.

## Responsive behavior

- Primary test widths: 320px, 360px, 390px and 430px, plus landscape and desktop.
- The hero is compact by design: label-above chips on mobile, a single reassurance line, no oversized headline. On short portrait screens (≤650px tall) pathway descriptions and the city note hide while labels remain, keeping all four choices within reach.
- On landscape screens, pathways use two columns when space permits.
- Safe-area insets protect header, footer, status messages and controls on notched devices.
- Long Hindi strings, email addresses and telephone numbers must wrap without horizontal overflow at every width above.

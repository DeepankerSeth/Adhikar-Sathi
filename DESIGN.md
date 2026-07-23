# Adhikar Sathi design system

Adhikar Sathi is a mobile-first civic safety tool. The interface must help a person under stress identify the correct next action in seconds.

## Product rules

1. Safety and human help come before documentation or complaints.
2. The first screen presents exactly four pathways, in a fixed order, and all four must be visible without scrolling on a 320×568 screen.
3. Legal detail stays behind clear disclosure controls until requested.
4. No interface may imply that Adhikar Sathi dispatches help, assigns a lawyer, files a case or receives evidence.
5. The page remains usable without remote fonts, images, frameworks, analytics or third-party scripts.

## Visual system

Two automatic schemes share one token vocabulary (`styles.css` `:root` plus a `prefers-color-scheme: dark` override). Every text/surface pair meets WCAG AA (4.5:1 text, 3:1 UI boundaries) in both schemes; pairs are verified computationally before any palette change ships.

- Canvas `--paper`: warm off-white `#F6F3EB` / near-black green `#111714`.
- Surfaces: `--card` `#FFFDF8`/`#181F1B`, `--soft` `#EEEEE7`/`#212A25`, form fields `--field` white/`#131A16`.
- Text: `--ink` `#1C2A24`/`#E9EFE9`, `--text` `#26352F`/`#CFD9D1`, `--muted` `#4C5B54`/`#9FADA4`.
- Interactive text `--accent` (`#14524A`/`#93CBB4`) is a separate token from filled action `--action` (`#14524A`/`#2E7160`) so dark mode can diverge; hover states darken in both schemes.
- Red is reserved for emergency semantics only: `--danger` fills the 112 actions, `--danger-ink` colors emergency text. Amber = custody, blue = legal help, green = record/keep — each pathway's panel carries its color in the top rule, kicker and number chip.
- Form-field borders (`--field-border`) hold ≥3:1 against the field background; card borders stay decorative.
- Focus: 3px ring `--focus` (`#0B6BB0`/`#57A9E8`), offset 2px; dark surfaces override to `--focus-invert` so the ring never drops below 3:1.
- Corner radii: 8px controls, 12px rows, 16px major containers. Shadows are shallow and functional; dark scheme relies on borders instead.
- `color-scheme` metadata keeps native controls, scrollbars and the browser chrome (`theme-color`, one value per scheme) in step.

## Typography

- Body and Hindi: the device's native UI typeface for speed, legibility and zero network cost.
- English display headings: Georgia. The scale is deliberately calm — h1 `clamp(1.6rem … 2.35rem)`, h2 `clamp(1.25rem … 1.6rem)` — so the four pathways, not the headline, dominate the first screen.
- Hindi headings use the native UI typeface at weight 700, line-height 1.3; Hindi body line-height is 1.65 so matras never clip. Uppercase kickers reduce letter-spacing in Hindi.
- Body text is at least 16px. Secondary labels are at least 12px and remain high contrast. Telephone numbers are tabular numerals.

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

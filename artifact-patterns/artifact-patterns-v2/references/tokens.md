# Tokens Reference — Custom Palettes, Brand Colors & Font Pairs

The three built-in palettes (Studio, Crisp, Warm) cover most cases. But when a
user brings a specific brand color, a hex code they love, or wants something
completely their own, the skill needs rules for extending the system — not
abandoning it.

This reference teaches how to build a custom palette from a single seed color,
how to incorporate real brand assets, and how to introduce new font pairings
while staying inside the design system.

---

## Building a Custom Palette from a Seed Color

When a user provides a color — "I want it in purple" or "use #E63946" — don't
just apply that color as the accent and leave everything else as a template
default. Build a proper palette around it.

**Step 1 — Classify the seed color**

First, determine the seed color's properties: is it light or dark? Warm or cool?
Saturated or muted? This classification determines which of the three base
palettes to extend.

- Dark seed (e.g. navy, forest green, deep burgundy) → extend Studio structure
- Light/bright seed (e.g. sky blue, coral, lime) → extend Crisp structure
- Warm/earthy seed (e.g. terracotta, ochre, sage) → extend Warm structure

**Step 2 — Derive the full palette**

From the seed color, derive the other palette values systematically:

*Background:* Take the seed color's hue, push saturation down to 5–8%, push
lightness to 97–98% for light mode or 8–10% for dark mode. This creates a
background that is harmonically related to the accent without competing with it.

*Surface:* For light mode: pure white or the background lightened 1–2%. For
dark mode: background lightened by 8–10 lightness points.

*Border:* For light mode: the background color darkened 8–10%. For dark mode:
the background lightened 10–12%. The border should be visible against both
background and surface without being heavy.

*Text — primary:* For light mode: the background color's hue, full saturation
reduced to 15–20%, lightness at 10–12%. This creates a near-black that is
harmonically related to the palette rather than a pure neutral. For dark mode:
the same approach but lightness at 92–95%.

*Text — secondary (muted):* Midpoint between the border and primary text colors.
Should achieve at least 4.5:1 contrast against the background.

*Accent (the seed color itself):* Verify its contrast ratio against both the
background and the surface. If it falls below 4.5:1 against either, adjust
lightness — lighter for dark backgrounds, darker for light backgrounds — until
it passes. Saturation should be maintained as close to the original as possible.

*Accent hover:* Darken the accent by 8–10% for light-mode hover, lighten by
8–10% for dark-mode hover.

*Accent subtle (for icon containers, tag backgrounds):* The accent color at
12–15% opacity over the surface color.

**Step 3 — Name and document the custom palette**

Give it a name based on the user's context or brand. Define it as CSS custom
properties using the semantic naming system from `darkmode.md`. Every value
is derived, named, and explained — not arbitrary.

---

## Incorporating Real Brand Colors

When a user provides brand colors from an existing identity system:

**Primary brand color → accent token.** The primary brand color goes directly
to `--color-accent`. If the brand has multiple colors, the most action-oriented
one (typically the one used on CTAs in their existing materials) becomes accent.

**Secondary brand color → accent-subtle or surface tint.** Used at low opacity
for backgrounds and containers, not as a second full accent.

**Brand background color (if provided):** If the brand specifies a background
color, use it. If not, derive it from the primary as described above.

**What NOT to do with brand colors:**
- Do not use multiple fully-saturated brand colors at full opacity across the
  same page — they will clash
- Do not introduce colors that aren't in the brand palette even if they "look nice"
- Do not use the brand color as the text color — it is almost certainly the
  wrong contrast level for body text
- Do not apply the full brand palette to every element — one accent color used
  consistently is more impactful than three colors used everywhere

**Working with existing brand typography:**
If the brand uses a specific typeface (Söhne, Canela, GT America, etc.), it
may not be on Google Fonts. In that case:
- Use the closest Google Fonts equivalent as a stand-in (see matching guide below)
- Note in a comment that the font should be swapped for the brand's licensed typeface
- Do not attempt to embed or reference paid fonts without a license

---

## Adding New Font Pairings

The three built-in pairings (Editorial, Modern, Neutral) cover the most common
design intents. Here is how to evaluate and add a new pairing when the context
calls for it.

**Pairing principles:**

*Contrast creates harmony.* A good pairing uses two fonts with clearly different
personalities — a serif paired with a sans-serif, a geometric sans paired with
a humanist sans, a display font paired with a workhorse neutral. Two fonts with
similar personalities compete rather than complement.

*Share a common proportion.* Fonts that pair well often have similar x-heights
or overall proportions. When x-heights are mismatched, one font will feel large
and one small even at the same point size — requiring manual compensation.

*One font does the work, one sets the tone.* The heading font can be expressive
and distinctive. The body font must be invisible — readable at 1rem with good
line spacing, nothing more. Never use an expressive display font for body copy.

**Five additional pairings for specific contexts:**

*Technical / developer tools:*
- Heading: JetBrains Mono (monospaced, but used for headings gives a code-native
  feeling)
- Body: Inter
- Character: precise, functional, signals technical credibility

*Luxury / high-end:*
- Heading: Cormorant Garamond (weight 600, not 400 — thin weights look fragile
  on screen)
- Body: Inter or DM Sans
- Character: elegant, rarefied, maximum contrast between heading and body

*Friendly / startup:*
- Heading: Plus Jakarta Sans (weight 700)
- Body: Plus Jakarta Sans (weight 400) — yes, same font family, different weights
- Character: approachable, modern, cohesive — single-family pairings feel
  tightly designed when the weight range is wide enough

*Editorial / journalism:*
- Heading: Libre Baskerville (weight 700)
- Body: Source Serif 4 (weight 400)
- Character: newspaper credibility, long-form reading optimized, serious

*Geometric / abstract:*
- Heading: Outfit (weight 800)
- Body: DM Sans
- Character: clean, slightly playful, tech-adjacent without feeling like
  another SaaS clone

**Font weight usage rules (universal):**

Never use a weight lighter than 400 for body text — it fails contrast and
readability at body size. Never use weight 400 for headings above H2 — they
will look timid. The standard heading weights are 600, 700, and 800. Use 800
only for display text and when the font was explicitly designed for it (not all
fonts look good at 800).

**Loading only what you need:**

Each weight of a font is a separate download. Every unused weight is wasted
bandwidth. For each font in a pairing, load only:
- 400 (regular body) and 700 (bold headings) for the heading font
- 400 (body), 500 (labels and UI), 600 (button labels) for the body font

For a pairing like Playfair Display + Inter, that is 5 weights total — the
minimum for full typographic control.

---

## Spacing System Extensions

The base spacing token system (xs through 3xl) covers most cases. When a
context requires additional resolution — a dense data table, a compact mobile
UI, a very large hero — extend the scale at the edges:

*Micro (below xs):*
- 2px — for pixel-precise borders, inline icon-to-text gaps
- 4px — for tight inline elements (badge padding, small tag gaps)

These micro values are used only for inline element spacing, never for
section-level layout. If you find yourself using 4px for anything larger
than a badge, the design is too dense — increase the element size or the
surrounding spacing to match the system.

*Macro (above 3xl):*
- 4xl: 12rem — for truly monumental hero sections on large screens
- 5xl: 16rem — for full-viewport-height hero sections with `min-height: 100vh`

Macro values apply only to hero section padding and intentional negative space
moments — never to gaps between content elements.

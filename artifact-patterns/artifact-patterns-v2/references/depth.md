# Depth Reference — Shadows, Blur, Glass & Layering

Depth is what makes UI feel physical rather than printed. It communicates
hierarchy — which elements are closer, which are behind, what can be
interacted with. A flat page with no depth system looks unfinished. A page
with depth applied inconsistently looks amateur. A page with a coherent depth
system looks designed.

The fundamental rule: **light comes from above.** Every shadow, highlight, and
blur decision should be consistent with a single imagined light source sitting
above and slightly in front of the screen.

---

## Shadow Tokens

Define shadows as a scale, not as one-off values. This is a five-level system.
Every shadow in an artifact must come from this scale — no arbitrary values.

**Shadow 1 — Hairline (subtle lift)**
A barely-there shadow for elements that are just slightly raised from the
surface — toggle handles, small chips, avatar rings. Almost invisible on
light palettes, invisible on dark.
Values: `0 1px 2px rgba(0,0,0,0.06), 0 1px 1px rgba(0,0,0,0.04)`

**Shadow 2 — Card (standard elevation)**
The default card shadow. Used for feature cards, testimonial cards, pricing
tiers, and any surface that should read as sitting on the page background.
Values: `0 4px 6px -1px rgba(0,0,0,0.07), 0 2px 4px -1px rgba(0,0,0,0.05)`

**Shadow 3 — Raised (interactive feedback)**
Used for cards on hover (combined with the card shadow for a lift effect),
dropdown menus, and popovers.
Values: `0 10px 15px -3px rgba(0,0,0,0.08), 0 4px 6px -2px rgba(0,0,0,0.04)`

**Shadow 4 — Float (modals, drawers)**
For elements that float above the page layer — modal dialogs, side drawers,
sticky elements that need to appear above scrolling content.
Values: `0 20px 25px -5px rgba(0,0,0,0.10), 0 10px 10px -5px rgba(0,0,0,0.04)`

**Shadow 5 — Deep (maximum elevation)**
Reserved for the highest-layer elements — full-screen overlays, command
palettes, high-priority notification toasts. Use very sparingly.
Values: `0 25px 50px -12px rgba(0,0,0,0.18)`

### Shadow rules by palette

**Studio (dark palette):** Shadows are nearly invisible on dark backgrounds —
the eye can't see a dark shadow on a dark surface. Instead of box-shadow,
use **inner highlight** (a 1px top border or inset box-shadow in a lighter
surface tone) to suggest elevation. Cards on Studio use `border: 1px solid
var(--border)` with a very subtle `box-shadow: inset 0 1px 0 rgba(255,255,255,0.05)`.

**Crisp (light palette):** The full shadow scale is available and effective.
Use Shadow 2 on all cards by default. Shadow 3 on hover. Shadow 4 for
modals. Keep shadow opacity values as given — light UI shadows should be
soft, never dark or dramatic.

**Warm palette:** Use the shadow scale but tint shadows slightly warm —
replace `rgba(0,0,0,...)` with `rgba(28,25,23,...)` (the primary text color)
at the same opacity values. This makes shadows feel warmer and more
integrated with the palette.

---

## Layering (Z-axis System)

Z-index values must be defined as a system, not assigned arbitrarily. Random
z-index values (z-index: 9999) are a sign that the stacking order was never
designed. Use this named scale:

- **Base (z-index: 0):** Page background, decorative SVG shapes behind content
- **Raised (z-index: 10):** Cards, panels, any surface above the page background
- **Sticky (z-index: 100):** Sticky nav bar, floating action buttons
- **Dropdown (z-index: 200):** Nav dropdowns, select menus, tooltips
- **Modal backdrop (z-index: 300):** The dark overlay behind a modal
- **Modal (z-index: 400):** The modal dialog itself
- **Toast (z-index: 500):** Notification toasts, which must appear above everything

Name these as CSS custom properties (`--z-sticky: 100` etc.) and reference
the variable everywhere instead of hardcoding the number. This makes the
stacking order easy to audit and adjust.

---

## Glassmorphism

Glassmorphism creates panels that appear to be frosted glass — slightly
transparent, with a blur effect that lets the background bleed through.
When used correctly it creates striking depth. When overused it creates
visual noise.

**How it works:** The glass effect combines three properties — `background`
with alpha transparency, `backdrop-filter: blur()` for the frosted texture,
and a semi-transparent border that catches the light.

**Glass panel recipe:**
- Background: the surface color at 60–75% opacity (use `rgba` or `color-mix`)
- Backdrop filter: `blur(12px) saturate(180%)` — the saturation boost makes
  colors behind the glass more vivid, increasing the illusion of depth
- Border: `1px solid rgba(255,255,255,0.12)` on dark palettes,
  `1px solid rgba(255,255,255,0.6)` on light palettes
- Border-radius: 12–16px — glass panels always have soft corners

**Where glassmorphism works:**
- Nav bar on scroll (the nav transitions from transparent to glass as the
  user scrolls past the hero — a highly polished effect)
- Floating cards overlaid on a colorful or illustrated hero background
- Modal dialogs that overlay a blurred version of the page beneath them
- Feature callout badges floating over a product screenshot

**Where glassmorphism does NOT work:**
- On a plain white or solid-color background — there is nothing behind the
  glass for the blur to work on; it just looks like a slightly transparent box
- On the Studio (dark) palette without careful tuning — dark glass on dark
  background is invisible
- As the primary card style across an entire page — it should be the
  exception, used for 1–2 hero elements maximum

**Browser support note:** `backdrop-filter` is not supported in Firefox by
default (requires a flag). Always provide a solid fallback background at full
opacity for browsers where backdrop-filter is unavailable.

---

## Highlight & Rim Light

On dark palettes especially, the top edge of raised surfaces should catch the
imaginary light source above. This is a 1px inset highlight — a subtle line
at the top of the element that simulates a light reflection.

Apply as: `box-shadow: inset 0 1px 0 rgba(255,255,255,0.08)`

This works on cards, nav bars, modal dialogs, and any surface that should
appear physically raised. The opacity should be low — 0.06 to 0.10. Higher
than that and it looks like a design accident rather than a light effect.

Paired with a bottom-edge shadow (the actual drop shadow), rim light + shadow
together convincingly simulate a physical card sitting on a table surface.

---

## Gradient Overlays & Backgrounds

Gradients add depth and visual interest to otherwise flat sections. Used
precisely, they create a sense of dimensionality. Overused, they look like
a 2013 web design portfolio.

**Noise texture gradient:** A subtle grain overlay (usually an SVG filter
or a noise texture at 3–5% opacity) on a gradient background kills the
plasticky look of flat color gradients. This is what separates the Stripe/
Linear aesthetic from a generic gradient.

**Radial glow:** A large radial gradient using the accent color at 8–12%
opacity, centered behind the hero headline. Creates a soft halo effect that
draws the eye to the most important text. Works exceptionally well on Studio
(dark) palette. Apply as a pseudo-element behind the text, not on the
background of the section itself, so it stays centered on the content.

**Fade-to-background:** A linear gradient from the surface color to the
background color used to softly terminate a section without a hard edge
or border. Applies at the bottom of sections that otherwise feel too rigidly
bounded. Use transparency, not solid color steps.

**Gradient rules:**
- Maximum 2 colors in any gradient — more creates a rainbow effect
- Both gradient colors must be from the current palette
- Gradient angle: 135° or 180° for most cases — diagonal feels dynamic,
  vertical feels grounded, horizontal feels like a band
- Never use a gradient where a solid color works — gradients should solve
  a problem, not be a stylistic default

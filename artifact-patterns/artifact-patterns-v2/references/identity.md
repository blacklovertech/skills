# Identity Reference — Logos, Wordmarks & Brand Marks

The logo is the first thing people see and remember. In an artifact, it appears in
the nav and footer at minimum. Getting it right — even as a placeholder — sets the
tone for everything that follows.

---

## The Three Logo Types

### Wordmark
The brand name set in a specific typeface, weight, and spacing. Nothing else.
The simplest form, and often the most powerful. Works well for personal portfolios,
freelancers, and early-stage products that haven't yet developed a symbol.

A good wordmark in an artifact uses:
- The heading font from the chosen type pairing (not the body font)
- Font weight 700 (bold) — lighter weights look uncertain
- Letter-spacing of −0.02em to −0.03em — tighter than normal to make it feel
  considered rather than typed
- The primary text color (never the accent — that's for CTAs, not identity)

In the nav, the wordmark sits at font-size 1.125rem. In the footer, it can be
slightly smaller (1rem). The wordmark should never be larger than the nav height.

### Icon mark + wordmark
A small symbol to the left of the name. The symbol reinforces the brand concept
visually and makes the brand more recognizable at small sizes (favicon, app icon).

Designing the icon mark:
- Build it from simple SVG shapes — a letter in a container, an abstract geometric
  symbol, or a simple object related to the brand's domain.
- The icon mark should fit in a square container. Use a consistent width equal to
  about 1.5× the cap height of the wordmark text beside it.
- Gap between icon and wordmark: 0.5rem (sm token).
- The icon mark uses the accent color as fill, or the primary text color — not both.
  Pick one and be consistent.

### Logomark only
The symbol without the name. Used at small sizes where the wordmark would be
illegible, or when the brand is established enough that the symbol alone is
recognizable. In artifacts, use this for favicon references and app icons, but
always include the wordmark in nav and footer.

---

## Constructing a Placeholder Logo

When the user hasn't provided a logo, construct a credible placeholder that fits
their brand context. Never use a generic globe icon or initials in a gray circle —
that reads as "design in progress."

**For a portfolio / personal brand:**
Set the person's name or initials as a wordmark using the Editorial font pairing.
If using initials, set them in a slightly larger size (1.25–1.5rem) with
letter-spacing of 0.05em — spaced out rather than tight, which reads as a
monogram rather than an abbreviation.

**For a product / SaaS:**
Use a geometric icon mark alongside the product name. The icon should abstractly
relate to the product's core function. Examples: an upward arrow for growth tools,
interlocking shapes for collaboration, a simplified chart line for analytics.
Build it in SVG using 2–3 basic shapes maximum.

**For a boutique / warm brand:**
Consider a simple hand-drawn-feeling mark built from SVG paths with slightly
irregular curves — not mechanically perfect. Pair it with the brand name in a
serif (Playfair Display at weight 400 for a lighter, more refined feel).

---

## Logo Placement Rules

**In the nav:**
- Always on the far left
- Vertically centered within the nav height
- Clickable (link to `#` or the page root)
- Never has a background, border, or container box around it — the logo stands alone

**In the footer:**
- Repeat the wordmark (icon mark optional at this size)
- Same far-left position, or centered on mobile
- Can be slightly smaller than the nav version

**On a hero section (optional):**
Some portfolio pages feature a large logomark as a decorative background element
behind the hero text — set at very low opacity (0.05–0.08) and scaled to 300–500px.
This works well on the Studio (dark) palette and creates a sense of depth without
being distracting.

---

## SVG Logo Construction Principles

When building a logo mark as an inline SVG:

- Set the `viewBox` to a square (e.g., `0 0 32 32` or `0 0 48 48`). This makes
  it easy to size by controlling only the `width` and `height` attributes.
- Use `currentColor` for fill and stroke so the logo inherits color from CSS —
  this makes it trivial to invert (dark on light, light on dark) by simply
  changing the parent element's color.
- Keep the path count low: 1–4 paths for a simple mark. Complex SVG paths are
  hard to read as brand marks at small sizes.
- Test at 24px width: if the mark is still legible at that size, it will work
  everywhere. If it's not, simplify it.
- Avoid strokes thinner than 1.5px at the displayed size — they disappear or
  alias badly on lower-resolution screens.

---

## Favicon Reference

Every artifact page should define a favicon — even a placeholder one. A missing
favicon produces a broken icon in the browser tab, which undercuts the polish of
even a great-looking page.

The quickest approach is an SVG favicon defined inline in the document head using
a data URI. Set it to the first letter of the brand name, in the accent color,
on a transparent background. This looks intentional, loads instantly, and requires
no external file.

For a more refined favicon, use a simplified version of the icon mark — just the
core shape, no wordmark, in the accent color. At 32×32px the wordmark is always
illegible anyway.

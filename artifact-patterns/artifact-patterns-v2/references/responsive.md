# Responsive Reference — Breakpoints, Layouts & Mobile Patterns

Every artifact built with this skill must work at mobile width. Not "acceptable at
mobile" — actually designed for it. Start with the mobile layout in mind, then
expand for desktop. A page that breaks at 375px is not a finished artifact.

---

## Breakpoint System

Use three breakpoints. More than three creates fragile layouts; fewer than three
leaves too much unstyled.

- **sm — 640px:** The point where single-column mobile layouts start gaining
  breathing room. Increase padding slightly. Some elements that were stacked
  can begin to align horizontally (e.g., two short stat items in a row).

- **md — 768px:** The primary tablet breakpoint. Two-column grids activate.
  Nav links may re-emerge from the mobile menu. Section padding increases
  to the full desktop values.

- **lg — 1024px:** Full desktop layout. Three-column grids, split heroes,
  max-width container becomes fully active. Most layout decisions resolve here.

Apply breakpoints using CSS media queries with `min-width` (mobile-first means
you style the small state first, then override upward). In Tailwind artifacts,
use the `md:` and `lg:` prefixes.

---

## Column Collapse Rules

Every multi-column layout needs explicit collapse behavior defined. These are
the rules by layout type:

**3-column feature grid:** Collapses to 1 column below md. At md, it can be
either 2 columns (with the third card full-width below) or still 1 column
if the cards are content-heavy. At lg, full 3 columns.

**2-column portfolio grid:** Collapses to 1 column below md. Each card becomes
full-width and stacks vertically. Below md, the preview image maintains its
aspect ratio; the text content flows naturally beneath it.

**Split hero (text + visual):** Collapses to a single centered column below md.
The visual element moves below the text, not above — users should see the
headline and CTA before scrolling to the visual. At mobile width, the visual
can be hidden entirely if it's purely decorative.

**Nav (desktop links):** At mobile (below md), collapse nav links behind a
hamburger button. Only the brand mark and the hamburger icon remain visible.
The mobile menu opens as a full-width dropdown or a slide-in panel — not a
tiny popover in the corner.

**Footer (3-element row):** Collapses to a centered vertical stack below md.
Order: brand mark → social links → copyright. All centered, sm gap between.

---

## Mobile Spacing Adjustments

The desktop spacing system is generous — it's designed for large screens where
content has room to breathe. On mobile, that generosity becomes waste.

**Section padding:** Reduce from xl (4rem) desktop to lg (2.5rem) mobile top
and bottom. Hero sections reduce from 2xl (6rem) to xl (4rem).

**Horizontal padding:** The container uses 1.5rem horizontal padding on desktop.
On mobile, keep this at 1rem (sm token) minimum — never let content touch the
screen edges.

**Gap between grid items:** Desktop gaps of md (1.5rem) reduce to sm (1rem) on
mobile. A 1.5rem gap between stacked full-width cards feels like wasted space;
1rem is appropriate.

**Between heading and content:** md (1.5rem) gap on desktop reduces to sm (1rem)
on mobile. The visual separation is still clear; you just don't need as much.

---

## Fluid Typography

Type sizes should scale fluidly between breakpoints, not jump abruptly. The
display size in particular — 3.5rem on desktop — is too large for a 375px screen.

**Mobile type scale overrides:**
- Display: scales down to 2.25rem on mobile (from 3.5rem desktop)
- H1: scales to 1.75rem on mobile (from 2.25rem)
- H2: stays at 1.375rem on mobile (from 1.5rem) — the change is subtle
- H3 and below: unchanged

Apply these as the base (mobile-first) size, then increase at the lg breakpoint.
For the smoothest result, use CSS `clamp()` on the display and H1 sizes:
display text clamped between 2.25rem and 3.5rem, scaling with viewport width.
This eliminates the abrupt size jump at the breakpoint.

---

## Mobile Navigation

The nav on mobile must be explicitly designed — not just "hidden." A nav that
is simply `display: none` on mobile without a replacement is a broken page.

**Hamburger pattern:**
- The icon is three horizontal lines (or an X when open) using the primary text
  color. Size: 24px. Placed at the far right of the nav bar.
- On tap, a full-width menu panel drops down below the nav bar, or the entire
  nav transforms into a menu overlay.
- Menu items stack vertically with generous tap targets — minimum 48px height
  per item, text at body size (not the small size used for desktop links).
- The mobile menu background is the surface color with full opacity. Never
  transparent or blurred — those feel unstable on small screens.
- The CTA button from the desktop nav appears at the bottom of the mobile menu
  as a full-width button.

**When to NOT use a hamburger:** If there are only 2–3 nav links, they can often
fit in the mobile nav bar at a smaller size. Test at 375px — if they fit without
crowding the brand mark, skip the hamburger. One fewer interaction is always better.

---

## Touch Target Rules

Mobile users tap with fingers, not mouse cursors. Every interactive element must
meet minimum tap target size.

- Minimum tap target: 44×44px (Apple HIG standard)
- Buttons: already large enough if they follow the padding rules in blocks.md
- Nav links: add `padding: 0.5rem 0` if they're too short to tap comfortably
- Icon-only buttons (hamburger, social icons in footer): wrap in a container
  with `padding: 12px` to expand the tap area without enlarging the visual icon
- Cards that are entirely clickable: the entire card surface is the tap target —
  no small "View →" links buried in the card

---

## Image Behavior on Mobile

Images in split layouts go full-width on mobile. The aspect ratio container
maintains its ratio, so a 16:9 image becomes a full-width 16:9 block — which
on a 375px screen is about 211px tall. That's appropriate for a content visual.

For portfolio grid cards at mobile (1-column), the 16:9 preview is full-width
at 375px = about 211px tall, which is enough to read the content clearly. Reduce
to a 4:3 or even a 2:1 aspect ratio on mobile if the preview needs to be shorter
to leave room for the card text without excessive scroll.

Hero background images (if used) should use `background-size: cover` and
`background-position: center` — this ensures the focal point of the image is
visible regardless of screen size. For portraits used as hero backgrounds, use
`background-position: center top` to keep faces visible.

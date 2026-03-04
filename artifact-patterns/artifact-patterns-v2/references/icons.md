# Icons Reference — Systems, Sizing & Usage Rules

Icons are visual shorthand. Used well, they accelerate comprehension. Used poorly,
they decorate without meaning and clutter the interface. Every icon decision should
start with the question: does this icon make the adjacent text faster to understand,
or is it just filling space?

---

## Choosing an Icon System

### Lucide (preferred for React artifacts)
Lucide is available in Claude.ai React artifacts via `lucide-react`. It has a
consistent 2px stroke width, a 24×24 viewBox, and covers most common UI and
concept icons. It is the default choice for feature grids, nav icons, and UI
elements in React artifacts.

Import only the icons you use: `import { Shield, Zap, BarChart2 } from 'lucide-react'`.
Never import the entire library.

Key props to set on every Lucide icon:
- `size` — set explicitly (see sizing rules below). Do not rely on default.
- `strokeWidth` — use 1.5 for large display icons (32px+), 2 for standard UI icons
  (20–24px), 2 for small inline icons. Thinner strokes feel more refined at large
  sizes; thicker strokes ensure legibility at small sizes.
- `color` — set via CSS `color` property on the parent or pass directly. Use palette
  tokens, not raw hex inside the component.

### Inline SVG (preferred for HTML artifacts)
For HTML artifacts without a component library, build icons as inline SVG paths.
Keep them simple — Lucide's source is open and you can reference the path data
for any icon. Set `width`, `height`, `viewBox="0 0 24 24"`, `fill="none"`,
`stroke="currentColor"`, and `stroke-width="2"` on the svg element, then paste
the path inside.

Using `currentColor` means the icon inherits its color from the CSS `color`
property of its parent — making color changes trivial.

### When to use emoji as icons
Only for informal, playful contexts — never in professional landing pages,
product pages, or portfolio work. Emoji render differently across operating
systems and cannot be styled via CSS.

---

## Icon Sizing Rules

Size icons relative to the typography they accompany. Optical consistency matters
more than mathematical consistency — a 20px icon next to 16px text often looks
more balanced than a 16px icon would.

- **Feature grid icons (display):** 32–40px, inside a container box (see below)
- **Nav icons (if used):** 20px, no container box
- **Inline text icons (e.g., bullet replacement):** 16–18px, vertically centered
  with the text baseline
- **Button icons (leading or trailing):** 16–18px, 6–8px gap from the label
- **Footer social icons:** 20px
- **Favicon-reference icons:** 24px viewBox (actual display size defined by context)

---

## The Icon Container Pattern

In feature grids and card layouts, icons should never float directly above text
without a container. A bare icon at the top of a card looks like clip art.

Instead, use an icon container — a small square or rounded square (border-radius
8–10px) that holds the icon. The container:
- Is 48–56px square
- Uses the accent color at 12–15% opacity as background fill
- Has no border (the tinted background is container enough)
- Gives the icon color using the accent color at full opacity

This creates a visual anchor for each card that relates to the palette without
overwhelming the card content.

---

## Matching Icons to Meaning

Icons must earn their place by adding meaning, not just visual texture. When
choosing icons for a feature grid or list, use the most literal, specific icon
available rather than a generic one.

Common mismatches to avoid:
- Using a gear icon for anything customization-related is overused to the point
  of meaninglessness. Prefer a slider icon for settings, a paintbrush for theming,
  or a toggle for on/off configuration.
- Using a rocket for "get started" or "launch" is equally overused. Prefer an
  arrow right, a play icon, or a map pin for destination-oriented CTAs.
- Using a lightbulb for "ideas" or "insights" is a cliché. Prefer a chart with
  an upward trend, a magnifying glass with a chart, or a target icon.
- Using a checkmark for "success" or "done" is fine — this is one case where the
  universal symbol is the right one.

---

## Icon + Text Alignment

Icons placed beside text must be optically aligned, not just vertically centered
by the layout engine.

- For single-line text: align the icon's optical center (usually the geometric
  center of the icon) to the text's cap height midpoint, not the line-height
  midpoint. This usually means a tiny negative top adjustment (1–2px).
- For multi-line text: align the icon to the first line of text (top-aligned),
  not centered to the block.
- Gap between icon and text: 8px (sm token) for inline icons, 12px (between sm
  and md) for larger display icons.

---

## What NOT to Do with Icons

**Don't use icons for decorative pattern fills.** A grid of icons with no
corresponding text reads as wallpaper, not interface.

**Don't mix icon families.** If you use Lucide for the feature grid, use Lucide
everywhere on the page. Mixing Lucide with Heroicons or Material icons produces
a visual inconsistency that most users can't name but everyone notices.

**Don't use color to carry meaning alone.** A red icon and a green icon that
differ only in color are inaccessible for color-blind users. Pair color
differences with shape or label differences.

**Don't animate icons unnecessarily.** A spinning gear on a loading state is
fine. Icons that spin, pulse, or bounce as decoration are distracting.

**Don't use more than one icon in a single button.** A button has one leading
icon or one trailing icon — not both. Leading icons support navigation and
categorical meaning (e.g., a download arrow). Trailing icons suggest outward
navigation (e.g., an external link arrow or a right chevron).

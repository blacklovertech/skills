# Dark Mode Reference — Theme Switching & CSS Variable Architecture

Dark mode is not "invert the colors." It is a fully considered alternate palette
where surfaces, shadows, and contrast behave differently from light mode. Done
badly, dark mode feels like an accessibility afterthought. Done well, it feels
like a premium feature.

This reference covers how to architect the CSS variable system for theme support,
how to build a toggle, and the specific design decisions that make dark mode feel
native rather than bolted on.

---

## CSS Variable Architecture for Theming

The key to supporting both light and dark modes without duplicating code is a
two-layer CSS variable system.

**Layer 1 — Raw color palette:**
Define all raw color values as named variables. These are never used directly
in components — they only exist to be referenced by the semantic layer.

**Layer 2 — Semantic tokens:**
Define variables with meaningful names (what the color is *for*, not what
color it is). Components reference only semantic tokens. Switching themes
means repointing the semantic tokens to different raw values — the component
code never changes.

**Semantic token names (use these consistently):**
- `--color-bg` — page background
- `--color-surface` — card and panel surfaces
- `--color-surface-raised` — elevated surfaces (modals, dropdowns)
- `--color-border` — dividers, card borders
- `--color-text-primary` — body and heading text
- `--color-text-secondary` — muted, supporting text
- `--color-text-inverse` — text on accent-colored backgrounds
- `--color-accent` — primary brand/action color
- `--color-accent-hover` — accent on hover (lighter or darker)
- `--color-accent-subtle` — accent at low opacity for backgrounds
- `--color-focus` — focus ring color (often = accent)
- `--color-error` — error states
- `--color-success` — success states

**Applying the light theme (on `:root` or `[data-theme="light"]`):**
Point each semantic token at the Crisp or Warm palette values.

**Applying the dark theme (on `[data-theme="dark"]` or via media query):**
Repoint semantic tokens at the Studio palette values (or a custom dark variant).

The swap is only in these two theme blocks. Every component, every block, every
animation uses only `var(--color-bg)`, `var(--color-accent)`, etc. — never
raw hex values inside component code.

---

## The `prefers-color-scheme` Media Query

Before building a toggle, respect the user's OS-level preference. Apply the
dark theme automatically when the user's system is in dark mode using:

```
@media (prefers-color-scheme: dark) { ... }
```

Inside this block, repoint the semantic tokens to dark palette values. This
gives users the correct theme on first load without any JavaScript — the page
respects system preference before the toggle code even runs.

The toggle then overrides this preference when the user explicitly chooses.
Store the user's explicit choice in a JavaScript variable or data attribute —
a `[data-theme]` attribute on the `<html>` element is the cleanest approach.

---

## The Dark Mode Toggle

**Anatomy:**
A toggle switch (not a button or checkbox) placed in the nav bar, typically
at the far right beside the CTA or in the top-right corner of the page. It
shows a sun icon (light mode) and a moon icon (dark mode). The active state
is the filled/highlighted icon; the inactive icon uses the muted color.

**Toggle design:**
A pill-shaped container (height 24px, width 44px, border-radius 9999px)
containing a sliding circle (20px diameter) that moves from one end to the
other. In light mode the circle is on the right (sun side), in dark mode it
is on the left (moon side). The container background uses the border color in
light mode and the accent color at 40% opacity in dark mode. The circle is
the surface color.

Alternatively: two icon buttons side by side without the pill mechanism —
the active icon uses the accent color, the inactive icon uses the muted color.
This is simpler to build and equally clear.

**Transition:**
When the theme switches, the entire page transitions with `transition: background
0.25s ease, color 0.2s ease` — declared on the body or root element. This
prevents the jarring instant flash. Only transition background and color, not
shadow or border values — those don't need to be animated.

**JavaScript:**
On toggle click: read the current `data-theme` attribute on `<html>`, swap it
to the opposite value, and store the choice in `localStorage` so it persists
across page loads. On page load: read from localStorage and apply the stored
theme before the page renders (inline script in the `<head>` to avoid flash
of wrong theme).

---

## Dark Mode Design Decisions

These are the specific choices that make dark mode feel designed rather than
derived.

**Elevation inverts:**
On light mode, higher elevation = darker shadow (dark shadow on light surface).
On dark mode, higher elevation = lighter surface (no shadow visible, but the
surface itself gets brighter as it rises). Use these surface brightness levels:

- Background: `#0F0F0F`
- Surface (base card): `#1A1A1A` (+11 lightness)
- Surface raised (modal, dropdown): `#222222` (+5 more)
- Surface overlay (tooltip): `#2A2A2A` (+5 more)

Each level is noticeably lighter than the one below it. This creates visual
depth using brightness rather than shadow.

**Reduce accent saturation slightly:**
Highly saturated colors vibrate against dark backgrounds and cause eye strain.
If using the Crisp palette's blue (`#2563EB`) in a dark context, desaturate
slightly and increase lightness — something closer to `#3B82F6` or `#60A5FA`
reads more comfortably on dark without losing its identity.

**Images need special handling:**
Photographs and full-color images do not need adjustment. But UI screenshots,
diagrams, and SVG illustrations that were designed for light mode will look
harsh on a dark background. Either provide dark-mode variants, invert their
colors via CSS `filter: invert(1) hue-rotate(180deg)` (works for simple
illustrations with neutral tones), or give them a rounded container with
`background: var(--color-surface)` so they sit on a lighter island rather
than directly on the dark background.

**Text contrast adjustments:**
Pure white (`#FFFFFF`) on a near-black (`#0F0F0F`) background has a contrast
ratio of ~21:1 — technically excellent, but perceptually harsh for long reading.
Use a slightly off-white primary text color instead (the Studio palette's
`#F0EDE8` at ~18:1 is the right level — high contrast but not glaring).

**Borders become more important:**
On light mode, card elevation is communicated by shadow. On dark mode, shadows
are invisible. Borders take over this job — they must be slightly more visible
in dark mode than light. Use `#2A2A2A` for the Studio border, which is
distinctly visible against `#1A1A1A` surfaces without being harsh.

---

## System Preference + User Toggle: Priority Order

1. **On first visit:** Check `localStorage` for a stored preference. If found,
   apply it immediately (inline script in `<head>`).
2. **If no stored preference:** Read `prefers-color-scheme` and apply the
   matching theme.
3. **When user toggles:** Apply the new theme, store it in `localStorage`.
4. **Result:** Users who've never set a preference get their OS setting.
   Users who've set a preference in the UI get their chosen setting, even
   if their OS setting changes.

This order means the user's explicit choice always wins, system preference
is the sensible default, and there is no flash of the wrong theme on load.

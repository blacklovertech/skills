# Accessibility Reference — Contrast, Focus, ARIA & Keyboard

An artifact that looks polished but fails basic accessibility is not finished.
Accessibility is not a layer you add at the end — it's built into every palette
choice, every font size, every interaction pattern. This reference covers the
rules that matter most for landing page and portfolio artifacts.

---

## Contrast Ratios

Color contrast determines whether text is readable for people with low vision or
color blindness. The minimum ratios are defined by WCAG (Web Content Accessibility
Guidelines):

- **Normal text (below 18px or 14px bold):** Minimum 4.5:1 ratio against its background
- **Large text (18px+ or 14px+ bold):** Minimum 3:1 ratio
- **UI components and icons:** Minimum 3:1 against adjacent background

### Palette contrast checks

Each palette has been designed with contrast in mind, but verify these combinations:

**Studio (dark palette):**
- Primary text (#F0EDE8) on background (#0F0F0F): ~18:1 — excellent
- Accent (#E8D5B7) on background (#0F0F0F): ~10:1 — passes for all text sizes
- Muted (#6B6560) on background (#0F0F0F): ~4.2:1 — borderline; use only for
  truly secondary content, never for primary readable text
- Muted (#6B6560) on surface (#1A1A1A): ~3.9:1 — fails for small text; increase
  size or use primary text color instead

**Crisp (light palette):**
- Primary text (#111827) on background (#FAFAFA): ~19:1 — excellent
- Accent (#2563EB) on background (#FAFAFA): ~4.7:1 — passes for normal text
- Muted (#6B7280) on background (#FAFAFA): ~4.6:1 — passes, barely
- Accent (#2563EB) on surface (#FFFFFF): ~4.7:1 — passes

**Warm palette:**
- Primary text (#1C1917) on background (#FDF6EE): ~17:1 — excellent
- Accent (#C2410C) on background (#FDF6EE): ~4.8:1 — passes for normal text
- Muted (#78716C) on background (#FDF6EE): ~4.5:1 — passes, verify at small sizes

**Rule:** Never use the muted color for text that is functionally important —
form labels, error messages, navigation links. Muted is for truly decorative or
supplementary copy (timestamps, footnotes, secondary metadata).

---

## Focus States

Every interactive element must have a visible focus state for keyboard users.
The browser default focus ring (blue outline) is functional but visually jarring
against custom palettes. Always override it with a palette-appropriate focus style.

**The focus ring pattern:**
Remove the default outline (`outline: none`) only if you immediately replace it.
Use a 2px solid ring in the accent color with a 2px offset (so the ring sits
just outside the element boundary). This approach is visible, on-brand, and
clearly indicates focus.

Apply this to: links, buttons, input fields, select menus, any element with
`tabindex`, and custom interactive components (tabs, accordion headers, etc.).

**For dark backgrounds:** The accent-colored focus ring works well if the accent
is light (Studio palette's warm sand). If the ring is too subtle on dark, add a
white inner ring (box-shadow inset 0 0 0 1px white, then 0 0 0 3px accent).

**Skip-to-content link:** Add a visually hidden "Skip to main content" link as
the first element in the document body. It becomes visible on focus and allows
keyboard users to bypass the nav and jump directly to the page content. This is
a one-line addition that significantly improves keyboard usability.

---

## ARIA Roles and Attributes

ARIA (Accessible Rich Internet Applications) attributes tell screen readers what
elements do when HTML semantics alone aren't sufficient. For landing pages, the
most important ARIA patterns are:

**Navigation landmark:**
The main nav element should use the HTML `<nav>` element, which implies the
`navigation` landmark role. If there are multiple nav elements on the page (e.g.,
main nav and footer nav), add `aria-label` to distinguish them:
`<nav aria-label="Main navigation">` and `<nav aria-label="Footer navigation">`.

**Mobile menu button:**
The hamburger button needs `aria-expanded` (true when open, false when closed)
and `aria-controls` pointing to the id of the menu panel. This tells screen
reader users whether the menu is currently open.

**Image alt text:**
Every `<img>` element needs an `alt` attribute. For informative images, write a
concise description of what the image shows and why it matters in context. For
decorative images (background shapes, texture patterns), use `alt=""` — an empty
alt attribute tells screen readers to skip the image entirely.

SVGs that convey meaning (icons, diagrams, illustrations) need either `<title>`
element as the first child of the SVG, or `aria-label` on the SVG element itself.
SVGs that are purely decorative use `aria-hidden="true"`.

**Icon buttons:**
A button containing only an icon with no text label needs `aria-label` describing
its action. Example: a close button with an X icon needs `aria-label="Close menu"`.
Never leave icon-only buttons without an accessible name.

**Tabs component:**
The tab container gets `role="tablist"`. Each tab button gets `role="tab"` and
`aria-selected` (true/false). Each content panel gets `role="tabpanel"` and
`aria-labelledby` pointing to the id of its corresponding tab. When a tab is
selected, the content panel it controls becomes visible and others are hidden
via `hidden` attribute or `display: none`.

**Accordion component:**
Each accordion trigger is a `<button>` with `aria-expanded` (true when open) and
`aria-controls` pointing to the id of the content panel. The content panel has
an id that matches the `aria-controls` value. When collapsed, the panel is hidden
via `hidden` attribute.

---

## Semantic HTML

The single most impactful accessibility improvement is using the correct HTML
element for the job. Semantic elements give screen readers structural information
about the page for free.

**Use these elements correctly:**
- `<header>` — the page header containing the nav
- `<nav>` — the navigation landmark
- `<main>` — the primary content of the page (one per page)
- `<section>` — a thematic grouping of content, each with a heading
- `<article>` — self-contained content (a blog post, a project card)
- `<footer>` — the page footer
- `<h1>` through `<h6>` — headings in strict hierarchy (never skip levels; never
  use heading elements for visual size alone — use CSS for that)
- `<button>` — any clickable control that triggers an action (not a link)
- `<a>` — any navigation that goes to a URL (not a button)
- `<ul>` / `<ol>` / `<li>` — any actual list content (feature bullets, nav links)

**Never use:**
- `<div>` or `<span>` for interactive elements — they have no keyboard accessibility
  by default and no semantic meaning for screen readers
- `<h3>` inside a section where an `<h2>` hasn't been used yet
- `<b>` or `<i>` for semantic emphasis — use `<strong>` or `<em>`

---

## Keyboard Navigation

Every interaction on the page must be reachable and operable by keyboard alone.
The tab order should follow the visual reading order: top to bottom, left to right.

**Natural tab order:** Elements receive focus in DOM order. Keep the DOM order
aligned with the visual layout — CSS that visually reorders elements (e.g., `order`
in flexbox, `grid-row` overrides) can create a mismatch between visual order and
tab order. Audit this on any grid layout where items are visually reordered.

**Interactive component keyboard behavior:**
- **Nav links:** Tab to each link, Enter to follow
- **Mobile menu button:** Tab to the button, Enter or Space to open, Escape to close
- **Tabs:** Tab to the tab list, arrow keys (left/right) to move between tabs,
  Enter or Space to activate. Tab moves focus into the active panel.
- **Accordion:** Tab to each header button, Enter or Space to expand/collapse
- **Forms:** Tab through fields in order, Enter to submit (when focused on a
  submit button or single-field form)

**Focus trapping:** Modal dialogs (if used) must trap focus inside them while
open — Tab should cycle through focusable elements within the modal, not escape
to the page behind it. Escape key should close the modal and return focus to
the element that triggered it.

---

## Motion and Animation Accessibility

The `prefers-reduced-motion` media query detects users who have requested reduced
motion at the OS level. This group includes people with vestibular disorders,
epilepsy, and motion sensitivity.

When this preference is set, disable or significantly reduce:
- Entrance animations (fade/translate — these can stay if subtle, but remove
  any translate larger than 5–10px)
- Scroll-triggered animations (remove entirely)
- Lottie animations (pause them, or replace with static final frame)
- Hover transform effects on cards (remove the translateY lift)
- Any animation with looping, spinning, or bouncing

Apply these reductions using a `@media (prefers-reduced-motion: reduce)` block
that overrides the animation and transition properties to `none` or `0ms`.
This is a one-block addition that significantly improves the experience for a
meaningful subset of users.

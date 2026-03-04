# Components Reference — UI Patterns Beyond Blocks

Blocks are page-level sections. Components are the smaller, reusable UI elements
that live inside those blocks. This reference covers the components that appear
most often in landing pages and portfolios and that have specific design rules
worth following.

---

## Badges & Pills

Small inline labels used to tag content, indicate status, or highlight a feature
as new, popular, or categorized.

**Anatomy:** A short text label (1–3 words) inside a small rounded container.
Border-radius should be large enough to fully round the short sides — either
`border-radius: 9999px` (fully pill-shaped) or `border-radius: 6px` (slightly
rounded rectangle, more formal).

**Sizing:** Height 20–24px. Font size 0.6875rem (11px) to 0.75rem (12px).
Font weight 500–600. Letter spacing +0.03em. Uppercase optional — use uppercase
for status/category tags, sentence case for feature labels.

**Color variants within a palette:**
- **Default tag:** Border color background, muted text. For neutral categorization.
- **Accent tag:** Accent color at 12% opacity background, accent color text.
  For highlighted or "featured" labels. Use sparingly — one or two per page max.
- **Inverted tag (on dark sections):** Surface color background, primary text.
  For tags that sit on the accent-colored CTA band.
- **Success tag:** A green tint that maps to the palette's warmth. Use only for
  genuinely positive statuses (active, complete, verified) — not for decoration.

**Placement rules:** Tags sit above a card title (as a category), beside a nav
item (as a "New" callout), or beside a feature title inline. Never stack multiple
tags in the same position — one tag per element.

---

## Stats Strip

A horizontal band of 3–4 key numbers that demonstrate scale, impact, or proof.
Often placed between the hero and the feature grid to anchor the page's credibility
before making specific claims.

**Anatomy:** A full-width section (or constrained to the container) with a row of
stat items. Each stat item has a large number, a short unit or symbol if needed,
and a 2–4 word label below.

**Number formatting:** Large and bold — 2.5rem to 3rem, font weight 700, using
the heading font. The accent color can be used for the numbers if the rest of the
section is on the background color — it makes the stats feel like highlights.
The label uses the muted color at small type size.

**Content rules:** Numbers should be specific and real-seeming, not round. "10,000+"
feels more credible than "Thousands." "98.7% uptime" feels more credible than
"Near-perfect reliability." If writing placeholders, use numbers that would be
appropriate for the product type and stage — a new startup's placeholder should
not claim 10 million users.

**Dividers between stats:** A vertical line using the border color separates
stats in a horizontal strip. On mobile, the divider disappears and stats stack
2×2 or in a single column.

---

## Pricing Table

The most structurally complex block on most product landing pages. Gets its own
component reference because the design decisions are specific.

**Layout:** 2–3 columns. Two tiers (free/paid) for simple products. Three tiers
(starter/pro/enterprise) for complex ones. More than three tiers overwhelms and
signals the product hasn't decided who it's for.

**Anatomy of each tier card:**
- Tier name (H3 size, weight 600)
- Price (display size for the number, body size for the billing period)
- One-sentence value prop for this tier in the muted color
- A divider
- Feature list (checkmarks + short labels, 4–8 items)
- CTA button at the bottom

**The featured tier:** One tier should be visually elevated — typically the middle
or the recommended option. Elevate it by: using the surface color on a page-background
section (or vice versa), adding an "Most popular" badge, and using the accent-filled
primary CTA while other tiers get ghost CTAs.

**Price display rules:** The number should be visually large — use the display or
H1 size. The currency symbol ($ £ €) is half the size of the number, aligned to
the top of the number (superscript). The billing period ("/month", "/year") sits
beside or below the number in the muted color at small size.

**Feature list design:** Each item has a checkmark icon (use the accent color for
included features, the border color for excluded/unavailable features). The label
is body size, primary text for included, muted for excluded. Never use an X mark
for excluded — it's visually aggressive. Use a dash or simply omit the item.

---

## Tabs

Used to switch between related content without navigating to a new page. Common
in feature sections where different user types need different views of the product.

**Design rules:**
- Tab labels sit in a row, separated by 0px gap (they share a bottom border).
- The active tab has a bottom border in the accent color, 2px thick. Inactive
  tabs have the shared bottom border in the border color.
- Tab label text: active tab uses primary text color, weight 500. Inactive tabs
  use the muted color, weight 400. On hover, inactive tabs brighten to primary.
- Tab content panel appears immediately below with a top margin of md (1.5rem).
  No animation needed — instant content swap is fine for tabs.
- On mobile, tabs that don't fit in a row scroll horizontally. Never wrap tab
  labels to two lines — shorten the labels instead.

**When to use tabs vs. an accordion:** Tabs work when the content panels are of
similar size and the user is likely to compare between them. Accordions work when
the content varies in length and the user is looking for specific information in
one section, not comparing across sections.

---

## Accordion / FAQ

A list of questions or topics where each item can expand to reveal its content.
Used for FAQ sections and documentation-style reference sections.

**Anatomy:** A list of items, each with a header row and a content panel. The
header row contains the question/title (body size, weight 500, primary text) and
a toggle indicator on the right (a chevron or plus/minus icon). The content panel
expands below the header with a smooth height transition.

**Design rules:**
- Items are separated by the border color divider (1px). The last item has a
  bottom divider too — the list is contained between two borders.
- Expanded state: the chevron rotates 180° (or the plus becomes a minus). The
  content panel has pt-sm padding before the text begins.
- Only one item open at a time (default behavior for FAQ sections). For
  documentation sections where users compare multiple items, allow multiple
  open simultaneously.
- The content panel uses body size, muted color for the answer text. If answers
  are very short (1 sentence), consider whether an accordion is the right pattern
  — flat text might be more readable.

---

## Social Proof Logos

A horizontal strip of company logos shown as "trusted by" proof. Common below
the hero on product/SaaS pages.

**Design rules:**
- All logos rendered in a single color — either the muted color or the primary
  text color at 40% opacity. This visual consistency matters more than brand
  accuracy: a strip of multi-color logos looks chaotic.
- Logos should be approximately the same visual weight. Adjust the height of
  each logo so they feel balanced, not so all SVGs are the same pixel height
  (a thin wordmark and a chunky icon will look wildly different at the same px height).
- Section label above: "Trusted by teams at" or similar, in the label type style
  (small, uppercase, muted). Not "Our clients include" — that sounds formal and
  self-congratulatory.
- Number of logos: 4–7. Fewer than 4 doesn't feel like social proof. More than 7
  looks like a list, not a curated selection.
- For placeholder logos, use simple recognizable wordmarks constructed from SVG
  text — the company name in a sans-serif, at the muted color. Better than a
  generic "Logo" placeholder rectangle.

---

## Form Elements

Input fields, text areas, and select menus that appear in contact sections,
newsletter signups, and hero lead capture.

**Input field anatomy:** A rounded rectangle with the border color as the stroke
(1px). Background is the surface color. Inside: placeholder text in the muted
color, typed text in the primary color. Height 44px minimum (touch target rule).
Border-radius 8px. On focus, the border changes to the accent color with no
box shadow — clean focus indication without the default browser glow.

**Labels:** Always include a label above the field (not placeholder-only). The
label uses the small type size, weight 500, primary text color. Required fields
get an asterisk in the accent color, not the word "required."

**Error states:** Border changes to a semantic error color (a desaturated red that
fits the palette's warmth level). An error message in the same red appears below
the field in small type. Error messages are specific: "Please enter a valid email
address" not "Invalid input."

**Submit button:** Always the primary button style from the palette. Full-width
inside narrow forms (contact forms, newsletter signups). Auto-width inside wide
or side-by-side layouts.

**The single-field email capture:** The most common form on landing pages — one
email input and a submit button side by side. The input takes ~65% of the width,
the button takes ~35%. On mobile, they stack: input full-width, button full-width
below it.

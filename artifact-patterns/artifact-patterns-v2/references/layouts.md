# Layouts Reference — Bento, Editorial & Advanced Grid Systems

The skill's block system covers standard layouts: equal-column grids, split
hero, centered content. This reference covers the layouts that make a page
genuinely interesting — ones that have visual hierarchy built into the structure
itself, not just the content.

A layout is doing its job when you can remove all the text and still read the
page's hierarchy from the shapes alone.

---

## Bento Grid

The dominant modern layout pattern. Named after the Japanese bento box — a
single tray divided into compartments of different sizes, each holding something
different. Used by Linear, Vercel, Apple, and most high-quality product pages
since 2022.

**The principle:** A CSS grid where cells span different numbers of columns
and rows. Larger cells = more important features. Smaller cells = supporting
details. The grid itself communicates hierarchy before a word is read.

**Standard bento configurations:**

*3-column, 4-row bento (most common):*
- Cell A: spans 2 columns, 2 rows — the hero feature cell, top-left
- Cell B: spans 1 column, 2 rows — a tall narrow cell, top-right
- Cell C: spans 1 column, 1 row — bottom-left
- Cell D: spans 1 column, 1 row — bottom-center  
- Cell E: spans 1 column, 1 row — bottom-right

*2-column bento (simpler, works at any width):*
- Cell A: spans 2 columns, 1 row — full-width feature at top
- Cell B: spans 1 column, 2 rows — tall feature on left
- Cell C: spans 1 column, 1 row — normal cell, right-top
- Cell D: spans 1 column, 1 row — normal cell, right-bottom

**Design rules for bento:**
- Every cell is a card — surface color, border, border-radius 16px minimum
- Gap between cells: 12–16px (less than standard grid gaps — tighter creates
  the "tray" effect)
- Cells do not all need the same content type — mix: one cell has text + icon,
  another has a small graphic, another has a stat, another has a testimonial quote
- The largest cell should have the most visual weight — use it for an illustration,
  an animated graphic, or the primary feature
- On mobile: all cells stack to full-width, in order of size (largest first)
- Never put the same density of content in every cell — variation in content
  density is part of what makes bento feel designed rather than listed

**What each cell can hold:**
- A feature icon + title + 1–2 sentence description (standard)
- A stat (large number + label)
- A short testimonial quote + attribution
- A small SVG illustration or Lottie animation
- A product interface screenshot at small scale
- A simple diagram (3-step flow, a connection diagram)
- A pure typography statement (one bold phrase, no icon)

---

## Editorial / Magazine Layout

Layouts borrowed from print design — asymmetric, text-dominant, with deliberate
tension between elements. Associated with high-end portfolios, editorial agencies,
cultural institutions, and anyone who wants to feel different from the SaaS default.

**Key techniques:**

*Large numeral indexing:*
Section numbers displayed at enormous size (6–10rem, weight 700, accent color
at 15% opacity) positioned absolutely behind the section heading. The number is
a decorative element that creates depth, not informational content. It bleeds
off the edge of the container or sits at the top-left corner of the section.

*Oversized pull quotes:*
A single sentence from the section body, extracted and displayed at 2–3× the
body size, with a left border in the accent color (4px, solid). The quote breaks
the column structure and extends into the margin or across a wider column than
the body text. This creates rhythm and draws the eye to the most important idea.

*Mixed column grids:*
A section that uses a 12-column grid with columns assigned asymmetrically. For
example: the heading occupies columns 1–4, the body text occupies columns 5–10,
columns 11–12 are empty (intentional negative space). This creates a justified
feeling of visual weight without boxing everything into equal halves.

*Staggered card rows:*
Cards in alternating rows are offset vertically — the first card sits at the
top of the row, the second is pushed down by 2–3rem, the third is back at the
top. The offset creates a flowing rhythm rather than a rigid grid.

*Full-bleed type:*
A headline that is sized to exactly fill the container width — using `font-size`
tuned to fill 100% of the line width at a given viewport. This requires viewport
units (vw) or manual tuning. Used sparingly, it creates a commanding typographic
moment. Used for one headline per page maximum.

---

## Asymmetric Split Layouts

The standard split layout is 50/50. Asymmetric splits break this — 60/40, 65/35,
or 70/30. The unequal division creates visual tension and hierarchy that equal
splits lack.

**When to use asymmetric splits:**
- When one side has significantly more content than the other (naturally unequal)
- When the visual element is illustrative and needs less space than the text
- When the text is the product (a writer's portfolio, a content tool) and
  deserves dominant space

**Common asymmetric configurations:**

*65/35 — Content-heavy left:*
Large left column holds headline, body, and CTA. Narrow right column holds
a supporting visual, a stat block, or a floating card with a testimonial.
The right element should be compact — a tall narrow column that holds a wide
element looks squeezed and wrong.

*35/65 — Visual-dominant right:*
Narrow text column on the left, large visual on the right. Works when the visual
is the primary selling point (a product screenshot, an interactive demo, a
portfolio piece). The narrow text column forces tight, punchy copy — a constraint
that usually improves the writing.

*40/60 with overlap:*
The boundary between columns is not a gap — one element overlaps the other by
1–2rem using negative margin or absolute positioning. A card on the left slightly
overlaps the image on the right. This creates depth and breaks the grid in a
controlled way. Use sparingly — once per page maximum.

---

## Sticky Sidebar Layout

A two-column layout where the left column (30–35% width) scrolls with the page
to a point and then sticks in place, while the right column (65–70% width) is
the primary scrolling content. Used for documentation, long-form case studies,
article pages with table of contents, and settings panels.

**Implementation:** The left sidebar container has `position: sticky; top:
[nav-height + md]; height: fit-content`. This means it scrolls normally until
it reaches the defined top offset, then stays fixed while the content beside
it continues scrolling.

**What goes in the sticky sidebar:**
- Table of contents for a long article (links that highlight the current section
  as the user scrolls — requires scroll tracking)
- A summary card for a case study (client, role, timeline, tools)
- A filter panel for a portfolio or blog
- A persistent CTA or contact card

**Design rules:**
- The sidebar and content column are separated by the border color (1px) rather
  than a gap — this reinforces the panel feeling
- Sidebar content is smaller and more compact than main content — label type size
  for navigation items, secondary text for metadata
- On mobile: the sidebar moves to the top of the page (above the main content)
  and loses its sticky behavior

---

## Full-Bleed + Container Hybrid

A layout technique where some elements break out of the container to go
full-width, while others stay constrained. Creates visual drama without
losing readability.

**The technique:**
The page uses a standard max-width container for text content. But certain
elements are allowed to escape: a hero background image, a large illustration,
a section that uses the accent color as full-width background, a quote that
spans the full page width at large type.

In practice: use a full-width section for the background color or image, but
keep text content inside the standard container div within it. For elements
that should genuinely escape (a full-width image caption, an oversized quote),
use negative horizontal margins equal to the container padding to push them to
the edges.

**What should go full-bleed:**
- Section backgrounds (color, gradient, image)
- Horizontal rule / divider moments
- A single oversized typographic statement
- The CTA band section
- A cinematic hero image

**What should stay in the container:**
- All headline and body text
- All cards and grids
- Nav and footer content
- Any content the user needs to read carefully

---

## Z-Pattern & F-Pattern Composition

The Z-pattern and F-pattern describe how users' eyes move across a page.
Designing with these patterns means placing important content where eyes
naturally land.

**Z-Pattern (used for landing pages with clear conversion goal):**
The eye moves: top-left → top-right (the nav bar), then diagonally down to
bottom-left, then across to bottom-right. For a landing page:
- Top-left: brand mark
- Top-right: primary CTA button
- Bottom-left: the most important benefit or proof point
- Bottom-right: the secondary CTA or supporting action

Design the hero so its elements follow this diagonal — a left-aligned headline
that connects to a right-side visual, which then leads down to a left-aligned CTA.

**F-Pattern (used for content-heavy pages, portfolios, blogs):**
The eye reads the first line fully (the headline), then scans the second line
partially (the subtext), then moves down the left edge looking for starting points.
This means:
- Left edges of content are the most read — put the most important word of each
  line at the start, not buried mid-sentence
- Right sides of content are often missed — don't bury CTAs or key info there
- Use visual anchors (icons, numbers, bold lead words) at the left edge of
  list items to intercept the scanning eye and draw it across the full line

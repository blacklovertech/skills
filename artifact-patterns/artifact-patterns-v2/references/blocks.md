# Block Reference — Anatomy & Design Rules

Each block below describes its purpose, what it must contain, and the design
decisions that make it work. When building a block, apply the chosen palette
and type scale from SKILL.md throughout. Think of these as design specs, not
code templates.

---

## Nav

**Purpose:** Establish identity at the top of every page and give users a clear
path to the most important action.

**Anatomy:**
- Left side: the brand mark (name or logo). Should use the primary text color
  at slightly higher weight — not the accent, not a subdued muted tone.
- Center or right: 2–4 navigation links. These use the muted color so they
  recede behind the brand mark. On hover, they brighten to primary text color.
- Far right (optional): a single CTA button using the accent color. This is
  the only place on the nav that should draw attention.

**Design rules:**
- The nav is sticky — it stays at the top on scroll. This means its background
  must be opaque (never transparent on scroll) and it must have a bottom border
  or subtle shadow to separate it from page content.
- Keep it minimal. More than 4 nav links signals a site that doesn't know what
  it wants the user to do. If there are more pages, collapse them.
- On mobile, collapse navigation links behind a menu icon. The brand mark and
  CTA remain visible.
- Height should feel light — around 60–70px total. A bloated nav competes with
  the hero beneath it.

---

## Hero

**Purpose:** The first impression. It must answer three questions instantly:
what is this, who is it for, and what should I do next.

**Two layout variants:**

**Centered hero** — Headline, subtext, and CTA all centered on the page. Works
for personal brands, portfolios, and products with a strong single message.
The headline should fill most of the viewport width (aim for 2–3 lines max).
Subtext sits below at body size, muted color, max ~560px wide so lines don't
stretch too long. CTA buttons sit below that with generous top margin.

**Split hero** — Text on the left, visual element on the right. Use when there's
a meaningful product visual, illustration, or image to show. The left column
carries the hierarchy (eyebrow label → headline → subtext → CTA). The right
column is a placeholder or real visual contained in a rounded surface with a
soft border. Both columns align to center vertically.

**Design rules:**
- The hero headline uses the display type size (3.5rem) with tight tracking.
  It should feel large — if it doesn't command the page, make it bigger.
- The eyebrow label (small uppercase text above the headline) uses the accent
  color. It sets context without being the main message.
- There is one primary CTA and optionally one secondary CTA. Never more.
  The primary uses accent fill. The secondary is ghost/outline.
- Hero section gets the most vertical padding on the page — 2xl minimum top
  and bottom. It needs room to breathe.
- Never start the hero with generic copy like "Welcome to our website." The
  headline should be the most specific, confident thing on the page.

---

## Feature Grid

**Purpose:** Show the value proposition in structured, scannable form. This is
where you make the case, not tell a story.

**Anatomy:**
- Section heading (H2) centered above the grid, or left-aligned for a more
  editorial feel. Optional subtext below it.
- Grid of 3 cards (preferred) or 2 cards. 4+ cards make the section feel
  overwhelming. If there are more features, pick the best 3.
- Each card: an icon or visual mark at top, followed by a short H3 title,
  followed by 2–3 sentences of body copy. Nothing more.

**Design rules:**
- Cards sit on the surface color with a border. They should not have drop shadows
  — flat cards with borders feel more intentional and modern.
- The icon or mark should use the accent color at reduced opacity (around 15–20%)
  as a background fill, so it relates to the palette without overpowering the text.
- Card padding should be generous — md on all sides minimum.
- All cards in a row must be the same height. Use consistent copy length or
  CSS grid to enforce this.
- The grid itself has a top margin from the section heading of lg to xl — the
  heading and grid should not feel fused together.

---

## Testimonial Strip

**Purpose:** Social proof. Give visitors a reason to trust what they just read
in the feature grid.

**Anatomy:**
- An eyebrow label above: "What people say" or similar, in the label style
  (small, uppercase, muted color).
- A grid of 2–3 testimonial cards. Each card contains: the quote text, then
  below it an avatar placeholder circle + name + role/company.

**Design rules:**
- Quotes should be short — one or two punchy sentences. Long quotes lose the
  reader. If the real quote is long, edit it (use an ellipsis or paraphrase if
  placeholder).
- Quote text uses the primary text color at body size. Attribution (name + role)
  uses the muted color at small size.
- The avatar circle is a placeholder using a soft accent tint — not a real image
  unless the user provides one.
- This section typically sits on the page background (not the surface color),
  so the cards use the surface color to provide lift.
- Do not add quotation mark decorations in a large typographic style — it reads
  as dated. The quotation marks are part of the text, not a design element.

---

## CTA Band

**Purpose:** One focused ask, at high contrast, after the user has read enough
to be interested.

**Anatomy:**
- Full-width background using the accent color.
- A headline (H2 size) in the background color (inverted from the rest of the
  page) — this creates the high contrast.
- One sentence of supporting subtext, same inverted color at 70–75% opacity.
- One button: background color fill, accent-colored text. Essentially a dark
  or light button inverted from the rest of the page.

**Design rules:**
- This section has no other purpose than conversion. No features, no quotes,
  no nav — just the ask.
- The headline should state a benefit, not a command. "Build something people
  love" lands better than "Sign up now."
- Padding is xl top and bottom. The section should feel bold, not cramped.
- There is exactly one button. If you're tempted to add a second, the CTA is
  not clear enough — rewrite the headline instead.

---

## Portfolio Grid

**Purpose:** Show work. Let the projects speak before the user reads any copy.

**Anatomy:**
- Section heading ("Selected work" or similar) left-aligned at H2 size.
- 2-column grid of project cards. Each card: a preview area (aspect ratio 16:9)
  on top, then a content area below with category label, project title (H3),
  and a short description.

**Design rules:**
- The preview area is a surface-colored placeholder when no real image is
  available. It should not look broken — give it a border and a subtle label
  like "Preview" in muted text to signal intent.
- Category label above the project title uses the accent color in the label
  style (small, uppercase). It adds color without competing with the title.
- The card has a border and rounded corners (12–16px radius). No drop shadow.
- On hover, the card can lift slightly with a border color change — keep it
  subtle, not dramatic.
- Limit to 4–6 projects. If the user has more, show the best and add a "View
  all work" link below the grid.

---

## Footer

**Purpose:** Close the page, reinforce identity, and provide escape hatches
for people who scrolled all the way down.

**Anatomy:**
- A top border separating it from the last section above.
- Three elements in a row: brand name (left), social/nav links (center or
  right), copyright line (far right or below on mobile).
- Optional: email input for newsletter or contact — place this above the
  link row if used.

**Design rules:**
- The footer should feel light, not heavy. Use muted colors throughout. The
  brand name repeats here but doesn't need to be as prominent as in the nav.
- Links are in the muted color, no underline, underline on hover.
- Copyright uses the small type size. Keep it factual — year and name.
- Padding is md top and bottom — the footer should not dwarf the last section.
- On mobile, stack the three elements vertically with sm gaps between them.

---
name: artifact-patterns
description: >
  Design system and layout patterns for building polished landing pages and portfolio
  artifacts. Use this skill whenever the user asks to build a landing page, portfolio,
  personal site, product page, or any single-page web artifact where visual quality and
  consistency matter. Also trigger for requests like "make it look professional",
  "give it a clean design", "build me a hero section", or any artifact that needs a
  cohesive color, type, and spacing system. Do NOT skip this skill just because the
  request seems simple — even a one-section page benefits from a solid design foundation.
---

# Artifact Patterns — Landing Pages & Portfolios

Every artifact you build with this skill should feel **intentional** — not like
a framework default. The goal is design consistency: one palette, one type
scale, one spatial logic, applied throughout. Read this skill fully before
writing a single line.

---

## 1. Choose a Palette

The palette is the foundation. Pick exactly one. Never mix values across palettes
or introduce colors not listed here unless the user explicitly provides a brand color.

### Studio — dark, creative, portfolio-first
A near-black background with warm sand as the accent. The warmth in the accent
prevents the dark background from feeling cold or generic. Use for creative
portfolios, design showcases, photography, and anything with an editorial feel.

- Background: `#0F0F0F` — near-black, not pure black
- Surface (cards, panels): `#1A1A1A`
- Accent: `#E8D5B7` — warm sand, used sparingly for CTAs and highlights
- Primary text: `#F0EDE8` — slightly warm white, easier on the eyes than pure white
- Secondary text / muted: `#6B6560`
- Borders and dividers: `#2A2A2A`

### Crisp — light, professional, product-ready
A near-white background with a confident blue accent. Clean and trustworthy.
Use for SaaS products, startups, tools, and anything where clarity and
credibility are the message.

- Background: `#FAFAFA`
- Surface: `#FFFFFF`
- Accent: `#2563EB` — a strong, classic blue
- Primary text: `#111827`
- Secondary text / muted: `#6B7280`
- Borders: `#E5E7EB`

### Warm — soft, personal, boutique
An off-white warm background with a terracotta accent. Approachable and
distinctive without being loud. Use for personal sites, freelancers, small
brands, and anyone who wants to feel human, not corporate.

- Background: `#FDF6EE`
- Surface: `#FFFFFF`
- Accent: `#C2410C` — deep terracotta, not orange
- Primary text: `#1C1917`
- Secondary text / muted: `#78716C`
- Borders: `#E7DDD1`

**Palette selection logic:** If the user mentions "dark", "portfolio", "creative",
or "editorial" → Studio. If they mention "product", "SaaS", "clean", or "modern"
→ Crisp. If they mention "personal", "boutique", "warm", or "minimal" → Warm.
If the vibe is unclear, ask one question before proceeding.

---

## 2. Apply the Type System

Typography carries hierarchy. The difference between a polished artifact and a
generic one is almost always in how type is sized, weighted, and spaced.

### Font pairings

Pair headings and body from this list. Never use a system font stack for a
landing page artifact — always load from Google Fonts.

- **Editorial pair** — Playfair Display (headings) + Inter (body). Best for
  portfolios, creative work, anything with a cultured tone.
- **Modern pair** — Space Grotesk (headings) + Inter (body). Best for tech
  products, startups, and tools. Geometric but with personality.
- **Neutral pair** — Inter for everything. Safe, readable, never wrong. Use
  when the content is the focus and type should stay invisible.

Match the pair to the palette: Studio → Editorial, Crisp → Modern, Warm → either
Editorial or Neutral depending on how refined the user's content feels.

### Type scale

Apply these sizes and line heights consistently. The tracking (letter-spacing)
values are especially important — they prevent headings from looking loose.

- Display (hero headline): 3.5rem, line-height 1.1, tracking −0.02em
- H1 (page-level heading): 2.25rem, line-height 1.2, tracking −0.015em
- H2 (section heading): 1.5rem, line-height 1.3, tracking −0.01em
- H3 (card or subsection heading): 1.125rem, line-height 1.4, no tracking
- Body: 1rem, line-height 1.7, no tracking
- Small / caption: 0.875rem, line-height 1.6, tracking +0.01em
- Label / eyebrow: 0.75rem, line-height 1, tracking +0.08em, UPPERCASE

The display and h1 sizes should be noticeably larger than h2. If they look
similar at a glance, the hierarchy has failed — increase the display size or
reduce h2.

---

## 3. Use the Spacing System

Spacing is rhythm. Consistent spacing makes a page feel considered rather than
assembled. Use this token ladder — never pick arbitrary values.

- xs: 0.5rem
- sm: 1rem
- md: 1.5rem
- lg: 2.5rem
- xl: 4rem
- 2xl: 6rem
- 3xl: 9rem

**Container:** All content sits inside a max-width of 1120px, centered, with
1.5rem horizontal padding. Text content columns should be narrower — headlines
max around 760px wide, body copy max around 620px, for comfortable line lengths.

**Section rhythm:** Sections breathe. Minimum top/bottom padding per section is
xl (4rem). The hero gets 2xl (6rem) or more. Between a section heading and its
content, use md to lg. Within card content, use sm to md.

At least one section on every page should have a moment of generous spacing —
a place where the eye can rest. This is intentional negative space, not waste.

---

## 4. Compose from Blocks

Every page is assembled from named blocks. Read `references/blocks.md` for the
anatomy, purpose, and design rules for each one. The available blocks are:

- **Nav** — identity, wayfinding, and optional primary CTA
- **Hero** — the first impression; headline, subtext, and one clear action
- **Feature grid** — structured proof of value, 2 or 3 columns
- **Testimonial strip** — social proof, anchored by real attribution
- **CTA band** — a single focused ask, full-width, high contrast
- **Portfolio grid** — visual project showcase, 2-column
- **Footer** — closing identity, links, legal

A typical landing page flows: Nav → Hero → Feature grid → Testimonial → CTA band
→ Footer. A portfolio flows: Nav → Hero → Portfolio grid → About strip → Footer.
Reorder or omit blocks based on the user's goals, but always start with Nav and
end with Footer.

---

## 4b. Enrich with Visual & Motion References

After composing the blocks, consult these references to add depth, identity, and
motion. Read the relevant file based on what the page needs — you don't need to
read all of them every time.

**`references/identity.md`** — Read when the page needs a logo, wordmark, icon
mark, or favicon. Covers how to construct a placeholder logo that fits the brand
context, SVG logo construction rules, and logo placement in nav and footer.
Read this for every artifact — every page has a logo.

**`references/motion.md`** — Read when the page should feel alive. Covers Lottie
animation setup and placement (hero visuals, feature icons, CTA bands), CSS
entrance animations, scroll-triggered reveals, and hover micro-interactions.
Read this when the user asks for animation, or when a hero feels static.

**`references/graphics.md`** — Read when the page needs an illustration, diagram,
or decorative SVG element. Covers palette-aware SVG patterns, background shapes,
process/step diagrams, architecture charts, and iconographic illustrations.
Read this for any section that has a "visual goes here" placeholder.

**`references/icons.md`** — Read before placing any icons. Covers Lucide vs inline
SVG choice, sizing rules, the icon container pattern, matching icons to meaning,
and alignment with text. Read this whenever building a feature grid or any section
with icons.

**`references/imagery.md`** — Read before placing any images or image placeholders.
Covers aspect ratio selection, placeholder design levels, image styling rules,
mockup frames (browser, phone), and image grid layouts.
Read this for any portfolio grid, hero visual, or testimonial with avatars.

**`references/responsive.md`** — Read before writing any layout code. Covers the
3-breakpoint system, column collapse rules for every layout type, mobile spacing
adjustments, fluid typography, mobile nav pattern, touch targets, and image
behavior at small sizes. Read this for every artifact — all pages must be mobile-aware.

**`references/copy.md`** — Read before writing any placeholder text. Covers
headline formulas (outcome, contrast, direct, question), subtext rules, CTA button
copy patterns, eyebrow label guidelines, feature card copy structure, testimonial
writing, and footer microcopy. Read this whenever the user hasn't provided their
own copy — Lorem Ipsum is never acceptable.

**`references/components.md`** — Read when the page needs UI elements beyond the
named blocks. Covers badges and pills, stats strips, pricing tables, tabs,
accordions, social proof logo strips, and form elements including the single-field
email capture. Read this for any product or SaaS page — these components appear
on nearly every commercial landing page.

**`references/accessibility.md`** — Read before finalizing any artifact. Covers
contrast ratios for all three palettes, focus state patterns, ARIA roles for nav,
mobile menus, tabs, and accordions, semantic HTML rules, keyboard navigation
behavior, and reduced-motion handling. Read this as a final checklist — a polished
page that fails basic accessibility is not done.

**`references/depth.md`** — Read when surfaces need to feel physical and layered.
Covers the five-level shadow token scale, palette-specific shadow rules, the z-index
naming system, glassmorphism recipe and when to use it, rim light highlights, and
gradient overlay techniques. Read this when building modals, sticky navs, glass
panels, or any section that needs tactile depth.

**`references/states.md`** — Read for any interactive artifact. Covers skeleton
loading screens with shimmer, spinner design and button loading states, empty state
anatomy (illustration + headline + CTA), inline and page-level error patterns, toast
and inline success states, and disabled element treatment. Read this whenever the
page has interactive elements, forms, or dynamic content.

**`references/layouts.md`** — Read when standard equal-column grids aren't enough.
Covers bento grid configurations and design rules, editorial magazine layouts,
asymmetric splits (60/40, 65/35, overlap variants), sticky sidebar patterns,
full-bleed + container hybrid, and Z-pattern / F-pattern eye-tracking composition.
Read this when the user wants something that feels designed rather than templated.

**`references/darkmode.md`** — Read when building any artifact that should support
light/dark theme switching. Covers the two-layer CSS variable architecture (raw
colors + semantic tokens), prefers-color-scheme media query, toggle design and
JavaScript logic, elevation-as-brightness for dark surfaces, accent saturation
adjustments, and the priority order for OS preference vs user choice. Read this
whenever a theme toggle is requested or when the page should feel premium.

**`references/tokens.md`** — Read when a user provides a brand color, specific hex,
or requests something beyond the three built-in palettes. Covers building a full
palette from a single seed color, incorporating real brand colors and typography,
five additional font pairings (technical, luxury, friendly, editorial, geometric),
font loading optimization, and spacing system extensions. Read this for any
branded or custom-palette artifact.

**`references/showcase.md`** — Read for every portfolio artifact. Covers the
two-level portfolio architecture (index + case study), portfolio-specific hero
patterns (availability badge, photo treatment), project card anatomy and featured
card rules, full case study structure (brief → problem → process → outcome →
reflection), before/after comparisons, about section writing, skills and tools
display patterns, and contact section design. Read this before any other reference
when the request is a portfolio — it sets the specific intent the other references serve.

**`references/interactions.md`** — Read when the portfolio should feel memorable
and premium. Covers custom cursor design and cursor states, magnetic button effects,
scroll-linked parallax, 3D card tilt with highlight layer, smooth scroll and scroll
progress indicators, and page transition patterns. Read this when the user says
"make it feel special" or for any portfolio targeting a design-industry audience
where the craft of the site is itself a portfolio piece.

**`references/performance.md`** — Read before finalising any artifact with images,
fonts, or animations. Covers font loading strategy, image optimisation (lazy
loading, WebP, explicit dimensions), critical CSS, JavaScript defer vs async,
Lottie performance, perceived performance techniques, and Core Web Vitals targets.
Read this for every portfolio artifact — a slow portfolio loses clients before
a single word is read.

**`references/seo.md`** — Read before outputting the final `<head>` block of any
artifact. Covers the essential meta tag set, all six Open Graph tags, Twitter Card
tags, canonical URLs, JSON-LD structured data (Person schema + CreativeWork),
sitemap guidance, and portfolio-specific copy strategy. Read this for every
artifact — a portfolio that cannot be found or shared properly is not complete.

---

## 5. Quality Checks Before Outputting

Run through this before you output the artifact. These are the most common
failure modes that make a page feel unpolished.

**Color discipline** — Is only one palette in use? Are there any framework-default
grays or blues that crept in? Every color must trace back to a named palette token.

**Hierarchy clarity** — Can you scan the page and immediately see what matters
most? Display text dominates. Subtext recedes. CTAs pop. If everything competes
for attention, nothing has it.

**Spacing generosity** — Did any section end up cramped? Every section needs room
to breathe. Tighten copy before tightening whitespace.

**Type consistency** — Are the right font families applied to headings vs body?
Is the scale followed, or did some headings end up similar in size to body text?

**Copy quality** — No Lorem Ipsum. Write real placeholder copy that fits the
user's context — their name, their field, their product. A page with filler text
is not a finished artifact.

**Mobile awareness** — Columns should stack below ~768px. Display text should
scale down to around 2.25rem on small screens. Horizontal padding stays at sm
(1rem) on mobile so content doesn't touch the edges.

**Button discipline** — One primary button (accent fill) and at most one secondary
(ghost/outline) per section. Never three button styles in the same view.

---

## Implementation Guidance

Declare the palette as CSS custom properties at the top of your style block so
every color is defined once and referenced everywhere. This makes the palette
instantly auditable — if a color appears in the page that isn't in that root
block, it doesn't belong.

For fonts, load from Google Fonts using a preconnect + stylesheet link in the
document head. Load only the weights you'll actually use — typically 400 and 700
for headings, 400 through 600 for body and UI elements.

In React artifacts, define the theme as a constant at the module level and
reference it through inline styles. Do not rely on a framework's default palette
for semantic values like accent, muted, or surface — those must reflect the
chosen palette, not a library default.

**Portfolio completeness** — If this is a portfolio: does it have an availability
badge in the hero? Does every project card have a category label, title, and
one-line description? Is there an about section with a real paragraph (not a list)?
Is there a visible email address in the contact section?

**`<head>` completeness** — Does the page have a title tag, meta description,
viewport meta, theme-color, and all six Open Graph tags including og:image at
1200×630px? Is there a JSON-LD Person schema block? A `<head>` without these
is an unfinished artifact.

**Performance baseline** — Are fonts loaded with preconnect + display=swap?
Do all below-fold images have loading="lazy" and explicit width and height?
Are interaction scripts loaded with defer? Is the hero image loading="eager"?

**Interactions appropriateness** — If custom cursor, parallax, or 3D tilt are
included: are they disabled under prefers-reduced-motion? Is the cursor hidden
on touch devices? Do interactions serve the design or compete with it?

# Graphics Reference — SVG Diagrams & Inline Illustrations

SVGs in landing pages serve two purposes: decorative (background shapes, texture,
personality) and communicative (diagrams, process flows, architecture charts). Know
which one you're making before you start drawing.

---

## When to Use Inline SVG vs Lottie vs Image

Use **inline SVG** when:
- The graphic is a diagram that conveys structure (flow, steps, relationships)
- You need the graphic to respond to the page's palette (use CSS variables inside SVG)
- The graphic is static or only needs simple CSS hover/transition effects
- You want the SVG to be accessible and indexable

Use **Lottie** when:
- The graphic needs to animate in a complex, sequenced way
- You're illustrating a concept that has motion as part of its meaning

Use an **image** when:
- The visual is photographic or has texture that SVG can't represent
- You have a real asset from the user

---

## Palette-Aware SVGs

The most important rule: SVGs on landing pages should use CSS custom properties
for color, not hardcoded hex values. This means the SVG adapts automatically to
whichever palette is active, and you can change the look of the whole page by
changing one root block.

Inside an inline SVG, use `fill="var(--accent)"`, `stroke="var(--border)"`, and
so on. Any color token defined in the root block is available inside the SVG.

For decorative elements that should be subtle, use the palette color with reduced
opacity — either via `opacity` attribute on the element, or `fill-opacity`. A
value of 0.08–0.15 creates a ghosted background shape that adds depth without
competing with content.

---

## Decorative Background Shapes

Background SVGs give sections personality and visual structure without requiring
illustrations. They work especially well behind hero sections and CTA bands.

### Blob / organic shapes
Organic, asymmetric shapes placed in the background of a hero or CTA section.
They should be large (often wider than the container), clipped to the section,
and set at low opacity (0.06–0.12). Use the accent color. The shape itself is
a closed path with rounded curves — think of it as a rounded, irregular polygon.
Place it partially behind text, partially off-screen, so it doesn't frame the
text but instead bleeds off one edge.

### Grid pattern
A repeating dot or line grid as a full-section background texture. Create it as
an SVG pattern element with a `patternUnits="userSpaceOnUse"` definition, then
reference it as a fill on a full-size rect. Use the border color at 0.5 opacity.
The grid should be fine — 24–32px spacing — so it reads as texture, not structure.

### Gradient mesh
A few large overlapping circles with radial gradients, blended via `mix-blend-mode:
screen` (light palettes) or `multiply` (dark palettes). Each circle uses the accent
color at 20–40% opacity. Together they create a soft, ambient glow in the background.
This works especially well on the Studio (dark) palette hero section.

---

## Process / Step Diagrams

Use for showing how something works, a timeline, or a numbered sequence of steps.

### Horizontal step flow
A row of steps connected by arrows or lines. Each step is a circle or rounded square
containing a step number, with a label below and optional short description beneath
that. The connector is a simple horizontal line or arrow between steps.

Design rules:
- Steps use the surface color as fill with an accent border, or the accent color fill
  with the background color for the number text.
- Connectors use the border color. They should not be as visually strong as the steps.
- Keep to 3–5 steps. More than 5 steps in a horizontal row breaks on mobile — switch
  to a vertical timeline for longer sequences.
- The active or current step (if the diagram is interactive) gets the full accent
  color fill. Completed steps can use a muted fill.

### Vertical timeline
Steps stacked vertically with a continuous line on the left side. Each row has the
step indicator (circle) on the left, aligned to the line, with heading and body text
to the right. 

Works better than horizontal flow for 5+ steps, for content-heavy steps, and for
mobile-first designs.

---

## Architecture / Relationship Diagrams

For showing how components connect — APIs, system design, product integrations.

### Node and connector style
Nodes are rounded rectangles (border-radius 8–12px) using the surface color fill,
border color stroke, and primary text for labels. Connectors are straight or
L-shaped lines using the border color, with optional arrowheads (a simple filled
triangle or chevron, not an ornate arrow). 

Keep the arrowhead small relative to the line — an arrowhead that's too large
makes the diagram look like a flowchart from the 1990s.

### Grouping
Use a large rounded rectangle with a dashed stroke at the border color to group
related nodes (e.g., "Frontend", "Backend"). The group label sits in the top-left
corner of the group box in the label type style (small, uppercase, muted).

### Layout principle
Diagrams flow left-to-right (for processes) or top-to-bottom (for hierarchy). Never
lay out nodes randomly — pick one dominant axis and stick to it. Nodes should have
consistent spacing: 40–60px between connected nodes, 80–120px between groups.

---

## Iconographic Illustrations

Simple flat illustrations used as hero visuals or feature section graphics, built
entirely from basic SVG shapes (rect, circle, path, line). These are not complex
illustrations — they're geometric representations of concepts.

### Composition rules
- Use 2–3 colors maximum: the accent color, the surface color, and the border color.
- Every shape has a clear purpose. No decorative shapes that don't mean something.
- Leave negative space. A cramped illustration reads as noise, not image.
- Size to the container: illustrations should fill ~70–80% of their container,
  with breathing room at the edges.

### Common concepts to illustrate geometrically
- **Speed / performance**: diagonal lines suggesting motion, a circle with a tick mark
- **Security / trust**: a shield shape, a lock glyph built from rect + arc
- **Data / analytics**: bars of varying height, a simple line chart with dots
- **Collaboration**: overlapping circles (Venn), person outlines in a row
- **Connectivity**: nodes connected by lines (see architecture diagrams above)
- **Global / scale**: a simplified globe (circle + horizontal ellipses for latitude lines)

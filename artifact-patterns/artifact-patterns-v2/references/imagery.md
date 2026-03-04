# Imagery Reference — Containers, Placeholders & Aspect Ratios

Images in landing page artifacts are usually placeholders — real images come from
the user. But a well-designed placeholder communicates exactly what the real image
should be, preserves the layout, and doesn't make the page look unfinished.

The rules here apply both to placeholder design and to the containers that will
hold real images when the user provides them.

---

## Always Use a Container

Never place an image (or image placeholder) directly in the layout flow without a
container. The container:
- Defines the aspect ratio so the layout doesn't shift when the image loads
- Clips the image to its intended bounds (overflow: hidden)
- Applies border-radius consistently
- Provides the placeholder background when no image is present

The container is a block element with a defined aspect ratio and `overflow: hidden`.
The image (or placeholder content) fills the container completely using `object-fit:
cover` and `width: 100%; height: 100%`.

---

## Aspect Ratio Reference

Use these ratios consistently across sections. Mixing arbitrary ratios on the same
page creates visual chaos.

- **16:9** — Product screenshots, mockup frames, video thumbnails, hero visuals in
  split layouts. The default for anything that represents a screen or application.
- **4:3** — Slightly more square than 16:9. Good for editorial photography, team
  photos, and contexts where the subject matter is centered and important (not
  widescreen cinematic content).
- **1:1** — Profile photos, avatars, testimonial portraits, small card thumbnails.
  Always use for anything that represents a person at small size.
- **3:2** — Photography-oriented ratio. Use for blog post covers, case study heroes,
  and magazine-style editorial images.
- **2:3** — Tall/portrait orientation. Use for mobile screenshot mockups, book covers,
  and poster-style cards.
- **21:9** — Cinematic. Use only for full-width hero banners where the landscape
  dimension is part of the design intention.

---

## Placeholder Design

A placeholder must look intentional. The three levels of placeholder quality:

### Level 1 — Gradient placeholder (best)
A subtle gradient using palette colors. Use the border color as the base and the
surface color as the highlight, or use the accent color at 8–12% opacity as an
overlay. The gradient direction can hint at the expected image composition — for
a landscape photo, horizontal gradient; for a portrait, vertical.

Add a small centered icon (using the appropriate icon from the icons reference)
that indicates the type of content expected: a monitor icon for screenshots, a
person icon for portraits, a image icon for general photography. The icon uses
the muted color at 40% opacity — visible but clearly secondary.

### Level 2 — Tinted block (acceptable)
A flat fill using the border color or surface color, with a centered label text
("Product screenshot", "Team photo", etc.) in the muted color using the small
type size. This is less polished than Level 1 but always acceptable.

### Level 3 — Raw gray box (never)
A plain `background: gray` or `background: #ccc` with nothing inside. This is
the default placeholder and it signals that design decisions were deferred, not
made. Always do at least Level 2.

---

## Image Styling Rules

When real images are provided or referenced:

**Border radius:** Match the container's border radius to the rest of the page's
card and surface elements. If cards use 12px radius, image containers use 12px.
If the page is very geometric, 0–4px is fine. Never use more border radius on an
image than on its neighboring cards — it looks like a different design system.

**Object position:** The default `object-position: center` works for most images.
For portrait photos (testimonials, about sections), use `object-position: center top`
to ensure faces are visible when the image is cropped. For landscape architecture
or product shots, keep center.

**Overlay for text on images:** When text sits on top of an image (e.g., a hero
with a full-bleed background image), apply a gradient overlay between the image
and the text. Dark palettes use a dark-to-transparent gradient from the bottom.
Light palettes use a light-to-transparent gradient. Never just reduce image opacity
to create contrast — it looks washed out.

**Max displayed resolution:** Images wider than 1400px provide no visual benefit
on most screens and slow down the page. If referencing external images via URL,
prefer CDN URLs that support size parameters (e.g., Unsplash's `?w=1400&q=80`).

---

## Mockup Frames

For product/SaaS landing pages, wrapping a screenshot in a device frame elevates
it from "screenshot" to "product". Build simple device frames from SVG or CSS:

**Browser frame:** A rounded rectangle with a small top bar containing three small
circles (the traffic lights) on the left and a faux URL bar in the center. The
content area below holds the screenshot. Use the surface color for the frame, the
border color for the edges, and keep the frame itself thin — 8–12px top bar height,
1px border. The frame should not visually dominate the content inside it.

**Phone frame:** A tall rounded rectangle (border-radius around 40px) with a small
notch or camera cutout at the top. Screen area inside has a slightly inset border.
Use a 9:19.5 aspect ratio for a modern phone shape. Same color rules as the browser
frame — surface color body, border color edges.

**Laptop frame:** Rarely needed in artifacts, but if used: a wide screen section
atop a thin base/keyboard section. The hinge is represented by a thin horizontal
line. Keep it abstract — detailed laptop illustrations compete with the content
they're meant to showcase.

---

## Image Grids and Galleries

For portfolio grids and project showcases, apply masonry or uniform grid layouts:

**Uniform grid (preferred):** All images the same aspect ratio (16:9 or 4:3), equal
gap between cells (use md token — 1.5rem), same border-radius. Clean, structured,
professional. Best for 2 or 3 columns.

**Masonry (use sparingly):** Variable heights, packed like a newspaper layout.
More dynamic, but harder to control and can feel chaotic. Only use if the images
genuinely vary in composition and the variety is part of the design intention.
Limit to contexts where the photography/artwork IS the product (a photo portfolio,
an artist's site).

**Featured + grid (good for 5+ items):** One large image spanning 2 columns on the
left, a stack of 2–3 smaller images on the right. This creates visual hierarchy
within the gallery and highlights the strongest piece of work.

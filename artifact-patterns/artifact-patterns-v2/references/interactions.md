# Interactions Reference — Advanced UI Interactions

Interactions are what make a portfolio page feel *alive* and memorable. A visitor
who says "I don't know what it was, but it just felt different" is responding to
interactions they noticed subconsciously. This reference covers the specific
techniques that separate a standard portfolio from one that gets remembered —
and shared.

Use these deliberately. One or two strong interactions per page is impactful.
Five competing interactions is a carnival. The best portfolios choose their
signature interaction and execute it perfectly.

---

## Custom Cursor

A custom cursor is the highest-impact single interaction change on a portfolio.
It signals immediately that this is a designed experience, not a template.

**The minimal custom cursor:**
Replace the default cursor with a small circle (10–14px diameter) that follows
the mouse position with a slight lag — a smooth lerp (linear interpolation)
rather than snapping. The circle uses the accent color at full opacity with no
fill (stroke only, 1.5px). This small change makes the entire page feel crafted.

**Implementation principle:**
Create a div positioned fixed, pointer-events none, z-index above everything.
On every `mousemove` event, update a target position. On each animation frame,
move the cursor element toward the target by a lerp factor of 0.12–0.15 — this
creates the characteristic smooth lag. Hide the system cursor via `cursor: none`
on the body.

**Cursor states (contextual transformation):**
The cursor should change when hovering different element types — this reinforces
interactivity and adds richness.

- On plain text and background: small circle (default state)
- On links and buttons: the circle expands to 40–48px and fills with the accent
  color at 15% opacity — a soft "magnetised" halo effect
- On project cards: the circle expands further (60–80px) and shows a text label
  in the center: "View" or "Open" at 0.6875rem, weight 500, primary text color
  on a filled accent background — the cursor becomes a label
- On images: the circle inverts (fill becomes background color, stroke becomes
  accent) to stay visible against image content
- On draggable elements: the circle changes to a left-right arrow icon

**Cursor blend mode:**
For an advanced effect, apply `mix-blend-mode: difference` to the cursor element.
This makes the cursor invert whatever color is beneath it — white cursor on dark
sections, dark cursor on light sections, automatically. Works especially well on
the Studio palette and on pages with alternating dark/light sections.

**Mobile:** Disable the custom cursor entirely on touch devices (`pointer: coarse`
media query). Touch interfaces have no cursor — a custom cursor element left active
on mobile becomes a phantom element that captures taps.

---

## Magnetic Button Effect

Buttons and links appear to gently pull toward the cursor as it approaches — a
subtle gravitational effect that makes interactive elements feel tactile and alive.

**How it works:**
On `mousemove`, calculate the distance between the cursor and the center of
each magnetic element. Within a defined radius (typically 80–120px from the
element's center), translate the element toward the cursor position by a fraction
of the distance — 0.3–0.4× the offset. Outside the radius, the element returns
to its natural position with a spring-back transition (250ms, cubic-bezier
easing with slight overshoot: 0.34, 1.56, 0.64, 1).

**What to make magnetic:**
Primary CTA buttons, social link icons in the footer, and the "View project"
label on portfolio cards. Do not make every interactive element magnetic — the
effect loses meaning when applied universally.

**The inner element offset:**
For a richer effect, the button's outer container moves slightly (at 0.3× the
cursor offset) and the inner text/label moves slightly more (at 0.5× the cursor
offset). The differential movement creates a parallax within the button itself —
a subtle sense that the label is floating inside the container.

**Strength calibration:**
The magnetic pull should feel like a gentle suggestion, not a aggressive snap.
If the element visibly lurches toward the cursor, reduce the multiplier. The
effect should be something the user discovers, not something that startles them.

---

## Scroll-Linked Parallax

Elements move at different rates as the user scrolls, creating a sense of depth
and dimension that flat layouts cannot achieve.

**The principle:**
Elements on different visual "layers" scroll at different speeds. Background
elements move slower than foreground elements. This mimics how the physical
world works — distant objects appear to move slower than nearby ones.

**Implementation approach:**
Use the Intersection Observer API combined with a scroll event listener.
Track the scroll position relative to each parallax element's natural position.
Apply a `transform: translateY()` that is a fraction of the scroll offset —
0.1–0.3 for slow background elements, 0.4–0.6 for mid-ground, negative values
for elements that move upward against the scroll direction.

**Where to apply parallax:**

*Hero section:*
The background element (decorative shape, gradient, or illustration) scrolls
at 0.3× speed while the text content scrolls at the normal rate. As the user
scrolls down, the background appears to sink while the text rises — the hero
peels apart.

*Section decorative elements:*
Background SVG shapes, large typography watermarks (like the section number
technique from `layouts.md`), and blob shapes all benefit from subtle parallax
at 0.15–0.2× speed. They drift slowly as the page scrolls, giving sections
a breathing quality.

*Project card hover:*
On hovering a project card, the image inside translates slightly in the
direction of the cursor (left/right, 5–10px range). This is a micro-parallax
that makes the card feel like it has depth — like looking through a window
at the content inside.

**Performance rule:**
Only parallax elements using `transform: translateY()` — never `top`, `margin`,
or `height`. Transforms are GPU-accelerated; layout properties trigger reflow
on every scroll tick, causing jank. Apply `will-change: transform` to parallax
elements to hint the browser to promote them to their own compositing layer.

**Respect `prefers-reduced-motion`:** Disable all parallax under the
`prefers-reduced-motion: reduce` media query. Scroll-linked movement is one
of the most disorienting effects for users with vestibular disorders.

---

## 3D Card Tilt

Project cards and feature cards tilt in 3D space in response to cursor position
within the card — a gyroscopic effect that makes cards feel physical.

**How it works:**
On `mousemove` within a card, calculate the cursor's position relative to the
card's center as a percentage (-0.5 to 0.5 on each axis). Apply `rotateX` and
`rotateY` transforms based on these values — maximum tilt of 8–12° in each
direction. Apply `perspective: 800px` to the card's parent container to establish
the 3D viewing context.

**The highlight layer:**
Add a pseudo-element overlay to the card that moves its radial gradient based on
cursor position — the gradient center follows the cursor, creating a soft
specular highlight as if light is reflecting off a physical surface. The gradient
is white at the center (0% opacity at edges, 10–15% at center) and moves in the
opposite direction from the tilt to simulate a real light source.

**Transition on enter and exit:**
When the cursor enters the card, the tilt begins with a 150ms transition.
When the cursor leaves, the card returns to flat with a 500ms spring-like
transition (cubic-bezier with slight overshoot). The asymmetric timing — fast
in, slow out — is what makes the interaction feel elastic rather than mechanical.

**Apply perspective at the parent level:**
The `perspective` property must be on the parent element, not the card itself.
Applying perspective to the transforming element creates a different, less
realistic effect. The parent perspective creates a shared vanishing point for
all cards in a grid — they all tilt within the same 3D space.

**Limit to card elements:** Do not apply 3D tilt to buttons, nav, or text content.
The effect works on contained, image-led cards. Applied to text blocks it causes
readability issues and feels gimmicky.

---

## Smooth Scroll Behaviour

The default browser scroll behaviour — instant jump between sections — feels
abrupt and breaks the sense of a continuous experience. Smooth scrolling
connects the page's sections into a coherent flow.

**CSS smooth scroll:**
Apply `scroll-behavior: smooth` to the `html` element. This enables smooth
scrolling for all anchor links (`href="#section"`) automatically. It is the
minimum viable smooth scroll implementation — one line of CSS.

**JavaScript scroll with easing (advanced):**
The CSS smooth scroll uses the browser's default easing. For more control,
implement scroll with `requestAnimationFrame` — animate `window.scrollY`
toward the target position using an ease-in-out curve over 600–900ms. This
allows custom easing, callback on completion, and abort on user interruption.

**Scroll progress indicator:**
A thin line (2px height) fixed at the top of the viewport, using the accent
color, that grows from 0% to 100% width as the user scrolls from top to bottom.
Built by tracking `window.scrollY / (document.body.scrollHeight - window.innerHeight)`
and applying that percentage to the indicator's width. This gives users a clear
sense of where they are in the page — useful for long case studies.

**Section snapping (use carefully):**
CSS scroll snap (`scroll-snap-type: y mandatory` on the container, `scroll-snap-align:
start` on each section) creates a full-page slide effect where sections snap into
place on scroll. This is a strong design statement — it works for very intentional,
minimal portfolios with large sections. It completely breaks for content-heavy pages
where sections vary in height. Never apply snap to a portfolio with a case study
or long-form content.

---

## Page Transition

When navigating between pages (index → case study), a transition disguises the
navigation and makes the portfolio feel like a single continuous app rather than
a collection of HTML files.

**Fade transition (simplest):**
The current page fades out over 300ms. The new page fades in. Implemented with
a full-viewport overlay div that fades in on link click, then the new page loads
and the overlay fades out. Works in pure HTML with no framework.

**Clip transition:**
The portfolio card that was clicked expands to fill the viewport — its image
grows outward using `clip-path` animation — and then the case study page appears
beneath it. This requires JavaScript to capture the card's position and animate
the clip path, but creates an elegant spatial sense that the user has "zoomed
into" the project.

**Slide transition (for multi-page React artifacts):**
Content slides left as new content slides in from the right (for forward navigation)
or vice versa. Applied using a wrapper with overflow hidden and translate transforms
on the entering and exiting content panels. Duration 400ms, ease-in-out.

**The most important rule for page transitions:**
The transition must never delay the display of the destination content beyond
500ms. A beautiful 1-second transition that makes the user wait is worse than no
transition at all. Always prioritise speed over spectacle.

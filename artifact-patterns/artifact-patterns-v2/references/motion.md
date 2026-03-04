# Motion Reference — Lottie, Transitions & Scroll Reveals

Motion should reinforce meaning, not decorate. Every animation decision should
answer: does this help the user understand what just happened, or what they should
do next? If the answer is no, remove it.

---

## Lottie Animations

Lottie plays JSON-based vector animations that are lightweight, scalable, and
palette-aware. Use them for hero illustrations, feature icons, empty states, and
loading indicators — anywhere a static image would feel flat but a video would
feel heavy.

### Loading Lottie in an artifact

Load the Lottie Web player via CDN from unpkg or jsdelivr. The script exposes a
`lottie` global. To render an animation, call `lottie.loadAnimation()` with a
target DOM element, the animation data URL or JSON object, and a renderer type.

Always use the `svg` renderer for landing pages — it scales perfectly and stays
crisp at any size. Use `canvas` only when rendering dozens of instances
simultaneously (performance-sensitive cases).

### Key parameters to set

- `loop` — set to true for ambient background animations (hero illustrations,
  loading states). Set to false for one-shot animations (success states,
  onboarding steps) so they stop at the final frame.
- `autoplay` — true for animations visible on load. False for animations that
  should wait for a user interaction or scroll trigger.
- `speed` — default is 1. Subtle ambient animations feel better at 0.6–0.8.
  Micro-interactions on hover can play at 1.2–1.5 for snappiness.
- `renderer` — always `"svg"` for artifacts unless you have a specific reason.

### Where Lottie works well in landing pages

**Hero visual (split layout):** Replace the static placeholder on the right
side of a split hero with a Lottie animation. An abstract, looping illustration
here makes the page feel alive without requiring a real product screenshot.

**Feature section icons:** Each feature card can have a small Lottie icon (around
48–64px) that plays on hover. The animation should match the feature's meaning —
a shield animating for a security feature, a chart growing for an analytics feature.

**CTA band illustration:** A looping abstract animation in the background or beside
the CTA button adds energy to what is otherwise a flat-color section.

**Empty states and loading:** For interactive artifacts with states (dashboards,
tools), use Lottie for empty state illustrations and loading spinners instead of
CSS spinners.

### Where NOT to use Lottie

Do not use Lottie for decorative background motion that spans the full page width —
this becomes visually noisy and competes with content. Do not use it for text
animations — those are handled by CSS transitions. Do not use more than 3–4 Lottie
instances per page or performance degrades on lower-end devices.

### Finding Lottie files

LottieFiles.com has a free library. When selecting animations:
- Choose ones with a color palette that can be overridden or that's neutral (black,
  white, or simple shapes) so they don't clash with the chosen palette.
- Prefer animations under 100KB JSON file size.
- Avoid animations with photographic elements or raster textures — they lose the
  vector advantage.

---

## CSS Entrance Animations

Use CSS transitions for elements that appear on page load or scroll into view.
Keep them subtle — the goal is a sense of polish, not a magic show.

### The entrance pattern

The standard entrance is a fade combined with a small upward translate. The element
starts at opacity 0 and translateY of around 20px, then transitions to opacity 1
and translateY 0. Duration should be 400–600ms. Use an ease-out curve (cubic-bezier
roughly 0.16, 1, 0.3, 1) — elements should arrive quickly and settle gently.

Apply entrance animations to: hero headlines, section headings, cards entering
the viewport, and CTA buttons. Do not apply them to nav, footer, or inline body
text — these feel jarring.

### Stagger for grids

When a feature grid or portfolio grid enters the viewport, stagger each card's
entrance by 80–100ms. Cards animate in left to right, top row first. This creates
a sense of the page assembling in front of the user rather than all appearing at once.

### Scroll-triggered reveals

Use the Intersection Observer API to trigger animations when elements enter the
viewport. The threshold should be around 0.15 — meaning 15% of the element must
be visible before the animation fires. This prevents animations from triggering
before the user can actually see them.

Set a `data-animate` attribute on elements that should reveal on scroll, then
observe all matching elements and toggle an active class when they intersect.
Remove the observer after the animation fires — elements should not re-animate
on scroll back.

### Timing rules

- Fast (100–200ms): hover states, button active states, color transitions
- Medium (300–500ms): entrance animations, panel opens, tab switches
- Slow (600–900ms): hero entrance, page load reveals, modal overlays

Never animate layout properties (width, height, top, left) — these cause reflow
and jank. Only animate transform (translate, scale, rotate) and opacity.

---

## Hover Micro-interactions

Micro-interactions on hover signal interactivity and add tactile quality to flat
interfaces.

**Cards:** On hover, translate the card up by 4–6px and brighten or thin the
border. Duration 200ms, ease-out. Do not add a drop shadow on dark-palette pages
— it reads as a light-mode pattern.

**Buttons:** The primary button should lighten or darken slightly (10%) on hover.
The secondary/ghost button should fill in with a translucent accent tint. Both
should scale up very slightly (1.01–1.02) for a press-ready feel.

**Nav links:** Underline slides in from the left on hover — achieved with a
pseudo-element that scales from scaleX(0) to scaleX(1) with a 200ms transition.
Color shifts from muted to primary text simultaneously.

**Portfolio cards:** The preview image or placeholder should scale slightly inside
its container (scale 1.0 to 1.04) using overflow:hidden on the container to clip
it. This creates a subtle zoom-on-hover effect.

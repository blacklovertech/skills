# Performance Reference — Speed, Loading & Optimisation

A portfolio that loads slowly is a portfolio that loses clients. Page speed is
not a technical detail — it is a first impression. A hiring manager who clicks
a portfolio link and stares at a blank screen for three seconds has already
formed an opinion before a single pixel renders.

This reference covers the specific performance decisions that matter most for
landing pages and portfolio artifacts. These are not premature optimisations —
they are the minimum standard for professional-quality web work.

---

## The Critical Rendering Path

The browser must parse HTML, load CSS, load fonts, and execute JavaScript before
it can paint anything meaningful to the screen. Every resource in that chain
that blocks rendering adds to the time the user stares at nothing.

**The two rules that matter most:**
1. Get the first meaningful paint (the hero section, visible above the fold)
   on screen as fast as possible
2. Everything below the fold (invisible on load) can wait

Everything in this reference flows from these two principles.

---

## Font Loading Strategy

Fonts are one of the most common causes of poor First Contentful Paint on
portfolio sites. Loading a font incorrectly means the user sees either a
blank space where text should be (FOIT — flash of invisible text) or a system
font that then snaps to the web font (FOUT — flash of unstyled text).

**The correct loading pattern:**

Step 1: Preconnect to the font CDN in the `<head>`, before the stylesheet link.
Two tags: one for the connection itself, one for the DNS prefetch. This tells
the browser to establish the connection to Google Fonts (or whichever CDN)
before it even encounters the font request, saving 100–200ms.

Step 2: Load the font stylesheet with `rel="stylesheet"`. Specify only the
weights and styles you will actually use. Loading extra weights costs bandwidth
and adds latency for every unused variant.

Step 3: Add a `<noscript>` fallback with the same stylesheet link, for browsers
with JavaScript disabled.

Step 4: In your CSS, declare a `font-display: swap` equivalent — Google Fonts
does this by default when you append `&display=swap` to the font URL. This tells
the browser to use the system font immediately while the web font loads, then swap
when ready. The swap is visible but brief. It is always better than invisible text.

**Fallback font stack:**
Declare a fallback font stack that matches the proportions of your web font as
closely as possible. For Inter: `Inter, system-ui, -apple-system, sans-serif`.
For Playfair Display: `'Playfair Display', Georgia, serif`. A well-matched
fallback makes the font swap imperceptible — the layout doesn't shift because
the fallback has similar metrics.

**Variable fonts (when available):**
Variable fonts package all weights in a single file, loaded once. Inter and
some other common fonts have variable versions on Google Fonts — use these
when loading more than two weights from the same family. A single variable
font file is almost always smaller than two or more static weight files combined.

---

## Image Optimisation

Images are typically the largest resources on a portfolio page. Unoptimised
images are the single most common cause of slow portfolio sites.

**The lazy loading attribute:**
Every `<img>` element below the fold should have `loading="lazy"`. This is a
single HTML attribute that tells the browser not to fetch the image until it
is near the viewport. Images above the fold (the hero image) should have
`loading="eager"` (the default) — never lazy-load above-the-fold images,
as this delays the most important content.

**Explicit width and height:**
Every `<img>` should have explicit `width` and `height` attributes matching
the image's natural dimensions. This allows the browser to reserve space for
the image before it loads, preventing layout shift (CLS — Cumulative Layout
Shift) when the image arrives. With CSS you can then override these with
`width: 100%; height: auto` for responsive behaviour — the attributes serve
as an aspect ratio hint, not a fixed size.

**The `decoding="async"` attribute:**
Adding `decoding="async"` to images below the fold tells the browser to decode
the image data off the main thread, so it does not block rendering. A small
attribute with a noticeable effect on perceived performance for image-heavy pages.

**Image format priority:**
Use WebP for photographs and complex images. WebP is typically 25–35% smaller
than equivalent JPEG at the same visual quality. Use SVG for icons, logos, and
diagrams — SVG is infinitely scalable and often a fraction of the size of a
raster equivalent. For the `<picture>` element, offer WebP with a JPEG fallback
for browsers without WebP support.

**Sizing images correctly:**
A project card preview displayed at 560px wide should not be loaded at 2400px
wide. Serve images at approximately the size they will be displayed, multiplied
by the device pixel ratio (2× for retina). For a 560px card on a retina display,
serve an image approximately 1120px wide. Loading images at 4× the display size
is pure wasted bandwidth.

---

## Critical CSS

Critical CSS refers to the styles needed to render above-the-fold content —
the nav and hero section. Loading all CSS in a single file means the browser
must download and parse the entire stylesheet before painting anything.

**For artifacts:** Inline the critical styles (nav + hero) in a `<style>` tag
in the document `<head>`. Load the remaining styles (feature grids, testimonials,
footer, components) in a linked stylesheet with `media="print"` that gets changed
to `all` in a JavaScript `onload` handler. This pattern causes only the critical
styles to block rendering — the rest load asynchronously.

**For simpler artifacts:** At minimum, ensure the background color and primary
text color are set in an inline style on the body or in a `<style>` tag in the
`<head>`. This prevents the white flash before CSS loads for dark-palette pages
(which is jarring) and ensures something useful appears on screen immediately.

---

## JavaScript Loading

JavaScript is the most render-blocking resource. A script in the `<head>` without
attributes will halt all parsing until it is downloaded and executed.

**The `defer` attribute:**
Add `defer` to every script tag that is not needed for initial rendering.
Deferred scripts are downloaded in parallel with HTML parsing and executed
after parsing is complete — they never block the initial render. This is the
correct attribute for analytics scripts, interaction libraries (custom cursor,
tilt effects), and Lottie.

**The `async` attribute:**
Use `async` for scripts that are fully independent — third-party widgets, chat
scripts, and analytics tags that do not depend on the page's DOM structure.
Async scripts download in parallel and execute as soon as they are ready, which
may be before or after parsing completes. Do not use async for scripts that
interact with DOM elements — they may execute before those elements exist.

**Lottie performance:**
Load the Lottie library with `defer`. Initialise animations only when their
container enters the viewport (using Intersection Observer), not on page load.
Lottie JSON files can be large — host them locally in the artifact or load from
a CDN, never inline the full JSON in the HTML. For animations not in the initial
viewport, consider loading the JSON file lazily when the container is observed.

---

## Perceived Performance Techniques

Perceived performance is how fast the page *feels* — independent of how fast
it actually is. These techniques improve the subjective experience of speed.

**Skeleton screens for dynamic content:**
If any content loads asynchronously (project data from an API, dynamic stats),
show skeleton screens immediately rather than empty containers or spinners.
See `states.md` for skeleton design. The perception of loading changes completely
when the user can see the shape of the incoming content.

**Progressive content reveal:**
Content that fades and rises into view on scroll (from `motion.md`) makes the
page feel faster because the user's attention is on the newly revealed content
rather than on content that hasn't loaded yet. The animation provides a visual
acknowledgment that something is happening.

**Preload the next project:**
When a user hovers a project card on the portfolio index, preload the case study
page's hero image by creating a hidden `<link rel="preload">` tag for that image.
By the time the user clicks and the page navigates, the hero image is already
in the browser cache. The case study appears to load instantly.

**Instant hover feedback:**
Buttons and interactive elements should respond to hover and active states within
100ms. CSS transitions on `background-color`, `transform`, and `opacity` are GPU-
accelerated and respond instantly. If a button feels sluggish on hover, it is
likely because a layout property (like `border-width` that affects layout) is being
transitioned instead. See `motion.md` for correct hover transition properties.

---

## Core Web Vitals Targets

Core Web Vitals are Google's metrics for page experience. For a portfolio these
matter both for indexability (Google's ranking signal) and for the direct
impression they make on technical hiring managers who know to check them.

**Largest Contentful Paint (LCP) — target under 2.5 seconds:**
The time until the largest visible element (usually the hero image or headline)
finishes rendering. Improve LCP by: preloading the hero image, inlining critical
CSS, and using a fast font loading strategy as described above.

**Cumulative Layout Shift (CLS) — target under 0.1:**
The total of all unexpected layout shifts during load. Caused by: images without
explicit dimensions, fonts loading and reflowing text, and dynamically injected
content. Fix with explicit image dimensions, font-display: swap with matched
fallback fonts, and reserving space for dynamic content with min-height.

**Interaction to Next Paint (INP) — target under 200ms:**
The time from user interaction (click, tap, key press) to the next visual update.
Keep JavaScript execution lean on interaction handlers. Avoid synchronous tasks
longer than 50ms on the main thread during user interactions.

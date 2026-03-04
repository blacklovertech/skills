# Showcase Reference — Portfolio-Specific Patterns

A portfolio is not a landing page with a project grid bolted on. It is a
curated argument for why someone should hire, collaborate with, or commission
the person behind it. Every pattern in this reference serves that argument —
either by showing the quality of the work, explaining the thinking behind it,
or making the person feel like someone worth working with.

---

## Portfolio Architecture

A portfolio lives across two levels: the index (the main page showing all work)
and the case study (a deep-dive on one project). Even a single-page portfolio
should have both levels — a grid of project cards on the main page, and
expandable or linked case study content for each.

**Index page flow:**
Nav → Hero (who you are, one line) → Selected work grid → About strip →
Skills / tools → Contact → Footer

**Case study flow:**
Nav → Project hero (title, role, year, a compelling image) → Overview (the
problem and your role in solving it) → Process (how you worked, key decisions)
→ Outcome (results, what shipped, what you'd do differently) → Next project
link → Footer

The case study is where the portfolio wins or loses. A grid of pretty images
gets a "nice work." A grid of pretty images with case studies that explain
thinking gets a job offer.

---

## Hero for a Personal Portfolio

The portfolio hero is different from a product hero. It is introducing a person,
not pitching a feature. It should feel like a handshake — confident, specific,
and human.

**What the portfolio hero must contain:**
- A greeting or role statement (not "Hi, I'm [Name]" — that is what a resume
  says. Instead: what you do and for whom)
- One sentence that differentiates: not "I'm a designer" but "I design products
  that ship, for teams that move fast"
- Availability status — a small badge ("Available for freelance" / "Open to
  opportunities" / "Currently at [Company]") in the eyebrow label position,
  accent color, before the headline. This is one of the highest-value elements
  on a portfolio — it tells a potential client immediately whether to continue
- A photo or illustrated avatar — optional but humanizing. People hire people,
  not websites. Place it to the right in a split hero at 280–320px diameter,
  circular crop, or as a subtle background element at low opacity
- One CTA: "View my work" (scrolls to the project grid) or "Get in touch"
  (links to contact). Never both at equal weight

**What NOT to include in the hero:**
- A list of tools or skills — that belongs in a dedicated section lower on the page
- Years of experience as the headline — lead with what you do, not a credential
- A generic tagline like "Creative problem solver" or "Passionate designer" —
  these phrases appear on a million portfolios and mean nothing specific

---

## Project Card Design

Project cards in the portfolio grid are the most-clicked elements on the page.
They must communicate enough to earn the click — and communicate it fast.

**Card anatomy:**
- Preview area (top): 16:9 or 4:3 aspect ratio image or video thumbnail. The
  strongest visual from the project. Not a logo, not a mockup frame — the actual
  work, cropped to its most interesting moment
- Category label: the type of work (Brand Identity / Web Design / iOS App /
  Motion / Illustration) in the label type style — small, uppercase, accent color
- Project title: H3 size, primary text, weight 600
- One-line description: what was made and for whom. "E-commerce redesign for a
  sustainable fashion brand." Body size, muted color
- Optional: year, a "View case study →" link in the muted color that brightens
  on hover

**Card hover behaviour:**
Read `motion.md` for the specific hover transitions. The preview image scales
slightly (1.04), the card border brightens to the accent color, and a "View
project" overlay can fade in over the image at 80% opacity with a centered label.
One of these — do not stack all three on the same card.

**Featured project card:**
The first project in the grid (or the strongest piece of work) gets a featured
treatment: it spans the full container width (or 2 columns in a 2-column grid),
uses a taller aspect ratio (21:9 or 16:6), and may include an extra line of
description. This card visually anchors the grid and signals that this is the
work to pay attention to first.

**Number of projects to show:**
4–6 projects is the right range. Fewer than 4 suggests a thin portfolio. More
than 6 dilutes the curation signal — showing 12 projects suggests you don't
know which ones are your best. If the work exists in volume, filter it: show
the best 4–6 on the index, let a "View all work" link lead to the full archive.

---

## Case Study Anatomy

A case study is a story with three acts: the problem, how you solved it, and
what resulted. Every section below serves one of those three acts.

**Project hero:**
Full-width or contained hero with the project title (H1), role and year (label
style, muted color, in a row), and a large, high-quality project image. The
image should be the most visually striking deliverable from the project. If it
is a digital product, show the interface in a device frame (see `imagery.md`).
If it is brand work, show it applied in context (not just the logo on a white
background). Background: either the project's own brand color at low saturation,
or the page's standard surface color.

**Project overview (the brief):**
A compact metadata block above the narrative sets context fast. Use a 3–4 column
mini-grid with label + value pairs: Client (or "Personal project"), Role, Timeline,
Tools/stack. Label style for the headers, body text for the values. Below this
block: 2–3 sentences of narrative context — what the client needed, what the
challenge was, why it was interesting or difficult.

**The problem statement:**
A single, clearly stated problem. Not background information — the specific,
concrete problem that the project existed to solve. Format it as a pull quote
or visually elevated paragraph so it stands apart from surrounding text. This
is the most important sentence in the case study: if the reader doesn't
understand the problem, they can't evaluate how well you solved it.

**Process documentation:**
The most differentiating section — and the one most portfolios skip or do
poorly. Show how you thought, not just what you made.

What to include: sketches, wireframes, or early explorations (even rough ones —
especially rough ones, because they prove the work was genuinely designed rather
than output from a template), key decisions and why you made them, iterations
and what changed between versions, any constraints that shaped the outcome.

Layout for process: an alternating left/right image + text pattern works well
for sequential steps. A timeline (see `graphics.md`) works for projects with
clear phases. Side-by-side comparison (before/after) works for redesign projects.

**Before / After comparisons:**
For redesigns, show what existed before and what you replaced it with. Use a
side-by-side layout at equal widths, or a draggable slider if the artifact is
interactive. Label each side clearly ("Before" / "After") in the label style.
The before image should never be apologised for — it exists to make the after
look better. Show it neutrally.

**Outcome section:**
What shipped. Metrics if you have them (and be specific: "increased conversion
by 23%" not "improved conversion significantly"). User feedback or client quotes.
What launched vs. what was descoped and why. What you'd do differently with more
time or information.

This section is often omitted from portfolios because results are hard to
quantify. That omission is a mistake — even qualitative outcomes ("the client
launched on time and renewed for a second project") are more convincing than
silence.

**Reflection:**
One short paragraph at the end: what you learned, what surprised you, what you'd
approach differently. This is the most human section of a case study and the one
that most distinguishes a thoughtful designer from a production artist. Keep it
honest — claiming every project was a perfect success is less convincing than
acknowledging a real constraint you navigated.

**Navigation between projects:**
At the bottom of every case study, show the next project (and optionally the
previous one) as a large card with the project image and title. This keeps
the viewer in the portfolio rather than bouncing back to the index for each piece.

---

## About Section

The about section is not a resume. It is a paragraph about who you are as a
practitioner — how you think, what you care about, what kind of work energises
you. It should sound like a person, not a LinkedIn summary.

**Length:** 2–4 sentences for an inline about strip on the index page. A full
about page (if the portfolio warrants one) can run 3–4 paragraphs.

**Pairing with a photo:**
The about strip on the index page pairs a portrait photo with the text. The
photo sits in a 1:1 or 3:4 container, circular or soft-rounded corners. It
should be a real photo, not a professional headshot — natural light, not a studio
background. Warmth and approachability outweigh formality for most portfolio
contexts.

**What to cover:**
- What you do and how you approach it (craft statement, not job title)
- Where you are based and the kind of work you take on
- One thing outside work that informs your design perspective — optional but
  humanizing

**What to avoid:**
- Starting with "I am a passionate [role] who loves to..." — everyone writes
  this. Start with what makes your work or perspective specific
- Listing every tool you've used — that belongs in the skills section
- The word "leverage" in any context

---

## Skills & Tools Display

A skills section communicates capability at a glance. It should be scannable
in under five seconds.

**Two approaches:**

*Grouped list (clear and functional):*
Categories as label-style headings (Design, Development, Strategy) with skills
listed below in a compact row of pill badges. Pills use the border color as
background, primary text. Active or primary skills get the accent-subtle
background. Keep each category to 4–6 items — long lists signal inability to
prioritize.

*Proficiency-implied list (more confident):*
Skip the proficiency bars (percentage bars for skills are meaningless and look
outdated). Instead, lead with your primary skills at the largest size and
secondary skills at a smaller size — visual hierarchy communicates priority
without a fake percentage.

**Tools vs. skills:**
Separate these. Skills are transferable capabilities (UI design, user research,
motion design, front-end development). Tools are current implementations (Figma,
Framer, Webflow, React). A hiring manager wants to see skills first, tools second.
Tools change; skills compound.

---

## Contact Section

The contact section on a portfolio has one job: reduce the friction between
"I want to work with this person" and "I have sent them a message."

**What it must have:**
- A direct email address, displayed as clickable text (not buried in a form).
  People trust a visible email address more than a contact form — it signals
  that a real person will read the message
- A response time expectation: "I typically reply within 48 hours" in the
  muted color, small size. This small detail converts significantly — it tells
  the sender the message won't disappear into a void
- Links to professional profiles: LinkedIn, GitHub, Dribbble, Behance — whichever
  are relevant and actively maintained. Dead or empty profiles are worse than
  no links at all

**Optional form:**
A contact form is acceptable alongside a visible email address. Never instead of
it. The form fields: name, email, message (textarea), subject optional.
See `components.md` for form element design rules.

**Availability status (repeat from hero):**
Repeat the availability badge from the hero in the contact section. When a
potential client scrolls to contact, they want immediate confirmation that
reaching out is worthwhile.

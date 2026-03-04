# SEO Reference — Meta Tags, Open Graph & Discoverability

A portfolio that cannot be found is a portfolio that does not exist — to Google,
to recruiters searching LinkedIn, to clients who can't remember the URL and try
to search for the name. And a portfolio link shared on social media that renders
without a preview image looks broken next to every other link in the feed.

This reference covers the specific markup that makes a portfolio discoverable,
shareable, and professionally presented wherever it appears.

---

## The Essential `<head>` Block

Every artifact page should include this set of meta tags. These are the minimum
viable SEO and social sharing setup — none of them are optional for professional work.

**Title tag:**
The page title appears in browser tabs, search results, and as the default text
when the page is bookmarked. For a portfolio index: "[First Name Last Name] —
[Role] & [Secondary Role]" — for example "Alex Kim — Product Designer & Creative
Director." For a case study: "[Project Name] — [First Name Last Name]" so the
person's name is always present. Maximum 60 characters before search engines
truncate it.

**Meta description:**
The sentence Google shows beneath the page title in search results. It does not
directly affect ranking but it determines whether someone clicks. For a portfolio:
one sentence about the person's specialty and the type of work they do. For a
case study: one sentence about the project and its outcome. Maximum 155 characters.
Write it as a reason to click, not a summary of what the page contains.

**Viewport meta tag:**
`<meta name="viewport" content="width=device-width, initial-scale=1">` is required
for responsive behaviour on mobile devices. Without it, mobile browsers render the
page at desktop width and scale it down — undoing all responsive layout work.

**Charset declaration:**
`<meta charset="UTF-8">` must be the first tag in the `<head>` — before any other
tags — so the browser knows how to decode the document before reading anything else.

**Theme color:**
`<meta name="theme-color" content="[bg-color]">` sets the browser chrome color on
mobile (the address bar, tab bar). Set it to the page's background color from the
chosen palette. Studio palette: `#0F0F0F`. Crisp: `#FAFAFA`. Warm: `#FDF6EE`.
A small detail that makes the page feel fully considered on mobile.

---

## Open Graph (Social Sharing)

Open Graph tags control how the page appears when shared on LinkedIn, Twitter/X,
Slack, iMessage, WhatsApp, and any other platform that generates link previews.
A portfolio link shared without OG tags shows only a URL — no image, no title,
no description. This looks broken and unprofessional.

**The six required OG tags:**

`og:title` — The title as it appears in the social preview card. Can be slightly
longer and more expressive than the HTML title tag — up to 70 characters. For
a portfolio: the person's name and role. For a case study: the project name with
a short descriptor.

`og:description` — The description beneath the title in the preview card. 2–3
sentences. More conversational than the meta description since it appears in a
social context. What the person does, who they work with, what kind of projects
they take on.

`og:image` — The image that appears in the preview card. This is the most
important OG tag. A portfolio without an og:image has a grey box where the image
should be — on LinkedIn and Slack this looks like a broken link.

Dimensions: 1200×630px is the standard (1.91:1 ratio). This displays correctly
on all major platforms. Content: the most striking visual from the portfolio (for
the index) or the hero project image (for a case study). For artifacts without
real images, create a designed og:image using the palette — the person's name
in the heading font, their role below it, the accent color, the logo — a typographic
card that looks intentional rather than absent.

`og:url` — The canonical URL of the page. Use the full URL including `https://`.

`og:type` — `"website"` for the portfolio index. `"article"` for case study pages.

`og:site_name` — The portfolio's name. Usually the person's name: "Alex Kim Portfolio"
or just "Alex Kim."

**Twitter/X Card tags:**
Add Twitter-specific tags alongside OG tags. Twitter uses OG tags as fallback but
its own tags take priority on the platform.

`twitter:card` — `"summary_large_image"` for a large image preview (the standard
for portfolios). `"summary"` for a small thumbnail — rarely preferable.

`twitter:title`, `twitter:description`, `twitter:image` — Mirror the OG values.
Twitter's image is the same 1200×630px format.

`twitter:creator` — The person's Twitter/X handle, if they have one:
`@alexkim_design`.

---

## Canonical URL

`<link rel="canonical" href="[full-URL]">` tells search engines the definitive
URL for a page when the same content might be accessible at multiple URLs (http
vs https, with or without trailing slash, with or without www). For a portfolio,
this is a minor concern — but including it is correct practice and signals
technical awareness to anyone who reads the page source.

---

## Structured Data (JSON-LD)

Structured data is machine-readable information embedded in the page that helps
Google understand what the page is about and display rich results in search.
For a portfolio, two schema types are relevant.

**Person schema (for the portfolio index):**
A JSON-LD block in a `<script type="application/ld+json">` tag in the `<head>`
describing the portfolio owner as a Person entity. Include: name, job title,
url (the portfolio URL), sameAs (links to LinkedIn, GitHub, Twitter — these help
Google associate the portfolio with the person's wider web presence), and
optionally email and location.

The sameAs property is particularly valuable — it tells Google that this portfolio,
this LinkedIn profile, and this GitHub account are all the same person. This
improves how the person's name appears in Google Knowledge panels and entity
disambiguation.

**CreativeWork schema (for case study pages):**
A JSON-LD block describing each case study as a CreativeWork. Include: name
(the project name), description (the project summary), author (the Person entity
from above), dateCreated, and image (the project's hero image URL). This gives
Google structured information about the work that may appear in rich results.

---

## Sitemap and Robots

**Sitemap:**
For a multi-page portfolio (index + case study pages), a sitemap.xml file
lists all pages with their last-modified date. Google uses this to discover
and prioritise crawling. For a simple portfolio with 5–10 pages, the sitemap
is straightforward to generate manually. List each page's full URL and the
date it was last updated.

**Robots.txt:**
A robots.txt file at the domain root tells crawlers what they can and cannot
access. For a portfolio, the correct robots.txt is maximally permissive —
allow all crawlers to access all pages. Any restrictive robots.txt on a
portfolio is a mistake: the goal is to be found.

---

## Semantic HTML as SEO Foundation

Search engines read the semantic structure of a page to understand its content
hierarchy. This is why semantic HTML (from `accessibility.md`) is doubly
important — it serves both accessibility and SEO simultaneously.

**The heading hierarchy is a ranking signal:**
Google uses heading levels to understand the topic structure of a page.
The `<h1>` should contain the most important phrase — the person's name and
role. `<h2>` elements should name the major sections. `<h3>` elements should
name individual projects and subsections. Never use headings for visual size
alone — use CSS for that.

**Alt text as keyword carrier:**
The `alt` attribute on images is read by search engines as descriptive text.
For project images: describe what is shown and name the project or context.
"Homepage redesign for Orion Finance — mobile and desktop views" tells Google
about the image content, the project name, and the client domain. This is more
valuable than "project screenshot."

**Internal linking:**
From the portfolio index to each case study, use descriptive anchor text on
the links — "View the Orion Finance case study" rather than "Read more" or
"Click here." Descriptive anchor text tells Google what the linked page is about
before following the link. Every case study page should also link back to the
index and to the next/previous case study, creating a connected internal link
structure rather than isolated pages.

---

## Portfolio-Specific SEO Copy

**The about section as keyword territory:**
The about section is where a portfolio's most natural, readable keyword
integration lives. The words a designer uses to describe their specialty —
"product design," "UX research," "brand identity," "motion design" — are the
same words a recruiter or client uses to search. Write the about section the
way you would say it to someone you just met, and the keywords will appear
naturally.

**Case study titles as searchable assets:**
Case study titles that include the project name and deliverable type appear
in search results. "Orion Finance: Mobile Banking App Redesign" is more
searchable than "Orion Finance Project." If the client permits naming, use it.

**Location signals (for freelancers):**
Freelancers who want local clients should include their city and region in
the about section and in the Person structured data. "Based in Berlin, working
with clients across Europe" is a natural language location signal that improves
local search visibility.

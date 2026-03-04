---
name: case-study-writer
description: >
  Use this skill whenever anyone wants to write, research, or produce a case study on
  ANY topic — a company, product, platform, system, technology, infrastructure,
  engineering decision, startup, or real-world event. Triggers include: "write a case
  study on X", "deep dive into Y", "how does Z work", "analyze X", "break down Y",
  "tell me everything about Z", "case study for my portfolio", "research X for me",
  "explain how X was built", "why did X succeed/fail". Always use this skill for case
  study requests regardless of domain — tech, cloud, business, fintech, identity
  systems, AI, games, startups, infrastructure, products, or failures. ALWAYS ask the
  user intake questions first to understand exactly what angle they want — the output
  is then a complete, beautifully designed self-contained HTML/CSS/JS website covering
  the full 360° picture: Why/What/How/When, timeline, features, benefits, data centers,
  revenue, users, purpose, failures, and lessons. Never just start writing — ask first.
---

# Case Study Writer

This skill produces deep, well-researched 360° case studies on any topic and delivers
them as a **complete, beautiful, self-contained HTML/CSS/JS website**.

Every case study must cover the full picture: origin story, why it was built, how it
works, who uses it, what it costs, where it runs, what went wrong, and what it teaches.
The output must look like a professionally designed editorial website — not a doc,
not markdown, not a plain page.

---

## Phase 0 — Intake: Ask the User First

Before researching or writing anything, ALWAYS ask the user these questions.
Present them clearly and wait for answers. This shapes the entire case study.

**Ask:**

1. **What is the topic?** (company, product, system, event, technology — be specific)
2. **What angle do you want?** Choose one or more:
   - 🏗 Technical deep dive (architecture, how it works, engineering decisions)
   - 📈 Business & growth story (why built, revenue, users, market position)
   - 🕐 History & timeline (origin, milestones, evolution over time)
   - 🌍 Infrastructure focus (data centers, regions, scale, global presence)
   - ⚠️ Failures & lessons (what went wrong, postmortems, takeaways)
   - 🔬 Full 360° (all of the above)
3. **Who is the audience?**
   - Engineers / developers
   - Business / product people
   - Students / general audience
   - Mix
4. **Depth preference?**
   - Quick overview (5-8 min read)
   - Standard depth (15-20 min read)
   - Maximum depth (25-35 min read, everything you can find)
5. **Any specific aspect you want emphasized?** (e.g., "focus on the AI angle",
   "I want the revenue numbers", "emphasis on India operations", "include competitor
   comparison") — or "no preference"

Use the answers to tailor which sections to include and how deep to go.
If the user says "full 360°" or "everything", include ALL sections below at maximum depth.

---

## Phase 1 — Research

After intake, research thoroughly before writing anything.

**Research strategy — search in this order:**
1. Official sources — company blogs, official docs, whitepapers, annual reports, investor filings
2. Conference talks — Google I/O, AWS re:Invent, KubeCon, QCon, InfoQ, GDC, USENIX
3. Academic papers, patents, technical filings
4. Job postings — reveal real tech stacks, team scale, and internal priorities
5. GitHub repos and open-source components
6. Reputable secondary sources — analyst reports (Gartner, IDC, Canalys), High Scalability, ACM

**Useful search patterns:**
- `"[topic] history founding timeline"`
- `"[company] revenue [year] annual report"`
- `"[company] data centers regions infrastructure"`
- `"[company] engineering blog architecture"`
- `"[system] how it works at scale"`
- `"[company] users customers market share"`
- `"[company] failures outages postmortem"`
- `"[product] features benefits use cases"`
- `"[company] vs [competitor] comparison"`

Run at least 3-5 searches. Cross-reference numbers. Aim for 5-8 distinct sources.

**Track during research:**
- ✓ Confirmed — primary source exists
- ~ Estimated — reasoned from secondary evidence
- ? Inferred — based on patterns, job postings, or industry norms
- ✗ Unknown — publicly undisclosed, flag it

---

## Phase 2 — Case Study Sections

Include sections based on the angle the user selected. For "Full 360°", include all.
Adapt depth and sub-headings to the topic naturally.

---

### Section 1 — Overview / Identity Card
Always include. A quick-reference hero block:
- Full name and what it is (one crisp sentence)
- Parent organization / founded by / headquarters
- Category tags (e.g., Cloud Infrastructure · AI Platform · Fintech)
- Founded / launched year
- Current status (active / acquired / discontinued)
- Key numbers at a glance: users, revenue, regions, uptime, market share
- One-sentence core purpose: "Built to solve ___"

---

### Section 2 — Why It Was Built (Origin & Purpose)
For business/history angle. Cover:
- What problem existed before this?
- What was the trigger moment or decision to build it?
- Who was the target user initially? Has that changed?
- What was the strategic bet / vision behind it?
- What would the world look like without it?

---

### Section 3 — History & Timeline
For history angle. A visual, scannable timeline of key milestones:
- Founding / announcement year
- First product / launch
- Key feature launches (with year)
- Acquisitions and partnerships
- Leadership changes
- Revenue milestones
- Market share milestones
- Major pivots or rebrands
- Recent developments

Format as a proper visual timeline with years clearly visible.

---

### Section 4 — What It Is / Features & Services
For business + technical angle. Cover:
- Core product categories (IaaS, PaaS, SaaS, etc. — or equivalent for non-cloud topics)
- Key features and what problem each solves
- Unique differentiators vs. competitors
- Pricing model overview (pay-as-you-go, tiers, free tier, enterprise)
- What types of users / companies use which features

---

### Section 5 — How It Works (Technical Architecture)
For technical angle. Go deep. Structure sub-sections naturally:

**For cloud platforms:**
- Global network architecture (backbone, edge, regions, zones)
- Compute layer (VMs, containers, serverless)
- Storage architecture (object, block, database)
- Networking (VPC, load balancing, CDN)
- Security model (IAM, encryption, compliance)
- AI/ML infrastructure layer

**For SaaS / product:**
- System architecture overview
- Data model and storage choices
- API design and integration ecosystem

**For startups / companies:**
- Product architecture evolution
- Tech stack decisions and why
- Scaling journey

Always explain *why* choices were made, not just *what* they are.

---

### Section 6 — Infrastructure & Data Centers
For infrastructure angle. Cover:
- Number of regions and zones (with confirmed locations)
- Specific countries/cities with data center presence
- Network backbone (submarine cables, private fiber, edge PoPs)
- Data center investment figures (capex, announced spend)
- Sustainability and energy commitments
- Upcoming expansion plans
- How physical infrastructure enables product SLAs

---

### Section 7 — Users & Customers
For business angle. Cover:
- Total customer/user count
- Customer segments (enterprise, SMB, startup, individual)
- Notable customers (publicly confirmed)
- Geographic distribution of users
- Use case distribution
- Customer growth trajectory (YoY)
- Why customers choose this over competitors

---

### Section 8 — Revenue & Business Model
For business angle. Cover:
- Revenue figures by year (with source and date — these change)
- Revenue growth rate
- Revenue breakdown by product/segment if available
- Market share vs. competitors (with source and date)
- Profitability (operating income/loss if known)
- Business model mechanics
- Competitive position and trajectory
- Key risks to the business model

---

### Section 9 — Key Engineering / Business Decisions
For technical + business angles. 3-6 pivotal decisions:
- **What was chosen** — the actual decision
- **What was rejected** — alternatives considered
- **Why** — real reasoning, constraints, or failure modes that drove the decision

---

### Section 10 — Failures, Incidents & Controversies
For failures angle. Cover:
- Known outages or incidents (with dates, if public)
- Business failures or strategic missteps
- Public controversies or regulatory issues
- What was learned and how the team adapted
- Changes made after major incidents

---

### Section 11 — Competitive Landscape
Include when relevant. Cover:
- Direct competitors with honest comparison
- Where this product wins vs. loses
- Market position summary with supporting data
- Analyst consensus on competitive standing

---

### Section 12 — Benefits & Use Cases
For business/audience angle. Cover:
- Top use cases (specific, not generic)
- Who benefits most and why
- ROI or business impact evidence
- Adoption patterns

---

### Section 13 — Lessons Learned & Takeaways
Always include. 4-6 generalizable insights anyone can apply:
- What builders / engineers / product people can learn
- What this topic teaches about the industry broadly
- What anyone building something similar should know
- Non-obvious lessons

---

### Section 14 — Confidence Note
Always include. Flag clearly:
- ✓ High confidence — primary sources confirmed
- ~ Medium — estimated from public data
- ? Inferred — reasoned from secondary evidence
- ✗ Unknown — not publicly disclosed

---

### Section 15 — References
Always include. Group by type:
- **Primary** — official docs, investor reports, company blog posts
- **Conference Talks** — year + conference name
- **Research / Analyst** — Gartner, IDC, Canalys, academic papers
- **Secondary** — reputable journalism, industry publications
- **Inferred** — labeled, explain why included

---

## Uncertainty Rules (Non-Negotiable)

- Never state an inferred fact as confirmed
- Use: *"likely"*, *"inferred from..."*, *"not officially confirmed"*, *"estimated at"*
- If sources conflict: note it explicitly
- Label every number with its source and date

---

## Phase 3 — HTML/CSS/JS Website Output

The case study is ALWAYS delivered as a **single self-contained HTML file**.
No markdown. No plain text. A real, stunning, shareable web page.

### Design Direction

Choose a bold visual aesthetic matched to the topic before writing any HTML:
- Google Cloud → clean, colorful, Material-influenced, light with blue/red/yellow/green
- A startup failure → high-contrast, editorial, stark black/white with red accent
- A fintech system → terminal-style, dark, data-dense, monospaced elements
- A game engine → dark neon, geometric, gamer aesthetic
- An Indian gov system → clean governmental, saffron/navy/white palette
- A historical timeline → magazine editorial, serif typography, timeline-first layout
- An AI platform → deep dark, aurora gradients, futuristic

**Never use generic AI aesthetics.** No purple gradients on white. No Inter/Roboto.
Every case study must feel designed for that specific subject.

### Required Design Elements

**Typography (Google Fonts only — no system fonts, never Arial/Roboto/Inter):**
- One distinctive display/heading font per case study
- One clean body font for long-form reading
- One monospace font for data, stats, labels
- Vary fonts per topic — never reuse same combo

**Color System:**
- Full CSS custom properties for the entire palette
- Dominant background + 2-3 intentional accent colors + text hierarchy
- Bold commitment to dark or light theme based on topic
- Sharp, intentional palette — not safe neutral greys

**Layout:**
- Sticky sidebar (desktop) with section anchors + active tracking
- Reading progress bar at top of page
- Section numbers
- Responsive for mobile

**Content Components to Include:**
- **Hero** — name, category tags, animated stat strip
- **Timeline** — visual vertical timeline with year markers (when history section included)
- **Stat cards** — count-up animation on first scroll into view
- **Callout boxes** — 3 types: insight, warning, quote
- **Comparison table** — for competitive section
- **Feature cards** — for features section
- **Decision cards** — collapsible expand/collapse
- **References bibliography** — with source type badges (Primary / Analyst / Inferred)
- **Confidence grid** — end-of-page 3-tier confidence display

**Motion & Interactivity (CSS + vanilla JS only):**
- Staggered fade-up animations on hero load
- Section reveals via IntersectionObserver
- Progress bar updates on scroll
- Active nav tracking while scrolling
- Smooth scroll to anchors
- Decision cards expand/collapse
- Timeline entries animate in on scroll
- Stat number count-up animation on first view
- Copy button on code blocks

**Code Standards:**
- All CSS in `<style>` in `<head>`
- All JS in `<script>` before `</body>`
- Semantic HTML5 throughout
- No external dependencies except Google Fonts
- Opens directly in browser — no server needed
- Filename: `[kebab-case-topic].html`

---

## Full Workflow

1. **Intake** — ask the 5 questions, wait for answers
2. **Research** — run web searches based on the angle selected (3-5 minimum)
3. **Plan** — decide which of the 15 sections to include, how deep
4. **Design** — choose aesthetic, fonts, colors for this specific topic
5. **Build** — write the full HTML file with all content + design + interactions
6. **Deliver** — present the `.html` file to the user

Never start the HTML until intake is complete and research is done.

---

## Quality Bar

**Great output:**
- Covers Why + What + How + When + Who + Numbers — not just one angle
- Real researched facts with honest confidence labels on every claim
- Design that feels native to the subject — like a human designer made it for this topic
- Leaves the reader genuinely smarter
- Could be published on a real editorial site

**Bad output:**
- Describes *what* something is without *why*, *how*, or *who*
- Vague filler language ("highly scalable", "best-in-class", "innovative solution")
- Generic HTML template look
- No source attribution
- Only covers one dimension of the topic
- Could have been written without any research

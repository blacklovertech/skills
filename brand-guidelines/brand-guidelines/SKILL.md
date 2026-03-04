---
name: brand-guidelines
description: Applies BlackLoverTech's official brand colors and typography to any sort of artifact that may benefit from having BlackLoverTech's look-and-feel. Use it when brand colors or style guidelines, visual formatting, or company design standards apply.
license: Complete terms in LICENSE.txt
---

# BlackLoverTech Brand Styling

## Overview

To apply BlackLoverTech's official brand identity and style resources, use this skill.

**Keywords**: branding, corporate identity, visual identity, post-processing, styling, brand colors, typography, BlackLoverTech brand, visual formatting, visual design, BLT

## Brand Identity

**Company**: BlackLoverTech  
**Tagline**: "Code. Secure. Automate. Innovate."  
**Founded**: 2023 | **Location**: Tamil Nadu, IN  
**Focus**: High-performance web engineering, secure infrastructure, scalable startup architectures

---

## Brand Guidelines

### Colors

**Primary Palette** (dark tech aesthetic):

- **Deep Black**: `#0a0a0a` — Primary backgrounds, hero sections
- **Dark Surface**: `#111111` — Cards, panels, secondary backgrounds
- **Off-White**: `#f5f5f5` — Primary text on dark backgrounds
- **Muted White**: `#cccccc` — Secondary text, subtitles

**Accent Colors** (derived from site's tech-forward identity):

- **Electric Blue**: `#3b82f6` — Primary CTA buttons, links, highlights (tech/trust)
- **Cyan Glow**: `#06b6d4` — Hover states, status indicators, live ops elements
- **Amber**: `#f59e0b` — Warnings, badges, "verified" indicators
- **Emerald**: `#10b981` — Success states, "operational" status, positive metrics

**Utility**:
- **Border Gray**: `#222222` — Card borders, dividers
- **Code Background**: `#1a1a1a` — Code blocks, terminal snippets

### Typography

- **Headings**: Space Grotesk (with Arial fallback) — clean, modern, engineering feel
- **Body Text**: Inter (with system-ui fallback) — highly readable, neutral
- **Code / Monospace**: JetBrains Mono (with monospace fallback)

### Design Principles

- **Dark-first**: All layouts use dark backgrounds with light text
- **Minimal & technical**: Clean grid layouts, no decorative clutter
- **Code-forward**: Terminal/code blocks are first-class UI elements
- **Trust signals**: Badges, status dots, encryption labels reinforce credibility
- **Global but rooted**: Tamil Nadu origin, global delivery mindset

---

## Features

### Smart Font Application

- Applies Space Grotesk to headings (24pt and larger)
- Applies Inter to body text
- Applies JetBrains Mono to code blocks and terminal outputs
- Automatically falls back to Arial/system-ui/monospace if custom fonts unavailable

### Text Styling

- Headings (24pt+): Space Grotesk, bold, off-white `#f5f5f5`
- Body text: Inter, regular, muted white `#cccccc`
- Code/terminal: JetBrains Mono, cyan `#06b6d4` on dark `#1a1a1a`
- CTA labels: Inter, bold, electric blue `#3b82f6`

### Shape and Accent Colors

- Primary interactive elements use Electric Blue `#3b82f6`
- Status/live indicators use Cyan `#06b6d4`
- Badge/verification elements use Amber `#f59e0b`
- Success/operational indicators use Emerald `#10b981`

---

## Technical Details

### Color Application

- Uses RGB hex values for precise brand matching
- Applied via python-pptx's RGBColor class for presentations
- In CSS/HTML contexts, use hex values directly
- Dark backgrounds should always use `#0a0a0a` or `#111111`

### Logo Usage

- Logo file referenced as `/logo/s111.jpg` on site
- Always place on dark background
- Never place on white or light backgrounds — breaks brand identity

### Tone & Voice (for copy/content artifacts)

- Direct, engineering-first language
- No fluff — lead with capability and outcomes
- Use phrases like: "high-performance", "secure", "scalable", "production-ready"
- Avoid: overly casual language, buzzword soup without substance

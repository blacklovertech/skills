# Claude Code Skills

A collection of community skills for [Claude Code](https://claude.ai/code). Each skill teaches Claude how to behave like an expert in a specific domain.

## What are Skills?

Skills are folders containing a `SKILL.md` file that Claude reads at the start of a session to take on specialized capabilities. You can use them locally with Claude Code or share them via URL.

## Install a Skill

```bash
# Copy any skill folder into your project's .claude/skills/ directory
cp -r go-development/ your-project/.claude/skills/

# Or use the raw SKILL.md URL directly in Claude Code
```

**Via URL** — point Claude Code to the raw `SKILL.md` link for any skill below.

---

## Available Skills

| Skill                                                | Description                                                                    | Size   |
| ---------------------------------------------------- | ------------------------------------------------------------------------------ | ------ |
| [📱 Expo React Native](./expo-react-native/)         | Production-grade Expo/React Native apps — navigation, GraphQL, EAS, biometrics | 102 KB |
| [🐹 Go Development](./go-development/)               | Idiomatic, production-ready Go — REST, gRPC, sqlc, testing, observability      | 31 KB  |
| [🔍 Domain Recon & Pentest](./domain-recon-pentest/) | Subdomain enumeration, port scanning, OSINT, vulnerability assessment          | 101 KB |
| [🧩 Artifact Patterns](./artifact-patterns/)         | Beautiful self-contained HTML/CSS/JS artifact files and UI components          | 79 KB  |
| [🏭 Theme Factory](./theme-factory/)                 | Full CSS design token systems, dark/light modes, brand-matched palettes        | 120 KB |
| [⚡ Skill Creator](./skill-creator/)                 | Meta-skill for creating, structuring, and packaging Claude skills              | 71 KB  |
| [🌐 Web Artifacts Builder](./web-artifacts-builder/) | Rich, self-contained interactive web artifacts in a single HTML file           | 30 KB  |
| [🎨 Brand Guidelines](./brand-guidelines/)           | Consistent brand identity — typography, color systems, tone of voice           | 11 KB  |
| [📝 Case Study Writer](./case-study-writer/)         | Deep, research-backed case studies delivered as stunning HTML websites         | 14 KB  |
| [📋 Case Study Writer v2](./case-study-writer-v2/)   | 360° case studies with intake, timeline, revenue, infrastructure & failures    | 14 KB  |
| [📄 Doc Co-Authoring](./doc-coauthoring/)            | Collaborative document writing, structured documentation, technical writing    | 6 KB   |

---

## Skill Structure

Each skill follows this layout:

```
skill-name/
├── SKILL.md          ← Main skill file (required)
├── scripts/          ← Helper scripts (optional)
├── examples/         ← Example outputs (optional)
└── references/       ← Reference materials (optional)
```

The `SKILL.md` file uses YAML frontmatter + Markdown:

```yaml
---
name: skill-name
description: >
  One-paragraph description of when to trigger this skill.
---
# Skill Title

Instructions for Claude to follow...
```

---

## Browse & Download

Visit the [Skills Marketplace](./index.html) for a visual browser with one-click downloads.

## Contributing

1. Fork this repo
2. Create a folder: `your-skill-name/SKILL.md`
3. Follow the SKILL.md format above
4. Open a pull request

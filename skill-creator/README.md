# ⚡ Skill Creator

Meta-skill for creating, structuring, and packaging Claude skills. SKILL.md format, YAML frontmatter, reference modules, .skill packaging, and publishing.

## 📊 Details
- **Version**: 1.0.0
- **Category**: tools
- **Size**: 71 KB
- **Last Updated**: 2026-03-02
- **Badges**: 🔥 Popular


## 📚 Reference Files
- [schemas](references/schemas.md)
- [analyzer agent](agents/analyzer.md)
- [comparator agent](agents/comparator.md)
- [grader agent](agents/grader.md)

## ⚡ Installation

To install this skill, clone the repository or copy this folder into your project's `.claude/skills/` directory.

```bash
# Run this from your project root
mkdir -p .claude/skills/skill-creator
cp -r path/to/this/repo/skill-creator/* .claude/skills/skill-creator/
```

Claude Code will automatically detect and load the `SKILL.md` file.

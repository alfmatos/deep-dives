# Deep Dive Vault

This is a self-contained deep-dive Obsidian vault. Run Claude Code from this directory.

## Skills

| Skill | Command | Purpose |
|-------|---------|---------|
| deep-dive | `/deep-dive` | Generate wiki content (research → plan → write → index) |
| deep-dive-course | `/deep-dive-course` | Build a structured learning course from wiki content |

## Workflow

1. `/deep-dive` — Run iteratively to build out wiki content section by section
2. `/deep-dive-course` — Generate a structured course from the wiki's free content

## Obsidian Skills (from kepano/obsidian-skills)

- **obsidian-markdown** — Follow for ALL markdown output: wikilinks, embeds, callouts, properties, tags
- **defuddle** — **Preferred tool for web content extraction.** Use `defuddle parse <url> --md` via Bash to get clean markdown from URLs. Install with `npm install -g defuddle` if not available. Fall back to WebFetch only when defuddle fails.
- **obsidian-cli** — Use to interact with running Obsidian: open vaults, search notes, read/create/append content
- **obsidian-bases** — Use to create `.base` files for dashboards and trackers (e.g., course progress)
- **json-canvas** — Use to create `.canvas` files for concept maps and visual diagrams

## Key Rules

- **Never fabricate URLs.** Always use WebSearch to find real, current information, then use defuddle to extract content.
- **Use Obsidian-flavored markdown** for all output: `[[wikilinks]]`, YAML frontmatter, callouts, tags. Follow the obsidian-markdown skill.
- **Every article must have proper frontmatter** with at minimum: title, tags, category, date, status.
- **Update the manifest** (`_meta/manifest.yaml`) after every generation session.
- **Log all research** in `_meta/research-log.md` with timestamps and URLs.
- **Ask before overwriting** manually-edited articles (check if content differs from what was generated).
- **Article templates** in `_templates/` define the structure for each article type.
- **All paths are relative to the vault root** (the current working directory).

## Vault Structure

```
.
├── .claude/skills/          # All skills for operating on this vault
├── .obsidian/               # Obsidian configuration
├── _meta/                   # Manifest and research log
├── _templates/              # Article structure templates
├── 00 - Index.md            # Master index
├── 01 - Overview/
├── 02 - Fundamentals/
├── 03 - People/
├── 04 - Resources/
├── 05 - Learning/
├── 06 - Course/
└── 07 - Reference/
```

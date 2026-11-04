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

## Running unattended

When a run has no one at the keyboard, as with `claude -p "/deep-dive section people"` from a script, nobody will answer a question. This overrides the skills wherever they ask for approval:

- **Where a skill asks you to approve a plan or a change,** check it against the manifest's focus and areas instead. If it covers them, carry on; if not, revise it and carry on.
- **Never ask anything.** Where something is ambiguous, choose what best serves the manifest's focus, and note the choice in the research log.
- **Finish the section in this run:** research, plan, every planned article, the index and the manifest.
- **Write each article to disk as soon as it's done, and keep the manifest's article list current,** so a run that's cut short can carry on from where it stopped.
- **Keep titles unique across the vault,** since a wikilink names an article by its title. Before adding an article, check no other section has one with that title.
- **Treat what's already in an article as deliberate:** add to it, and leave anything you can't tell was generated.

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

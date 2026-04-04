# Deep Dive Wiki Generator

A Claude Code scaffolding tool that creates self-contained, research-backed deep-dive wikis as Obsidian vaults. Inspired by [Karpathy's LLM Knowledge Bases](https://x.com/karpathy/status/2039805659525644595) workflow and built on [kepano's obsidian-skills](https://github.com/kepano/obsidian-skills).

## How it works

This project is a **generator**. It stamps out self-contained Obsidian vaults for any topic. Each vault gets its own skills, templates, and config — after creation, it's fully independent and can be its own git repo.

```
deep-dives/                (this repo — the generator)
├── .claude/skills/        /deep-dive-init (the scaffolder)
└── templates/             source of truth for skills, config, templates

~/Projects/bass-deep-dive/ (generated vault — separate repo)
├── .claude/skills/        /deep-dive, /deep-dive-course + kepano skills
├── .obsidian/
├── 00 - Index.md
├── 01 - Overview/
└── ...
```

## Quick start

```bash
# 1. Clone the generator
git clone git@github.com:alfmatos/deep-dives.git
cd deep-dives

# 2. Create a vault for any topic
claude
/deep-dive-init
# → topic: chess
# → focus: Learning competitive chess from scratch
# → location: ~/Projects/chess-deep-dive

# 3. Enter the vault and generate content
cd ~/Projects/chess-deep-dive
claude
/deep-dive              # Generate wiki content section by section
/deep-dive-course       # Build a structured learning course

# 4. Push the vault as its own repo
git init && git add -A && git commit -m "Initial chess deep dive"
gh repo create chess-deep-dive --source . --push
```

## Skills

### Generator level (this repo)

| Skill | Description |
|-------|-------------|
| `/deep-dive-init` | Scaffolds a new self-contained vault with all skills, templates, and config |

### Vault level (each generated vault)

| Skill | Description |
|-------|-------------|
| `/deep-dive` | Research the web, plan articles, generate Obsidian-native content. Works incrementally. |
| `/deep-dive-course` | Build a progressive learning course from free online content |

### Included from [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills)

| Skill | Description |
|-------|-------------|
| `obsidian-markdown` | Obsidian-flavored markdown: wikilinks, embeds, callouts, properties |
| `obsidian-bases` | Database-like views with `.base` files |
| `json-canvas` | Visual diagrams and concept maps |
| `obsidian-cli` | CLI interaction with running Obsidian |
| `defuddle` | Extract clean markdown from web pages (preferred over WebFetch) |

## What gets generated

Each vault follows this structure:

```
<topic>-deep-dive/
├── .claude/                  # Skills and config (self-contained)
├── .obsidian/                # Obsidian vault config
├── _meta/
│   ├── manifest.yaml         # Topic definition + generation state
│   └── research-log.md       # Log of all web searches
├── _templates/               # Article templates (concept, person, resource, lesson)
├── 00 - Index.md             # Master index
├── 01 - Overview/            # Topic history and landscape
├── 02 - Fundamentals/        # Core concepts and theory
├── 03 - People/              # Key figures and content creators
├── 04 - Resources/           # Sites, tools, apps, communities
├── 05 - Learning/            # Methods, courses, learning paths
├── 06 - Course/              # Structured progressive curriculum
└── 07 - Reference/           # Glossary, FAQ, quick reference
```

## The `/deep-dive` workflow

The content generation skill works in four phases per section:

1. **Research** — WebSearch + defuddle to gather real, current information
2. **Plan** — Propose articles for user approval
3. **Generate** — Write articles with proper frontmatter, wikilinks, and verified URLs
4. **Index** — Update the master index and manifest

It works incrementally — run it multiple times, and it picks up where it left off via the manifest.

```
/deep-dive              # Continue next pending section
/deep-dive status       # Show progress
/deep-dive section people  # Generate a specific section
/deep-dive refresh people  # Re-research and update
/deep-dive add "topic"     # Add a specific article
```

## The `/deep-dive-course` workflow

After wiki content exists, this skill builds a structured learning course from free content:

1. Analyzes all existing wiki articles for resources and difficulty levels
2. Researches additional free content (YouTube, articles, interactive tools)
3. Designs a week-by-week progressive curriculum
4. Generates lesson notes with real, verified URLs
5. Creates practice material with progressive difficulty

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- [Obsidian](https://obsidian.md/) (for viewing vaults)
- [defuddle](https://github.com/anthropics/defuddle) (`npm install -g defuddle`) — for web content extraction

## License

MIT

---
name: deep-dive-init
description: Initialize a new self-contained deep-dive Obsidian vault for any topic. Creates the vault with all skills, templates, config, and manifest baked in. After init, the vault is fully independent — move it anywhere, git init, push to GitHub. Use when the user wants to start a new deep dive.
user_invocable: true
---

# Deep Dive Init

Scaffold a new self-contained deep-dive Obsidian vault for a specific topic.

## Trigger

Use when the user says: "init", "new deep dive", "create a deep dive on", "start a deep dive", or `/deep-dive-init`.

## Workflow

### Step 1: Collect Topic Info

Ask the user:

1. **Topic name** — short, lowercase, used as folder name (e.g., "bass", "chess", "cooking")
2. **Focus angle** — one sentence describing the angle (e.g., "Learning to play bass guitar from scratch" or "Competitive chess strategy for intermediate players")
3. **Specific areas** (optional) — any must-have areas beyond the defaults
4. **Output location** (optional) — defaults to `vaults/<topic>/`, but the user can specify any path (e.g., `~/Projects/chess-deep-dive/`)

### Step 2: Create Vault Structure

Create the vault at the target location with this structure:

```
<vault>/
├── .claude/                         # Copied from templates/claude/
│   ├── CLAUDE.md                    # Vault-level instructions
│   ├── settings.local.json          # Permissions
│   └── skills/                      # All operational skills
│       ├── obsidian-markdown/
│       ├── obsidian-bases/
│       ├── json-canvas/
│       ├── obsidian-cli/
│       ├── defuddle/
│       ├── deep-dive/
│       └── deep-dive-course/
├── .obsidian/                       # Copied from templates/vault-config/
│   ├── app.json
│   ├── appearance.json
│   └── core-plugins.json
├── .gitignore                       # Copied from templates/gitignore
├── _meta/
│   ├── manifest.yaml                # Generated
│   └── research-log.md              # Generated (empty)
├── _templates/                      # Copied from templates/article-templates/
│   ├── concept.md
│   ├── person.md
│   ├── resource.md
│   └── lesson.md
├── 00 - Index.md                    # Generated
├── 01 - Overview/
├── 02 - Fundamentals/
├── 03 - People/
├── 04 - Resources/
├── 05 - Learning/
├── 06 - Course/
└── 07 - Reference/
```

Use Bash to create directories and copy templates. The generator project root contains all templates:

```bash
VAULT="<target-path>"
ROOT="<generator-project-root>"

# Create directories
mkdir -p "$VAULT"/{.obsidian,_meta,_templates,"01 - Overview","02 - Fundamentals","03 - People","04 - Resources","05 - Learning","06 - Course","07 - Reference"}

# Copy .claude/ (skills, CLAUDE.md, settings)
cp -r "$ROOT/templates/claude" "$VAULT/.claude"

# Copy .obsidian/ config
cp "$ROOT/templates/vault-config/"*.json "$VAULT/.obsidian/"

# Copy article templates
cp "$ROOT/templates/article-templates/"*.md "$VAULT/_templates/"

# Copy .gitignore
cp "$ROOT/templates/gitignore" "$VAULT/.gitignore"
```

### Step 3: Generate manifest.yaml

Write `_meta/manifest.yaml` with this structure:

```yaml
topic: <topic>
slug: <topic-slug>
focus: "<focus angle from user>"
created: <today's date YYYY-MM-DD>
areas:
  - <area 1 based on topic>
  - <area 2>
  - <area 3>
  - <etc>
sections:
  overview:
    folder: "01 - Overview"
    status: pending
    articles: []
    last_researched: null
  fundamentals:
    folder: "02 - Fundamentals"
    status: pending
    articles: []
    last_researched: null
  people:
    folder: "03 - People"
    status: pending
    articles: []
    last_researched: null
  resources:
    folder: "04 - Resources"
    status: pending
    articles: []
    last_researched: null
  learning:
    folder: "05 - Learning"
    status: pending
    articles: []
    last_researched: null
  course:
    folder: "06 - Course"
    status: pending
    articles: []
    last_researched: null
  reference:
    folder: "07 - Reference"
    status: pending
    articles: []
    last_researched: null
last_generation: null
```

Tailor the `areas` list to the specific topic. For example:
- **bass**: fundamentals of bass guitar, key bass players and styles, content creators and teachers, learning methods and sites, free courses and structured content, songs by difficulty
- **chess**: opening theory, middlegame strategy, endgame technique, famous players and games, online platforms and tools, puzzle training, tournament preparation

### Step 4: Generate research-log.md

Write `_meta/research-log.md`:

```markdown
# Research Log

All web searches and content extractions performed during deep dive generation.

---
```

### Step 5: Generate 00 - Index.md

Write `00 - Index.md` as the vault's home page:

```markdown
---
title: "<Topic> Deep Dive"
tags:
  - index
  - <topic>
date: <today>
---

# <Topic> Deep Dive

> <Focus angle>

## Sections

- [[01 - Overview/|Overview]] — What is <topic>, history, and current landscape
- [[02 - Fundamentals/|Fundamentals]] — Core concepts and theory
- [[03 - People/|People]] — Key figures, players, content creators
- [[04 - Resources/|Resources]] — Best sites, tools, apps, communities
- [[05 - Learning/|Learning]] — Methods, courses, learning paths
- [[06 - Course/|Structured Course]] — Step-by-step curriculum from free content
- [[07 - Reference/|Reference]] — Glossary, FAQ, quick reference

## Status

> [!info] Generation Status
> Run `/deep-dive status` to see current progress.
> Run `/deep-dive` to generate content for the next section.
```

### Step 6: Initialize Git (optional)

Ask the user if they want to initialize a git repo:

```bash
cd "$VAULT" && git init && git add -A && git commit -m "Initial <topic> deep dive vault"
```

### Step 7: Print Next Steps

Tell the user:

```
Vault created at: <path>

The vault is fully self-contained. Next steps:

1. Open in Obsidian: Open Obsidian → Open another vault → select <path>
2. Generate content:
   cd <path>
   claude
   /deep-dive

3. To push to GitHub:
   cd <path>
   gh repo create <topic>-deep-dive --source . --push
```

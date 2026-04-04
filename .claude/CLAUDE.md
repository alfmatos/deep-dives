# Deep Dive Wiki Generator

A scaffolding tool that creates self-contained deep-dive Obsidian vaults on any topic.

## Usage

Run `/deep-dive-init` to create a new topic vault. The vault is created under `vaults/<topic>/` with everything it needs:

- All skills (`/deep-dive`, `/deep-dive-course`, plus kepano obsidian-skills)
- Obsidian config
- Article templates
- Manifest and research log

After init, `cd` into the vault and run Claude Code from there. The vault is fully self-contained — you can move it anywhere, init a git repo, push it to GitHub.

## Workflow

```
# 1. Create a vault (from this directory)
/deep-dive-init

# 2. Enter the vault and generate content
cd vaults/chess
claude

# 3. Inside the vault, use:
/deep-dive              # Generate wiki content section by section
/deep-dive-course       # Build a structured learning course
```

## What lives here vs. in each vault

| Here (generator) | In each vault |
|-------------------|---------------|
| `/deep-dive-init` skill | `/deep-dive` and `/deep-dive-course` skills |
| Templates (source of truth) | Copied templates (`_templates/`) |
| Vault config templates | Copied `.obsidian/` config |
| — | All 5 kepano obsidian-skills |
| — | Manifest, research log, all content |

## Templates

- `templates/vault-config/` — Obsidian config files copied into new vaults
- `templates/article-templates/` — Article structure templates copied into vaults as `_templates/`
- `templates/claude/` — `.claude/` directory copied into each vault (CLAUDE.md, settings, skills)
- `templates/gitignore` — `.gitignore` copied into each vault

---
name: deep-dive
description: Generate deep-dive wiki content for an existing topic vault. Researches the web, plans articles, writes Obsidian-native content, and maintains the index. Works incrementally — each run builds more content. Use when the user wants to add content to a deep dive vault.
user_invocable: true
---

# Deep Dive Content Generator

Research, plan, and generate wiki content for a deep-dive topic vault. Works incrementally — each invocation builds more content guided by the manifest.

## Trigger

Use when the user says: "deep dive", "generate content", "continue the deep dive", "build out the wiki", or `/deep-dive`.

Arguments:
- `/deep-dive` — Continue where left off (next pending section)
- `/deep-dive status` — Show vault status
- `/deep-dive section <name>` — Generate a specific section (overview, fundamentals, people, resources, learning, reference)
- `/deep-dive refresh <name>` — Re-research and update a section
- `/deep-dive add "<article title>"` — Add a specific article to the current section

## Before Starting

1. **You are inside the vault.** The current working directory IS the vault root. All paths are relative to it.
2. **Read the manifest.** Read `_meta/manifest.yaml` to understand topic, focus, areas, and section statuses.
3. **Determine what to do.** If the user specified a section, target that. Otherwise, find the first section with status `pending` or `in-progress`.

## Status Mode

When the user runs `/deep-dive status`:

1. Read the manifest
2. Display a summary table:

```
## <Topic> Deep Dive — Status

| Section | Status | Articles | Last Researched |
|---------|--------|----------|-----------------|
| Overview | done | 5 | 2026-04-04 |
| Fundamentals | in-progress | 3/7 | 2026-04-04 |
| People | pending | 0 | — |
| Resources | pending | 0 | — |
| Learning | pending | 0 | — |
| Course | pending | 0 | — |
| Reference | pending | 0 | — |
```

## Generation Workflow

For each section, follow these four phases:

### Phase 1: Research

Perform targeted web searches using WebSearch. **Use defuddle** (`defuddle parse <url> --md` via Bash) to extract clean content from the most relevant URLs. Fall back to WebFetch only if defuddle is unavailable.

**Search strategies by section:**

| Section | Search Queries |
|---------|---------------|
| Overview | "[topic] history", "[topic] overview guide", "[topic] for beginners", "what is [topic]" |
| Fundamentals | "[topic] basics", "[topic] core concepts", "[topic] theory fundamentals", "[topic] essential knowledge" |
| People | "best [topic] players all time", "greatest [topic] practitioners", "top [topic] content creators youtube 2025 2026", "current [topic] artists to follow", "[topic] influencers teachers" |
| Resources | "best [topic] websites 2025 2026", "best [topic] apps tools", "best [topic] communities reddit discord", "best [topic] youtube channels", "best [topic] forums" |
| Learning | "how to learn [topic]", "best free [topic] courses 2025 2026", "best [topic] youtube tutorials", "[topic] learning path beginner to intermediate", "[topic] practice routine", "best [topic] methods" |
| Reference | "[topic] glossary terms", "[topic] FAQ beginners", "[topic] cheat sheet", "[topic] common mistakes" |

**Research procedure:**

1. Run 3-5 WebSearch queries for the section
2. From the results, pick the 3-5 most relevant and authoritative URLs
3. Extract content from each using defuddle: `defuddle parse "<url>" --md`
4. Log every search and extraction in `_meta/research-log.md`:

```markdown
## <Section Name> — <date>

### Searches
- "<query 1>" — <number of results>
- "<query 2>" — <number of results>

### Extracted Content
- [<Title>](<url>) — extracted via defuddle
- [<Title>](<url>) — extracted via defuddle
```

### Phase 2: Plan

Based on research findings, plan the articles for this section:

1. Create a list of articles with:
   - Article title
   - Article type (concept, person, resource — from `templates/article-templates/`)
   - One-line description
   - Key sources that will inform the article

2. **Show the plan to the user for approval.** Example:

```
## Planned Articles for People

1. **Jaco Pastorius** (person) — Legendary fretless bass pioneer
2. **Victor Wooten** (person) — Technical virtuoso and groove master
3. **Marcus Miller** (person) — Slap pioneer, jazz fusion
4. **Scott's Bass Lessons** (resource) — Largest online bass education platform
5. **BassBuzz** (resource) — Beginner-focused YouTube channel

Approve this plan? I can add, remove, or reorder articles.
```

3. Wait for user approval before proceeding.

### Phase 3: Generate

For each approved article:

1. **Check the article template** in `_templates/` for the appropriate type (concept.md, person.md, resource.md)
2. **Perform additional targeted searches** if the research phase didn't cover this specific article well enough
3. **Write the article** following these rules:

#### Formatting Rules (follow obsidian-markdown skill)

- **Frontmatter**: Every article MUST have YAML frontmatter with: title, tags, aliases (if applicable), category, date, status
- **Wikilinks**: Use `[[Article Title]]` for internal links to other vault articles. Use `[[Article Title|display text]]` for custom display.
- **External links**: Use standard markdown `[text](url)` for external URLs. These must be REAL URLs found during research — **never fabricated**.
- **Callouts**: Use for tips, warnings, and important notes:
  ```
  > [!tip] Pro Tip
  > Content here
  ```
- **Tags**: Use in frontmatter only, not inline. Tags should be lowercase, hyphenated.
- **Headers**: Use `##` for main sections, `###` for subsections. One `#` title at the top matching the frontmatter title.

#### Content Quality Rules

- **Accuracy**: Only include information found during research. Cite sources.
- **Real URLs**: Every external link must come from WebSearch results or defuddle extractions. Never invent a URL.
- **Wikilinks**: Link to other articles that exist or will exist in the vault. Use the planned article titles.
- **Depth**: Articles should be substantive (300-800 words typically). Not just bullet points — include context, analysis, and practical information.
- **Voice**: Informative and practical. Write as a knowledgeable guide, not a textbook.

#### Generation Procedure

For each article:

1. Write the article to the section folder: `<section-folder>/<Article Title>.md`
2. After writing, update the manifest:
   - Add the article filename to the section's `articles` list
   - If all planned articles are written, set section status to `done`
   - If some remain, set status to `in-progress`

### Phase 4: Index Update

After generating articles for a section:

1. **Update `00 - Index.md`**: Add links to all new articles under the appropriate section. Use wikilinks:
   ```markdown
   ## People
   - [[Jaco Pastorius]] — Legendary fretless bass pioneer
   - [[Victor Wooten]] — Technical virtuoso and groove master
   ```

2. **Update the manifest**: Set `last_researched` to today's date. Update section status.

3. **Update research-log.md**: Ensure all searches and extractions are logged.

4. **Report to user**: Summarize what was created:
   ```
   Generated 5 articles for People:
   - Jaco Pastorius.md (person, 450 words)
   - Victor Wooten.md (person, 380 words)
   - ...

   Section "People" is now complete.
   Next pending section: Resources
   Run `/deep-dive` to continue.
   ```

## Refresh Mode

When the user runs `/deep-dive refresh <section>`:

1. Read existing articles in the section
2. Re-run research (Phase 1) for the section
3. Compare new research with existing articles
4. Suggest updates: new information to add, outdated info to update, new articles to create
5. Ask the user what to update before making changes
6. Preserve any manual edits the user has made (check if content differs from the generated version)

## Add Mode

When the user runs `/deep-dive add "<title>"`:

1. Determine which section the article belongs to (ask if unclear)
2. Research the specific topic
3. Generate the article
4. Update the index and manifest

## Context Window Management

- Generate articles **one at a time**, not all at once
- After generating each article, write it to disk immediately
- If the context is getting large, summarize the research findings before generating articles
- Use the manifest to track progress so work can resume across sessions

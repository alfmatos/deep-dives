---
name: deep-dive-course
description: Build a structured learning course from an existing deep-dive wiki vault. Organizes free online content into a progressive curriculum with exercises and practice material at each level. Run after wiki content has been generated with /deep-dive.
user_invocable: true
---

# Deep Dive Course Builder

Build a structured, progressive learning course from the wiki content in a deep-dive vault. The course is built entirely from free, publicly available content (YouTube videos, articles, interactive tools, etc.).

## Trigger

Use when the user says: "build a course", "create a course", "generate the course", "structured course", or `/deep-dive-course`.

## Before Starting

1. **You are inside the vault.** The current working directory IS the vault root. All paths are relative to it.
2. **Read the manifest.** Check that at least Overview, Fundamentals, People, and Resources sections have status `done`. If not, warn the user that the course works best with existing wiki content and suggest running `/deep-dive` first.
3. **Read all existing articles.** Scan every `.md` file in sections 01-05 and 07 to catalog:
   - All external URLs (resources, videos, lessons)
   - All mentioned tools, platforms, and sites
   - Difficulty levels mentioned anywhere
   - Learning progressions and recommendations

## Course Design Workflow

### Phase 1: Analyze Existing Content

1. Read all wiki articles and extract:
   - Free resources and their URLs
   - Content creators and their channels/sites
   - Recommended learning paths mentioned in articles
   - Difficulty ratings and skill progressions
   - Practice material (songs, exercises, puzzles, projects)

2. Summarize findings before planning:
   ```
   Found:
   - 12 free YouTube channels
   - 5 free lesson sites
   - 3 interactive tools
   - 15 beginner songs/exercises
   - 8 intermediate songs/exercises
   ```

### Phase 2: Research Additional Free Content

Use WebSearch + defuddle to fill gaps:

- "best free [topic] lessons youtube beginner"
- "free [topic] course online 2025 2026"
- "[topic] practice exercises beginner"
- "[topic] progressive difficulty [songs/puzzles/projects]"
- "free [topic] curriculum learning path"

For each promising URL, extract content with `defuddle parse "<url>" --md` to verify it's real, free, and high quality.

### Phase 3: Design Course Structure

Create a week-by-week curriculum. Present it to the user for approval before generating files.

**Course structure guidelines:**

- **Duration**: Typically 8-16 weeks depending on topic complexity
- **Progression**: Absolute beginner → beginner → intermediate
- **Each week includes**:
  - 2-3 lessons with specific free content to study
  - Practice exercises or activities
  - A practical project/song/puzzle that applies the week's learning
- **Every lesson links to REAL, FREE content** — verified URLs only
- **Cross-link back to wiki articles** for deeper context

**Example course outline:**

```
## Proposed Course Structure: Bass Guitar (12 weeks)

### Phase 1: Getting Started (Weeks 1-3)
- Week 1: Your First Bass — anatomy, tuning, posture
- Week 2: First Notes — open strings, simple patterns
- Week 3: Basic Rhythm — quarter notes, eighth notes
  - Song: "Stand By Me" (simple root notes)

### Phase 2: Building Foundation (Weeks 4-6)
- Week 4: Major Scale Basics
- Week 5: Simple Bass Lines — root-fifth patterns
- Week 6: Timing and Feel
  - Song: "Another One Bites the Dust"

### Phase 3: Developing Skills (Weeks 7-9)
...

Approve this structure? I can adjust duration, add/remove weeks, or change the focus.
```

### Phase 4: Generate Course Files

After approval, generate the course in `06 - Course/`:

```
06 - Course/
├── Course Overview.md
├── Week 01 - <Title>/
│   ├── Week 01 Overview.md
│   ├── Lesson 01 - <Title>.md
│   ├── Lesson 02 - <Title>.md
│   └── Practice - <Song or Exercise>.md
├── Week 02 - <Title>/
│   ├── Week 02 Overview.md
│   ├── Lesson 03 - <Title>.md
│   └── Practice - <Song or Exercise>.md
└── ...
```

#### Course Overview.md

The master curriculum document:

```markdown
---
title: "<Topic> Learning Course"
tags:
  - course
  - index
category: course
date: <today>
status: complete
---

# <Topic> Learning Course

> A structured, progressive curriculum built entirely from free online content.

## Course Philosophy

<!-- Why this course structure, what approach it takes -->

## Prerequisites

<!-- What you need before starting -->

## How to Use This Course

<!-- Instructions: follow sequentially, spend X time per week, etc. -->

## Curriculum

### Phase 1: Getting Started (Weeks 1-3)
- [[Week 01 Overview|Week 1: <Title>]]
- [[Week 02 Overview|Week 2: <Title>]]
- [[Week 03 Overview|Week 3: <Title>]]

### Phase 2: Building Foundation (Weeks 4-6)
...

## Resources Used

<!-- Summary of all platforms and channels referenced -->
```

#### Week Overview Notes

Each week gets an overview:

```markdown
---
title: "Week <N>: <Title>"
tags:
  - course
  - week-<N>
category: course
week: <N>
difficulty: <beginner|intermediate>
date: <today>
status: complete
---

# Week <N>: <Title>

## This Week's Goals

- [ ] Goal 1
- [ ] Goal 2
- [ ] Goal 3

## Lessons

1. [[Lesson <N> - <Title>]]
2. [[Lesson <N+1> - <Title>]]

## Practice

- [[Practice - <Song or Exercise>]]

## Concepts Covered

- [[Concept 1]] — brief description
- [[Concept 2]] — brief description
```

#### Lesson Notes

Follow the lesson template from `templates/article-templates/lesson.md`. Each lesson must include:

- **Specific free content**: Real YouTube URLs, real article URLs, real interactive tool URLs
- **Clear instructions**: What to watch/read, what to focus on, how long to practice
- **Practice material**: Exercises, songs, puzzles, or projects with progressive difficulty

#### Practice Notes

For songs/exercises/projects:

```markdown
---
title: "Practice - <Title>"
tags:
  - practice
  - course
  - week-<N>
category: course
week: <N>
difficulty: <level>
date: <today>
status: complete
---

# <Title>

## Why This Piece

<!-- Why this was chosen for this point in the course -->

## Resources

- [Tutorial/Tab/Guide](real-url) — description
- [Performance to study](real-url) — description

## Key Techniques

- [[Technique 1]] — how it appears in this piece
- [[Technique 2]] — how it appears in this piece

## Practice Tips

<!-- Specific guidance: tempo, sections to focus on, common mistakes -->
```

### Phase 5: Create Course Dashboard (optional)

If appropriate, create a `.base` file for tracking progress. Follow the obsidian-bases skill:

Write `06 - Course/Course Progress.base`:

```json
{
  "filter": {
    "conjunction": "and",
    "conditions": [
      { "field": "file.tags", "operator": "contains", "value": "course" },
      { "field": "file.folder", "operator": "starts with", "value": "06 - Course" }
    ]
  },
  "views": [
    {
      "id": "progress",
      "name": "Course Progress",
      "type": "table",
      "order": [
        { "field": "week", "direction": "asc" },
        { "field": "file.name", "direction": "asc" }
      ]
    }
  ]
}
```

### Phase 6: Update Index

1. Update `00 - Index.md` to include course links
2. Update the manifest: set course section status to `done`
3. Report to user what was generated:

```
Generated course: 12 weeks, 24 lessons, 12 practice pieces
All content uses free resources from:
- YouTube channels: BassBuzz, TalkingBass, Scott's Bass Lessons
- Websites: studybass.com, musictheory.net
- Interactive: fretboard trainer app

Open the vault in Obsidian to explore!
```

## URL Verification

Before including any URL in a lesson:
1. The URL must have been found via WebSearch
2. Prefer URLs that were verified by extracting content with defuddle
3. If a URL cannot be verified, use a callout:
   ```
   > [!warning] Unverified Link
   > This link was found in search results but could not be fully verified. Check before using.
   ```

## Content Quality

- Every lesson must have at least one video or article link
- Songs/exercises must have a tutorial or tab link
- Difficulty must progress: each week builds on the previous
- No two consecutive weeks should have the same type of practice — mix it up
- Cross-link aggressively to wiki articles for deeper context

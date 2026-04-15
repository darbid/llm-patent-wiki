---
name: obsidian-markdown
description: "Write correct Obsidian Flavored Markdown: wikilinks, embeds, callouts, properties, tags, highlights, math, and canvas syntax. Reference this when creating or editing any wiki page. Triggers on: write obsidian note, obsidian syntax, wikilink, callout, embed, obsidian markdown, wikilink format, callout syntax, embed syntax, obsidian formatting, how to write obsidian markdown."
allowed-tools: Read Write Edit
---

# obsidian-markdown: Obsidian Flavored Markdown

Reference this skill when writing any wiki page. Obsidian extends standard Markdown with wikilinks, embeds, callouts, and properties. Getting syntax wrong causes broken links, invisible callouts, or malformed frontmatter.

**Cross-reference**: If the kepano/obsidian-skills plugin is installed, prefer its canonical obsidian-markdown skill for authoritative Obsidian syntax reference. Otherwise, use the reference below. See also [github.com/kepano/obsidian-skills](https://github.com/kepano/obsidian-skills).

---

## Wikilinks

Internal links use double brackets. The filename without extension.

| Syntax | What it does |
|---|---|
| `[[Note Name]]` | Basic link |
| `[[Note Name\|Display Text]]` | Aliased link (shows "Display Text") |
| `[[Note Name#Heading]]` | Link to a specific heading |
| `[[Note Name#^block-id]]` | Link to a specific block |

Rules:
- Case-sensitive on some systems. Match the exact filename.
- No path needed: Obsidian resolves by filename uniqueness.
- If two files have the same name, use `[[Folder/Note Name]]` to disambiguate.

---

## Embeds

Embeds use `!` before the wikilink. They display the content inline.

| Syntax | What it does |
|---|---|
| `![[Note Name]]` | Embed a full note |
| `![[Note Name#Heading]]` | Embed a section |
| `![[image.png]]` | Embed an image |
| `![[image.png\|300]]` | Embed image with width 300px |
| `![[document.pdf]]` | Embed a PDF (Obsidian renders natively) |
| `![[audio.mp3]]` | Embed audio |

---

## Callouts

Callouts are blockquotes with a type keyword. They render as styled alert boxes.

```markdown
> [!note]
> Default informational callout.

> [!note] Custom Title
> Callout with a custom title.

> [!note]- Collapsible (closed by default)
> Click to expand.

> [!note]+ Collapsible (open by default)
> Click to collapse.
```

### Standard callout types

| Type | Aliases | Use for |
|------|---------|---------|
| `note` | | General notes |
| `abstract` | `summary`, `tldr` | Summaries |
| `info` | | Information |
| `todo` | | Action items |
| `tip` | `hint`, `important` | Tips and highlights |
| `success` | `check`, `done` | Positive outcomes |
| `question` | `help`, `faq` | Open questions |
| `warning` | `caution`, `attention` | Warnings |
| `failure` | `fail`, `missing` | Errors or failures |
| `danger` | `error` | Critical issues |
| `bug` | | Known bugs |
| `example` | | Examples |
| `quote` | `cite` | Quotations |

### Patent wiki custom callouts

| Type | Color | Use for |
|------|-------|---------|
| `disclosed` | Green | [D] Disclosed statements |
| `synthesis` | Blue | [S] Attorney-drafted synthesis |
| `inference` | Amber | [I] Inference / hypothesis |
| `contradiction` | Brown | Conflicting information |
| `gap` | Amber | Missing cross-reference |
| `key-insight` | Blue | Important insights |
| `stale` | Gray | Outdated information |

---

## Properties (Frontmatter)

Obsidian renders YAML frontmatter as a Properties panel. Rules:

```yaml
---
type: claim
title: "Note Title"
created: 2026-04-14
updated: 2026-04-14
tags:
  - claim
  - US20260072718A1
status: developing
related:
  - "[[Other Note]]"
sources:
  - "[[.raw/US20260072718A1.md]]"
---
```

Rules:
- Flat YAML only. Never nest objects.
- Dates as `YYYY-MM-DD`, not `2026-04-08T00:00:00`.
- Lists as `- item`, not inline `[a, b, c]`.
- Wikilinks in YAML must be quoted: `"[[Page]]"`.
- `tags` field: Obsidian reads this as the tag list, searchable in vault.

---

## Tags

Two valid forms:

```markdown
#tag-name             : inline tag anywhere in the body
#parent/child-tag     : nested tag (shows hierarchy in tag pane)
```

In frontmatter:
```yaml
tags:
  - research
  - patent/claims
```

Do not use `#` inside frontmatter tag lists. Just the tag name.

---

## Text Formatting

Standard Markdown plus Obsidian extensions:

| Syntax | Result |
|---|---|
| `**bold**` | Bold |
| `*italic*` | Italic |
| `~~strikethrough~~` | Strikethrough |
| `==highlight==` | Highlighted text (yellow in Obsidian) |
| `` `inline code` `` | Inline code |

---

## Tables

Standard Markdown tables:

```markdown
| Column A | Column B | Column C |
|----------|----------|----------|
| Value    | Value    | Value    |
```

---

## What NOT to Do

- Do not use `[link text](path/to/note.md)` for internal links: use `[[Note Name]]` instead.
- Do not use HTML inside callouts: stick to Markdown.
- Do not use `##` inside a callout body: headings don't render inside callouts.
- Do not write `tags: [a, b, c]` inline in frontmatter: Obsidian prefers the list format.
- Do not write ISO datetimes in frontmatter (`2026-04-08T00:00:00Z`): use `2026-04-08`.

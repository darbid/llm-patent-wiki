# LLM-Patent-Wiki

<p align="center">
  <img src="_attachments/llm-patent-wiki.gif" alt="LLM-Patent-Wiki" width="100%" />
</p>

[![GitHub stars](https://img.shields.io/github/stars/darbid/llm-patent-wiki?style=flat&color=e8734a)](https://github.com/darbid/llm-patent-wiki/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude_Code-plugin-8B5CF6)](https://code.claude.com/docs/en/discover-plugins)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-David_Bowen-0077B5?logo=linkedin)](https://www.linkedin.com/in/david-bowen-411a0255/)

A persistent, compounding patent knowledge base that an AI builds and maintains through use — so you never start from scratch again.

---

## Built on the LLM Wiki Pattern

Every time you ask an AI about your patent portfolio, it starts cold. It has never seen your patents before. It does not know your terminology, your claim strategy, your prior art concerns, or the answers to questions you asked last week. You get a competent but generic response, and the next conversation is identical.

This is the core observation behind [Andrej Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f): **LLMs rediscover knowledge from scratch every time. There is no accumulation.**

LLM-Patent-Wiki is an adaptation of that concept for patent portfolios. It shows how the LLM Wiki pattern can be applied to patent work — and the same approach can be adapted to any specialised domain.

You feed it your patents once. The AI decomposes them into a structured wiki — claims, embodiments, figures, technical effects, risks, use cases, prior art distinctions, and more. Every future conversation reads that wiki first. Answers get better the more you use it, because the knowledge compounds.

But ingestion is only the beginning. When you ask a question and get an answer you agree with — or one you refine and correct — that exchange does not disappear with the chat window. It is added to the wiki, linked to every relevant claim, effect, and risk page it touches. The next person who asks a related question gets the benefit of that answer. Zero extra effort. Zero manual curation. One place, one portfolio, one growing body of knowledge — not a hundred individual chat windows that evaporate.

Traditional wikis take enormous effort to create and even more to maintain. This one builds itself through use.

---

## What It Is

A folder you clone onto your computer. It contains:

- A set of instructions that tell Claude exactly how to analyse a patent
- A place to drop your patent documents
- A wiki that Claude builds and fills as you work

It runs as a Claude plugin — currently supported in Claude.ai (Projects), Claude Code, and in the future any application that supports Claude plugins. You bring your patents. The wiki grows from there.

---

## How It Works

```
patents/
  .raw/        ← you drop patent files here (never touched after that)
  wiki/        ← Claude builds this; it grows with every ingestion and question
  CLAUDE.md    ← the schema and rules that govern everything
```

**CLAUDE.md** is the brain of the system. It defines the page types, relationship types, how statements must be classified, and what Claude must do when you say "ingest" or "query". You can customise it for your specific portfolio.

**`.raw/`** is a locked archive. Patent source documents go in and are never modified.

**`wiki/`** is the living knowledge base. It is made of plain Markdown files, organised into typed subfolders — claims, embodiments, effects, risks, use cases, terms, figures, and more. These files are excluded from git (they are yours, not shared with the public repo). The folder structure is committed; the content is local.

### Ingestion: Two Passes

Patent analysis is too large for a single AI pass. Ingestion is split into two steps:

**Pass A — Decomposition**

Claude reads the entire patent from `.raw/` and extracts everything: metadata, every claim (broken into elements), every figure and its components, every technical term, every embodiment, every disclosed effect, every example. It writes a structured decomposition file to `wiki/meta/`. You review it before anything is committed to the wiki.

**Pass B — Structuring**

Claude reads the decomposition and creates all the wiki pages: claim pages with element support chains, figure pages, embodiment pages, effect pages, use-case pages, prior-art distinction pages, risk pages, term pages, and more. Every statement is classified as directly disclosed, synthesised, or inferred. Every relationship is recorded on both sides.

After Pass B, your wiki has a complete structured picture of the patent. Future questions read the wiki rather than re-reading the raw patent.

---

## Prerequisites

You need two things installed before you start.

### 1. A Claude-enabled application

This runs as a Claude plugin. You will need one of the following:

- **Claude Code** — Anthropic's AI assistant for your terminal and IDE. Install from **https://claude.ai/code**
- **Claude.ai Projects** — the web interface at **https://claude.ai**, using a Project with the repository files uploaded
- Any future application that supports Claude plugins

All options require an Anthropic account.

### 2. Obsidian (optional but recommended)

Obsidian is a free note-taking app that reads Markdown files. The wiki is plain Markdown, so it works without Obsidian — but Obsidian adds a graph view that makes the relationships between pages visible as a network.

Install it from: **https://obsidian.md**

---

## Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/your-username/LLM-Patent-Wiki.git
cd LLM-Patent-Wiki
```

### 2. Open the folder in your Claude-enabled application

**Claude Code (terminal):** run `claude` from inside the `LLM-Patent-Wiki` folder. The `CLAUDE.md` instructions are picked up automatically.

**Claude Code (VS Code):** open the `LLM-Patent-Wiki` folder in VS Code with the Claude Code extension active.

**Claude.ai Projects:** create a new Project and upload the repository files. The `CLAUDE.md` file will be read as project instructions.

### 3. Drop your patent into `.raw/`

Copy your patent document into the `.raw/` folder. Patents should be in plain text (`.txt`) or Markdown (`.md`) format — these are recommended as Claude reads them directly. The filename should be the patent number, for example:

```
.raw/US20260072718A1.md
```

### 4. Tell Claude to ingest it

In the Claude Code chat, type:

```
ingest US20260072718A1
```

Claude will run Pass A. It will read the entire patent and write a decomposition to `wiki/meta/decomposition-US20260072718A1.md`. It will then pause and ask you to review before continuing.

### 5. Review and continue

Open the decomposition file and check that the extraction looks correct. When you are ready:

```
continue to Pass B
```

Claude will build all the wiki pages. When it finishes, it will tell you how many pages were created.

### 6. Ask questions

Once ingested, you can ask questions in plain language:

```
What are the independent claims in US20260072718A1?
```

```
What are the enablement risks in the sandboxed execution claim?
```

```
What use cases does this patent support?
```

Claude reads the wiki before answering. It does not re-read the raw patent each time.

---

## Customising for Your Portfolio

`CLAUDE.md` defines everything. You can edit it to match your specific needs without changing any code.

Common customisations:

**Add your own taxonomy**
If your portfolio has consistent themes (e.g. "inference acceleration", "privacy-preserving ML"), you can add a section to `CLAUDE.md` that tells Claude to tag and cross-reference pages with your taxonomy. Every subsequent ingestion will apply it automatically.

**Add portfolio-level questions**
Add a section called `## Standing Questions` with questions you always want answered for every new patent — e.g. "Does this patent cover our flagship product?" or "What is the narrowest claim that still covers X?" Claude will answer these during Pass B.

**Add competitor references**
Add a section listing known prior art or competitor products. Claude will check each new patent against them during ingestion and create prior-art delta pages.

**Change the page structure**
Each page type has a template in `_templates/`. Edit the templates to add or remove sections. The templates are reference scaffolding — Claude uses them as a guide when creating pages.

---

## Directory Structure

```
LLM-Patent-Wiki/
│
├── .raw/                        ← drop patent files here
│   └── US20260072718A1.md       ← example: one patent per file
│
├── wiki/                        ← built by Claude, local only (not in public git)
│   ├── index.md                 ← master catalog of all pages
│   ├── log.md                   ← append-only record of every operation
│   ├── hot.md                   ← short summary of recent context
│   ├── overview.md              ← executive summary of the whole portfolio
│   │
│   ├── sources/                 ← one page per patent (invention overview)
│   ├── claims/                  ← one page per independent claim
│   ├── embodiments/             ← concrete implementation variants
│   ├── figures/                 ← one page per drawing
│   ├── effects/                 ← technical effects and what produces them
│   ├── use-cases/               ← real-world deployment scenarios
│   ├── prior-art-deltas/        ← feature-by-feature distinctions from prior art
│   ├── fallbacks/               ← narrower claim positions already disclosed
│   ├── risks/                   ← enablement gaps, breadth issues
│   ├── terms/                   ← canonical technical term glossary
│   ├── parameters/              ← specific values, ranges, thresholds
│   ├── evidence/                ← examples, tests, prototypes
│   ├── comparisons/             ← cross-patent analysis
│   ├── questions/               ← answers to questions you have filed
│   └── meta/                    ← decompositions, dashboards, lint reports
│
├── _templates/                  ← page templates (reference scaffolding for Claude)
├── skills/                      ← Claude skill definitions
├── agents/                      ← Claude agent definitions
├── commands/                    ← Claude slash commands
├── hooks/                       ← automation hooks
│
└── CLAUDE.md                    ← schema, rules, and instructions (edit this)
```

---

## Viewing the Wiki in Obsidian

The wiki is plain Markdown, but Obsidian's graph view makes the relationships between pages visible as a network. This is useful for understanding how claims, embodiments, effects, and risks connect across your portfolio.

### Open the vault

1. Open Obsidian
2. Click **Open folder as vault**
3. Select the `LLM-Patent-Wiki` folder (the root of this repository)
4. Obsidian will index all the Markdown files

### View the graph

1. Click the graph icon in the left sidebar, or press `Ctrl+G` (Windows/Linux) / `Cmd+G` (Mac)
2. Each node is a wiki page. Each edge is a relationship (wikilinks in the page body)
3. Nodes cluster by folder — claims, embodiments, effects, risks each form their own group
4. Click any node to open that page

As you ingest more patents, the graph grows. Pages that share terms, effects, or prior art references will link across patents, making cross-portfolio relationships visible without any manual effort.

---

## The Skills (What Claude Knows How To Do)

This repository includes pre-written skill definitions in `skills/`. These tell Claude how to perform each operation. You do not need to configure them — Claude Code picks them up automatically.

| You say | What happens |
|---|---|
| `ingest [patent]` | Runs Pass A decomposition |
| `continue to Pass B` | Runs Pass B structuring |
| `query: [question]` | Reads the wiki and answers |
| `lint` | Checks the wiki for gaps and broken cross-references |
| `scaffold` | Sets up the wiki folder structure from scratch |

---

## What Gets Shared vs What Stays Local

The public repository contains the scaffold: instructions, templates, skill definitions, and the empty folder structure. It contains no patent content.

Your patent documents (`.raw/`) and your generated wiki (`wiki/`) are excluded from git. They live only on your machine. If you push to GitHub, no patent content is ever uploaded.

This means you can share the tool publicly while keeping your portfolio private.

---

## Credits

This project is built on [Andrej Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) concept: the observation that LLMs should not rediscover knowledge from scratch on every conversation, and that a persistent, structured wiki — built and maintained by the LLM itself — can fix this. The patent domain is one where this matters enormously.

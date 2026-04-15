---
name: patent-ingest
description: >
  Two-pass patent ingestion into the Obsidian wiki vault. Pass A reads the full
  patent and produces a structured technical decomposition. Pass B converts the
  decomposition into typed wiki pages with cross-references. Supports single
  patent and batch mode.
  Triggers on: ingest, process this patent, add this to the wiki, pass A, pass B,
  continue to pass B, batch ingest.
allowed-tools: Read Write Edit Glob Grep Bash
---

# patent-ingest: Two-Pass Patent Ingestion

Read the patent. Decompose it. Structure it. Cross-reference everything.

**Syntax standard**: Write all Obsidian Markdown using proper Obsidian Flavored Markdown. Wikilinks as `[[Note Name]]`, callouts as `> [!type] Title`, properties as YAML frontmatter.

---

## Delta Tracking

Before ingesting any file, check `.raw/.manifest.json` to avoid re-processing.

```bash
[ -f .raw/.manifest.json ] && echo "exists" || echo "no manifest yet"
```

**Manifest format** (create if missing):
```json
{
  "sources": {
    ".raw/US20260072718A1.md": {
      "hash": "abc123",
      "ingested_at": "2026-04-14",
      "pass_a_completed": true,
      "pass_b_completed": true,
      "patent_number": "US20260072718A1",
      "claims_extracted": 21,
      "figures_extracted": 36,
      "pages_created": ["wiki/sources/US20260072718A1.md"],
      "pages_updated": ["wiki/index.md"]
    }
  }
}
```

**Before ingesting**: Compute hash, check manifest. If hash matches and both passes complete, skip (unless `force`).
**After each pass**: Update manifest with pass completion status and pages created/updated.

---

## Pass A: Technical Decomposition

Trigger: `ingest [patent-number]` or `ingest .raw/US20260072718A1.md` or `pass A [patent-number]`

### Purpose

Read the entire patent and extract ALL technical content into a structured decomposition file. This is the "knowledge compilation" step.

### Steps

1. **Read** the entire patent file from `.raw/`. Do not skim. Read every paragraph.

2. **Check manifest** for prior ingestion. Skip if hash matches (unless `force`).

3. **Extract metadata**:
   - Patent number, title, inventors, assignee
   - Filing date, publication date, priority date
   - Application number, classifications, status

4. **Extract claims** — for each claim:
   - Claim number
   - Type: independent or dependent
   - If dependent: which claim it depends on
   - Decompose into discrete elements (one per functional limitation)
   - For each element: mark as required or optional
     - "comprising" = required
     - "may", "can", "in some embodiments", "optionally" = optional
   - Note the claim category: method, system, or computer-readable medium

5. **Extract figures** — for each FIG reference:
   - Figure number (e.g., FIG. 1, FIG. 2A)
   - Figure type: block-diagram, flow-diagram, UI, data-chart, architecture, sequence-diagram
   - All labeled components with reference numbers (e.g., "generative response engine 101")
   - Paragraph range in description that discusses this figure

6. **Extract technical terms**:
   - Every term that is defined explicitly ("As used herein...") or implicitly (first use with explanation)
   - Canonical form
   - Aliases (alternative names used in the patent)
   - Paragraph where defined
   - Which claims use this term

7. **Extract system components**:
   - Every named component, module, engine, subsystem, interface
   - Reference number if assigned
   - Function/role
   - Connections to other components

8. **Extract process steps**:
   - Every method step described in claims or description
   - Order/sequence
   - Input/output for each step

9. **Extract control/data flows**:
   - How data moves between components
   - What triggers each step
   - Decision points and branching logic

10. **Extract parameters and ranges**:
    - Any specific values, thresholds, ranges, conditions
    - Whether claim-level or description-only
    - "Preferably" vs absolute limits

11. **Extract technical problems**:
    - What the Background section identifies as limitations of prior art
    - What problems the invention solves
    - Paragraph references (typically [0003]-[0005])

12. **Extract technical effects**:
    - What advantages or results the invention achieves
    - Found in: Summary section, Detailed Description, claim preambles
    - Paragraph references

13. **Extract embodiments**:
    - Each distinct implementation variant
    - Required vs optional features
    - Components and configuration
    - Supporting paragraph references
    - Which figures illustrate this embodiment

14. **Extract examples/tests/results**:
    - Any concrete demonstrations
    - Performance data, benchmarks
    - Test conditions and results

15. **Extract drawing references**:
    - Complete mapping of reference numbers to component names
    - Which figure each reference number appears in

### Output

Write the structured decomposition to `wiki/meta/decomposition-{patent-number}.md`:

```markdown
---
type: meta
title: "Decomposition: {patent-number}"
patent: "{patent-number}"
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags:
  - meta
  - decomposition
status: developing
---

# Technical Decomposition: {patent-number}

## Metadata

| Field | Value |
|-------|-------|
| Patent Number | US20260072718A1 |
| Title | ... |
| Inventors | ... |
| Assignee | ... |
| Filing Date | ... |
| Publication Date | ... |
| Classifications | ... |
| Total Claims | ... |
| Independent Claims | ... |

## Claims

### Independent Claim {N}

**Category**: method | system | computer-readable medium

**Elements**:
1. [Element text] — Required
2. [Element text] — Required
3. [Element text] — Optional

**Dependent claims**: {list}

[Repeat for each independent claim]

### Dependent Claims

| Claim | Depends On | Adds |
|-------|-----------|------|
| 2 | 1 | [additional limitation] |

## Figures

### FIG. {N}

**Type**: block-diagram
**Paragraph range**: [0054]-[0058]

**Components**:
| Ref # | Name | Description |
|-------|------|-------------|
| 101 | generative response engine | ... |
| 102 | code interpreter | ... |

[Repeat for each figure]

## Technical Terms

| Term | Aliases | Defined At | Used in Claims |
|------|---------|-----------|---------------|
| code interpreter | code execution tool | [0056] | 21, 33, 39 |

## System Components

| Component | Ref # | Function | Connections |
|-----------|-------|----------|------------|
| generative response engine | 101 | ... | sends to 102 |

## Process Steps

| Step | Description | Input | Output |
|------|-------------|-------|--------|
| 1 | Receive input | user query + files | input data |
| 2 | Determine code need | input data | decision |

## Control/Data Flows

[Describe data flow between components]

## Parameters and Ranges

| Parameter | Values | Claim Refs | Paragraph Refs |
|-----------|--------|-----------|---------------|
| execution env type | sandboxed, firewalled | 33, 34 | [0056] |

## Technical Problems

| Problem | Paragraph | Summary |
|---------|-----------|---------|
| Limited multimodal processing | [0003] | ... |

## Technical Effects

| Effect | Paragraph | Summary |
|--------|-----------|---------|
| Automated code execution | [0008] | ... |

## Embodiments

### Embodiment: {Name}

**Type**: system | method | variant
**Optional**: yes | no
**Paragraph refs**: [0054]-[0060]
**Illustrated by**: FIG. 1

**Components**: [list]
**Configuration**: [description]

## Examples / Tests / Results

| Type | Description | Paragraph |
|------|-------------|-----------|
| example | OCR processing | [0063] |

## Drawing Reference Map

| Ref # | Name | Figures |
|-------|------|---------|
| 101 | generative response engine | FIG. 1, FIG. 2 |
| 102 | code interpreter | FIG. 1 |
```

### After Writing Decomposition

1. Update manifest: set `pass_a_completed: true`
2. Update `wiki/hot.md` with decomposition summary
3. Append to `wiki/log.md`
4. **Pause and tell the user**:
   > Decomposition complete for {patent-number}. Review `wiki/meta/decomposition-{patent-number}.md` and say "continue to Pass B" when ready.

---

## Pass B: Patent Structuring

Trigger: `continue to Pass B`, `pass B [patent-number]`, or after user confirms Pass A

### Purpose

Convert the structured decomposition into typed wiki pages with full cross-references and statement classification.

### Prerequisites

- Pass A decomposition file must exist at `wiki/meta/decomposition-{patent-number}.md`
- Read the decomposition file. Do NOT re-read the raw patent.

### Steps

#### 1. Create Source Page

File: `wiki/sources/{patent-number}.md`

- Invention overview: problem, solution core, differentiators, broad inventive concept
- Full metadata from decomposition
- Summary of all claims, embodiments, figures
- Use frontmatter schema from `references/frontmatter.md` (source type)

#### 2. Create Claim Pages

File: `wiki/claims/{patent-number} - Claim {N}.md` (one per independent claim)

For each independent claim:
- Full verbatim claim text
- Claim elements table: element text, required/optional, classification [D]
- Element support chains: for each element, link to:
  - Disclosure text (paragraph ref) — find in decomposition
  - Embodiment that implements it — create link
  - Figure that illustrates it — create link
  - Technical effect — create link
  - Fallback position — create link if applicable
- Dependent claims as subsections
- Deployment chain:
  - Use case link
  - System context
  - Hardware/software environment
  - Actors
  - Measurable effect
  - Commercial embodiment possibility

Apply statement classification to every assertion.

#### 3. Create Embodiment Pages

File: `wiki/embodiments/{patent-number} - Embodiment {Name}.md`

One per distinct embodiment from decomposition:
- Description, components, configuration
- Optional vs required marking
- Links to claims it implements
- Links to figures that illustrate it
- Paragraph references

#### 4. Create Figure Pages

File: `wiki/figures/{patent-number} - FIG {N}.md`

One per figure from decomposition:
- Figure description and type
- Component table: ref #, name, description, [D] classification
- Links to claims this figure illustrates
- Links to embodiments this figure depicts
- Paragraph references

#### 5. Create Technical Effect Pages

File: `wiki/effects/{patent-number} - Effect {Name}.md`

One per significant technical effect from decomposition:
- Effect description
- Which features/claim elements produce it
- How it is measured (link to parameter if applicable)
- Evidence (link to evidence page if applicable)
- Statement classification

#### 6. Create Use-Case Pages

File: `wiki/use-cases/{patent-number} - UseCase {Name}.md`

Create at least one per independent claim:
- Real-world deployment scenario
- System boundaries, actors, operating conditions
- Required hardware/software environment
- Links to implementing claims
- Measurable practical effects
- Possible commercial embodiment
- Most use-case content will be [S] or [I] — classify accordingly

#### 7. Create Prior-Art Delta Pages

File: `wiki/prior-art-deltas/{patent-number} - vs {Prior Art}.md`

Based on Background section analysis:
- Feature-by-feature distinction table
- Key differentiators
- Paragraph references

#### 8. Create Fallback Position Pages

File: `wiki/fallbacks/{patent-number} - Fallback {Name}.md`

Identify narrower claim combinations already disclosed:
- What narrowing feature is added
- Which broader claim this narrows from
- Paragraph support
- Typical fallbacks: dependent claim features that could narrow independent claims

#### 9. Create Risk Pages

File: `wiki/risks/{patent-number} - Risk {Name}.md`

Identify risks for each independent claim:
- Risk type: breadth, ambiguity, enablement, definitional
- Severity: low, medium, high
- Which claims affected
- Mitigation path (usually a fallback)
- Risks are inherently [S] or [I]

#### 10. Create Term Pages

File: `wiki/terms/{Term Name}.md`

For significant technical terms:
- Canonical definition from patent
- Aliases
- Claim and paragraph usage
- Cross-patent: if term already exists from another patent, update the existing page

#### 11. Create Parameter Pages

File: `wiki/parameters/{patent-number} - Param {Name}.md`

Only for parameters with specific disclosed values:
- Values/ranges
- Claim and paragraph references

#### 12. Create Evidence Pages

File: `wiki/evidence/{patent-number} - Evidence {Name}.md`

For examples, tests, prototypes:
- Description, supporting text
- Which claims/effects this supports

### Cross-Reference Validation

After creating all pages, verify the two required patterns:

**Pattern A — Claim Element Support Chain**:
For every claim element on every claim page:
- [ ] At least one disclosure text link (paragraph ref)
- [ ] At least one embodiment link
- [ ] At least one figure/region link
- [ ] Technical effect noted (or "none identified")
- [ ] Fallback noted (or "none identified")

If any link is missing, add `> [!gap] Missing [type] link for this element`.

**Pattern B — Claim Practical Deployment Chain**:
For every independent claim:
- [ ] At least one use-case link
- [ ] System context documented
- [ ] Hardware/software environment noted
- [ ] Actors identified
- [ ] Measurable practical effect linked
- [ ] Commercial embodiment noted

If any is missing, create a stub page or add `> [!gap]`.

### Index and Metadata Updates

After all pages are created:

1. **Update sub-indexes**: Add entries to every affected `_index.md`
2. **Update master index**: `wiki/index.md` — add all new pages under their sections
3. **Update overview**: `wiki/overview.md` — mark patent as ingested, update statistics
4. **Update dashboard**: `wiki/meta/dashboard.md` — update page counts and ingestion status
5. **Update hot cache**: `wiki/hot.md` — summarize what was created
6. **Append log**: `wiki/log.md` (new entry at TOP):
   ```markdown
   ## YYYY-MM-DD ingest-pass-b | {patent-number}
   - Source: `.raw/{patent-number}.md`
   - Decomposition: [[Decomposition: {patent-number}]]
   - Pages created: [count] ({list of page types})
   - Cross-reference gaps: [count] (see [!gap] callouts)
   - Key insight: [one sentence]
   ```
7. **Update manifest**: Set `pass_b_completed: true`, record pages_created and pages_updated

### Report to User

After Pass B completes:
```
Patent {patent-number} fully ingested.

Pass B created {N} pages:
  - {X} claim pages
  - {X} embodiment pages
  - {X} figure pages
  - {X} effect pages
  - {X} use-case pages
  - {X} prior-art delta pages
  - {X} fallback pages
  - {X} risk pages
  - {X} term pages
  - {X} parameter pages
  - {X} evidence pages

Cross-reference gaps: {N} (marked with [!gap] callouts)
Recommend running patent-lint to verify completeness.
```

---

## Batch Ingest

Trigger: "ingest all", "batch ingest"

Steps:
1. List all `.raw/*.md` files not yet in manifest (or with different hash).
2. Confirm with user before starting.
3. Process each patent: Pass A, pause for review, Pass B.
4. After all patents: cross-reference pass for shared terms and cross-patent connections.
5. Update all indexes once at the end.

---

## Context Window Discipline

- Each raw patent is 112K-160K chars (~30-40K tokens). Pass A consumes most of the context.
- **One patent per session** for Pass A. Pass B can use the decomposition file (much smaller).
- Do not re-read the raw patent during Pass B. Use the decomposition file.
- If running Pass A and Pass B in the same session, be aware of context limits.

---

## What Not to Do

- Do not modify anything in `.raw/`
- Do not create pages without statement classification
- Do not skip cross-reference validation
- Do not skip the log entry or hot cache update
- Do not create duplicate pages (check indexes first)
- Do not classify statements as [D] without citing a paragraph number

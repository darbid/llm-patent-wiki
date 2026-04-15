# Patent Wiki: LLM-Powered Patent Analysis

Purpose: Structured patent analysis, cross-referencing, and knowledge compilation for patent portfolios
Created: 2026-04-14

## Architecture

Three layers:

```
patents/
  .raw/        # Layer 1: immutable source patent documents
  wiki/        # Layer 2: LLM-generated patent knowledge base
  CLAUDE.md    # Layer 3: schema and instructions (this file)
```

## Structure

```
wiki/
  index.md              # master catalog of all pages
  log.md                # append-only operation log
  hot.md                # session hot cache (~500 words)
  overview.md           # portfolio executive summary
  sources/              # one page per patent (invention overview)
  claims/               # one page per independent claim (+ dependents)
  embodiments/          # concrete implementations, variants
  figures/              # one page per figure
  effects/              # technical effect pages
  use-cases/            # real-world deployment scenarios
  prior-art-deltas/     # feature-by-feature distinctions
  fallbacks/            # narrower claim positions
  risks/                # enablement gaps, breadth issues
  terms/                # canonical technical term glossary
  parameters/           # specific values, ranges, thresholds
  evidence/             # examples, tests, prototypes
  comparisons/          # cross-patent analysis
  questions/            # filed query answers
  meta/                 # dashboards, lint reports, decompositions
```

Every subfolder has a `_index.md` sub-index.

## Page Types (14)

| Type | Folder | Purpose |
|------|--------|---------|
| source | sources/ | Invention overview: problem, solution, differentiators |
| claim | claims/ | One per independent claim with dependent claim subsections |
| embodiment | embodiments/ | Concrete implementation variants |
| figure | figures/ | One per drawing with component tables |
| effect | effects/ | Technical effects linked to producing features |
| use-case | use-cases/ | Real-world deployment scenarios |
| prior-art-delta | prior-art-deltas/ | Feature-by-feature distinction from prior art |
| fallback | fallbacks/ | Narrower claim positions already disclosed |
| risk | risks/ | Unsupported breadth, ambiguous terms, enablement gaps |
| term | terms/ | Canonical technical term definitions |
| parameter | parameters/ | Specific values, ranges, thresholds |
| evidence | evidence/ | Examples, tests, prototypes, field results |
| comparison | comparisons/ | Cross-patent or cross-feature analysis |
| question | questions/ | Filed answers to user queries |

## Node Types (13)

Claim, Claim Element, Embodiment, Figure, Technical Effect, Use Case, Term, Parameter, Prior Art Reference, Fallback Position, Risk, Evidence, Commercial Product / Workflow

## Relationship Types (13)

| Relationship | Frontmatter Field | Inverse Field |
|---|---|---|
| supports | `supports` | `supported_by` |
| illustrated_by | `illustrated_by` | `illustrates` |
| implemented_by | `implemented_by` | `implements` |
| optional_in | `optional_in` | `optional_features` |
| required_for | `required_for` | `requires` |
| produces_effect | `produces_effect` | `produced_by` |
| distinguished_from | `distinguished_from` | `distinguished_from` |
| narrows_to | `narrows_to` | `narrows_from` |
| depends_on | `depends_on` | `depended_on_by` |
| measured_by | `measured_by` | `measures` |
| used_in | `used_in` | `uses` |
| at_risk_from | `at_risk_from` | `affects_claim` |
| evidenced_by | `evidenced_by` | `evidence_for` |

Both sides of every relationship are recorded in frontmatter.

## Statement Classification

Every extracted statement carries one of three classifications:

- **[D] Disclosed**: Verbatim or near-verbatim from patent text. Must cite paragraph number.
- **[S] Attorney-drafted synthesis**: Reasonable combination of disclosed elements. Must cite component paragraphs.
- **[I] Inference / hypothesis**: Analytical conclusion not directly derivable from text. Always flagged.

Block format uses callouts: `> [!disclosed]`, `> [!synthesis]`, `> [!inference]`
Inline format uses brackets: `[D]`, `[S]`, `[I]`

This single distinction prevents silent overreach in the knowledge base.

## Cross-Reference Patterns

### A. Claim Element Support Chain

Every claim element must link to:
- Supporting disclosure text (paragraph reference)
- At least one embodiment
- At least one figure or figure region
- Any technical effect it contributes to
- Any fallback narrowing already disclosed

Missing links get a `> [!gap]` callout.

### B. Claim Practical Deployment Chain

Every independent claim must link to:
- Real-world use cases
- Required system context
- Typical hardware/software environment
- External actors or interfaces
- Measurable practical effect
- Possible commercial embodiments

## Conventions

- All notes use flat YAML frontmatter (no nested objects)
- Wikilinks use `[[Note Name]]` format; filenames are unique, no paths needed
- Patent numbers prefix all page names: `US20260072718A1 - Claim 21`
- `.raw/` is immutable: never modify source documents
- `wiki/index.md` is the master catalog: update on every ingest
- `wiki/log.md` is append-only: new entries at the TOP, never edit past entries
- Callouts: `[!disclosed]`, `[!synthesis]`, `[!inference]`, `[!contradiction]`, `[!gap]`, `[!key-insight]`
- Cross-references recorded in BOTH frontmatter and body text
- Relationships are bidirectional: if page A links to B, B links back to A
- Pages should stay under 300 lines; split if longer
- Status values: seed, developing, mature, evergreen

## Operations

| User says | Operation | Skill |
|-----------|-----------|-------|
| "scaffold", "set up wiki" | SCAFFOLD | patent-wiki |
| "ingest [patent]", "pass A", "pass B" | INGEST | patent-ingest |
| "what do you know about X", "query:" | QUERY | patent-query |
| "lint", "health check" | LINT | patent-lint |
| "save this", "/save" | SAVE | save |

## Two-Pass Ingestion

- **Pass A** (Technical Decomposition): Reads the entire patent. Extracts metadata, claims, figures, terms, components, steps, flows, parameters, problems, effects, embodiments, examples, drawing references. Writes structured output to `wiki/meta/decomposition-{patent-number}.md`. Pauses for user review.
- **Pass B** (Patent Structuring): Reads the decomposition file. Creates all wiki pages. Validates cross-reference patterns. Updates indexes, hot cache, log, manifest.

One patent per ingestion session.

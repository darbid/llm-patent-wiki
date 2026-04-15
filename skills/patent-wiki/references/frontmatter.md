# Frontmatter Schema

Every wiki page starts with flat YAML frontmatter. No nested objects. Obsidian's Properties UI requires flat structure.

---

## Universal Fields

Every page, no exceptions:

```yaml
---
type: <source|claim|embodiment|figure|effect|use-case|prior-art-delta|fallback|risk|term|parameter|evidence|comparison|question|meta>
title: "Human-Readable Title"
patent: "US20260072718A1"
created: 2026-04-14
updated: 2026-04-14
tags:
  - <type-tag>
  - <patent-number>
status: <seed|developing|mature|evergreen>
related:
  - "[[Other Page]]"
sources:
  - "[[.raw/US20260072718A1.md]]"
---
```

**status values:**
- `seed`: exists, barely populated
- `developing`: has real content, not yet complete
- `mature`: comprehensive, well-linked
- `evergreen`: unlikely to need updates

---

## Type-Specific Additions

### source

```yaml
patent_number: "US20260072718A1"
application_number: "US19/392,965"
assignee: "OpenAI Opco LLC"
inventors:
  - "Jerry Tworek"
  - "Nicholas Turley"
priority_date: 2025-11-18
filing_date: 2025-11-18
publication_date: 2026-03-12
patent_status: pending
classification_primary: "G06F9/45508"
total_claims: 21
independent_claims:
  - 21
  - 39
  - 40
```

### claim

```yaml
claim_number: 21
claim_type: independent
depends_on: []
claim_elements:
  - "receiving input"
  - "determining code requirement"
  - "generating code"
supported_by:
  - "[[US20260072718A1 - Embodiment Sandbox Execution]]"
illustrated_by:
  - "[[US20260072718A1 - FIG 1]]"
produces_effect:
  - "[[US20260072718A1 - Effect Code Execution]]"
fallback_to:
  - "[[US20260072718A1 - Fallback No Network Access]]"
at_risk_from:
  - "[[US20260072718A1 - Risk Broad Code Determination]]"
```

### embodiment

```yaml
embodiment_type: system
optional: false
implements:
  - "[[US20260072718A1 - Claim 21]]"
illustrated_by:
  - "[[US20260072718A1 - FIG 1]]"
components:
  - "generative response engine"
  - "code interpreter"
paragraph_refs:
  - "[0054]"
  - "[0055]"
```

### figure

```yaml
figure_number: "FIG. 1"
figure_type: block-diagram
components_shown:
  - "generative response engine 101"
  - "code interpreter 102"
illustrates_claims:
  - "[[US20260072718A1 - Claim 21]]"
illustrates_embodiments:
  - "[[US20260072718A1 - Embodiment System Architecture]]"
paragraph_refs:
  - "[0054]"
```

### effect

```yaml
effect_type: technical
produced_by:
  - "[[US20260072718A1 - Claim 21]]"
measured_by:
  - "[[US20260072718A1 - Parameter Error Recovery Iterations]]"
evidenced_by:
  - "[[US20260072718A1 - Evidence OCR Example]]"
```

### use-case

```yaml
actors:
  - "end user"
  - "multimodal ML model"
system_context: "cloud-hosted ML platform"
hardware_env: "server with GPU, client device with browser"
implements_claim:
  - "[[US20260072718A1 - Claim 36]]"
measurable_effect:
  - "[[US20260072718A1 - Effect Data Analysis]]"
```

### prior-art-delta

```yaml
prior_art_ref: "Conventional multimodal ML models"
distinguished_features:
  - "code generation and execution"
  - "sandboxed environment"
prior_art_paragraph: "[0003]"
```

### fallback

```yaml
narrows_from:
  - "[[US20260072718A1 - Claim 21]]"
narrowing_feature: "sandboxed execution without network access"
supported_by_disclosure: true
paragraph_refs:
  - "[0056]"
```

### risk

```yaml
risk_type: breadth
affects_claim:
  - "[[US20260072718A1 - Claim 21]]"
severity: medium
mitigation: "Narrow to sandboxed execution (Claim 33)"
```

### term

```yaml
canonical_form: "code interpreter"
aliases:
  - "code execution tool"
  - "language interpreter"
defined_at: "[0056]"
used_in_claims:
  - 21
  - 33
```

### parameter

```yaml
parameter_name: "execution environment type"
values:
  - "sandboxed"
  - "firewalled"
claim_refs:
  - 33
  - 34
paragraph_refs:
  - "[0056]"
```

### evidence

```yaml
evidence_type: example
supports:
  - "[[US20260072718A1 - Claim 36]]"
paragraph_refs:
  - "[0063]"
```

### comparison

```yaml
subjects:
  - "[[US20260072718A1]]"
  - "[[US20260087598A1]]"
dimensions:
  - "claim scope"
  - "ML architecture"
verdict: "One-line conclusion."
```

### question

```yaml
question: "The original query as asked."
answer_quality: draft
```

---

## Rules

1. Use flat YAML only. Never nest objects.
2. Dates as `YYYY-MM-DD` strings, not ISO datetime.
3. Lists always use the `- item` format, not inline `[a, b, c]`.
4. Wikilinks in YAML fields must be quoted: `"[[Page Name]]"`.
5. Keep `related` and `sources` as wikilinks, not plain URLs.
6. Update `updated` every time you edit the page content.
7. Patent number prefix on all page names: `US20260072718A1 - Claim 21`.
8. Relationship fields must be bidirectional: if A links to B, B links to A.

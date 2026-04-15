# Pass B: Patent Structuring — Detailed Guide

This reference supplements the main patent-ingest skill with page creation guidance.

---

## Page Creation Order

Create pages in this order to ensure links resolve:

1. **Source page** first (everything links to this)
2. **Term pages** (claims reference terms)
3. **Figure pages** (claims and embodiments reference figures)
4. **Embodiment pages** (claims reference embodiments)
5. **Effect pages** (claims and embodiments reference effects)
6. **Claim pages** (the central node — links to everything above)
7. **Evidence pages** (referenced by claims and effects)
8. **Parameter pages** (referenced by effects)
9. **Use-case pages** (reference claims and effects)
10. **Prior-art delta pages** (reference claims)
11. **Fallback pages** (reference claims)
12. **Risk pages** (reference claims and fallbacks)

---

## Naming Conventions

All patent-specific pages use the patent number as a namespace prefix:

```
wiki/sources/US20260072718A1.md
wiki/claims/US20260072718A1 - Claim 21.md
wiki/embodiments/US20260072718A1 - Embodiment Sandbox Execution.md
wiki/figures/US20260072718A1 - FIG 1.md
wiki/effects/US20260072718A1 - Effect Safe Code Execution.md
wiki/use-cases/US20260072718A1 - UseCase Data Analysis.md
wiki/prior-art-deltas/US20260072718A1 - vs Conventional ML Models.md
wiki/fallbacks/US20260072718A1 - Fallback Sandboxed Only.md
wiki/risks/US20260072718A1 - Risk Broad Code Determination.md
wiki/parameters/US20260072718A1 - Param Execution Environment.md
wiki/evidence/US20260072718A1 - Evidence OCR Example.md
```

**Exception**: Term pages do NOT use patent prefix (they aggregate cross-patent):
```
wiki/terms/Code Interpreter.md
wiki/terms/Multimodal Machine Learning Model.md
```

**Naming rules**:
- Use Title Case with spaces
- Keep names descriptive but concise (under 60 chars)
- Embodiment names describe the distinguishing feature
- Effect names describe the result, not the mechanism
- Use-case names describe the scenario
- Risk names describe the vulnerability

---

## Claim Page Structure (most complex page type)

The claim page is the central node of the patent wiki. It must be thorough.

### Claim Text Section

Copy the full verbatim claim text. This is always [D].

### Claim Elements Table

| # | Element | Required/Optional | Classification |
|---|---------|-------------------|---------------|
| 1 | "receiving, by a computing device, an input comprising at least one of text, an image, audio, video, or a file" | Required | [D] |
| 2 | "determining, using a multimodal machine learning model, whether code implementation is needed" | Required | [D] |

### Element Support Chains

For EACH element, provide:

```markdown
### Element 1: Receiving Input

> [!disclosed] Disclosure support
> "The generative response engine 101 can receive any of a variety of types of 
> input data from a user device 104." [0055]

**Embodiment**: [[US20260072718A1 - Embodiment System Architecture]] — The generative 
response engine receives input via API endpoint [D]

**Figure**: [[US20260072718A1 - FIG 1]] — Components 101 and 104 show the input path [D]

**Effect**: [[US20260072718A1 - Effect Multimodal Processing]] — Enables processing 
of diverse input types [S]

**Fallback**: [[US20260072718A1 - Fallback Text Only Input]] — If narrowing is needed, 
the patent supports text-only input as a subset [S]
```

If a link cannot be established:
```markdown
**Figure**: 
> [!gap] No figure specifically illustrates the input receiving step.
```

### Deployment Chain

```markdown
## Deployment Chain

- **Use case**: [[US20260072718A1 - UseCase Data Analysis]] — An analyst uploads 
  a dataset and asks natural language questions [S]
- **System context**: Cloud-hosted ML platform with API access [S]
- **Hardware/software**: GPU-equipped server, web browser client [I]
- **Actors**: End user (data analyst), ML model, code interpreter [S]
- **Measurable effect**: [[US20260072718A1 - Effect Automated Analysis]] [S]
- **Commercial embodiment**: ChatGPT Code Interpreter feature [I]
```

---

## Bidirectional Cross-Reference Procedure

When creating a link from page A to page B, you MUST also add the inverse link on page B.

**Example**:
Creating claim page that links to embodiment:
1. On claim page: `supported_by: ["[[US20260072718A1 - Embodiment X]]"]`
2. On embodiment page: `implements: ["[[US20260072718A1 - Claim 21]]"]`

**Procedure during Pass B**:
- Keep a running list of all bidirectional links that need to be created
- After creating each page, note which existing pages need inverse links added
- After all pages are created, do a sweep to add all inverse links
- The patent-lint skill will catch any missed bidirectional links

---

## Page Length Guidelines

| Page Type | Target Lines | Max Lines |
|-----------|-------------|-----------|
| Source | 100-200 | 300 |
| Claim (independent) | 150-250 | 300 |
| Embodiment | 80-150 | 250 |
| Figure | 50-100 | 150 |
| Effect | 50-80 | 150 |
| Use Case | 80-120 | 200 |
| Prior Art Delta | 50-80 | 150 |
| Fallback | 40-60 | 100 |
| Risk | 40-60 | 100 |
| Term | 30-50 | 100 |
| Parameter | 30-40 | 80 |
| Evidence | 30-60 | 100 |

If a page exceeds the max, split it. Common splits:
- Large claims: split "Element Support Chains" into a subpage
- Many embodiments: split into "core" and "variant" pages

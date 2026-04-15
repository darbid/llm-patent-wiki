# Pass A: Technical Decomposition — Detailed Guide

This reference supplements the main patent-ingest skill with detailed extraction guidance.

---

## Extraction Checklist

Use this as a checklist when processing each patent. Every item must be extracted or explicitly marked as "not present in this patent."

### Metadata Extraction

- [ ] Patent number (from header)
- [ ] Title (from title section)
- [ ] All inventors (from header)
- [ ] Assignee (from header)
- [ ] Filing date, publication date, priority date
- [ ] Application number
- [ ] Primary IPC/CPC classification
- [ ] Patent status (pending, granted, etc.)

### Claim Extraction

For EVERY claim:

- [ ] Claim number
- [ ] Independent or dependent
- [ ] If dependent: parent claim number
- [ ] Category: method, system, or computer-readable medium
- [ ] Full verbatim text
- [ ] Decomposed elements (one per functional limitation)
- [ ] Required/optional marking for each element

**How to decompose claims into elements**:

A claim like:
> "A method comprising: receiving, by a computing device, an input; determining, using a machine learning model, whether code implementation is needed; generating code based on the determination; and providing an output."

Decomposes into:
1. "receiving, by a computing device, an input" — Required
2. "determining, using a machine learning model, whether code implementation is needed" — Required
3. "generating code based on the determination" — Required
4. "providing an output" — Required

**Signals for optional elements**:
- "may further comprise" → optional
- "in some embodiments" → optional
- "optionally" → optional
- "can include" → optional
- "wherein" without "may" → typically a required refinement

### Figure Extraction

For EVERY figure mentioned:

- [ ] Figure number (FIG. 1, FIG. 2A, etc.)
- [ ] Figure type classification
- [ ] All labeled components with reference numbers
- [ ] Paragraph range discussing this figure
- [ ] Brief description (from "Brief Description of Drawings" section)

**Figure type classification**:
| Type | Characteristics |
|------|----------------|
| block-diagram | System architecture, boxes with arrows |
| flow-diagram | Sequential process steps |
| UI | User interface mockup |
| data-chart | Data visualization, tables |
| architecture | Network/system topology |
| sequence-diagram | Temporal message passing |
| state-diagram | State transitions |
| detail-view | Zoomed-in component view |

### Technical Term Extraction

Look for these patterns:
- "As used herein, '[term]' refers to..."
- "The term '[term]' may include..."
- "[Term] (also referred to as [alias])"
- First significant use with contextual definition

For each term:
- [ ] Canonical form (most common usage in claims)
- [ ] All aliases used in the patent
- [ ] Paragraph where first defined/explained
- [ ] Which claim numbers use this term
- [ ] Is it a term of art or a patent-specific definition?

### Component and Architecture Extraction

- [ ] Every named component (with reference number if assigned)
- [ ] Function/role of each component
- [ ] Data connections between components (what flows where)
- [ ] Hierarchy (which components contain other components)

### Embodiment Extraction

Markers to look for:
- "In one embodiment..."
- "In another embodiment..."
- "In some implementations..."
- "According to one aspect..."
- "In a preferred embodiment..."

For each embodiment:
- [ ] Descriptive name (you choose — based on distinguishing feature)
- [ ] Type: system, method, variant, optimization
- [ ] Required or optional
- [ ] Which components it includes
- [ ] Configuration specifics
- [ ] Supporting paragraph references
- [ ] Which figures illustrate it

### Problem and Effect Extraction

**Problems** (from Background section):
- Look for: "However,", "Unfortunately,", "A need exists for", "It would be desirable"
- [ ] Each problem identified
- [ ] Paragraph reference

**Effects** (from Summary, claims preamble, Detailed Description):
- Look for: "thereby", "resulting in", "advantageously", "enables", "improves"
- [ ] Each effect identified
- [ ] Which features produce it
- [ ] Paragraph reference

---

## Decomposition File Quality Criteria

The decomposition file is correct when:

1. Every claim is represented with all its elements
2. Every figure is listed with all its labeled components
3. Every technical term used in claims has an entry
4. The component map matches the figure labels
5. Problems from Background and Effects from Summary are captured
6. At least one embodiment is identified per independent claim
7. Parameter values are extracted where disclosed

If a section has nothing to extract (e.g., no examples/tests), write "Not disclosed in this patent" rather than omitting the section.

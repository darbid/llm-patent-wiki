# Statement Classification

Every statement extracted from a patent and written into the wiki must be classified into exactly one of three categories. This prevents silent overreach in the knowledge base.

---

## The Three Classes

### [D] Disclosed

The statement is **verbatim or near-verbatim** from the patent text.

**Requirements**:
- Must cite a specific paragraph number: `[0054]`
- The cited paragraph must actually contain the substance of the statement
- Paraphrasing is acceptable if the meaning is unchanged
- Combining two statements from the **same paragraph** is still [D]

**Examples**:
```markdown
> [!disclosed] The code interpreter executes code in a sandboxed environment
> The patent states: "The code interpreter 102 may execute the generated code in
> a sandboxed environment." [0056]

- **[D]** Input types include text, images, audio, and video — [0058]
- **[D]** The model uses a transformer architecture — [0042]
```

### [S] Attorney-drafted synthesis

The statement is a **reasonable combination** of disclosed elements that are not explicitly stated together as a unit in the patent text.

**Requirements**:
- Must cite the component paragraphs that support the synthesis
- The combination must be logically sound — not a stretch
- Useful for: claim element groupings, system-level descriptions, functional summaries

**Examples**:
```markdown
> [!synthesis] The system forms a three-stage pipeline: intent detection, code generation, and execution
> Intent detection is described at [0070], code generation at [0072], and execution
> at [0075]. The patent does not explicitly call this a "three-stage pipeline" but
> the sequential flow is clear from the description.

- **[S]** Combined with context analysis, this forms a complete data processing workflow — [0070]-[0078]
```

### [I] Inference / hypothesis

The statement is an **analytical conclusion** that cannot be directly derived from the patent text alone.

**Requirements**:
- Must always be flagged — never leave an inference unmarked
- Use when: drawing conclusions about implementation details not disclosed, comparing to external systems, predicting commercial applications, identifying risks
- All risk assessments are inherently [I] or [S]
- All use-case scenarios that go beyond what the patent explicitly describes are [I]

**Examples**:
```markdown
> [!inference] The code interpreter likely uses a container runtime
> Sandboxed execution with ephemeral disk and network isolation ([0056]) is
> consistent with container-based architectures, but no implementation detail
> is disclosed.

- **[I]** This approach would outperform traditional rule-based systems
- **[I]** The four processing steps likely execute as a single forward pass
```

---

## Classification Decision Tree

```
Is the statement verbatim or near-verbatim from a single paragraph?
  YES → [D] Disclosed. Cite the paragraph.

Is the statement a reasonable combination of elements from multiple paragraphs?
  YES → [S] Synthesis. Cite all component paragraphs.

Everything else → [I] Inference. Flag it.
```

---

## Special Cases

| Situation | Classification | Rationale |
|-----------|---------------|-----------|
| Claim text quoted verbatim | [D] | Direct from the patent |
| Two claim elements combined into a summary | [S] | Elements are disclosed, combination is synthesis |
| "The system would work well for X" | [I] | Application inference |
| Risk identification | [S] or [I] | Depends on whether the gap is visible in the text |
| Use case based on disclosed examples | [S] | Based on disclosed content |
| Use case extending beyond disclosure | [I] | Going beyond the text |
| Component described in one paragraph, function in another | [S] | Multi-paragraph combination |
| Industry comparison | [I] | External knowledge |

---

## Visual Rendering

**Block format** (use for significant statements):
```markdown
> [!disclosed] Statement title
> Body text with paragraph reference [0054]

> [!synthesis] Statement title
> Body text with component refs [0054]-[0058]

> [!inference] Statement title
> Body text explaining the reasoning
```

**Inline format** (use in tables, lists, compact sections):
```markdown
- **[D]** Statement text — [0054]
- **[S]** Statement text — [0054]-[0058]
- **[I]** Statement text
```

The CSS callouts render as: green (disclosed), blue (synthesis), amber (inference).

---

## Common Mistakes

1. **Unmarked statements**: Every factual assertion in the wiki must have a classification. If you're unsure, use [I].
2. **Over-claiming [D]**: If you're combining elements from different paragraphs, it's [S], not [D].
3. **Under-classifying [I]**: Risk assessments, commercial applications, and implementation predictions are always [I] unless explicitly disclosed.
4. **Missing citations**: [D] and [S] must always cite paragraph numbers. No exceptions.

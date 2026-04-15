---
name: patent-query
description: >
  Answer questions from the patent wiki. Reads hot cache, index, and specific
  pages to synthesize answers with citations and statement classification.
  Triggers on: "what do you know about", "query:", "find", "how does",
  "which claims", "what patents", "compare", "summarize".
allowed-tools: Read Write Edit Glob Grep
---

# patent-query: Patent Wiki Query

Answer from the wiki. Cite everything. Apply statement classification.

---

## Three Query Modes

| Mode | Trigger | Reads | Token Budget | Use |
|------|---------|-------|-------------|-----|
| **Quick** | `query quick:` or simple fact | hot.md + index | ~1,500 | "What patent covers code execution?" |
| **Standard** | default | hot.md + index + 3-5 pages | ~3,000 | Most questions |
| **Deep** | `query deep:` or "comprehensive", "compare all" | Full wiki scan | ~8,000+ | Cross-patent analysis, gap reports |

---

## Standard Query Workflow

1. **Read** `wiki/hot.md`. If it contains the answer, respond immediately.

2. **Read** `wiki/index.md` to identify 3-5 relevant pages.

3. **Read** those pages. Follow wikilinks to depth-2 for supporting context.

4. **Synthesize** answer with:
   - Citations: `(Source: [[Page Name]])` for every factual claim
   - Statement classification: mark assertions as [D], [S], or [I]
   - Cross-reference context: mention related pages the user might want to explore

5. **Offer to file**: "Worth saving as `wiki/questions/[name].md`?"

6. **Gap handling**: If the wiki doesn't have enough information:
   - Say clearly: "The wiki doesn't have coverage on [topic]."
   - Identify which patent(s) might contain relevant content
   - Suggest: "Want me to look at .raw/[patent].md directly?"

---

## Patent-Specific Query Types

### Claim Scope Questions

"What does Claim 21 cover?"
→ Read the claim page. Present elements, support chains, deployment chain.

### Cross-Reference Questions

"What supports the 'sandboxed execution' element?"
→ Follow the support chain: embodiment, figure, effect, fallback.

### Risk Questions

"What are the risks for US20260072718A1?"
→ Read all risk pages for that patent. Summarize severity and mitigation.

### Cross-Patent Questions

"Do any patents share the same technical effect?"
→ Deep mode. Read all effect pages. Group by similarity.

### Fallback Questions

"What narrower positions are available for Claim 21?"
→ Read fallback pages linked to that claim.

### Prior Art Questions

"How does this differ from prior art?"
→ Read prior-art-delta pages. Present feature-by-feature comparison.

---

## Citation Rules

Every factual assertion in the answer must cite a wiki page:

```markdown
The code interpreter executes in a sandboxed environment 
(Source: [[US20260072718A1 - Claim 21]], element 4 [D]).
```

For synthesized answers that combine multiple pages:
```markdown
The three patents share a common approach to ML-based content generation, 
though they target different modalities: code (Source: [[US20260072718A1]]), 
speech (Source: [[US20260073164A1]]), and images (Source: [[US20260087598A1]]) [S].
```

---

## Answer Classification

The answer itself should note the overall classification:

- **Fully [D]**: "All of the above is directly from the patent text."
- **Mostly [D] with [S]**: "The individual facts are disclosed; the synthesis connecting them is mine."
- **Contains [I]**: "Note: the commercial application analysis is inference, not from the patent."

---

## Do NOT

- Fabricate information not in the wiki
- Use training data for patent-specific questions
- Answer without citations
- Skip statement classification
- Read more than 5 pages for standard queries (use deep mode if needed)

---
name: save
description: >
  Save valuable conversation content into the patent wiki as permanent notes.
  Prevents insights from disappearing when the chat ends.
  Triggers on: "save this", "file this", "/save", "save this analysis",
  "keep this", "remember this".
allowed-tools: Read Write Edit Glob Grep
---

# save: File Conversations into Patent Wiki

Turn transient chat into permanent wiki knowledge.

---

## When to Save

**Save**:
- Non-obvious patent analysis insights
- Decisions with rationale (e.g., "we chose to narrow Claim 21 because...")
- Cross-patent synthesis or comparison
- Risk assessments developed during discussion
- Use-case scenarios developed in conversation
- Answers to complex patent questions

**Skip**:
- Mechanical Q&A already covered by existing pages
- Setup or configuration steps
- Temporary debugging of wiki structure
- Content that duplicates an existing page

---

## Workflow

1. **Scan** conversation for the most valuable content.
2. **Ask**: "What should I call this note?" (suggest a name based on content)
3. **Determine type**:
   - If it answers a question → `question` type in `wiki/questions/`
   - If it's a cross-patent comparison → `comparison` type in `wiki/comparisons/`
   - If it's a risk assessment → `risk` type in `wiki/risks/`
   - If it's a use-case scenario → `use-case` type in `wiki/use-cases/`
   - If it's general analysis → `question` type in `wiki/questions/`
4. **Extract content**. Rewrite in declarative present tense (not "the user asked").
5. **Apply statement classification** to all assertions: [D], [S], or [I].
6. **Create page** with full frontmatter from `references/frontmatter.md`.
7. **Collect related pages**: all wiki pages mentioned in the conversation → `related` field.
8. **Update `wiki/index.md`**: new entry in the appropriate section.
9. **Append `wiki/log.md`** (new entry at TOP):
   ```markdown
   ## YYYY-MM-DD save | {Note Title}
   - Saved as: [[Note Title]]
   - Type: {type}
   - Related: [[Page 1]], [[Page 2]]
   - Summary: One sentence on what was saved.
   ```
10. **Update `wiki/hot.md`** with the save.
11. **Confirm**: "Saved as [[Note Title]]."

---

## Writing Style

- Declarative present tense: "The system processes input" not "We discussed that the system processes input"
- Include citations to wiki pages: `(Source: [[Page Name]])`
- Apply statement classification to every assertion
- Write for future readers who have no context from this conversation

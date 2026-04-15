---
name: patent-lint
description: >
  Health check for the patent wiki vault. Validates cross-references, statement
  classification, frontmatter, and patent-specific completeness requirements.
  Produces a lint report in wiki/meta/.
  Triggers on: lint, health check, validate, clean up, check wiki.
allowed-tools: Read Write Edit Glob Grep Bash
---

# patent-lint: Patent Wiki Health Check

Validate the wiki structure, cross-references, and patent-specific requirements. Produce a report.

---

## When to Run

- After every patent ingestion (Pass B)
- After every 10-15 page edits
- When the user says "lint" or "health check"
- Weekly maintenance

---

## Lint Checks

### Standard Checks (inherited from wiki-lint pattern)

#### 1. Orphan Pages
Pages with no inbound wikilinks from other pages.
- **How**: Glob all wiki pages. For each, grep the rest of the wiki for `[[Page Name]]`.
- **Severity**: Warning (may be intentionally isolated)
- **Auto-fix**: No (needs human judgment)

#### 2. Dead Links
Wikilinks pointing to non-existent pages.
- **How**: Extract all `[[...]]` patterns from wiki pages. Check each target exists.
- **Severity**: Error
- **Auto-fix**: Create stub page or remove link (ask user)

#### 3. Frontmatter Gaps
Pages missing required frontmatter fields.
- **How**: Read each page. Check for: type, title, patent, created, updated, tags, status, related, sources.
- **Severity**: Warning
- **Auto-fix**: Safe to add missing fields with default values

#### 4. Empty Sections
Headings with no content below them.
- **How**: Scan for heading lines followed immediately by another heading or EOF.
- **Severity**: Warning
- **Auto-fix**: No (may need content from user)

#### 5. Stale Index Entries
Index items pointing to renamed or deleted pages.
- **How**: Read index.md and all _index.md files. Check each linked page exists.
- **Severity**: Error
- **Auto-fix**: Remove stale entries

### Patent-Specific Checks

#### 6. Unsupported Claim Elements
Claim elements without at least one embodiment link AND one figure link.
- **How**: Read each claim page. For each element in the support chain, check for embodiment and figure links.
- **Severity**: Warning (maps to `[!gap]` callouts)
- **Report**: List each unsupported element with its claim and patent.

#### 7. Unlinked Claims
Independent claims without at least one use-case page.
- **How**: Read each claim page. Check deployment chain section for use-case links.
- **Severity**: Warning
- **Report**: List claims without use cases.

#### 8. Missing Fallbacks
Independent claims without at least one fallback position.
- **How**: Check claim pages for fallback_to frontmatter or fallback links in body.
- **Severity**: Info (not all claims need fallbacks, but most should)
- **Report**: List claims without fallbacks.

#### 9. Unclassified Statements
Body-text assertions without [D], [S], or [I] markers.
- **How**: Scan page bodies for factual assertions not inside callouts and not prefixed with [D]/[S]/[I].
- **Severity**: Warning
- **Heuristic**: Look for lines that make factual claims about the patent (contain patent numbers, paragraph refs, or technical terms) but lack classification markers.

#### 10. Orphan Figures
Figure pages with no claim or embodiment links.
- **How**: Read figure pages. Check illustrates_claims and illustrates_embodiments frontmatter.
- **Severity**: Warning
- **Report**: List unlinked figures.

#### 11. Term Consistency
Terms used in claims but not defined in wiki/terms/.
- **How**: Extract terms from claim elements. Check for corresponding term pages.
- **Severity**: Info
- **Report**: List undefined terms.

#### 12. Cross-Reference Symmetry
If page A links to page B via relationship type X, page B must link back via the inverse.
- **How**: For each relationship found in frontmatter, check the target page for the inverse.
- **Severity**: Error
- **Auto-fix**: Add missing inverse links.
- **Relationship pairs to check**: supports/supported_by, illustrated_by/illustrates, implemented_by/implements, optional_in/optional_features, required_for/requires, produces_effect/produced_by, narrows_to/narrows_from, depends_on/depended_on_by, measured_by/measures, used_in/uses, at_risk_from/affects_claim, evidenced_by/evidence_for.

#### 13. Risk Coverage
Every independent claim should have at least one risk page.
- **How**: For each independent claim, check for at_risk_from links or corresponding risk pages.
- **Severity**: Info
- **Report**: List claims without risk assessment.

#### 14. Effect Traceability
Every technical effect should trace back to at least one claim element.
- **How**: Read effect pages. Check produced_by frontmatter.
- **Severity**: Warning
- **Report**: List effects without producing claims.

#### 15. Patent Completeness
After ingestion, verify that all claims and all figures have wiki pages.
- **How**: Read decomposition file. Compare claim/figure counts with actual pages.
- **Severity**: Error
- **Report**: List missing claim and figure pages.

---

## Report Format

Write report to `wiki/meta/lint-report-YYYY-MM-DD.md`:

```markdown
---
type: meta
title: "Lint Report YYYY-MM-DD"
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags:
  - meta
  - lint
status: evergreen
---

# Lint Report: YYYY-MM-DD

## Summary

| Category | Errors | Warnings | Info |
|----------|--------|----------|------|
| Orphan Pages | 0 | 2 | 0 |
| Dead Links | 1 | 0 | 0 |
| Frontmatter Gaps | 0 | 3 | 0 |
| ... | ... | ... | ... |
| **Total** | **X** | **Y** | **Z** |

## Errors (must fix)

### Dead Links
- [[Page A]] links to [[Missing Page]] (does not exist)

### Cross-Reference Symmetry
- [[Claim 21]] → supported_by → [[Embodiment X]], but [[Embodiment X]] missing implements → [[Claim 21]]

### Patent Completeness
- US20260072718A1: Missing figure page for FIG. 15

## Warnings (should fix)

### Unsupported Claim Elements
- US20260072718A1 Claim 21, Element 3: no figure link

### Unclassified Statements
- [[US20260072718A1 - Effect X]]: line 25 — assertion without classification

## Info (consider)

### Missing Fallbacks
- US20260072718A1 Claim 39: no fallback position identified

### Term Consistency
- "transformer architecture" used in claims but no term page exists

## Auto-Fix Applied

- Added missing `implements` link on [[Embodiment X]] → [[Claim 21]]
- Added missing `status: seed` to 3 pages
```

---

## After Lint

1. Write the report
2. Apply safe auto-fixes (cross-reference symmetry, missing frontmatter fields)
3. Update `wiki/hot.md` with lint summary
4. Append to `wiki/log.md`:
   ```markdown
   ## YYYY-MM-DD lint | Health check
   - Errors: X | Warnings: Y | Info: Z
   - Auto-fixed: [list]
   - Needs attention: [list]
   ```
5. Report to user with summary and recommended actions

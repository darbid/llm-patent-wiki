---
name: obsidian-bases
description: "Create and edit Obsidian Bases (.base files): Obsidian's native database layer for dynamic tables, card views, list views, filters, formulas, and summaries over vault notes. Triggers on: create a base, add a base file, obsidian bases, base view, filter notes, formula, database view, dynamic table."
allowed-tools: Read Write
---

# obsidian-bases: Obsidian's Database Layer

Obsidian Bases (launched 2025) turns vault notes into queryable, dynamic views. Tables, cards, lists. Defined in `.base` files. No plugin required; it is a core Obsidian feature.

**Authoritative reference**: If the kepano/obsidian-skills plugin is installed, prefer its canonical obsidian-bases skill. Official docs: https://help.obsidian.md/bases/syntax

---

## File Format

`.base` files contain valid YAML. Root keys: `filters`, `formulas`, `properties`, `summaries`, `views`.

```yaml
filters:
  and:
    - file.inFolder("wiki/")
    - 'status != "archived"'

formulas:
  age_days: '(now() - file.ctime).days.round(0)'

properties:
  formula.age_days:
    displayName: "Age (days)"

views:
  - type: table
    name: "All Pages"
    order:
      - file.name
      - type
      - status
      - updated
      - formula.age_days
```

---

## Filters

```yaml
# AND
filters:
  and:
    - 'status != "archived"'
    - file.hasTag("patent")

# OR
filters:
  or:
    - file.hasTag("claim")
    - file.hasTag("embodiment")

# NOT
filters:
  not:
    - file.inFolder("wiki/meta")
```

### Useful filter functions

| Function | Example |
|----------|---------|
| `file.hasTag("x")` | Notes with tag `x` |
| `file.inFolder("path/")` | Notes in folder |
| `file.hasLink("Note")` | Notes linking to Note |

---

## Patent Wiki Base Templates

### Claims Dashboard

```yaml
filters:
  and:
    - file.inFolder("wiki/claims/")
    - 'file.ext == "md"'

formulas:
  risk_count: 'if(at_risk_from, at_risk_from.length(), 0)'

views:
  - type: table
    name: "Claims"
    order:
      - file.name
      - patent
      - claim_number
      - claim_type
      - status
      - formula.risk_count
    groupBy:
      property: patent
      direction: ASC
```

### Risk Tracker

```yaml
filters:
  file.inFolder("wiki/risks/")

views:
  - type: table
    name: "Risks"
    order:
      - file.name
      - patent
      - risk_type
      - severity
      - status
    groupBy:
      property: severity
      direction: DESC
```

### Cross-Reference Coverage

```yaml
filters:
  and:
    - file.inFolder("wiki/")
    - not:
        - file.inFolder("wiki/meta")
    - 'file.ext == "md"'

formulas:
  age: '(now() - file.ctime).days.round(0)'

views:
  - type: table
    name: "All Patent Wiki Pages"
    order:
      - file.name
      - type
      - patent
      - status
      - updated
      - formula.age
    groupBy:
      property: type
      direction: ASC
```

---

## Embedding in Notes

```markdown
![[dashboard.base]]
![[dashboard.base#Claims]]
```

---

## Where to Save

Store `.base` files in `wiki/meta/`:
- `wiki/meta/dashboard.base`: main content view
- `wiki/meta/claims.base`: claims tracker
- `wiki/meta/risks.base`: risk tracker

---

## YAML Quoting Rules

- Formulas with double quotes: wrap in single quotes: `'if(done, "Yes", "No")'`
- Strings with colons: wrap in double quotes: `"Status: Active"`

---

## What Not to Do

- Do not use `from:` or `where:`: those are Dataview syntax, not Bases
- Do not use `sort:` at root: sorting is per-view via `order:` and `groupBy:`
- Do not put `.base` files outside the vault
- Do not reference `formula.X` without defining `X` in `formulas:`

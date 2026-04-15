# Cross-Reference Patterns

The two required cross-reference patterns that make the patent wiki genuinely useful.

---

## Pattern A: Claim Element Support Chain

**Purpose**: Ensure every claim element has traceable support in the disclosure.

**Rule**: Every claim element on every claim page must link to:

1. **Supporting disclosure text** (paragraph reference)
   - This is always [D]
   - Must quote or closely paraphrase the relevant text
   - If no direct support: `> [!gap] No direct disclosure support found`

2. **At least one embodiment**
   - Link to the embodiment page that implements this element
   - This is [D] if the embodiment explicitly implements the element
   - This is [S] if the connection requires combining multiple paragraphs
   - If no embodiment: `> [!gap] No embodiment implements this element`

3. **At least one figure or figure region**
   - Link to the figure page showing this element
   - Include the specific component reference numbers
   - This is always [D] (figures either show it or they don't)
   - If no figure: `> [!gap] No figure illustrates this element`

4. **Any technical effect it contributes to**
   - Link to the effect page
   - [D] if the patent explicitly connects the element to the effect
   - [S] if the connection is a reasonable inference from the text
   - "None identified" is acceptable

5. **Any fallback narrowing already disclosed**
   - Link to the fallback page
   - [S] since fallbacks are attorney-constructed from disclosure
   - "None identified" is acceptable

### Visual Template

```markdown
### Element {N}: {short name}

> [!disclosed] Disclosure support
> "{quoted text}" [{paragraph ref}]

**Embodiment**: [[{patent} - Embodiment {Name}]] — {how it implements} [{classification}]
**Figure**: [[{patent} - FIG {N}]] — {component refs} [{classification}]
**Effect**: [[{patent} - Effect {Name}]] — {how element contributes} [{classification}]
**Fallback**: [[{patent} - Fallback {Name}]] — {narrowing} [{classification}]
```

---

## Pattern B: Claim Practical Deployment Chain

**Purpose**: Connect every independent claim to real-world applicability.

**Rule**: Every independent claim page must include a Deployment Chain section linking to:

1. **Real-world use cases**
   - At least one use-case page per independent claim
   - Description of practical scenario
   - This is typically [S] (synthesized from disclosure) or [I] (inferred)

2. **Required system context**
   - What system infrastructure is needed
   - [S] if derivable from the patent's system description
   - [I] if requiring implementation knowledge

3. **Typical hardware/software environment**
   - Specific platforms, architectures, languages
   - [D] if mentioned in the patent, [I] if inferred

4. **External actors or interfaces**
   - Who/what interacts with the system from outside
   - [D] if mentioned (e.g., "user device"), [S] if synthesized

5. **Measurable practical effect**
   - Link to the most relevant effect page
   - What observable improvement occurs in the real world

6. **Possible commercial embodiments**
   - What commercial products could embody this claim
   - This is always [I] — it's speculation informed by the disclosure

### Visual Template

```markdown
## Deployment Chain

- **Use case**: [[{patent} - UseCase {Name}]] — {scenario description} [{classification}]
- **System context**: {infrastructure description} [{classification}]
- **Hardware/software**: {platform details} [{classification}]
- **Actors**: {list of external actors} [{classification}]
- **Measurable effect**: [[{patent} - Effect {Name}]] [{classification}]
- **Commercial embodiment**: {product/workflow description} [I]
```

---

## Cross-Reference Matrix

Use this matrix to verify completeness after Pass B. Every cell should have at least one link or an explicit "N/A":

```
                 Embodiment  Figure  Effect  Fallback  Use-Case  Risk
Claim Element 1    [ ]        [ ]     [ ]     [ ]       -        -
Claim Element 2    [ ]        [ ]     [ ]     [ ]       -        -
Claim Element 3    [ ]        [ ]     [ ]     [ ]       -        -
Independent Claim   -          -       -       -       [ ]      [ ]
```

Where `[ ]` means "must have link" and `-` means "not applicable at this level."

---

## Gap Handling

When a required link cannot be established:

1. **Add [!gap] callout** on the page where the link should appear
2. **Do not fabricate links** — a gap is better than a false link
3. **Gaps are informative** — they tell the user where the patent's disclosure may be thin
4. **Gaps may indicate risks** — an unsupported claim element is a risk worth documenting

Example:
```markdown
> [!gap] Missing figure link for Element 3: "providing an output"
> No figure in the patent specifically illustrates the output delivery step.
> This may indicate a disclosure gap. See [[{patent} - Risk Output Not Illustrated]].
```

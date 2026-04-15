# Relationship Types

The patent wiki uses 13 typed relationship types. Every relationship is bidirectional — recorded in frontmatter on BOTH pages.

---

## Relationship Definitions

### 1. supports / supported_by

**Meaning**: Source material that supports a claim element or assertion.
**Example**: An embodiment `supports` a claim element. The claim element is `supported_by` the embodiment.
**Typical source → target**: Embodiment → Claim, Evidence → Claim, Evidence → Effect

### 2. illustrated_by / illustrates

**Meaning**: A figure or drawing that visually depicts a component, step, or system.
**Example**: A claim is `illustrated_by` FIG 1. FIG 1 `illustrates` the claim.
**Typical source → target**: Claim → Figure, Embodiment → Figure

### 3. implemented_by / implements

**Meaning**: An embodiment that provides a concrete implementation of a claim.
**Example**: A claim is `implemented_by` an embodiment. The embodiment `implements` the claim.
**Typical source → target**: Claim → Embodiment

### 4. optional_in / optional_features

**Meaning**: A feature that is optional within a particular embodiment or claim scope.
**Example**: A feature is `optional_in` an embodiment. The embodiment has `optional_features` including that feature.
**Typical source → target**: Feature/Component → Embodiment

### 5. required_for / requires

**Meaning**: A necessary dependency.
**Example**: A term is `required_for` a claim. The claim `requires` that term.
**Typical source → target**: Term → Claim, Parameter → Effect

### 6. produces_effect / produced_by

**Meaning**: A causal relationship between a feature and a technical effect.
**Example**: A claim `produces_effect` of improved performance. The effect is `produced_by` the claim.
**Typical source → target**: Claim → Effect, Embodiment → Effect

### 7. distinguished_from / distinguished_from

**Meaning**: A differentiation from prior art or another approach. Symmetric relationship.
**Example**: This patent's approach is `distinguished_from` the prior art approach.
**Typical source → target**: Claim → Prior Art Delta

### 8. narrows_to / narrows_from

**Meaning**: A scope narrowing from a broader claim to a more specific fallback.
**Example**: Claim 21 `narrows_to` a fallback. The fallback `narrows_from` Claim 21.
**Typical source → target**: Claim → Fallback

### 9. depends_on / depended_on_by

**Meaning**: A dependency relationship between claims.
**Example**: Dependent claim 22 `depends_on` independent claim 21. Claim 21 is `depended_on_by` claim 22.
**Typical source → target**: Dependent Claim → Independent Claim

### 10. measured_by / measures

**Meaning**: A parameter or metric that quantifies an effect or feature.
**Example**: An effect is `measured_by` a parameter. The parameter `measures` the effect.
**Typical source → target**: Effect → Parameter

### 11. used_in / uses

**Meaning**: A term or component used within a claim or embodiment.
**Example**: A term is `used_in` claim 21. Claim 21 `uses` that term.
**Typical source → target**: Term → Claim, Component → Embodiment

### 12. at_risk_from / affects_claim

**Meaning**: A risk that threatens the validity or enforceability of a claim.
**Example**: A claim is `at_risk_from` a breadth risk. The risk `affects_claim` that claim.
**Typical source → target**: Claim → Risk

### 13. evidenced_by / evidence_for

**Meaning**: Evidence that demonstrates or supports a claim or effect.
**Example**: An effect is `evidenced_by` a test result. The test result is `evidence_for` the effect.
**Typical source → target**: Effect → Evidence, Claim → Evidence

---

## Recording Rules

1. **Bidirectional**: When you write `supports: ["[[Claim 21]]"]` on an embodiment page, you must also write `supported_by: ["[[Embodiment X]]"]` on the Claim 21 page.

2. **Frontmatter**: Use the field names exactly as listed above. They are the primary machine-readable representation.

3. **Body text**: Also mention the relationship in the page body using inline wikilinks with relationship context:
   ```markdown
   **Supported by** [[US20260072718A1 - Embodiment Sandbox Execution]] (paragraph [0056])
   ```

4. **Relationship table**: Every page should end with a Relationships table:
   ```markdown
   ## Relationships
   
   | Type | Target | Classification |
   |------|--------|---------------|
   | supports | [[Claim 21]] | [D] |
   | illustrated_by | [[FIG 1]] | [D] |
   ```

5. **Classification**: Every row in the relationship table must carry a statement classification: [D], [S], or [I].

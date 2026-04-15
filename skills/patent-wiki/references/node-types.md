# Node Types

The patent wiki uses 13 node types. Every wiki page is an instance of one of these types.

---

## 1. Claim

**Folder**: `wiki/claims/`
**Description**: One page per independent claim. Dependent claims appear as subsections within their parent claim page.
**Key fields**: claim_number, claim_type, depends_on, claim_elements
**Cross-reference patterns**: Support chain (Pattern A), Deployment chain (Pattern B)

## 2. Claim Element

**Representation**: Not a separate page — appears as rows in the claim page's "Claim Elements" table and "Element Support Chains" section.
**Description**: A discrete functional limitation within a claim. Each element must link to disclosure support, embodiment, and figure.

## 3. Embodiment

**Folder**: `wiki/embodiments/`
**Description**: A concrete implementation variant disclosed in the patent. May be required or optional.
**Key fields**: embodiment_type, optional, implements, components, paragraph_refs

## 4. Figure

**Folder**: `wiki/figures/`
**Description**: One page per patent drawing. Lists labeled components with reference numbers.
**Key fields**: figure_number, figure_type, components_shown, illustrates_claims, illustrates_embodiments
**Figure types**: block-diagram, flow-diagram, UI, data-chart, architecture, sequence-diagram, state-diagram

## 5. Technical Effect

**Folder**: `wiki/effects/`
**Description**: A technical advantage or result produced by the claimed invention.
**Key fields**: effect_type, produced_by, measured_by, evidenced_by

## 6. Use Case

**Folder**: `wiki/use-cases/`
**Description**: A practical real-world deployment scenario showing the invention in context.
**Key fields**: actors, system_context, hardware_env, implements_claim, measurable_effect

## 7. Term

**Folder**: `wiki/terms/`
**Description**: A canonical technical term defined or used with specific meaning in the patent claims.
**Key fields**: canonical_form, aliases, defined_at, used_in_claims
**Note**: Cross-patent — if the same term appears in multiple patents, the page aggregates usage from all.

## 8. Parameter

**Folder**: `wiki/parameters/`
**Description**: A specific value, range, or threshold disclosed in the patent.
**Key fields**: parameter_name, values, claim_refs, paragraph_refs

## 9. Prior Art Reference

**Folder**: `wiki/prior-art-deltas/`
**Description**: A feature-by-feature distinction showing how the patent differentiates from known prior art.
**Key fields**: prior_art_ref, distinguished_features, prior_art_paragraph

## 10. Fallback Position

**Folder**: `wiki/fallbacks/`
**Description**: A narrower claim combination already supported by the disclosure.
**Key fields**: narrows_from, narrowing_feature, supported_by_disclosure, paragraph_refs

## 11. Risk

**Folder**: `wiki/risks/`
**Description**: An identified weakness: unsupported breadth, ambiguous term, enablement gap, or definitional inconsistency.
**Key fields**: risk_type, affects_claim, severity, mitigation
**Risk types**: breadth, ambiguity, enablement, definitional

## 12. Evidence

**Folder**: `wiki/evidence/`
**Description**: An example, test, prototype, or field result disclosed in the patent.
**Key fields**: evidence_type, supports, paragraph_refs
**Evidence types**: example, test, prototype, field-result, performance-data

## 13. Commercial Product / Workflow

**Representation**: Appears within Use Case pages as the "Commercial Embodiment" section.
**Description**: A possible commercial product or workflow that embodies the claimed invention. Not a separate page type — documented inline within use-case pages.

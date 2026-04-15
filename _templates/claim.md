---
type: claim
title: "<% tp.file.title %>"
claim_number: 0
claim_type: independent
depends_on: []
patent: ""
claim_elements: []
supported_by: []
illustrated_by: []
produces_effect: []
fallback_to: []
at_risk_from: []
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
tags:
  - claim
status: seed
related: []
sources: []
---

# <% tp.file.title %>

Navigation: [[claims/_index|Claims]] | [[index]]

## Claim Text

[Full verbatim claim text]

## Claim Elements

| # | Element | Required/Optional | Classification |
|---|---------|-------------------|---------------|
| 1 | | Required | [D] |

## Element Support Chains

### Element 1: [short name]

> [!disclosed] Disclosure support
> [paragraph reference and quote]

**Embodiment**: [[]]
**Figure**: [[]] (component refs)
**Effect**: [[]]
**Fallback**: None identified

## Dependent Claims

### Claim [N]

- **Depends on**: This claim
- **Adds**: [what the dependent claim adds]
- **Classification**: [D]

## Deployment Chain

- **Use case**: [[]]
- **System context**: 
- **Hardware/software**: 
- **Actors**: 
- **Measurable effect**: [[]]
- **Commercial embodiment**: 

## Relationships

| Type | Target | Classification |
|------|--------|---------------|
| supported_by | [[]] | [D] |
| illustrated_by | [[]] | [D] |
| produces_effect | [[]] | [S] |

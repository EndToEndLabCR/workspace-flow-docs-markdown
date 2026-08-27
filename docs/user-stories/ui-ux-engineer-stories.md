# Workspace Flow UI/UX Engineer User Stories

## Estimation Convention

Story points include research, interaction design, responsive variants, accessibility review, prototype updates, and developer handoff. The UX allocation is **24 SP** of the 209 SP MVP; shared product and QA work accounts for the remainder. Role totals are workstream views and are not added to the product backlog total.

## Epic 0: Workspace Flow Design Foundation

**Estimate**: 8 SP

### US-UX-0.1: Map the Base Mockups (3 SP)

- **As a** UI/UX engineer,
- **I want to** annotate the six mockups in `docs/ui-ux/prototypes/images/`,
- **So that** each MVP screen has a clear source and intended flow.

### US-UX-0.2: Define State and Component Patterns (5 SP)

- **As a** UI/UX engineer,
- **I want to** define tokens, components, and states,
- **So that** memory, consent, tasks, and diagnosis feel like one product.

**Acceptance Criteria**:

- Loading, empty, error, retry, confirmation, and success states are specified
- Mobile and desktop variants are documented
- Severity and score never rely on color alone

## Epic 1: Memory Workspace Design

**Estimate**: 5 SP

### US-UX-1.1: Design Capture and AI Review (5 SP)

- **As a** UI/UX engineer,
- **I want to** design note capture, decision history, and suggestion review,
- **So that** users can preserve context quickly and control enrichment.

**Acceptance Criteria**:

- Accept, edit, and discard actions are distinct
- Raw content, generated proposal, confidence, and rationale have clear hierarchy
- Conversion to task preserves visible source context

## Epic 2: Diagnosis Design

**Estimate**: 5 SP

### US-UX-2.1: Design Consent and Source Scope (2 SP)

- **As a** UI/UX engineer,
- **I want to** make source scope and consent understandable,
- **So that** users know what will be inspected.

### US-UX-2.2: Design Explainable Findings (3 SP)

- **As a** UI/UX engineer,
- **I want to** design score, dimensions, findings, evidence, and recommendations,
- **So that** diagnosis supports a clear next action.

## Epic 3: Validation and Handoff

**Estimate**: 6 SP

### US-UX-3.1: Run Usability Sessions (3 SP)

- **As a** UI/UX engineer,
- **I want to** test the critical mockup-based journeys with developers,
- **So that** Workspace Flow solves context recovery rather than generic task tracking.

**Acceptance Criteria**:

- Users complete project creation, note capture, task conversion, consent, and diagnosis review
- Findings are prioritized by severity and impact
- Critical issues receive design resolutions

### US-UX-3.2: Complete Accessibility and Developer Handoff (3 SP)

- **As a** UI/UX engineer,
- **I want to** document keyboard, semantic, contrast, and responsive requirements,
- **So that** implementation can be verified.

## UX MVP Summary

| Epic | Story Points |
| ---- | ------------ |
| Design Foundation | 8 |
| Memory Workspace | 5 |
| Diagnosis Design | 5 |
| Validation and Handoff | 6 |
| **Total** | **24 SP** |

---

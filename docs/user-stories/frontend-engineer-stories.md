# Workspace Flow Frontend Engineer User Stories

## Estimation Convention

Story points include UI implementation, state management, responsive behavior, accessibility, tests, and API integration. The frontend allocation is **81 SP** of the 209 SP MVP. Role totals include implementation work only; shared acceptance and integration work is not double-counted in the product backlog.

## Epic 0: Application Foundation

**Estimate**: 13 SP

### US-FE-0.1: Build the Responsive Shell (5 SP)

- **As a** frontend engineer,
- **I want to** build navigation, layout, responsive tokens, and route boundaries,
- **So that** every Workspace Flow screen has a consistent structure.

### US-FE-0.2: Build Shared Async and Error States (8 SP)

- **As a** frontend engineer,
- **I want to** standardize loading, empty, validation, error, retry, and confirmation states,
- **So that** workflows remain understandable.

## Epic 1: Identity and Portfolio

**Estimate**: 13 SP

### US-FE-1.1: Implement Registration and Login (8 SP)

- **As a** developer,
- **I want to** register and sign in from desktop or mobile,
- **So that** I can reach my private workspace.

**Acceptance Criteria**:

- Forms validate fields and display actionable errors
- Session expiration returns the user to login without data loss
- Screens match the root authentication mockups

### US-FE-1.2: Implement Project Portfolio (5 SP)

- **As a** developer,
- **I want to** see project cards with activity and health signals,
- **So that** I can choose where to focus.

## Epic 2: Project Workspace and Memory

**Estimate**: 21 SP

### US-FE-2.1: Implement Project CRUD Views (8 SP)

- **As a** developer,
- **I want to** create, edit, archive, and delete a project,
- **So that** its context has a durable home.

### US-FE-2.2: Implement Note Capture and Timeline (5 SP)

- **As a** developer,
- **I want to** capture and browse project notes,
- **So that** ideas and decisions are recoverable.

### US-FE-2.3: Implement AI Suggestion Review (8 SP)

- **As a** developer,
- **I want to** accept, edit, or discard an AI suggestion,
- **So that** I remain in control of project memory.

**Acceptance Criteria**:

- Raw content and suggested content are distinguishable
- Provider failures preserve the note
- Task and architecture-decision proposals use distinct forms

## Epic 3: Contextual Work

**Estimate**: 13 SP

### US-FE-3.1: Implement Contextual Task Views (8 SP)

- **As a** developer,
- **I want to** manage contextual tasks in list and kanban views,
- **So that** approved ideas become visible work.

### US-FE-3.2: Implement Search, Filters, and Traceability (5 SP)

- **As a** developer,
- **I want to** filter tasks and open their source note,
- **So that** I can find work and understand its origin.

## Epic 4: Diagnosis and Reporting

**Estimate**: 21 SP

### US-FE-4.1: Implement Consent and Analysis Controls (5 SP)

- **As a** developer,
- **I want to** understand and approve source access before analysis,
- **So that** I know what Workspace Flow will inspect.

### US-FE-4.2: Implement Analysis Progress and Recovery (5 SP)

- **As a** developer,
- **I want to** see diagnosis progress, partial results, failure, and retry,
- **So that** long-running work is predictable.

### US-FE-4.3: Implement Findings and Score Views (8 SP)

- **As a** developer,
- **I want to** inspect score dimensions, findings, evidence, and recommendations,
- **So that** I can trust the diagnosis.

### US-FE-4.4: Implement Trend and Priority Dashboard (3 SP)

- **As a** developer,
- **I want to** compare project health trends,
- **So that** I can prioritize my next action.

## Frontend MVP Summary

| Epic | Story Points |
| ---- | ------------ |
| Application Foundation | 13 |
| Identity and Portfolio | 13 |
| Project Workspace and Memory | 21 |
| Contextual Work | 13 |
| Diagnosis and Reporting | 21 |
| **Total** | **81 SP** |

---

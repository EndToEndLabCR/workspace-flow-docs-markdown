# Workspace Flow Sprint Structure

## Sprint Duration

- **Length**: 2 weeks (10 working days)
- **Team Capacity**: 26-29 product story points per sprint, adjusted for story boundaries
- **MVP Capacity**: 8 sprints = 209 product story points

### Sprint Breakdown for MVP (16 weeks = 8 sprints)

#### Sprint 1: Domain and Design Foundation (26 SP)

**Goal**: Establish the application shell, authenticated ownership model, and design baseline.

**Stories**:

- US-0.1 Define the Workspace Flow Domain (5 SP)
- US-0.2 Align the Product with the Base Mockups (5 SP)
- US-0.3 Establish Delivery Quality Gates (8 SP)
- US-0.4 Privacy and Consent Contracts (8 SP)

**Deliverable**: Approved contracts, annotated mockup map, CI baseline, and consent model.

---

#### Sprint 2: Identity and Workspace API (29 SP)

**Goal**: Establish private access and the project workspace foundation.

**Stories**:

- US-1.1 Register and Sign In (8 SP)
- US-1.2 Enforce Project Ownership (8 SP)
- US-1.3 Manage Account and Session States (5 SP)
- US-2.1 Create and Edit a Project (8 SP)

**Deliverable**: A user can securely create and reopen a private project.

---

#### Sprint 3: Portfolio and Project Detail (26 SP)

**Goal**: Complete project navigation and the first memory capture flows.

**Stories**:

- US-2.2 Browse the Project Portfolio (5 SP)
- US-2.3 Open a Project Workspace (8 SP)
- US-2.4 Archive or Delete a Project (5 SP)
- US-3.1 Capture a Note (5 SP)
- Start note-history persistence and API integration (3 SP)

**Deliverable**: A user can navigate projects and capture raw memory.

---

#### Sprint 4: Memory and AI Enrichment (26 SP)

**Goal**: Structure notes through user-controlled AI enrichment.

**Stories**:

- US-3.2 Classify a Note (8 SP)
- US-3.3 Accept, Edit, or Discard a Suggestion (8 SP)
- US-3.4 Record an Architecture Decision (5 SP)
- US-3.5 Review Project Memory (5 SP)

**Deliverable**: A user can capture, review, and approve structured project memory.

---

#### Sprint 5: Contextual Work (26 SP)

**Goal**: Turn approved memory into traceable tasks.

**Stories**:

- US-4.1 Create a Task from a Note (8 SP)
- US-4.2 Manage Task Status and Priority (8 SP)
- US-4.3 Search and Filter Contextual Work (5 SP)
- US-4.4 Delete a Task Safely (5 SP)

**Deliverable**: A user can manage work without losing its source context.

---

#### Sprint 6: Source Connection and Consent (21 SP)

**Goal**: Make source access explicit, scoped, and auditable.

**Stories**:

- US-5.1 Connect a GitHub Repository (8 SP)
- US-5.2 Approve and Revoke Local Source Access (8 SP)
- US-5.3 View Consent and Source History (5 SP)

**Deliverable**: A user can approve, inspect, and revoke the source used for diagnosis.

---

#### Sprint 7: Diagnosis Agents and Evidence (26 SP)

**Goal**: Produce safe, structured findings from a consented project source.

**Stories**:

- US-6.1 Start an Analysis Run (8 SP)
- US-6.2 Inventory the Repository (5 SP)
- US-6.3 Run Specialized Diagnosis Agents (13 SP)

**Deliverable**: A completed analysis produces versioned findings without modifying source files.

---

#### Sprint 8: Score, Prioritization, and Release (29 SP)

**Goal**: Make diagnosis explainable and release the complete MVP journey.

**Stories**:

- US-6.4 Correlate Findings with Memory (8 SP)
- US-6.5 Calculate an Explainable Score (8 SP)
- US-6.6 Review Findings and Recommendations (8 SP)
- US-6.7 Compare Project Health Trends (5 SP)

**Deliverable**: Users can compare projects, understand score changes, and choose the next action.

---

### Post-MVP Sprint Planning

**Phase 2**: Improve analysis dimensions, source adapters, history, and cross-project comparison after MVP validation.

**Phase 3**: Harden performance, privacy, accessibility, observability, and evidence-based expansion.

---

## Summary

This plan structures Workspace Flow around the product loop that differentiates it: preserve context, enrich it with user approval, diagnose the project, and prioritize the next action.

### Next Steps

1. **Review & Refinement**: Approve product boundaries, score dimensions, and source-consent decisions.
2. **Environment Setup**: Configure backend, frontend, migrations, CI, secrets, and local startup.
3. **Mockup Alignment**: Annotate the root `docs/*.png` assets and map each to an MVP journey.
4. **Development Kickoff**: Start Sprint 1 with acceptance tests for ownership and authentication.

---

**Document Version**: 2.0
**Last Updated**: August 26, 2026
**Prepared By**: Workspace Flow Team
**Status**: Draft for Review
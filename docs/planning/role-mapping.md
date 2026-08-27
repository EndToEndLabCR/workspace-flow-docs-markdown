# Workspace Flow Role-to-Task Mapping

## Tech Lead Responsibilities

**Architecture & Planning** (Ongoing)

- Define the Workspace Flow domain and enforce its product boundaries
- Review project memory, consent, diagnosis, and scoring decisions
- Ensure adherence to Clean Architecture and DDD principles
- Own API contracts, source-access threat modeling, and release readiness
- Conduct architecture reviews and mitigate technical risks

**Key Tasks**:

1. Define domain and API contracts for projects, notes, tasks, sources, runs, findings, and scores
2. Establish the read-only analysis boundary and consent audit model
3. Coordinate mockup-to-screen decisions for the root `docs/*.png` assets
4. Set up CI, migrations, observability, and production safeguards
5. Review critical pull requests and acceptance evidence

**Epic Ownership**: Shared oversight of all Workspace Flow epics

---

### Backend Engineer Responsibilities

**Primary Focus**: API development, persistence, business rules, and analysis orchestration

**Key Tasks by Epic**:

**Epic 1: Identity and Ownership**

- Implement user authentication and project ownership checks
- Build session expiration and account-management endpoints
- Write authorization and security tests

**Epic 2: Project Workspace**

- Implement project and source domain models
- Create project CRUD, archive, deletion, and source-connection endpoints
- Enforce confirmation and cascade rules

**Epic 3: Memory and Contextual Work**

- Implement notes, architecture decisions, and AI suggestion persistence
- Implement contextual tasks and `source_note_id` traceability
- Add search, filtering, and status transitions

**Epic 4: Diagnosis and Reporting**

- Implement analysis-run orchestration and read-only source adapters
- Persist immutable findings and score calculations
- Expose evidence, recommendations, trends, and failure recovery

**Total Estimated Effort**: 109 SP across foundation, identity, memory, contextual work, and diagnosis backend stories

---

### Frontend Engineer Responsibilities

**Primary Focus**: Workspace UX implementation, state management, responsive design, and accessibility

**Key Tasks by Epic**:

**Epic 1: Identity and Application Shell**

- Create registration, login, session, and protected-route flows
- Implement authenticated navigation and account states

**Epic 2: Project Workspace**

- Create project portfolio, create/edit, detail, archive, and delete flows
- Implement source connection and consent states

**Epic 3: Memory and Contextual Work**

- Create note capture, history, decision, and suggestion-review interfaces
- Implement contextual task cards, kanban, list, search, and filters

**Epic 4: Diagnosis and Reporting**

- Create analysis controls, progress, error, and result views
- Visualize score dimensions, trends, findings, evidence, and recommendations
- Add cross-project prioritization to the dashboard

**Total Estimated Effort**: 81 SP across foundation, workspace, memory, contextual work, and diagnosis frontend stories

---

### UI/UX Engineer Responsibilities

**Primary Focus**: Workspace Flow design system, prototypes, research, and accessibility

**Key Tasks by Epic**:

**Epic 0: Design System and Mockup Alignment**

- Maintain tokens, component patterns, and responsive behavior
- Annotate the root `docs/*.png` mockups and map them to product flows
- Define empty, loading, error, consent, and confirmation states

**Epic 1: Workspace Memory Design**

- Design project portfolio, project detail, note capture, and decision history
- Design AI suggestion review with clear user control

**Epic 2: Diagnosis Design**

- Design source consent, analysis progress, score, trend, and finding detail states
- Make evidence and recommendation hierarchy easy to scan

**Epic 3: Accessibility and Usability**

- Validate keyboard navigation, semantic structure, contrast, and mobile layouts
- Conduct usability sessions with multi-project developers and technical leads

**Key Deliverables**:

- Workspace Flow design system and annotated mockup inventory
- Responsive screens for MVP journeys
- Usability findings and accessibility audit
- Developer handoff specifications

**Total Estimated Effort**: 24 SP across design foundation, memory, diagnosis, validation, and handoff stories

---

### Shared Responsibilities

**Tech Lead + Backend + Frontend + UI/UX** (Collaborative)

- Review product boundaries and MVP acceptance criteria
- Maintain API and screen contracts
- Test consent, ownership, traceability, and read-only behavior
- Verify root mockups against implemented flows
- Monitor quality, privacy, performance, and post-release feedback

---
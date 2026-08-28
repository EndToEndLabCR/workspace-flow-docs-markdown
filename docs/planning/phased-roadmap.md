# Workspace Flow Phased Roadmap

## Phase 0: Foundation and Design Alignment - 2 weeks

**Objective**: Establish the domain, design direction, privacy model, and delivery foundation.

**Tasks / Features**:

- Confirm project, note, task, source, analysis, finding, and score contracts
- Align flows with the root `docs/*.png` mockups
- Set up repositories, CI, migrations, API conventions, and responsive tokens
- Define consent, retention, audit, and read-only agent boundaries

**Deliverables**:

- Approved domain and API baseline
- Working backend and frontend skeletons
- Authenticated application shell
- Privacy and consent decisions

**Definition of Done**:

- The application starts from documented instructions
- CI runs formatting, linting, type checks, and tests
- Mockup inventory and unresolved decisions are documented

## Phase 1: Memory Workspace MVP - 8 weeks

**Objective**: Deliver the complete loop from project creation to captured context and actionable work.

**Tasks / Features**:

- Implement authentication, ownership, project CRUD, and empty states
- Implement notes, architecture decisions, and AI suggestion review
- Implement BYOK provider setup for Gemini and Groq with encrypted key storage
- Implement contextual tasks, kanban, list, search, filters, status, and priority
- Implement dashboard activity, open-task, and pending-suggestion summaries

**Deliverables**:

- Responsive Workspace Flow memory workspace
- Versioned MVP API and migrations
- Screens matching authentication, project, task, and dashboard mockups
- Automated coverage for ownership, notes, suggestions, and tasks

**Definition of Done**:

- US-MVP-01 through US-MVP-06 pass acceptance testing
- Note-to-task conversion preserves `source_note_id` and is idempotent
- Destructive, empty, loading, error, mobile, and keyboard states are covered

**Estimated Effort**: 107 product story points

## Phase 2: Read-Only Diagnosis and Prioritization - 6 weeks

**Objective**: Turn approved source access and project memory into explainable health insights.

**Tasks / Features**:

- Add GitHub and local-source connections with explicit consent
- Implement File System Access API directory selection, extension allowlist, and excluded paths
- Implement analysis orchestration and read-only agents
- Store immutable findings with evidence, severity, location, and recommendation
- Calculate scores, trends, and decision-aware project priorities
- Apply the fixed five-dimension 20% score model and severity deductions

**Deliverables**:

- Source connection and consent experience
- Analysis, finding, score, and trend APIs
- Diagnosis and portfolio prioritization views
- Privacy and audit evidence

**Definition of Done**:

- Analysis completes without source mutation
- Every displayed finding has evidence and a recommendation
- Score changes are traceable to findings and weights
- Consent revocation, retry, failure recovery, and audit history work

**Estimated Effort**: 76 product story points

## Phase 3: Hardening and Evidence-Based Expansion - 4 weeks

**Objective**: Validate real workflows and promote only evidence-supported improvements.

**Tasks / Features**:

- Conduct usability and accessibility testing against the root mockups
- Improve performance, observability, error recovery, and analysis-cost controls
- Evaluate richer history, comparison, and selective integrations
- Reassess collaboration and agent write capabilities separately

**Deliverables**:

- Accessibility and usability audit
- Production-readiness and monitoring checklist
- Evidence-based post-MVP backlog
- Updated release documentation

**Definition of Done**:

- Critical security, privacy, accessibility, and reliability findings are resolved or accepted
- Core journeys meet desktop and mobile performance targets
- Product metrics and analysis failures are observable
- Promoted features have owners and acceptance criteria

**Post-MVP Decision**: Documentation assistance may be evaluated in this phase. It may generate draft Markdown or changelog content from approved commits, notes, and completed tasks, but user approval is required before persistence.

**Estimated Effort**: To be estimated after MVP evidence; no Phase 3 points are included in the 209 SP MVP baseline

## MVP Effort Summary

| Phase | Duration | Story Points |
| ----- | -------- | ------------ |
| Phase 0: Foundation | 2 weeks | 26 |
| Phase 1: Memory Workspace | 8 weeks | 107 |
| Phase 2: Diagnosis | 6 weeks | 76 |
| **MVP Total** | **16 weeks** | **209 SP** |

---
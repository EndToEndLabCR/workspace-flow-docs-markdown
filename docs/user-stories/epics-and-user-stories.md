# Workspace Flow Epics & User Stories

## Estimation Convention

Story points use a modified Fibonacci scale: 1 (trivial), 2 (small), 3 (moderate), 5 (complex), 8 (large), and 13 (needs decomposition). Estimates include implementation, tests, accessibility, and documentation.

## Epic 0: Product Foundation and Design System

**Priority**: Must Have (Foundation)
**Owner**: Tech Lead + UI/UX Engineer
**Estimate**: 26 SP

### US-0.1: Define the Workspace Flow Domain (5 SP)

- **As a** product team,
- **I want to** define projects, notes, decisions, tasks, sources, analysis runs, findings, and scores,
- **So that** every feature uses the same product language.

**Acceptance Criteria**:

- Domain glossary and relationships are documented
- Ownership, traceability, consent, and read-only rules are explicit
- API and database contracts are reviewed

### US-0.2: Align the Product with the Base Mockups (5 SP)

- **As a** UI/UX engineer,
- **I want to** map the root `docs/*.png` mockups to MVP screens,
- **So that** implementation preserves the intended navigation and flows.

**Acceptance Criteria**:

- Authentication, portfolio, project workspace, task, empty, and confirmation screens are mapped
- Mobile and desktop states are documented
- Memory, consent, and diagnosis extensions are annotated

### US-0.3: Establish Delivery Quality Gates (8 SP)

- **As a** tech lead,
- **I want to** configure CI, migrations, linting, type checks, and test commands,
- **So that** every increment is reproducible and reviewable.

**Acceptance Criteria**:

- A new developer can start the system locally
- CI fails on formatting, type, migration, or test errors
- Coverage and acceptance evidence are attached to each completed story

### US-0.4: Define Privacy and Consent Contracts (8 SP)

- **As a** developer,
- **I want to** define source scope, consent, revocation, retention, and audit behavior,
- **So that** source analysis is safe and explainable.

**Acceptance Criteria**:

- Consent is explicit, timestamped, scoped, and revocable
- External transmission is visible before it occurs
- Agents cannot write to source files

## Epic 1: Identity and Private Ownership

**Priority**: Must Have (MVP)
**Estimate**: 21 SP

### US-1.1: Register and Sign In (8 SP)

- **As a** developer,
- **I want to** create an account and sign in securely,
- **So that** my project memory is private.

**Acceptance Criteria**:

- Invalid credentials produce actionable errors
- Passwords are protected and sessions expire
- Successful sign-in reaches the private portfolio

### US-1.2: Enforce Project Ownership (8 SP)

- **As a** user,
- **I want to** access only my projects and records,
- **So that** another user cannot read my context or source results.

**Acceptance Criteria**:

- Project, note, task, source, run, finding, and score queries enforce owner scope
- Unauthorized resource access does not reveal whether the resource exists
- Authorization tests cover every resource family

### US-1.3: Manage Account and Session States (5 SP)

- **As a** user,
- **I want to** sign out and recover from an expired session,
- **So that** access remains predictable.

**Acceptance Criteria**:

- Sign-out clears client state
- Expired sessions redirect to sign-in without losing saved memory
- Account errors are represented in desktop and mobile layouts

## Epic 2: Project Workspace

**Priority**: Must Have (MVP)
**Estimate**: 26 SP

### US-2.1: Create and Edit a Project (8 SP)

- **As a** developer,
- **I want to** create a project with purpose and source information,
- **So that** its context has a durable home.

**Acceptance Criteria**:

- Required fields are validated
- The project appears in the portfolio after creation
- Edit preserves existing memory and analysis history

### US-2.2: Browse the Project Portfolio (5 SP)

- **As a** developer,
- **I want to** see all my projects with activity and health signals,
- **So that** I can choose where to focus.

**Acceptance Criteria**:

- Portfolio supports loading, empty, and failure states
- Project cards show latest score, trend, open tasks, and recent activity
- The layout follows the base dashboard and project-list mockups

### US-2.3: Open a Project Workspace (8 SP)

- **As a** developer,
- **I want to** see memory, contextual work, and diagnosis in one project view,
- **So that** I can resume without reconstructing context.

**Acceptance Criteria**:

- Project detail includes notes, decisions, tasks, latest score, and next action
- Navigation preserves project identity
- No single panel hides the current project status

### US-2.4: Archive or Delete a Project (5 SP)

- **As a** developer,
- **I want to** archive or delete a project with confirmation,
- **So that** obsolete work is controlled without accidental loss.

**Acceptance Criteria**:

- Archive is reversible
- Delete confirmation names the records affected
- Deletion is authorized and cascades only within the project boundary

## Epic 3: Project Memory and AI Enrichment

**Priority**: Must Have (MVP)
**Estimate**: 34 SP

### US-3.1: Capture a Note (5 SP)

- **As a** developer,
- **I want to** write a free-form note in seconds,
- **So that** ideas are not lost.

**Acceptance Criteria**:

- Note is saved as raw content before enrichment
- Note is linked to the current project
- Save, failure, and offline/error feedback are visible

### US-3.2: Classify a Note (8 SP)

- **As a** developer,
- **I want to** receive a proposed note type,
- **So that** I do not need to structure every thought manually.

**Acceptance Criteria**:

- Context Classifier returns type and confidence
- Provider failure leaves the raw note usable
- No suggestion is applied without user action

### US-3.3: Accept, Edit, or Discard a Suggestion (8 SP)

- **As a** developer,
- **I want to** control AI-enriched content,
- **So that** the project record reflects my intent.

**Acceptance Criteria**:

- Accept, edit, and discard are separate actions
- Original note remains available
- Decision and task suggestions have distinct fields

### US-3.4: Record an Architecture Decision (5 SP)

- **As a** developer,
- **I want to** record rationale, alternatives, and consequences,
- **So that** future diagnosis understands deliberate trade-offs.

**Acceptance Criteria**:

- Decision stores context, decision, alternatives, consequences, and date
- Decision is searchable from the project workspace
- Decision history is immutable after acceptance, with amendments traceable

### US-3.5: Review Project Memory (8 SP)

- **As a** developer,
- **I want to** browse and filter notes and decisions,
- **So that** I can recover the reasoning behind the code.

**Acceptance Criteria**:

- Memory is ordered by recency and filterable by type
- Each entry shows origin, status, and related task if any
- Empty and no-results states guide the next action

## Epic 4: Contextual Work

**Priority**: Must Have (MVP)
**Estimate**: 26 SP

### US-4.1: Create a Task from a Note (8 SP)

- **As a** developer,
- **I want to** turn approved context into a task,
- **So that** work keeps its rationale.

**Acceptance Criteria**:

- Task stores `source_note_id`
- Conversion is idempotent
- User can edit title, description, priority, and status before saving

### US-4.2: Manage Task Status and Priority (8 SP)

- **As a** developer,
- **I want to** update task status and priority,
- **So that** contextual work reflects reality.

**Acceptance Criteria**:

- TODO, IN_PROGRESS, and DONE are persisted
- Status changes work in list and kanban views
- The source note remains discoverable

### US-4.3: Search and Filter Contextual Work (5 SP)

- **As a** developer,
- **I want to** search and filter tasks,
- **So that** I can find the next action quickly.

**Acceptance Criteria**:

- Filters include status, priority, source, and project
- Clearing filters restores the complete result
- No-results state explains how to reset the view

### US-4.4: Delete a Task Safely (5 SP)

- **As a** developer,
- **I want to** delete a task with confirmation,
- **So that** accidental removal is prevented.

**Acceptance Criteria**:

- Confirmation identifies the task
- Deletion does not delete the source note
- List and kanban update consistently

## Epic 5: Source Connection and Consent

**Priority**: Must Have (MVP)
**Estimate**: 21 SP

### US-5.1: Connect a GitHub Repository (8 SP)

- **As a** developer,
- **I want to** connect a repository,
- **So that** Workspace Flow can inspect the project I choose.

**Acceptance Criteria**:

- Repository reference is validated
- Scope and access are shown before consent
- Credentials are never stored in project memory

### US-5.2: Approve and Revoke Local Source Access (8 SP)

- **As a** developer,
- **I want to** approve a local source scope,
- **So that** analysis cannot read more than I intend.

**Acceptance Criteria**:

- Selected scope and timestamp are displayed
- Revocation prevents new analysis runs
- Existing findings remain attributable to their prior source

### US-5.3: View Consent and Source History (5 SP)

- **As a** developer,
- **I want to** inspect source access history,
- **So that** analysis is auditable.

**Acceptance Criteria**:

- Each run identifies source, consent, timestamp, and status
- Revoked sources cannot be used silently
- History is visible from project settings and diagnosis

## Epic 6: Diagnosis, Findings, and Score

**Priority**: Must Have (MVP)
**Estimate**: 55 SP

### US-6.1: Start an Analysis Run (8 SP)

- **As a** developer,
- **I want to** start diagnosis for a consented source,
- **So that** I can understand project health.

**Acceptance Criteria**:

- Run is queued, running, complete, or failed
- Progress and read-only status are visible
- A failed run can be retried without duplicating completed history

### US-6.2: Inventory the Repository (5 SP)

- **As a** system,
- **I want to** inventory languages, frameworks, modules, and entry points,
- **So that** specialized agents receive reliable context.

**Acceptance Criteria**:

- Inventory records evidence and source scope
- Unsupported files are reported, not silently ignored
- Inventory output is linked to the analysis run

### US-6.3: Run Specialized Diagnosis Agents (13 SP)

- **As a** system,
- **I want to** run architecture, quality, and security agents,
- **So that** diagnosis covers the dimensions that matter.

**Acceptance Criteria**:

- Agents run with scoped inputs and correlation IDs
- Each result includes evidence, severity, location, and recommendation
- Partial failure is visible and does not erase prior runs

### US-6.4: Correlate Findings with Memory (8 SP)

- **As a** developer,
- **I want to** see relevant decisions beside findings,
- **So that** intentional trade-offs are not misread as defects.

**Acceptance Criteria**:

- Correlation never silently removes evidence
- Related decisions are linked from the finding
- User can distinguish confirmed, contextualized, and unresolved findings

### US-6.5: Calculate an Explainable Score (8 SP)

- **As a** developer,
- **I want to** understand how the score was calculated,
- **So that** I can trust its prioritization signal.

**Acceptance Criteria**:

- Score includes dimensions, weights, calculation version, and timestamp
- Overall score is reproducible from validated findings
- Score change links to contributing findings

### US-6.6: Review Findings and Recommendations (8 SP)

- **As a** developer,
- **I want to** review prioritized findings,
- **So that** I know what to address next.

**Acceptance Criteria**:

- Findings are filterable by dimension and severity
- Evidence and location are visible before recommendation
- Finding detail works on desktop and mobile

### US-6.7: Compare Project Health Trends (5 SP)

- **As a** developer,
- **I want to** compare scores across projects and runs,
- **So that** I can prioritize limited time.

**Acceptance Criteria**:

- Trend compares completed runs only
- Missing or failed runs are clearly represented
- Portfolio links to the diagnosis responsible for the change

## MVP Estimate Summary

| Epic | Story Points |
| ---- | ------------ |
| Epic 0: Foundation | 26 |
| Epic 1: Identity | 21 |
| Epic 2: Project Workspace | 26 |
| Epic 3: Memory and AI Enrichment | 34 |
| Epic 4: Contextual Work | 26 |
| Epic 5: Source and Consent | 21 |
| Epic 6: Diagnosis and Score | 55 |
| **Total** | **209 SP** |

---
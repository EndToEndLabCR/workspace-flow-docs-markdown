# Workspace Flow Backend Engineer User Stories

## Estimation Convention

Story points include implementation, tests, migrations, observability, and documentation. The backend allocation is **117 SP** of the 209 SP MVP, with shared work distributed across frontend and UX. The agent runtime contract is defined in [Agent Documentation](../architecture/agents.md).

## Epic 0: Foundation

**Estimate**: 18 SP

### US-BE-0.1: Define Domain and Persistence Contracts (8 SP)

- **As a** backend engineer,
- **I want to** define entities for users, projects, notes, tasks, sources, runs, findings, and scores,
- **So that** all use cases share stable contracts.

**Acceptance Criteria**:

- Entities, relationships, enums, and ownership rules are documented
- Alembic migration creates the MVP schema
- Repository interfaces are framework-independent

### US-BE-0.2: Establish API and Test Foundations (5 SP)

- **As a** backend engineer,
- **I want to** configure FastAPI, validation, error handling, and test fixtures,
- **So that** features are consistent and testable.

**Acceptance Criteria**:

- OpenAPI exposes versioned endpoints
- Validation errors use one response shape
- Unit and integration fixtures isolate users and projects

### US-BE-0.3: Add Audit and Correlation Infrastructure (5 SP)

- **As a** backend engineer,
- **I want to** record consent, authorization, and agent correlation events,
- **So that** diagnosis is auditable.

**Acceptance Criteria**:

- Critical events include actor, project, timestamp, and correlation ID
- Sensitive source content is excluded from ordinary logs
- Failed operations remain diagnosable

## Epic 1: Identity and Ownership

**Estimate**: 13 SP

### US-BE-1.1: Implement Secure Authentication (8 SP)

- **As a** backend engineer,
- **I want to** implement registration, login, password hashing, and expiring tokens,
- **So that** users have private sessions.

**Acceptance Criteria**:

- Duplicate accounts and invalid credentials are rejected safely
- Passwords are never returned or logged
- Expired tokens cannot access project resources

### US-BE-1.2: Enforce Owner Scope (5 SP)

- **As a** backend engineer,
- **I want to** apply owner checks to every repository query,
- **So that** cross-user access is impossible.

**Acceptance Criteria**:

- Project, note, task, source, run, finding, and score endpoints enforce ownership
- Unauthorized resources return a non-enumerating response
- Tests cover direct-ID and list endpoint access

## Epic 2: Project Workspace and Memory

**Estimate**: 26 SP

### US-BE-2.1: Implement Project CRUD (5 SP)

- **As a** backend engineer,
- **I want to** expose project create, list, update, archive, and delete operations,
- **So that** users can maintain their workspace.

### US-BE-2.2: Implement Notes and Decisions (8 SP)

- **As a** backend engineer,
- **I want to** persist raw notes, enriched suggestions, and architecture decisions,
- **So that** project reasoning survives sessions.

**Acceptance Criteria**:

- Raw note content remains unchanged after enrichment
- Note types and decision fields are validated
- History records acceptance and amendments

### US-BE-2.3: Implement Note Enrichment Service (8 SP)

- **As a** backend engineer,
- **I want to** integrate the Context Classifier, Task Proposal, and Decision Structuring agents,
- **So that** notes can become structured context.

**Acceptance Criteria**:

- Agent inputs exclude unauthorized projects and sources
- Output is schema-validated with confidence and rationale
- Provider failure preserves the raw note and returns a retryable state

### US-BE-2.4: Implement Contextual Tasks (5 SP)

- **As a** backend engineer,
- **I want to** create tasks with optional `source_note_id`,
- **So that** work remains connected to its rationale.

**Acceptance Criteria**:

- Source note belongs to the same project
- Conversion is idempotent
- Status and priority transitions are validated

## Epic 3: Source Consent and Diagnosis

**Estimate**: 60 SP

### US-BE-3.1: Implement Source Adapters (8 SP)

- **As a** backend engineer,
- **I want to** read GitHub and approved local sources through one interface,
- **So that** agents are independent of transport.

### US-BE-3.2: Implement Consent Lifecycle (8 SP)

- **As a** backend engineer,
- **I want to** store scoped consent and revocation,
- **So that** source access is explicit and auditable.

### US-BE-3.3: Implement Analysis Run Orchestration (8 SP)

- **As a** backend engineer,
- **I want to** execute inventory and specialist agents with run states,
- **So that** long-running diagnosis is recoverable.

### US-BE-3.4: Implement Agent Registry and Contracts (8 SP)

- **As a** backend engineer,
- **I want to** register Context Classifier, Repository Inventory, Architecture, Quality, Security, Correlation, and Score Aggregator agents,
- **So that** each agent has bounded inputs and typed outputs.

**Acceptance Criteria**:

- Agents receive only authorized input
- Agent outputs validate against versioned schemas
- No adapter exposes a write operation

### US-BE-3.5: Implement LangGraph Diagnosis Workflow (8 SP)

- **As a** backend engineer,
- **I want to** orchestrate inventory, specialist agents, correlation, and scoring with LangGraph,
- **So that** analysis order, state, retry, and partial results are explicit.

**Acceptance Criteria**:

- Inventory runs before Architecture, Quality, and Security agents
- LangGraph checkpoints run status and partial findings
- Failed nodes can be retried without duplicating completed findings
- The graph exposes no source-writing tool

### US-BE-3.6: Persist Findings and Evidence (8 SP)

- **As a** backend engineer,
- **I want to** persist immutable findings,
- **So that** recommendations can be reviewed later.

### US-BE-3.7: Calculate Scores and Trends (5 SP)

- **As a** backend engineer,
- **I want to** calculate weighted dimensions and compare completed runs,
- **So that** score changes are reproducible.

### US-BE-3.8: Handle Partial Failure and Retry (5 SP)

- **As a** backend engineer,
- **I want to** retry failed agents without duplicating completed evidence,
- **So that** diagnosis remains resilient.

## Backend MVP Summary

| Epic | Story Points |
| ---- | ------------ |
| Foundation | 18 |
| Identity and Ownership | 13 |
| Project Workspace and Memory | 26 |
| Source Consent and Diagnosis | 60 |
| **Total** | **117 SP** |

---

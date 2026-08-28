# Workspace Flow Functional Requirements (FRs)

## FR-1: User Authentication & Authorization

- Users can register, sign in, sign out, and manage their account
- Sessions expire securely and authentication errors are actionable
- Users can access only their own projects and project-owned records

## FR-2: Project Workspace

- Users can create, view, edit, archive, and delete projects
- Projects store purpose and one active GitHub or local source reference
- Deletion requires confirmation and removes owned memory, tasks, and analysis history
- Project detail presents memory, contextual work, latest diagnosis, and next actions

## FR-3: Project Memory

- Users can capture free-form notes linked to a project
- Users can preserve note history and record architecture decisions with rationale
- AI may classify notes and propose a task or decision
- Users must accept, edit, or discard every AI suggestion

## FR-4: Contextual Task Management

- Users can create, update, prioritize, complete, and delete project tasks
- Tasks created from notes preserve `source_note_id`
- Users can view tasks in kanban and list layouts and filter or search them

## FR-5: Source Connection & Consent

- Users can connect one GitHub repository or approved local source per project
- The interface explains what will be read and requests explicit consent
- Users can revoke access and see the source used by each analysis run

## FR-6: AI Provider Configuration

- Users can configure a supported AI provider using their own API key (BYOK)
- Gemini is the default provider and Groq is the initial alternative
- Users can select a provider and optional model for note enrichment or diagnosis
- API keys are encrypted at rest when stored and are never logged or sent to analytics
- Provider failures expose a retryable state and do not prevent raw note capture

## FR-7: Read-Only Project Diagnosis

- Users can start analysis for a connected, consented source
- Agents inspect source content without modifying files
- Each run stores timestamp, source, status, result, and recoverable failures

## FR-8: Findings & Explainable Health Score

- Findings include dimension, severity, evidence or location, and recommendation
- Users can inspect findings by dimension and severity
- Scores use documented weights and expose the cause of score changes
- Findings can reference relevant architecture decisions

## FR-9: Portfolio Dashboard

- Dashboard shows projects, latest score, trend, open tasks, and recent activity
- Users can prioritize projects by health, trend, or activity
- Empty, loading, failure, and first-use states are supported

## FR-10: Responsive & Accessible Experience

- Core authentication, project, memory, task, consent, and diagnosis flows work on desktop and mobile
- Controls support keyboard navigation and assistive technology
- Loading, validation, error, success, and confirmation states are visible
- The experience follows the root `docs/*.png` mockups
- Local-source access uses File System Access API permission controls where supported

## Out of Scope for MVP

- Real-time collaboration, shared projects, roles, and team permissions
- Agent-generated commits, patches, or automatic code changes
- Calendar synchronization, deadline notifications, and broad integrations

---
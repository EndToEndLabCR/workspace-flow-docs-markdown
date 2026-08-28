# Workspace Flow

A project-centered workspace that preserves technical context, turns ideas into actionable work, and diagnoses project health so developers can return to their code with confidence.

---

## Project Overview

Workspace Flow is a full-stack application with two independent projects:

- **Backend**: FastAPI REST API using Clean Architecture and DDD
- **Frontend**: React and TypeScript SPA with responsive Workspace Flow views

### Core Features

- Project memory: notes, ideas, and architecture decisions
- AI enrichment with user-controlled accept, edit, and discard actions
- Contextual tasks that preserve their source note
- Read-only diagnosis of approved GitHub or local sources
- Explainable health score, findings, evidence, recommendations, and trends
- Portfolio dashboard for cross-project prioritization

### Product Boundaries

- Agents do not modify source code during the MVP.
- Source access requires explicit, visible, and revocable consent.
- The MVP supports one owner and private projects; collaboration is deferred.

## MVP Definition

The MVP must support authentication, private project workspaces, memory capture, AI suggestion review, contextual task management, source consent, read-only diagnosis, explainable scoring, and project prioritization.

### Base Mockups

The root mockups [`1.png`](./1.png), [`2.png`](./2.png), [`3.png`](./3.png), [`4.png`](./4.png), [`home.png`](./home.png), and [`screen.png`](./screen.png) define the baseline for authentication, project navigation, workspace detail, and diagnosis-oriented views.

## Documentation Navigation

### Product and Planning

- [Product Definition](./product-definition.md)
- [Executive Summary](./executive-summary.md)
- [User Personas](./user-personas.md)
- [Functional Requirements](./requirements/functional-requirements.md)
- [Non-Functional Requirements](./requirements/non-functional-requirements.md)
- [Success Metrics](./metrics/success-metrics.md)
- [Open Questions](./open-questions.md)
- [Phased Roadmap](./planning/phased-roadmap.md)
- [Role Mapping](./planning/role-mapping.md)
- [Sprint Structure](./planning/sprint-structure.md)

### Architecture and Design

- [Technical Architecture](./architecture/technical-architecture.md)
- [Dependencies](./architecture/dependencies.md)
- [Agent Documentation](./architecture/agents.md)
- [API Contracts](./architecture/api-contracts.md)
- [UI/UX Guidelines](./projects/frontend/prototype/ui-ux-guidelines.md)
- [Workspace Flow Mockups](./ui-ux/prototypes/images/)

### User Stories by Role

- [Epics and User Stories](./user-stories/epics-and-user-stories.md)
- [Tech Lead User Stories](./user-stories/tech-lead-stories.md)
- [Backend Engineer User Stories](./user-stories/backend-engineer-stories.md)
- [Frontend Engineer User Stories](./user-stories/frontend-engineer-stories.md)
- [UI/UX Engineer User Stories](./user-stories/ui-ux-engineer-stories.md)

## Project Structure

### Backend Project

**Location**: [`./projects/backend/`](./projects/backend/)

A Python FastAPI application for identity, project memory, contextual work, source consent, and read-only diagnosis.

**Documentation**: [Backend README](./projects/backend/README.md)

### Frontend Project

**Location**: [`./projects/frontend/`](./projects/frontend/)

A React and TypeScript application for the Workspace Flow portfolio, project workspace, memory, tasks, consent, and diagnosis journeys.

**Documentation**: [Frontend README](./projects/frontend/README.md)

## Architecture Principles

- Clean Architecture with framework-independent domain rules
- DDD bounded contexts for identity, workspace, memory, work, diagnosis, and reporting
- Explicit ownership and consent checks at application boundaries
- Immutable analysis evidence and explainable score calculations
- Read-only source adapters and auditable agent operations

---

**Version**: 2.0
**Last Updated**: August 26, 2026
**Status**: Draft for Review
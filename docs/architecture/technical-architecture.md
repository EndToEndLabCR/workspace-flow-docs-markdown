# Workspace Flow Technical Architecture Overview

## Architecture Pattern

- **Clean Architecture** with Domain-Driven Design (DDD)
- **Bounded Contexts**: Identity, Project Workspace, Memory, Contextual Work, Diagnosis, and Reporting
- **Read-only analysis boundary**: agents inspect approved sources and return evidence; they never mutate source files

### System Layers

#### 1. Presentation Layer (Frontend)

- React with TypeScript
- Ant Design components aligned with Workspace Flow mockups
- Redux Toolkit for authenticated and workspace state
- React Router for protected navigation
- Responsive project portfolio, workspace, memory, task, and diagnosis views

#### 2. Application Layer (Backend API)

- FastAPI with async support
- RESTful resources for projects, notes, tasks, sources, analysis runs, findings, and scores
- JWT authentication and ownership checks at every project boundary
- Request validation, rate limiting, consent checks, and analysis orchestration

#### 3. Domain Layer

- Core rules for project ownership, note enrichment, source consent, contextual tasks, and immutable analysis evidence
- Domain models: User, Project, Note, Task, ProjectSource, AnalysisRun, Finding, and Score
- Repository interfaces and services for score calculation and diagnosis prioritization

#### 4. Infrastructure Layer

- PostgreSQL database with SQLAlchemy ORM and Alembic migrations
- GitHub and local-source adapters behind a read-only source interface
- **Multi-provider AI adapters** (Gemini, Groq, OpenAI, Anthropic) with user-configured API keys
- **LangGraph agent runtime** for explicit, inspectable multi-agent workflows
- **Tree-sitter repository parser** for language-aware project inventory
- Pluggable analysis-agent adapters with auditable requests and responses
- Background execution for long-running analysis runs
- API key encryption at rest (user BYOK model)

### Data Models (Core Entities)

```python
# Simplified domain model structure

User
  - id: UUID
  - email: string (unique)
  - password_hash: string
  - created_at: datetime

Project
  - id: UUID
  - owner_id: UUID (FK to User)
  - name: string
  - description: text
  - status: enum (ACTIVE, ARCHIVED)
  - github_repo_url: string (nullable)
  - created_at: datetime
  - updated_at: datetime

Note
  - id: UUID
  - project_id: UUID (FK to Project)
  - content: text
  - note_type: enum (RAW, IDEA, ARCHITECTURE_DECISION)
  - is_enriched: boolean
  - ai_suggestion: JSON (nullable)
  - created_at: datetime

Task
  - id: UUID
  - project_id: UUID (FK to Project)
  - source_note_id: UUID (nullable FK to Note)
  - title: string
  - description: text
  - status: enum (TODO, IN_PROGRESS, DONE)
  - priority: enum (LOW, MEDIUM, HIGH, URGENT)
  - created_at: datetime
  - updated_at: datetime

ProjectSource
  - id: UUID
  - project_id: UUID (unique FK to Project)
  - source_type: enum (GITHUB, LOCAL)
  - reference: string
  - consented_at: datetime
  - revoked_at: datetime (nullable)

AnalysisRun
  - id: UUID
  - project_id: UUID (FK to Project)
  - source_id: UUID (FK to ProjectSource)
  - status: enum (QUEUED, RUNNING, COMPLETE, FAILED)
  - started_at: datetime
  - completed_at: datetime (nullable)

Finding
  - id: UUID
  - analysis_run_id: UUID (FK to AnalysisRun)
  - dimension: string
  - severity: enum (LOW, MEDIUM, HIGH, CRITICAL)
  - evidence: text
  - location: string (nullable)
  - recommendation: text

Score
  - id: UUID
  - analysis_run_id: UUID (unique FK to AnalysisRun)
  - overall: integer
  - dimensions: JSON
  - calculated_at: datetime

AIProviderConfig
  - id: UUID
  - user_id: UUID (FK to User)
  - provider: enum (GEMINI, GROQ, OPENAI, ANTHROPIC)
  - api_key_encrypted: string
  - model: string (nullable)
  - is_active: boolean
  - created_at: datetime
```

### AI Provider Architecture

Workspace Flow uses a **BYOK (Bring Your Own Key)** model where users configure their own AI provider credentials:

```python
# Provider adapter interface
class AnalysisProvider(ABC):
    @abstractmethod
    async def analyze(self, code: str, context: ProjectContext) -> AnalysisResult:
        pass

# Concrete implementations
class GeminiProvider(AnalysisProvider):
    """Google Gemini provider - default for MVP"""
    
class GroqProvider(AnalysisProvider):
    """Groq provider - optimized for fast inference"""
    
class OpenAIProvider(AnalysisProvider):
    """OpenAI provider - future extensibility"""
    
class AnthropicProvider(AnalysisProvider):
    """Anthropic Claude provider - future extensibility"""
```

**Key principles**:
- Users provide and own their API keys
- Keys are encrypted at rest in the database
- No AI service costs borne by Workspace Flow
- Provider selection per user or per analysis run
- Auditable AI requests and responses (metadata only, not full payloads)

### Agent Orchestration Architecture

Workspace Flow uses **LangGraph** to execute agents as a directed workflow with explicit state, checkpoints, and failure paths. LangGraph is responsible for orchestration only; model calls go through the BYOK provider adapters. The full agent catalog and operational contract are documented in [Agent Documentation](./agents.md).

```text
Analysis Request
  |
  v
Consent and Scope Check
  |
  v
Repository Inventory (Tree-sitter)
  |
  +--> Architecture Agent --+
  +--> Quality Agent -------+--> Findings Correlation --> Score Aggregator
  +--> Security Agent ------+
```

**Agent runtime rules**:

- Each node receives a typed state containing project ID, run ID, source scope, inventory, and approved memory context.
- LangGraph checkpoints status and partial results; a failed node can be retried without duplicating completed findings.
- Tools exposed to agents are read-only: directory listing, file reading within scope, Tree-sitter parsing, and metadata inspection.
- Source code is held in memory only for the run and is not written to logs, traces, analytics, or the database.
- Human approval is required for note enrichment and any future documentation draft before persistence.

### Local Source Access

**File System Access API** (web standard) for MVP:
- User explicitly selects project directory via browser dialog
- Extension whitelist: `.js`, `.ts`, `.py`, `.java`, `.go`, `.rb`, `.php`, etc.
- Exclusion patterns: `node_modules/**`, `.git/**`, `.env*`, `dist/**`, `build/**`
- Browser-native security handles permission model

**Post-MVP**: Consider Electron app or CLI tool for enhanced workflows

### Health Scoring Model

**Five dimensions at 20% weight each**:

| Dimension | Weight | Key Signals |
|-----------|--------|-------------|
| Architecture | 20% | Separation of concerns, dependencies, layer violations |
| Code Quality | 20% | Duplication, complexity, naming, code smells |
| Testing & Reliability | 20% | Coverage, test quality, critical paths |
| Security & Safety | 20% | Vulnerabilities, validation, secrets, auth patterns |
| Documentation & Delivery | 20% | README, API docs, setup instructions, changelog |

**Severity-based scoring**:
```python
SEVERITY_IMPACT = {
    'CRITICAL': -15,  # -15 points from dimension
    'HIGH': -8,
    'MEDIUM': -4,
    'LOW': -2
}

# Dimension score = 100 + sum(severity impacts)
# Overall score = weighted average of dimensions
```

### Data Retention Policy

**Stored after analysis**:
- ✅ Findings with textual evidence (no code fragments)
- ✅ File paths and line ranges
- ✅ Scores and dimension breakdowns
- ✅ Source snapshot hash (detect changes)

**NOT stored**:
- ❌ Source code files
- ❌ Code snippets or fragments
- ❌ Binary or compiled artifacts

**Rationale**: Privacy-first, GDPR-compliant, explainable without retaining user code

---
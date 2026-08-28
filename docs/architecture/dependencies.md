# Workspace Flow Dependencies

## Technical Dependencies

### Backend Dependencies

| Dependency | Version | Purpose | Risk Level |
| ---------- | ------- | ------- | ---------- |
| FastAPI | 0.104+ | API framework and OpenAPI contracts | Low |
| SQLAlchemy | 2.0+ | Domain persistence | Low |
| Alembic | 1.12+ | Database migrations | Low |
| PostgreSQL | 14+ | Durable project memory and analysis history | Low |
| Pydantic | 2.0+ | Request, agent-state, and result validation | Low |
| JWT library | Current supported | Authentication sessions | Medium |
| cryptography | 41+ | API key encryption at rest | Medium |
| LangGraph | Current stable | Agent workflow orchestration and state transitions | Medium |
| Tree-sitter | Current stable | Repository parsing and structural inventory | Low |
| pytest | 7.4+ | Backend tests | Low |

### AI Provider SDKs (user-configured)

| Provider | SDK | Purpose | Notes |
| -------- | --- | ------- | ----- |
| Google Gemini | google-generativeai | Code analysis and documentation | Default provider, free tier available |
| Groq | groq | Fast inference for analysis | Alternative provider, optimized for speed |
| OpenAI | openai | Future extensibility | Optional, user-configured |
| Anthropic | anthropic | Future extensibility | Optional, user-configured |

**Note**: Users provide their own API keys (BYOK model). Workspace Flow does not include AI service costs.

### Agent Tooling

| Tool | Role | Scope |
| ---- | ---- | ----- |
| LangGraph | Orchestrate the inventory, specialist-agent, correlation, and scoring workflow | MVP |
| Tree-sitter | Parse supported languages and build a structural repository inventory | MVP |
| Pydantic | Validate agent state, tool inputs, and structured findings | MVP |
| LangSmith or OpenTelemetry | Trace agent steps without recording source code or prompts containing source | Phase 3 evaluation |

**Decision**: LangGraph is the agent orchestration tool. Gemini and Groq remain model providers, not orchestration frameworks. Agent nodes must use the existing provider adapter and return Pydantic-validated results.

### Frontend Dependencies

| Dependency | Version | Purpose | Risk Level |
| ---------- | ------- | ------- | ---------- |
| React | 18+ | UI framework | Low |
| TypeScript | 5.0+ | Type safety | Low |
| Ant Design | 5.0+ | Workspace components and theming | Low |
| Redux Toolkit | 2.0+ | Authenticated application state | Low |
| React Router | 6.0+ | Protected navigation | Low |
| Vite | 5.0+ | Build tool | Low |
| Axios | 1.6+ | API client | Low |
| Jest | 29+ | Frontend tests | Low |

## External Service Dependencies

| Service | Purpose | Criticality | Fallback Strategy |
| ------- | ------- | ----------- | ----------------- |
| PostgreSQL Database | Project memory and diagnosis persistence | Critical | Backups and restore procedure |
| GitHub API | Optional remote source access | High | Local-source connection or retry |
| User AI Provider (Gemini/Groq/etc) | Note enrichment and diagnosis recommendations | High | Show unavailable state, preserve raw notes and allow retry |
| File System Access API | Browser-native local folder access | High | Require modern browser, show compatibility warning |
| Redis or queue worker | Long-running analysis execution | Medium | Synchronous development fallback |

## Infrastructure Dependencies

- **Hosting**: Container-capable cloud provider (e.g., AWS, GCP, Azure, DigitalOcean)
- **Domain & SSL**: Domain registration and managed certificate
- **CI/CD**: GitHub Actions for linting, testing, and deployment
- **Monitoring**: API health, worker status, analysis-run metrics, error tracking
- **Secrets Management**: 
  - JWT signing keys (server-managed)
  - Database encryption keys (server-managed)
  - User API keys (encrypted at rest, user-owned)

## Cross-Team Dependencies

- **Design Assets**: Workspace Flow root mockups and UI/UX design system before MVP implementation
- **Privacy Review**: Consent, code handling, retention, and deletion behavior before diagnosis work
- **Architecture Review**: Domain and API contracts before backend and frontend parallel work
- **Testing**: End-to-end smoke coverage for registration, memory capture, task conversion, and diagnosis

---
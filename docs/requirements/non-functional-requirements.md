# Non-Functional Requirements (NFRs)

## NFR-1: Performance

| Requirement          | Target                     | Priority    |
| -------------------- | -------------------------- | ----------- |
| API Response Time    | < 200ms (p95)              | Must Have   |
| Page Load Time       | < 2s (p95)                 | Must Have   |
| Task Board Rendering | < 1s for 100 tasks         | Must Have   |
| Database Query Time  | < 100ms (p95)              | Should Have |
| Concurrent Users     | Support 1,000 simultaneous | Should Have |

## NFR-2: Security

- **Authentication**: JWT with 24-hour expiration, refresh tokens
- **Password Policy**: Min 8 characters, complexity requirements
- **Data Encryption**: 
  - HTTPS/TLS for all communications
  - API keys encrypted at rest using industry-standard encryption (e.g., AES-256)
- **SQL Injection Prevention**: ORM with parameterized queries
- **XSS Prevention**: Input sanitization, Content Security Policy
- **CSRF Protection**: CSRF tokens for state-changing operations
- **Rate Limiting**: Max 100 requests/minute per user
- **Data Privacy**: 
  - GDPR compliance considerations
  - No source code retention after analysis (findings only)
  - User-controlled API keys (BYOK model)
  - API keys never logged or transmitted to analytics
- **Source Code Access**:
  - Explicit user consent via File System Access API
  - Browser-native permission model
  - Configurable exclusion patterns (secrets, dependencies, build artifacts)

## NFR-3: Scalability

- **Horizontal Scaling**: Stateless API servers
- **Database**: Connection pooling, query optimization
- **Caching**: Redis for session storage and frequent queries
- **CDN**: Static assets served via CDN
- **Load Balancing**: Nginx or cloud load balancer
- **Database Sharding**: Strategy for 100k+ users (future)
- **AI Provider Scaling**: 
  - User BYOK model eliminates AI service bottleneck
  - Analysis runs queued and executed asynchronously
  - Configurable timeout and retry policies per provider

## NFR-4: Reliability & Availability

- **Uptime Target**: 99.5% monthly
- **Backup Strategy**: Daily automated backups, 30-day retention
- **Disaster Recovery**: RPO < 24 hours, RTO < 4 hours
- **Monitoring**: Health checks every 60 seconds
- **Error Tracking**: Sentry or similar for error reporting
- **Logging**: Structured logging with correlation IDs
- **Analysis Resilience**:
  - Graceful degradation when AI provider unavailable
  - Memory and task features remain functional without analysis
  - Retry mechanism with exponential backoff for failed analyses
  - Clear error messages for API key or quota issues

## NFR-5: Agent Safety and Execution

- **Orchestration**: LangGraph manages explicit agent states, ordering, checkpoints, and retry paths
- **Repository Inventory**: Tree-sitter parses only files within the user's approved source scope
- **Typed Contracts**: Pydantic validates every agent input, output, finding, and score calculation
- **Read-Only Tools**: Agents may list, read, parse, and inspect metadata; no write, commit, or patch tool is exposed
- **Traceability**: Every agent step includes analysis run ID and correlation ID
- **Privacy**: Agent traces contain metadata only and never persist source code or code snippets

## NFR-6: Observability

- **Logging**: Centralized logging (ELK stack or cloud equivalent)
- **Metrics**: Prometheus + Grafana for system metrics
- **Tracing**: OpenTelemetry for distributed tracing
- **Alerting**: PagerDuty or equivalent for critical issues
- **Dashboards**: Real-time monitoring of KPIs

## NFR-7: Maintainability

- **Code Coverage**: Minimum 80% for backend, 70% for frontend
- **Documentation**: OpenAPI/Swagger for all API endpoints
- **Code Style**: Automated linting (ESLint, Black)
- **CI/CD**: Automated testing and deployment pipeline
- **Code Review**: Mandatory PR reviews before merge

## NFR-8: Usability

- **Accessibility**: WCAG 2.1 Level AA compliance
- **Browser Support**: 
  - Latest 2 versions of Chrome, Firefox, Safari, Edge
  - File System Access API compatibility check on load
- **Mobile Support**: iOS 14+, Android 10+ (responsive design)
- **Internationalization**: Support for RTL languages (future)
- **Help Documentation**: 
  - In-app help tooltips and user guide
  - AI provider setup wizard with clear instructions
  - Example API key configuration for Gemini and Groq

---

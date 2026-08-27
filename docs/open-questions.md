# Workspace Flow Open Questions & Decisions

## Resolved Architecture Decisions

### 1. Analysis Provider and Model Boundary

**Decision**: Multi-provider architecture with user-configured API keys (BYOK - Bring Your Own Key)

**Implementation**:
- Users configure their own API keys in settings
- Initial providers: **Gemini** (default, free tier available) and **Groq** (fast inference)
- Extensible adapter pattern allows adding OpenAI, Anthropic, or other providers later
- Workspace Flow does not provide AI services directly

**Provider Adapter Pattern**:
```typescript
interface AnalysisProvider {
  analyze(code: string, context: ProjectContext): Promise<AnalysisResult>;
}

interface AIProviderConfig {
  provider: 'gemini' | 'groq' | 'openai' | 'anthropic';
  apiKey: string;  // User-provided
  model?: string;  // Optional model override
}
```

**Model Boundary**:
- ✅ Model receives: source code, project structure, analysis prompts
- ✅ Model returns: structured findings (JSON with evidence, severity, recommendations)
- ❌ API keys never logged or transmitted to Workspace Flow servers
- ❌ Keys encrypted at rest if stored in database (or kept client-side only)
- ❌ No code fragments sent to Workspace Flow analytics or telemetry

**Rationale**: 
- Privacy-first: users control their data and API relationship
- Cost-effective: no AI service markup or quota management
- Flexible: users choose providers based on their needs and budgets

---

### 2. Secure Local-Source Access Mechanism

**Decision**: File System Access API (web standard) for MVP

**Implementation**:
```javascript
// User explicitly selects project directory
const dirHandle = await window.showDirectoryPicker();

// Workspace Flow scans only consented files
const allowedExtensions = ['.js', '.ts', '.py', '.java', '.go', '.rb', '.php'];
const excludedPatterns = ['node_modules/**', '.git/**', '.env*', 'dist/**', 'build/**'];
```

**Security controls**:
- Explicit folder selection by user (no silent access)
- Extension whitelist and pattern exclusion
- Consent stored with timestamp and revocable
- Permission prompts handled by browser security model

**Post-MVP evolution**:
- Phase 3+: Evaluate Electron desktop app or CLI tool for enhanced workflows
- Current approach works on Chrome, Edge, and Safari (modern browsers)

**Rationale**:
- Zero additional infrastructure or installation
- Browser-native security handles permissions
- Fast to implement and test
- Sufficient for MVP validation

---

### 3. Initial Health Dimensions and Weights

**Decision**: Five balanced dimensions at 20% each, severity-based scoring

**Health Dimensions**:

| Dimension | Weight | Signals Evaluated |
|-----------|--------|-------------------|
| **Architecture** | 20% | Separation of concerns, dependency direction, circular dependencies, layer violations |
| **Code Quality** | 20% | Code duplication, function complexity, naming consistency, code smells |
| **Testing & Reliability** | 20% | Test coverage, test quality, critical paths tested, flaky or missing tests |
| **Security & Safety** | 20% | Known vulnerabilities, input validation, secret exposure, auth patterns |
| **Documentation & Delivery** | 20% | README completeness, API docs, setup instructions, changelog maintenance |

**Scoring Model**:
```python
# Each finding has a severity
SEVERITY_IMPACT = {
    'CRITICAL': -15,  # -15 points from dimension score
    'HIGH': -8,
    'MEDIUM': -4,
    'LOW': -2
}

# Dimension score = 100 + sum(impacts from findings in that dimension)
# Clamped to [0, 100]

# Overall score = weighted average of dimension scores
overall_score = sum(dimension_score * weight for each dimension)
```

**Future configurability**:
- Phase 3+: Allow users to customize weights per project type
- MVP uses fixed 20% weights for consistency and explainability

**Rationale**:
- Balanced approach prevents over-weighting any single aspect
- Severity-based deductions are intuitive and explainable
- Easy to communicate: "Your security score dropped 15 points due to a critical finding"

---

### 4. Source-Fragment Retention Policy

**Decision**: Store findings and textual evidence only; discard source code after analysis

**What is stored**:
```python
class Finding:
    id: UUID
    analysis_run_id: UUID
    dimension: str  # e.g., "Security & Safety"
    severity: Severity  # CRITICAL, HIGH, MEDIUM, LOW
    evidence: str  # ⭐ Textual description, NOT code
    location: str  # "src/auth/service.py:45-67"
    recommendation: str
```

**What is NOT stored**:
- ❌ Complete source code files
- ❌ Code fragments or snippets
- ❌ Binary files or compiled artifacts

**Stored metadata**:
- ✅ File paths and line ranges
- ✅ Source snapshot hash (to detect if code changed)
- ✅ Findings with textual evidence
- ✅ Score calculations and dimension breakdowns

**Example evidence format**:
```
"Function authenticate() in auth.py:45-67 does not validate JWT 
signature. Token comparison uses === without cryptographic verification."
```

**Rationale**:
- Privacy-first: no persistent storage of user source code
- GDPR/compliance-friendly: data deletion is straightforward  
- Explainability: textual evidence + location is sufficient for users
- Re-analysis: users can run new analysis if code has changed

---

### 5. Collaboration and Agent Write Capability

**Collaboration Decision**: Not in MVP (Phase 0-2)

Post-MVP criteria for enabling collaboration:
- [ ] MVP validated with 50+ single-user workflows
- [ ] Proven demand for multi-user features
- [ ] Role-based access control (Owner, Editor, Viewer) implemented
- [ ] Complete audit log of all project changes
- [ ] Real-time sync and conflict resolution architecture
- [ ] Team pricing model defined

**Agent Write Decision**: Limited scope - documentation assistance only

**What "Agent Write" means in Workspace Flow**:
- ❌ NOT code generation or automated refactoring
- ❌ NOT automated commits or pull requests  
- ✅ Documentation assistance: summarize daily work into notes

**Documentation Assistant (Phase 3+)**:
```typescript
interface DocumentationRequest {
  projectId: string;
  timeRange: { from: Date; to: Date };
  sources: {
    includeCommits: boolean;
    includeNotes: boolean;
    includeCompletedTasks: boolean;
  };
  outputFormat: 'markdown' | 'changelog' | 'summary';
}

// Agent reads: commits, notes, completed tasks
// Agent generates: draft documentation
// User reviews and approves before saving as Note
```

**Risk assessment**:
- Low risk: only reads existing project data
- User always reviews generated content before saving
- No direct source code modification
- Optional utility feature, not core value proposition

**Rationale**:
- Code-writing agents require extensive safety infrastructure (sandboxing, rollback, liability)
- Documentation assistance provides value without high-risk concerns
- Feature can be added in Phase 3 after MVP validation without architectural changes

---

## Remaining Open Questions

7. **Pricing Model**

   - Q: Will pricing be based on users, projects, or analysis volume?
   - A: Start with a limited beta; AI costs are borne by users via BYOK model

8. **Deployment Environment**

   - Q: Which cloud and region provide the required privacy and model availability?
   - A: Select during Phase 0 after reviewing data residency requirements

9. **Compliance Requirements**

   - Q: Which privacy and retention obligations apply to source-code analysis?
   - A: Complete a privacy review before enabling external analysis; BYOK model simplifies compliance

### Assumptions Made

1. **User Base**: Initial users are developers managing multiple personal projects
2. **Ownership**: The MVP uses a single-owner project model
3. **Source Access**: Users grant explicit consent for every source connection
4. **Agent Safety**: Analysis agents are read-only and auditable
5. **Design Assets**: Root `docs/*.png` mockups are the visual baseline
6. **Testing**: Developers own unit, integration, accessibility, and smoke testing
7. **Documentation**: API documentation is generated from FastAPI contracts
8. **Persistence**: Analysis history and project memory remain available to the owner

### Risks & Mitigation

| Risk | Probability | Impact | Mitigation Strategy |
| ---- | ----------- | ------ | ------------------- |
| Scope drift toward a generic task manager | High | High | Enforce product boundaries and MVP acceptance criteria |
| Unauthorized source disclosure | Medium | Critical | Explicit consent, minimization, audit logs, and provider review |
| Unexplainable or generic findings | Medium | High | Require evidence, location, recommendation, and decision context |
| Analysis cost or latency grows unexpectedly | Medium | High | Queue runs, cap source scope, measure provider usage, and retry safely |
| Local-source access is technically limited | Medium | High | Validate adapter early and support GitHub as a separate source path |
| Score is not trusted by users | Medium | High | Expose dimensions and weights; validate with usability sessions |
| Database migration or history loss | Low | High | Test migrations, backups, and immutable analysis records |
| External provider outage | Medium | Medium | Preserve memory workflow and show recoverable diagnosis failures |

---
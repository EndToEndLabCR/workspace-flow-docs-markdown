# Workspace Flow Agent Documentation

## Purpose

Workspace Flow uses specialized agents to preserve project context, enrich notes, inspect approved source code, and produce explainable project-health results. Agents are coordinated by **LangGraph** and execute through the BYOK provider adapters for Gemini or Groq.

Agents are not autonomous developers. They do not modify source code, create commits, open pull requests, or silently change user records.

---

## Agent Technology Stack

| Component | Tool | Responsibility |
| --------- | ---- | -------------- |
| Workflow orchestration | LangGraph | State graph, node ordering, checkpoints, retries, and partial results |
| Model execution | Gemini SDK / Groq SDK | Provider-specific model calls through the `AnalysisProvider` adapter |
| Structured contracts | Pydantic | Validate graph state, prompts, findings, proposals, and scores |
| Repository parsing | Tree-sitter | Language-aware inventory of supported source files |
| Source access | File System Access API / GitHub adapter | Read only the source explicitly approved by the user |
| Persistence | PostgreSQL | Store notes, decisions, tasks, runs, findings, and score metadata |

Gemini and Groq are **model providers**. LangGraph is the **agent orchestration tool**. The provider adapter keeps the graph independent from a particular model vendor.

## Agent Catalog

### 1. Context Classifier Agent

**Purpose**: Identify the intent of a raw note without changing the original content.

**Inputs**:

- Raw note content
- Project name and description
- Existing note types

**Output**:

- `RAW`, `IDEA`, or `ARCHITECTURE_DECISION`
- Confidence score
- Suggested title
- Short classification rationale

**Tools**: None beyond the selected model provider.

**Phase**: Phase 1 - Memory Workspace  
**Estimate**: 3 SP

### 2. Task Proposal Agent

**Purpose**: Convert an actionable note into a draft task for user review.

**Inputs**:

- Original note
- Accepted classification
- Project context

**Output**:

- Task title and description
- Suggested priority
- Suggested status
- Draft acceptance criteria

**Tools**: Model provider only. It cannot create the task directly.

**Phase**: Phase 1 - Memory Workspace  
**Estimate**: 5 SP

### 3. Decision Structuring Agent

**Purpose**: Structure a candidate architecture decision while preserving the user's original reasoning.

**Inputs**:

- Original note
- Project context
- Related decisions, when explicitly authorized

**Output**:

- Decision title
- Context
- Decision statement
- Alternatives considered
- Consequences and trade-offs
- Confidence and missing information

**Tools**: Model provider only. The user must approve persistence.

**Phase**: Phase 1 - Memory Workspace  
**Estimate**: 5 SP

### 4. Repository Inventory Agent

**Purpose**: Establish a factual project map before specialist diagnosis begins.

**Inputs**:

- Approved source scope
- File extension allowlist
- Exclusion patterns
- File System Access API or GitHub adapter

**Output**:

- Languages and frameworks
- Modules, entry points, and configuration files
- Test locations
- Dependency manifests
- File-level evidence map
- Unsupported or skipped files

**Tools**: Read-only directory listing, file reading within scope, Tree-sitter parsing, and metadata inspection.

**Phase**: Phase 2 - Diagnosis  
**Estimate**: 5 SP

### 5. Architecture Agent

**Purpose**: Detect structural risks and compare implementation boundaries with recorded decisions.

**Inputs**:

- Repository inventory
- Read-only source content within scope
- Approved architecture decisions

**Output**:

- Architecture findings
- Affected files and line ranges
- Severity and confidence
- Evidence and recommendation

**Tools**: Tree-sitter queries, dependency metadata, scoped file reads, and model provider.

**Phase**: Phase 2 - Diagnosis  
**Estimate**: 8 SP

### 6. Quality Agent

**Purpose**: Evaluate maintainability, complexity, duplication, testing signals, and documentation quality.

**Inputs**:

- Repository inventory
- Source files within scope
- Test and documentation metadata

**Output**:

- Code Quality findings
- Testing and Reliability findings
- Documentation and Delivery findings
- Evidence, severity, location, and recommendation

**Tools**: Tree-sitter, test-file discovery, documentation-file discovery, and model provider.

**Phase**: Phase 2 - Diagnosis  
**Estimate**: 8 SP

### 7. Security Agent

**Purpose**: Identify security and safety risks without exposing secrets in results or telemetry.

**Inputs**:

- Approved source scope
- Configuration metadata
- Dependency manifests
- Authentication and input-validation code

**Output**:

- Security and Safety findings
- Evidence description without secret values
- Severity, location, confidence, and recommendation

**Tools**: Secret-pattern checks that return redacted matches, dependency metadata, Tree-sitter, and model provider.

**Phase**: Phase 2 - Diagnosis  
**Estimate**: 8 SP

### 8. Findings Correlation Agent

**Purpose**: Compare specialist findings with project memory so intentional decisions are not treated as unexplained defects.

**Inputs**:

- Findings from specialist agents
- Architecture decisions
- Relevant notes and task context

**Output**:

- `CONFIRMED`, `CONTEXTUALIZED`, or `UNRESOLVED` finding status
- Related decision IDs
- Correlation explanation
- Recommendation priority

**Tools**: Project-memory repository and model provider. It cannot delete or suppress a finding.

**Phase**: Phase 2 - Diagnosis  
**Estimate**: 8 SP

### 9. Score Aggregator Agent

**Purpose**: Calculate the reproducible health score from validated findings.

**Inputs**:

- Correlated findings
- Fixed MVP dimension weights
- Severity impact rules
- Scoring model version

**Output**:

- Five dimension scores
- Overall score from 0 to 100
- Finding counts by severity
- Calculation version and explanation

**Tools**: Deterministic scoring function. Model inference is not required.

**Phase**: Phase 2 - Diagnosis  
**Estimate**: 5 SP

---

## LangGraph Workflow

```text
START
  |
  v
Validate Consent and Source Scope
  |
  v
Build Repository Inventory with Tree-sitter
  |
  +--> Architecture Agent --+
  +--> Quality Agent -------+--> Findings Correlation --> Score Aggregator --> END
  +--> Security Agent ------+
```

### Graph State

Each node receives and returns a typed state containing:

```python
class AnalysisState(BaseModel):
    project_id: UUID
    analysis_run_id: UUID
    source_scope: SourceScope
    provider_config_id: UUID
    inventory: RepositoryInventory | None
    findings: list[FindingDraft]
    correlated_findings: list[CorrelatedFinding]
    score: ScoreDraft | None
    completed_nodes: list[str]
    errors: list[AgentError]
```

### Execution Rules

1. Consent and source scope are validated before any source read.
2. Inventory must complete before specialist agents run.
3. Architecture, Quality, and Security agents may run in parallel after inventory.
4. Correlation runs only after specialist results are validated.
5. Score Aggregator runs only on validated and correlated findings.
6. LangGraph checkpoints completed nodes and partial results.
7. A failed node can be retried without duplicating findings from completed nodes.
8. A partial result is visible to the user and never replaces a previous completed analysis.

## Provider Selection

Provider selection is handled outside the graph by `AIProviderConfig` and the `AnalysisProvider` adapter:

```text
LangGraph node
    -> AnalysisProvider
        -> GeminiProvider -> Google Gemini SDK
        -> GroqProvider   -> Groq SDK
```

- Gemini is the default MVP provider.
- Groq is the initial alternative for fast inference.
- OpenAI and Anthropic can be added without changing graph nodes.
- The user's API key is loaded only for the provider call and is never included in graph state, logs, or telemetry.

## Tool Permissions

| Tool | Read | Write | Allowed Agents |
| ---- | ---- | ----- | -------------- |
| Scoped directory listing | Yes | No | Inventory, Architecture, Quality, Security |
| Scoped file reader | Yes | No | Inventory, Architecture, Quality, Security |
| Tree-sitter parser | Yes | No | Inventory, Architecture, Quality, Security |
| Dependency metadata reader | Yes | No | Inventory, Quality, Security |
| Project memory reader | Yes | No | Correlation, Decision Structuring |
| Task creation API | No | Yes | User action only |
| Note persistence API | No | Yes | User approval only |
| Source filesystem writer | No | No | Not exposed |
| Git commit or pull-request tool | No | No | Not exposed |

## Privacy and Retention

- Source files are held only for the duration required by the analysis run.
- Agents must not return source snippets as evidence; evidence is textual and points to a path and line range.
- Prompts, source content, API keys, and raw provider payloads are excluded from logs and analytics.
- Persisted results contain findings, redacted evidence, locations, snapshot hash, score metadata, and run metadata only.
- BYOK credentials are encrypted at rest when stored and are never sent to Workspace Flow analytics.

## Error Handling

| Error | Behavior |
| ----- | -------- |
| Missing provider key | Stop the model node, preserve raw note/source state, show setup guidance |
| Provider timeout or quota | Mark node failed, retain partial result, allow retry |
| Invalid model output | Reject with Pydantic validation error and retry once with a constrained response format |
| Unsupported file type | Record it in inventory as skipped; continue analysis |
| Permission revoked | Stop source reads, mark run failed, preserve prior findings |
| Specialist agent failure | Continue other specialists, show partial diagnosis, allow targeted retry |
| Score calculation failure | Do not publish an incomplete score; preserve validated findings |

## Definition of Done for Agents

- Every agent has a versioned input and output schema.
- Every source-reading tool enforces the approved scope.
- Every finding includes dimension, severity, evidence, location, recommendation, and confidence.
- No agent has access to source-writing, commit, pull-request, or unrestricted filesystem tools.
- LangGraph supports checkpoints, retry, partial results, and correlation IDs.
- Provider keys and source code are absent from logs, traces, analytics, and persisted analysis results.
- Unit tests cover each agent contract; integration tests cover the complete graph and failure paths.

---

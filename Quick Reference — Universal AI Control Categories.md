# Universal AI Control Categories

**A quick reference guide for building production-grade AI systems**

Based on industry best practices and the Production-Grade AI Systems catalog.
https://pdfhost.io/v/G6UzXJZX8H_Production-Grade_AI_Systems 

---

## What Are AI Controls?

AI controls are safeguards that keep your system predictable, safe, and accountable. They're the difference between a prototype and something you can run in production.

Think of them as the infrastructure that lets you sleep at night.

---

## The 16 Control Categories

### 1. Identity & Ownership Controls (SEC-01)

**Purpose:** Tie every action, cost, and state change to a stable principal.

**Why it matters:** Without consistent identity, accountability and cost tracking collapse under load or retries.

**What to implement:**
- Every request carries authenticated user/token context
- Logging and cost limits tied to user identity
- Permission checks at every boundary

**Example:** A request comes in with a user ID. Every model call, every state change, every cost is attributed to that user.

---

### 2. Execution Containment (SEC-02)

**Purpose:** Sandbox all execution that invokes models or untrusted inputs.

**Why it matters:** Unbounded AI execution can exhaust resources or produce unsafe effects.

**What to implement:**
- Time limits for model calls
- CPU and memory boundaries
- Container isolation for user code
- Execution timeouts

**Example:** User-supplied code runs inside a sandbox with a 30-second timeout and 512MB memory limit.

---

### 3. Output Handling & Downstream Safety (SEC-03)

**Purpose:** Validate, sanitize, and contextualize AI outputs before use.

**Why it matters:** AI outputs look plausible even when incorrect. They shouldn't be used without checks.

**What to implement:**
- Content filtering for harmful outputs
- Output validation against schemas
- Confidence scoring
- Human review for high-risk decisions

**Example:** Before returning generated text to a user, scan for toxic content, validate structure, and attach a confidence score.

---

### 4. Observability & Traceability Controls (OBS-05)

**Purpose:** Make the chain of decisions and context reconstructable.

**Why it matters:** Without traceability, diagnosing failures or compliance issues is nearly impossible.

**What to implement:**
- Structured logging for every model call
- Request/response IDs for tracing
- Model version tracking
- Token usage recording
- Prompt storage (with privacy considerations)

**Example:** Log: `{request_id: "abc123", user: "user_42", model: "gpt-4-v1.2", tokens_used: 145, timestamp: "2024-01-15T10:30:00Z"}`

---

### 5. Retrieval Integrity & Drift Control (AI-02)

**Purpose:** Guard retrieval mechanisms so drift doesn't silently break assumptions.

**Why it matters:** Retrieved context directly influences AI behavior and long-term correctness.

**What to implement:**
- Vector store freshness checks
- Retrieval quality scoring
- Index versioning
- Drift detection alerts

**Example:** When retrieving vectors for context, check that the source index version matches what the system expects.

---

### 6. Explicit State Transitions (REL-01)

**Purpose:** Treat state changes as atomic, well-defined transitions.

**Why it matters:** Implicit or untracked state leads to inconsistency under retries or partial failures.

**What to implement:**
- Event-sourced state updates
- Before/after version tracking
- Atomic state operations
- Clear transition documentation

**Example:** State change: `{from: state_v5, action: "process_payment", to: state_v6, timestamp: "..."}`

---

### 7. Bounded Retries & Degraded Modes (REL-04)

**Purpose:** Limit retries and degrade predictably when dependencies fail.

**Why it matters:** Unbounded retries amplify resource consumption and costs.

**What to implement:**
- Circuit breakers for failing services
- Retry quotas per user/principal
- Exponential backoff with jitter
- Fallback paths to degraded behavior

**Example:** If the model API is down, return cached results or a "service degraded" message instead of retrying forever.

---

### 8. Cost Budget & Abuse Guards (CST-03)

**Purpose:** Enforce per-user or per-tenant cost limits.

**Why it matters:** AI costs scale with uncertainty; unbounded usage leads to runaway bills or denial of service.

**What to implement:**
- Token count budgets per user
- Monthly usage caps
- Rate limiting
- Cost alerts and warnings

**Example:** User "alice" has a $50/month budget. Track tokens spent. Alert at 80%. Block at 100%.

---

### 9. Storage & Artifact Boundary Controls (OPS-02)

**Purpose:** Write logs and artifacts only to designated safe locations with retention policies.

**Why it matters:** Artifacts can leak sensitive info or fill disks if unbounded.

**What to implement:**
- Separate writable volumes for different data types
- Storage quotas per user/tenant
- Data retention policies
- Backup and archival rules

**Example:** Logs live on `/logs` with 30-day retention. User data lives on `/data` with per-user quotas.

---

### 10. Human-in-the-Loop & Recovery (OPS-04)

**Purpose:** Provide clear paths for operator intervention and recovery.

**Why it matters:** Fully automated AI actions without intervention paths can escalate incidents.

**What to implement:**
- Admin dashboards with rollback buttons
- Audit trails of all actions
- Anomaly indicators with explanations
- Clear escalation paths

**Example:** An admin can see that 1,000 tokens were used in 10 seconds, click "investigate," see the request details, and rollback if needed.

---

### 11. Data Quality & Governance

**Purpose:** Ensure data used for training, retrieval, or decisions is accurate and versioned.

**Why it matters:** Bad data leads to bias and cascading errors—often silently.

**What to implement:**
- Data lineage tracking (where data comes from)
- Validation schemas before use
- Bias detection checks
- Version control for datasets
- Documentation of data sources

**Example:** Before retraining a model, validate that training data matches the expected schema and check for known bias sources.

---

### 12. Transparency & Explainability

**Purpose:** Communicate limitations, uncertainties, and reasoning when appropriate.

**Why it matters:** Users need to understand AI behavior, especially when it's wrong.

**What to implement:**
- Confidence scores on outputs
- Explanation of reasoning steps
- Clear disclaimers about limitations
- Uncertainty quantification

**Example:** Instead of just returning "The answer is 42," return: `{answer: "42", confidence: 0.87, reasoning: "Based on vectors 5, 12, and 23", disclaimer: "This is a summarization; verify critical facts."}`

---

### 13. Lifecycle & Version Control

**Purpose:** Track versions of models, data, indices, and pipelines.

**Why it matters:** Mismatched versions silently create nondeterministic behavior.

**What to implement:**
- Version tags baked into responses
- Matching runtime artifacts
- Model and data versioning
- Deprecation policies

**Example:** Every API response includes `{model_version: "v1.2.3", retrieval_index: "vectors_2024-01", ...}`

---

### 14. Continuous Monitoring & Alarms

**Purpose:** Detect anomalies in latency, cost, correctness, or usage.

**Why it matters:** Silent drift is the most insidious AI failure—only metrics catch it early.

**What to implement:**
- Latency tracking (p50, p95, p99)
- Token usage trends per user
- Error rate monitoring
- Cost anomaly detection
- Custom metric dashboards

**Example:** Alert when p99 latency exceeds 5 seconds, or when a user's daily token usage spikes 10x above normal.

---

### 15. Security Controls (Traditional + AI-Specific)

**Purpose:** Protect against unauthorized access, injection attacks, and model hijacking.

**Why it matters:** AI systems expose new attack surfaces (prompt injection, model exploits).

**What to implement:**
- Input sanitization and validation
- Role-based access control (RBAC)
- Rate limiting per IP/user
- Prompt injection detection
- API authentication and encryption

**Example:** Reject requests with suspicious patterns in prompts. Only authenticated users can access the API.

---

### 16. Governance & Accountability (Org-Level)

**Purpose:** Define policies, risk tiers, review cycles, and ownership.

**Why it matters:** Tech controls must be backed by organizational accountability.

**What to implement:**
- RACI matrices for AI development
- Regular risk reviews
- KPI tracking and reporting
- Change control processes
- Incident response procedures

**Example:** "The ML team owns model updates. The ops team owns deployment. Both must sign off on high-risk changes."

---

## How to Use This Guide

### 🎯 As a Design Checklist

Walk through each category during design and ask: *"Have we defined this in our system?"*

### 📊 As a Scoring Rubric

Rate each control on implementation maturity:
- 0 = Not addressed
- 1 = Planned
- 2 = Partially implemented
- 3 = Fully implemented
- 4 = Monitoring and improving

### 🔍 As an Audit Tool

Align security/compliance reports to these 16 categories for clarity and consistency.

### 📚 As a Learning Path

Study in this order (by operational risk):

1. **Identity & Ownership** → Know who is doing what
2. **Observability & Traceability** → See what happened
3. **Cost Budget & Abuse Guards** → Control runaway spending
4. **Execution Containment** → Bound resource use
5. **Output Handling** → Validate before use
6. **Storage & Artifacts** → Protect what you store
7. **State Transitions** → Keep data consistent
8. **Retrieval Integrity** → Keep context accurate
9. **Retries & Degradation** → Handle failures gracefully
10. **Monitoring & Alarms** → Catch drift early
11. **Human-in-the-Loop** → Enable intervention
12. **Data Quality** → Prevent bad inputs
13. **Lifecycle & Versions** → Track changes
14. **Transparency** → Explain decisions
15. **Security** → Defend the system
16. **Governance** → Align the organization

---

## Key Principle

**Controls are infrastructure, not afterthoughts.**

The best time to add them is before you need them. The second-best time is today.

---

## References

- [Production-Grade AI Systems](https://pdfhost.io/v/G6UzXJZX8H_Production-Grade_AI_Systems)
- [NIST AI Risk Framework](https://www.nist.gov/artificial-intelligence)
- [Microsoft Trusted AI](https://www.microsoft.com/en-us/ai/responsible-ai)

# **System Design – Personal Guideline & Architecture Playbook**

> A copy-pasteable, public-facing Markdown note that shows how I think, what I analyze, how I document, and the checklists I use.

---

## **Table of Contents**

1. [Purpose & Scope](#purpose--scope)
2. [End-to-End Process (Steps)](#endtoend-process-steps)
3. [What I Analyze (Quality Attributes & Constraints)](#what-i-analyze-quality-attributes--constraints)
4. [Discovery & Requirements Questionnaire](#discovery--requirements-questionnaire)
5. [Decision-Making Framework (ADR, Trade-offs, ATAM)](#decisionmaking-framework-adr-tradeoffs-atam)
6. [Modeling & Documentation (C4, SDD, Diagrams)](#modeling--documentation-c4-sdd-diagrams)
7. [Data Strategy (Consistency, Durability, Backups)](#data-strategy-consistency-durability-backups)
8. [Architecture & Integration Patterns](#architecture--integration-patterns)
9. [Reliability & Resilience](#reliability--resilience)
10. [Security & Privacy](#security--privacy)
11. [Observability & Monitoring](#observability--monitoring)
12. [Performance & Capacity](#performance--capacity)
13. [Delivery & Operations (CI/CD, IaC, Releases)](#delivery--operations-cicd-iac-releases)
14. [Compliance, Risk & Governance](#compliance-risk--governance)
15. [Cost & FinOps](#cost--finops)
16. [Test Strategy & Quality Gates](#test-strategy--quality-gates)
17. [Recovery & Continuity (RTO/RPO, DR, Chaos)](#recovery--continuity-rto-rpo-dr-chaos)
21. [My Architecture Playbook (Principles & Heuristics)](MY_PLAYBOOK.md)
19. [Checklists](#checklists)
    * [Security Checklist](checklists/security_checklist.md)
    * [Testing Checklist](checklists/testing_checklist.md)
    * [Monitoring/Observability Checklist](checklists/monitoring_observability_checklist.md)
20. [Templates](#templates)
    * [ADR Template](templates/adr_template.md)
    * [SDD Outline](templates/sdd_template.md)
    * [SLO Template](templates/slo_template.md)
    * [Runbook Template](templates/runbook_template.md)
    * [Incident Postmortem Template](templates/incident_postmortem_template.md)
21. [Glossary](GLOSSARY.md)

---

## Purpose & Scope

The **purpose** of this playbook is to serve as a **comprehensive, practical, and evolving guide** to system design.  
It captures the structured approach, principles, and standards that I apply when designing, documenting, and operating modern systems.  
It is not tied to a specific technology stack but instead emphasizes **timeless architectural practices** and **decision-making frameworks** that scale across projects, organizations, and domains.

### Why this playbook exists
- To create a **repeatable framework** for approaching system design rather than relying on ad-hoc decisions.
- To provide **reference material** that speeds up analysis, design, and reviews.
- To document **heuristics and trade-offs** that often remain implicit or undocumented in projects.
- To ensure that important aspects such as **reliability, security, observability, and cost** are always considered from the start.
- To act as a **learning resource** for team members, mentees, or contributors, showcasing how I think about architecture.
- To maintain a **living knowledge base**, evolving as practices, technologies, and organizational needs change.

### Scope of the playbook
- **End-to-end lifecycle** of systems: from business requirements discovery → architecture design → implementation → operations → evolution.
- Covers **both functional and non-functional requirements**, with a focus on non-functionals that drive real-world success (availability, latency, security, etc.).
- Provides **templates and checklists** that can be directly applied in design sessions, reviews, and production readiness assessments.
- Offers **patterns and principles** rather than prescriptive solutions, so it can adapt to cloud, on-premises, or hybrid environments.
- Incorporates **industry standards** (e.g., ISO, NIST, OWASP, SRE) to ensure compatibility with common compliance and governance requirements.
- Applies equally to **new greenfield systems** and **legacy modernization efforts**, helping with both design from scratch and incremental evolution.

### Intended audience
- **System architects** who need structured approaches and repeatable methods.
- **Developers/engineers** who want to understand architectural decision-making.
- **DevOps/SRE teams** responsible for reliability, observability, and security in production.
- **Tech leads & mentors** who want to coach teams in design thinking.
- **Myself**, as a reference and checklist to avoid overlooking critical aspects during fast-paced projects.

---

## **End-to-End Process (Steps)**

1. **Context & Goals** – Define business outcomes, critical user journeys (CUJs), success metrics, constraints.
2. **Non-Functionals (NFRs)** – Quantify availability, latency, throughput, durability, security, compliance, cost.
3. **Domain Discovery** – Event storming / DDD to find bounded contexts, core entities, and workflows.
4. **Architecture Candidates** – Sketch 2–3 options (C4-level), list trade-offs; prototype risks if needed.
5. **Decide** – Compare with requirements; record **ADRs** for major choices.
6. **Data Strategy** – Consistency model, schema/contract, retention, indexing, backup/restore, migration plan.
7. **Reliability Plan** – SLOs/SLIs, failure modes, resilience patterns, capacity & scaling plan.
8. **Security & Privacy Plan** – Threat model (STRIDE/PASTA), authn/z, secrets, encryption, supply chain.
9. **Observability Plan** – Metrics/logs/traces, dashboards, alerts, runbooks, on-call.
10. **Delivery Plan** – CI/CD, environments, IaC, release strategy (blue/green, canary), rollback.
11. **Test Strategy** – Unit→Contract→Integration→E2E; performance, chaos, and security testing.
12. **Compliance & Risk** – Data handling, auditability, risk register, approvals.
13. **Cost/FinOps** – Capacity vs. spend, budgets, autoscaling, cost alerts.
14. **Document** – C4 diagrams, SDD, APIs, data flows; publish & version docs.
15. **Review & Gate** – Architecture review against checklists; sign-offs on SLOs, security, and cost.
16. **Operate & Evolve** – Post-launch telemetry, incident learning, ADRs for changes, deprecation policy.

---

## What I Analyze (Quality Attributes & Constraints)

When designing a system, functional requirements (features, workflows, business rules) usually define **what** the system must do.  
However, **quality attributes** and **constraints** determine **how well** the system performs under real-world conditions and whether it can be trusted, maintained, and evolved.  
Ignoring them early often leads to costly redesigns, outages, or security incidents.  
This section summarizes the **dimensions I always analyze and quantify** before committing to architecture decisions.

### Core quality attributes

#### 1. **Availability**
- Target SLAs/SLOs (e.g., 99.9%, 99.99%).
- Redundancy model (multi-AZ, multi-region).
- Single points of failure and mitigation strategies.
- How to measure uptime and communicate outages.

#### 2. **Reliability & Resilience**
- MTBF (Mean Time Between Failures) and MTTR (Mean Time To Recovery).
- Failure isolation (bulkheads, circuit breakers).
- Graceful degradation vs. hard failure.
- Chaos testing and recovery drills.

#### 3. **Consistency**
- Which consistency model applies to each workflow:
    - Strong consistency (e.g., financial transactions).
    - Causal or read-your-writes (UX-sensitive flows).
    - Eventual consistency (analytics, caching, replicated search).
- Trade-off analysis: **CAP theorem** and **PACELC** considerations.

#### 4. **Durability**
- Guarantees that once data is committed, it remains safe.
- Storage replication factors, write acknowledgements, journaling.
- Backup and archival policies.
- Validation through restore drills, not just backup schedules.

#### 5. **Recoverability**
- RTO (Recovery Time Objective): how fast must the system be restored.
- RPO (Recovery Point Objective): how much data can be lost.
- DR (Disaster Recovery) strategy: hot–warm–cold sites.
- Testing: chaos drills, failover simulations, region evacuation.

#### 6. **Performance & Latency**
- End-to-end latency budgets decomposed across services.
- Throughput requirements (requests/sec, MB/s).
- Tail latency (p95, p99), not just averages.
- Concurrency levels and queuing behavior under load.

#### 7. **Scalability**
- Vertical vs. horizontal scaling options.
- Elasticity: ability to scale up/down automatically.
- State management (stateless services, sticky sessions, distributed caches).
- Partitioning/sharding strategies for databases.

#### 8. **Security**
- Authentication (users, services) and authorization models (RBAC, ABAC).
- Data protection in transit and at rest.
- Threat modeling for critical workflows.
- Secrets management and key rotation.
- Secure defaults and supply-chain security (SCA, SBOM).

#### 9. **Privacy & Compliance**
- Data classification (public, confidential, PII, financial).
- Regional storage requirements (GDPR, HIPAA, etc.).
- Data minimization and retention rules.
- Audit logging and tamper-proof evidence.

#### 10. **Observability & Operability**
- Logging, metrics, tracing strategy.
- Error budgets and SLO monitoring.
- Actionable alerting and runbooks.
- Operational toil minimization (automation, self-healing).

#### 11. **Maintainability**
- Modularity and separation of concerns.
- Dependency management (internal & external).
- Code and infra testability.
- Ability to onboard new engineers quickly.

#### 12. **Portability**
- Vendor lock-in risks.
- Cloud-agnostic vs. cloud-native trade-offs.
- Ability to migrate workloads without redesign.

#### 13. **Cost & Efficiency**
- CapEx vs. OpEx impact of choices.
- Unit economics (cost per request, per tenant, per GB stored).
- Autoscaling and lifecycle management for unused resources.
- Budget thresholds and cost anomaly detection.

### Constraints
Constraints are non-negotiable boundaries within which design decisions must fit. Examples include:
- **Technology constraints**: mandated languages, frameworks, or platforms.
- **Organizational constraints**: team skillsets, legacy systems, release cycles.
- **Regulatory/legal constraints**: GDPR, HIPAA, PCI DSS, ISO 27001.
- **Operational constraints**: on-call maturity, monitoring tooling, incident response capability.
- **Timeline constraints**: deadlines that limit experimentation or scope.
- **Data gravity constraints**: where data already resides and cost of moving it.

### Why this matters
Every system is a negotiation between **competing attributes**:
- Availability vs. Consistency (CAP).
- Cost vs. Performance.
- Simplicity vs. Extensibility.
- Time-to-market vs. Long-term maintainability.

By **explicitly analyzing and documenting these dimensions upfront**, I make trade-offs transparent and ensure stakeholders understand **what is optimized, what is compromised, and why**.

---

## Discovery & Requirements Questionnaire

System design begins with **discovery** — gathering context, aligning on business needs, and uncovering risks before making architectural decisions.  
Without a structured discovery phase, designs risk solving the wrong problem, missing critical requirements, or over-engineering.  
I use a **requirements questionnaire** to guide workshops, stakeholder interviews, and early design sessions.  
It ensures I cover both **functional goals** and **non-functional expectations** (NFRs).

### 1. Business & product context
- **Business goals**: What outcome is the system expected to achieve? (e.g., increase revenue, reduce churn, enable new product line)
- **Critical User Journeys (CUJs)**: Which workflows are business-critical and must never fail?
- **KPIs & success metrics**: How will success be measured (e.g., uptime %, latency targets, user growth)?
- **Stakeholders**: Who are the primary users, operators, and decision-makers?
- **Lifecycle stage**: Is this a prototype, MVP, or production-grade system?
- **Growth forecast**: Expected traffic/usage in 6, 12, 24 months.

### 2. Functional requirements
- **Core features**: What the system must do (MVP scope vs. long-term vision).
- **User journeys**: High-level flows (onboarding, checkout, reporting, etc.).
- **Integration points**: Which external systems/APIs must it talk to?
- **Domain boundaries**: Are there clear bounded contexts (DDD perspective)?
- **Edge cases**: What failure scenarios must be handled gracefully?

### 3. Data requirements
- **Data entities**: What are the core objects (users, transactions, logs, etc.)?
- **Data volume**: Estimated size at launch, daily growth, long-term projections.
- **Access patterns**: Read-heavy, write-heavy, mixed?
- **Retention policies**: How long should data be stored? Is archiving required?
- **Compliance classification**: PII, PCI, HIPAA, financial records?
- **Migration & lineage**: Is legacy data to be imported? How do we track provenance?

### 4. Non-functional requirements (NFRs)
- **Availability**: Target SLA/SLO (99.9%, 99.99%, etc.).
- **Performance**: Expected response times, throughput, concurrency levels.
- **Scalability**: Anticipated user growth, seasonal spikes, regional distribution.
- **Reliability**: Error budgets, MTTR expectations.
- **Security**: Authentication/authorization, encryption, threat modeling needed.
- **Privacy**: Data minimization, anonymization, retention.
- **Compliance**: Industry standards (GDPR, ISO, SOC2, HIPAA, PCI DSS).
- **Observability**: What level of monitoring/logging/tracing is required?
- **Maintainability**: Team skills, code ownership, ease of onboarding.
- **Cost constraints**: Budget thresholds, cloud credits, OPEX vs. CAPEX.

### 5. Operational requirements
- **Release cadence**: How often should new features be deployed?
- **Rollback expectations**: Do we need instant rollback?
- **On-call maturity**: Is there a team ready to operate 24/7?
- **Monitoring & alerting**: Who gets paged, for what, and how?
- **Incident response**: Are runbooks/playbooks already defined?
- **Change management**: Is CI/CD required? Manual approvals?

### 6. Risks & assumptions
- **What must never happen?** (e.g., losing customer payments, data corruption).
- **What risks are acceptable?** (e.g., eventual consistency, small downtime during deploy).
- **Known technical risks**: New/untested tech stack, large migrations, vendor lock-in.
- **Organizational risks**: Limited skillsets, competing priorities, unclear ownership.
- **Assumptions**: What are we assuming about users, traffic, integrations, or regulations?

### 7. Constraints
- **Technical**: mandated frameworks, languages, cloud providers.
- **Regulatory**: data residency, certifications (ISO, SOC2, FedRAMP).
- **Timeline**: go-live deadline, must-have milestones.
- **Resources**: team size, expertise, availability.
- **Legacy dependencies**: what must be integrated or preserved.

### 8. Example Questions I Always Ask
- What is the **most important thing** the system must never fail at?
- What is the **expected peak load** (DAU/MAU, TPS/QPS)?
- What is the **worst-case incident** we should design for?
- How much **data loss is acceptable** (RPO)?
- How much **downtime is acceptable** (RTO)?
- Who will **operate the system** once it is in production?
- What is the **budget ceiling** for infra and operations?

---

## Decision-making framework (ADR, Trade-offs, ATAM)

Architecture is not about finding a "perfect" design — it is about making **informed, documented, and transparent decisions** under constraints.  
A good system design acknowledges **trade-offs** and ensures that the reasoning behind key decisions is explicit, reviewable, and adaptable.  
To achieve this, I use a combination of **Architecture Decision Records (ADRs)**, **trade-off analysis**, and **ATAM-lite (Architecture Tradeoff Analysis Method)**.

### 1. Architecture Decision Records (ADRs)
ADRs are **lightweight documents** that capture important architectural choices, their context, and consequences.

I create an ADR whenever a decision:
- Is **hard to reverse** (e.g., database type, cloud provider, tenancy model).
- Has a **long-term cost** (e.g., build vs. buy, protocol choice, data consistency model).
- Involves a **significant trade-off** (e.g., latency vs. availability).
- Impacts **multiple teams or services**.

**ADR structure I follow**:
1. **Context**: what problem we are solving, what constraints exist.
2. **Decision**: the selected option and its scope.
3. **Alternatives considered**: options rejected and why.
4. **Consequences**: pros, cons, risks, follow-ups.

👉 This ensures **decision traceability** — future engineers can understand *why* something was chosen, not just *what* was chosen.

### 2. Trade-off analysis
Every design involves compromises. To make them explicit, I frame trade-offs along **common axes**:

- **Performance vs. Consistency**
- **Availability vs. Cost**
- **Security vs. Usability**
- **Simplicity vs. Extensibility**
- **Time-to-market vs. Maintainability**
- **Autonomy vs. Standardization**
- **Build vs. Buy (or Open Source vs. Managed Service)**

For each axis, I:
- Define what matters most for this system.
- Document which side of the trade-off we are favoring.
- Communicate impact to stakeholders (e.g., eventual consistency means users may see stale data for up to 5 seconds).

### 3. ATAM-lite (Architecture Tradeoff Analysis Method)
The full ATAM process is heavy; I apply a **lightweight version** to evaluate designs quickly.

Steps:
1. **Identify business drivers**: what success means for the system.
2. **List quality attribute scenarios** (e.g., "System must handle 1M requests/day with <200ms latency at p95").
3. **Generate candidate architectures**: sketch at least 2–3 possible designs.
4. **Evaluate trade-offs**: compare candidates against quality scenarios.
5. **Document sensitivity points**: parts of the system where small changes have big effects (e.g., caching layer, DB replication).
6. **Document risks**: potential weaknesses and how to mitigate them.

👉 This ensures that architecture is not selected by gut feeling, but evaluated against **explicit quality goals**.

### 4. Risk Management
For every major decision, I maintain a **risk log**:
- **Risk description**: what could go wrong.
- **Likelihood/impact**: probability and severity.
- **Mitigation**: fallback plan, guardrails, experiments.
- **Status**: open, mitigated, closed.

I also use **time-boxed spikes** for uncertainty:
- Prototype quickly to reduce unknowns (e.g., test DB scalability, API latency).
- Define **exit criteria**: what success/failure looks like.
- Record outcomes in an ADR.

### 5. My principles
- **Document, don’t debate endlessly**: ADRs prevent re-arguing settled decisions.
- **Make reversibility explicit**: “one-way vs. two-way doors” (Amazon principle).
- **Prefer evidence over opinion**: benchmarks, small experiments, or references.
- **Communicate trade-offs clearly**: stakeholders must understand costs, not just benefits.
- **Decide with context, not trends**: choose tools because they fit requirements, not hype.

---

## Modeling & Documentation (C4, SDD, Diagrams)

Good documentation ensures that architecture is **understood, reviewable, and maintainable**.  
Models provide shared language between engineers, architects, stakeholders, and operators.  
Without documentation, architectural intent is lost, and teams resort to tribal knowledge.  
The key is to **document just enough**: capture decisions, structures, and flows without drowning in detail.

### 1. C4 Model
I use the **C4 model** as the baseline for system visualization because it balances **abstraction vs. detail**.

- **C1: Context Diagram**
    - Shows the system and its external actors (users, other systems).
    - Answers: *Who uses the system? What external dependencies exist?*
    - Useful for stakeholders and non-technical audiences.

- **C2: Container Diagram**
    - Shows high-level building blocks (apps, services, databases, message brokers).
    - Defines boundaries and protocols between components.
    - Answers: *How is the system structured? Where do responsibilities lie?*

- **C3: Component Diagram**
    - Zoom into a single container and show major components/classes/modules.
    - Answers: *What responsibilities exist inside each service? How are they organized?*

- **C4: Code Diagram** *(only for critical areas)*
    - Lowest level: classes, functions, modules.
    - Generated via tooling (PlantUML, Structurizr, etc.).
    - Useful for onboarding or deep dives, not for every component.

👉 Not every system needs all 4 levels; typically C1–C2 for all systems, C3 for critical services, C4 only where complexity demands it.

### 2. Complementary views
Beyond C4, I include additional diagrams depending on context:

- **Sequence diagrams** – for critical workflows (e.g., login flow, checkout process).
- **Deployment diagrams** – mapping components to infrastructure (nodes, clusters, regions).
- **Data flow diagrams (DFD)** – to show lineage, transformations, and compliance-relevant paths.
- **State diagrams** – for systems with complex lifecycles (e.g., order processing, driver availability).
- **Error flow diagrams** – to visualize failure handling, retries, timeouts, and fallbacks.

👉 These views prevent blind spots: they clarify **runtime interactions** and **failure handling**, not just static structure.

### 3. System Design Document (SDD)
I use a **System Design Document (SDD)** as a structured artifact that consolidates architecture into a single reference.  
It is lightweight but comprehensive — an **engineering-first document**, not an academic paper.

**Outline I follow:**
1. **Overview & Goals** – what we are building and why.
2. **Scope & Assumptions** – boundaries, out-of-scope items.
3. **Functional Overview** – key use cases and CUJs.
4. **Non-Functional Requirements (NFRs)** – SLOs, security, compliance, cost.
5. **Architecture**
    - C4 diagrams (C1–C3, optionally C4).
    - Deployment diagram.
    - Key integration flows.
6. **Data Model & Contracts**
    - Entities, relationships, schemas, API contracts.
    - Versioning and migration strategy.
7. **Reliability & Resilience**
    - Failure modes, fallback strategies, chaos considerations.
8. **Security & Privacy**
    - Threat model, authentication/authorization, secrets, compliance.
9. **Observability**
    - SLIs/SLOs, dashboards, alerting, logging strategy.
10. **Performance & Capacity**
    - Load estimates, scaling strategy, capacity planning.
11. **Delivery & Operations**
    - CI/CD, release strategy, rollback.
12. **Risks & Decisions**
    - ADR references, open risks, mitigation.
13. **Cost/FinOps** – efficiency, budget ceilings, autoscaling strategy.
14. **Appendices** – glossary, references, links.

👉 The SDD ensures architecture reviews are **structured** and that systems are **production-ready** before launch.

### 4. Documentation principles
- **Living documentation**: update docs as code evolves; diagrams as code (PlantUML, Mermaid, Structurizr).
- **Version-controlled**: docs live in the repo alongside code (not static Confluence pages).
- **Linked ADRs**: decisions are cross-referenced so context is never lost.
- **Right audience, right depth**: executives get context (C1), engineers get detail (C2–C3), operators get deployment/runbooks.
- **Clarity over beauty**: diagrams should be readable in plain text (Mermaid, PlantUML), not just polished slideware.

### 5. Tooling
- **Diagrams as Code**: Mermaid, PlantUML, Structurizr DSL → versionable and automatable.
- **Data lineage**: tools like OpenLineage or custom flow maps.
- **CI integration**: diagrams auto-generated during pipeline runs, kept in sync with repo.

---

## Data strategy (consistency, durability, backups)

Data strategy aligns **business correctness** with **system behavior under failure**.  
This section defines how I choose consistency models, guarantee durability, design lifecycle policies, and **prove** recoverability (not just assume it).

### 1) Consistency: choosing the right model per workflow

Consistency must be **scoped to the use case**, not the whole system.

**Common models & when I use them**
- **Strong**: financial transfers, auth/session issuance, inventory decrement on checkout.
- **Read-your-writes (RYW)**: settings/profile updates reflected immediately for the same user.
- **Causal**: collaborative features, timelines where “happens-before” matters.
- **Bounded staleness**: read replicas with max lag (e.g., ≤ 5s) for dashboards.
- **Eventual**: recommendations, counters/analytics, caches, search indexes.

**Heuristics**
- Default to **strong** where violating invariants is expensive (money, identity, legal).
- Prefer **eventual** for **derived** or **non-critical** views when latency/availability wins.
- Make staleness **explicit** (SLO: “replica lag ≤ 5s p95”).
- **Document per endpoint**: required guarantees, acceptable staleness, fallback.

**Patterns**
- **CQRS**: separate write model (strong) from read model (fast/eventual).
- **Transactional Outbox**: atomically persist events with the write, then publish asynchronously to avoid dual-write loss.
- **SAGA** (orchestration/choreography): maintain cross-service invariants with compensations, not distributed transactions.
- **Idempotency keys**: ensure retried writes don’t duplicate effects.
- **Monotonic reads**: session affinity or read-after-write tokens for UX paths.

### 2) Data modeling, contracts, and versioning

- **Schema governance**
    - Semantic versioning for APIs/schemas; **backward compatible by default**.
    - Use **nullable + defaults** for additive changes; avoid breaking removals (expand → migrate → contract).
    - Contract tests (consumer-driven) to prevent regressions.

- **Indexing & access patterns**
    - Design indexes from **queries**, not from fields you “might need.”
    - Periodically review **slow queries**; cap index count to avoid write amplification.
    - For full-text/search facets, offload to a **search engine** (avoid abusing the OLTP DB).

- **Data locality & partitioning**
    - **Hot vs. cold** data split (tiered storage).
    - **Sharding keys** chosen from dominant access paths; avoid **hot shards**.
    - Co-locate compute with data; minimize cross-region chatty patterns.

### 3) Durability & replication

Durability means “once acknowledged, not lost,” across **hardware**, **software**, and **ops** failures.

- **Replication strategy**
    - **Intra-AZ**: baseline (not sufficient for HA alone).
    - **Multi-AZ synchronous**: protects from AZ loss; typically higher write latency.
    - **Multi-region**:
        - **Sync** (rare, latency heavy): only if RPO=0 and low latency regions.
        - **Async**: common; yields near-zero RPO locally, non-zero cross-region RPO.

- **Write acknowledgment policy**
    - Require **majority quorum** for commit on consensus systems (e.g., Raft).
    - For leader/follower DBs, configure minimum **sync replicas** for commit where supported.

- **Storage durability**
    - Journaling/WAL enabled; periodic **fsync** validation.
    - Checksums/end-to-end verification for silent data corruption.

### 4) Backups, restores, and proving recoverability

Backups are useless unless **restores are tested** and **timed**.

**Backup types**
- **Full**: baseline snapshot.
- **Incremental**: changes since last backup (daily).
- **Point-in-time recovery (PITR)**: WAL/binlog for fine-grained restore.

**Policy (typical starting point)**
- **Prod OLTP**: daily full + 15-min incremental/PITR; retain 35 days.
- **Analytics/warehouse**: weekly full + daily incremental; retain 90 days (legal varies).
- **Object/blob**: lifecycle to colder tiers after 30–90 days.

**Geo-redundancy**
- Keep at least **one copy off-region** (and off-account/subscription) to mitigate region & credential compromise.

**Restore drills (quarterly minimum)**
- Restore **to an isolated env**; measure **RTO** and check **RPO**.
- Validate **data integrity** (checksums, row counts, critical invariants).
- Capture a **runbook artifact**: timings, issues, improvements.

**Verification checklist**
- Can we restore **to a timestamp**?
- Can we restore **subset datasets** (e.g., single tenant)?
- Are **encryption keys** for old backups still accessible & rotated safely?
- Are we able to **rebuild derived stores** (search/caches) from the source of truth?

### 5) Disaster recovery (RTO/RPO) & failover

- Define **per capability**:
    - **RTO** (how fast to recover) and **RPO** (how much data loss).
- Align infra:
    - **Hot-hot** (active/active): lowest RTO, complex conflict resolution.
    - **Hot-warm** (active/passive): seconds–minutes RTO, async replication, data lag = RPO.
    - **Warm-cold** / **backup-restore**: hours RTO, cheapest.

**Failover playbooks**
- Promote replicas, update routing/DNS, invalidate caches, rehydrate queues.
- Pre-bake **readiness probes** for the promoted region (schema, secrets, feature flags).
- Reconciliation plan for **in-flight writes** during region split-brain (idempotency + dedupe).

### 6) Data lifecycle, retention, and privacy

- **Classification**: PII, PHI, PCI, confidential, public; drive encryption & access controls.
- **Retention**:
    - Define legal/business retention windows per entity.
    - **TTL policies** on hot stores; archive to cheap storage with audit trail.
- **Deletion**:
    - Implement **verifiable deletion** (tombstones + compaction or hard-delete pipelines).
    - Tenant-scoped deletion jobs; DSAR support (export/delete).
- **Minimization**:
    - Store **only what you use**; tokenize/pseudonymize where possible.
- **Encryption**:
    - **At rest** (KMS-managed keys, per-tenant keys if multi-tenant).
    - **In transit** (TLS 1.2+); mutual TLS for service-to-service where feasible.
- **Access control**:
    - Principle of least privilege (row/column/field-level where needed).
    - Tamper-evident **audit logs** for data access & admin actions.

### 7) Derived data, caches, and search

- **Source of truth**: designate one system (OLTP). Everything else is **derived**.
- **Change Data Capture (CDC)**: stream from truth to **read models** (search, analytics) via outbox or logs.
- **Cache policies**:
    - **Cache-aside** + **TTL** + negative caching; stampede protection (locks/jitter).
    - Purge on **write** for strict paths; eventual for derived views.
- **Search & analytics**:
    - Accept **eventual consistency**; rebuildability from CDC/OLTP snapshots is mandatory.

### 8) Migrations & evolution

- **Expand → Migrate → Contract** for schema changes.
- Dual-write/dual-read phases guarded by **feature flags**.
- Backfill jobs designed to be **idempotent** and **resumable** (chunking + checkpoints).
- Maintain **compat windows** for consumers (N-1/N-2 protocol support).

### 9) Data quality & reconciliation

- **Invariants**: define and assert (e.g., sum of ledger entries = account balance).
- **Reconciliation jobs**:
    - Periodic checks between source and derived stores (hash totals, counts).
    - Alert on drift; auto-heal where safe (replay events, rebuild materialized views).
- **Observability**
    - **SLIs**: replication lag, failed CDC events, cache hit ratio, rebuild duration.
    - **Error budgets** for data freshness on derived views.

### 10) Quick decision matrix (starter)

| Dimension   | Option              | Use when…                    | Trade-off                    |
|-------------|---------------------|------------------------------|------------------------------|
| Consistency | Strong              | Money/auth/invariants matter | Higher latency, lower avail. |
|             | Eventual            | Derived views, search, recs  | Stale reads possible         |
| Replication | Multi-AZ sync       | AZ fault tolerance           | Write latency ↑              |
|             | Multi-region async  | DR with low cost             | Non-zero RPO                 |
| DR posture  | Active/Active       | Near-zero RTO needed         | Complexity (conflicts)       |
| Backups     | Full + PITR         | Regulated, low RPO targets   | Storage/ops cost ↑           |
| Lifecycle   | Hot→Warm→Cold tiers | Large volumes, cost control  | Retrieval latency ↑          |

---

## Architecture & Integration Patterns

This section catalogs **structural**, **communication**, and **reliability** patterns I consider when composing systems.  
Each pattern includes **when to use**, **benefits**, **trade-offs**, and **pitfalls**. The goal is to choose the **simplest viable** option that satisfies NFRs and evolves safely.

### 1) Structural patterns (system shape)

#### Modular Monolith
- **Use when**: one team, cohesive domain, fast iteration required, uncertain boundaries.
- **Benefits**: simple deployment, transactional consistency, easy refactoring.
- **Trade-offs**: risk of hidden coupling, eventual scaling limits.
- **Pitfalls**: “distributed monolith” if modules leak across boundaries.

#### Microservices
- **Use when**: clear bounded contexts, independent scaling, team autonomy matters.
- **Benefits**: independent deployability, fault isolation, polyglot freedom.
- **Trade-offs**: operational complexity (networking, observability), data consistency.
- **Pitfalls**: too many tiny services, chatty calls, premature decomposition.

#### Self-Contained Systems (SCS)
- **Use when**: product-aligned verticals (UI+API+DB per slice) make sense.
- **Benefits**: independence, local optimization per slice, fewer cross-team collisions.
- **Trade-offs**: duplicated functionality, cross-slice consistency challenges.

#### Hexagonal / Clean Architecture
- **Use when**: need long-term maintainability, testability, adapter swapping.
- **Benefits**: domain isolated from delivery/infrastructure; easy stubbing.
- **Trade-offs**: initial boilerplate & learning curve.

### 2) Deployment & release patterns

#### Blue/Green
- **Use when**: need safe, reversible rollouts.
- **Pros**: instant rollback, simple mental model.
- **Cons**: double capacity; DB migrations must be backward-compatible.

#### Canary / Progressive Delivery
- **Use when**: reduce blast radius with real traffic.
- **Pros**: metrics-driven gates; align with SLOs.
- **Cons**: requires solid observability & automation.

#### Feature Flags
- **Use when**: decouple deploy from release; test in prod.
- **Pros**: instant kill switch; enable A/B.
- **Cons**: config debt; stale flags must be pruned.

### 3) Synchronous integration (request/response)

#### REST (JSON/HTTP)
- **Use when**: broad compatibility, human-friendly, public APIs.
- **Pros**: ubiquitous tooling, caching via HTTP.
- **Cons**: weak typing; versioning discipline needed.
- **Pitfalls**: N+1 endpoints; overfetch/underfetch.

#### gRPC (HTTP/2, protobuf)
- **Use when**: low-latency, high-throughput, typed internal RPC.
- **Pros**: IDL, streaming, codegen, contract-first.
- **Cons**: browser-unfriendly without proxies; versioning policies required.

#### GraphQL
- **Use when**: client-driven data needs vary; multiple frontends.
- **Pros**: single endpoint, avoids over/underfetch, schema as contract.
- **Cons**: server complexity, query cost control needed; caching harder.

**Heuristics**
- External/public → REST.
- Internal performance-critical → gRPC.
- Many UI clients varying needs → GraphQL behind a BFF.

### 4) Asynchronous integration (event/message)

#### Message Broker (AMQP, MQTT)
- **Use when**: decouple producers/consumers, work queues, back-pressure.
- **Pros**: reliability, retries, DLQs.
- **Cons**: at-least-once requires idempotency; ordering not guaranteed globally.

#### Log/Event Streams (Kafka/Pulsar)
- **Use when**: high-throughput events, CDC, replay & stateful consumers.
- **Pros**: durable history, scale-out consumption, stream processing.
- **Cons**: schema evolution & consumer lag management; exactly-once is nuanced.

#### Event Sourcing
- **Use when**: auditability, temporal queries, complex domain invariants.
- **Pros**: full history, rebuild projections, powerful debugging.
- **Cons**: projection complexity, upcasting/versioning of events.

#### Pub/Sub Fan-out
- **Use when**: multiple consumers with independent pace.
- **Pros**: decouples teams; easy to add consumers.
- **Cons**: hard-to-track side effects; needs governance.

### 5) Workflow coordination

#### Orchestration (central saga)
- **Use when**: multi-step processes need visibility, compensations, SLAs.
- **Pros**: explicit control flow, monitoring, retries in one place.
- **Cons**: orchestrator as critical dependency; vendor/tool coupling.

#### Choreography (event-driven saga)
- **Use when**: services are autonomous; simple compensations.
- **Pros**: no central brain; scalable autonomy.
- **Cons**: flow becomes implicit; debugging & evolution can be hard.

**Rule of Thumb**: start choreography; move to orchestration when visibility/compliance/SLA pressure requires it.

### 6) Reliability & resilience patterns

#### Timeouts & Retries (with jitter)
- **Use when**: any remote call.
- **Pitfalls**: retry storms; respect idempotency and budgets.

#### Circuit Breaker
- **Use when**: downstream flaps or fails slowly.
- **Pros**: fail fast; avoid resource exhaustion.
- **Cons**: tune thresholds; ensure fallback UX.

#### Bulkheads
- **Use when**: isolate pools per dependency/tenant.
- **Pros**: blast radius reduction.
- **Cons**: capacity planning per pool.

#### Hedging / Duplicate Request
- **Use when**: long tail latency dominates.
- **Pros**: better tail latency.
- **Cons**: extra load; only for idempotent ops.

#### Load Shedding & Queueing
- **Use when**: protect core while overloaded.
- **Pros**: graceful degradation.
- **Cons**: backlogs require SLAs & DLQs.

### 7) Caching patterns

#### Cache-aside
- **Use when**: general-purpose read caching.
- **Pros**: simple; on-demand population.
- **Cons**: stale reads; stampede on miss → use locks/jitter.

#### Write-through / Write-behind
- **Use when**: need write performance (behind) or guaranteed cache correctness (through).
- **Trade-offs**: behind risks data loss on cache failure; through adds latency.

#### CDN / Edge Caching
- **Use when**: static assets, APIs suited for cache keys/TTL.
- **Pros**: latency & offload wins.
- **Cons**: invalidation needs discipline.

### 8) Data integration patterns

#### Transactional Outbox + Relay
- **Use when**: avoid dual-write between DB and broker.
- **Pros**: atomicity, reliability.
- **Cons**: outbox table growth → archiving; exactly-once semantics still need idempotency.

#### Change Data Capture (CDC)
- **Use when**: build read models/search/analytics from OLTP.
- **Pros**: minimal app changes; replayable.
- **Cons**: schema evolution & reorder; consumer backfill complexity.

#### Materialized views / projections
- **Use when**: read-optimized slices; CQRS read side.
- **Pros**: latency and composition wins.
- **Cons**: eventual consistency; rebuild pipelines required.

### 9) API Composition & aggregation

#### Backend for Frontend (BFF)
- **Use when**: different clients need tailored APIs.
- **Pros**: reduces client complexity; caching per client.
- **Cons**: proliferation of services; keep boundaries clear.

#### API Gateway / Ingress
- **Use when**: cross-cutting concerns (auth, rate limits, WAF).
- **Pros**: central policy; observability.
- **Cons**: avoid business logic creep in gateway.

#### GraphQL Federation / Schema Stitching
- **Use when**: unify multiple domains behind one graph.
- **Pros**: single graph for clients; team autonomy.
- **Cons**: ownership boundaries & performance need governance.

### 10) AuthN/Z & tenancy patterns

#### OAuth2/OIDC, mTLS (service-to-service)
- Prefer short-lived tokens; audience-bound scopes; workload identity where offered.

#### Authorization
- **RBAC** for roles, **ABAC** for attributes, **ReBAC** for relationships (complex sharing).
- Central policy engines (e.g., OPA) with **decision logging**.

#### Multi-tenancy
- **Silo (per-tenant DB)**: strongest isolation; higher cost/ops.
- **Pooled (shared schema)**: efficient; needs row-level isolation and noisy-neighbor controls.
- **Bridge**: pooled with per-tenant keys & quotas.

### 11) Concurrency, ordering, idempotency

- **Idempotency keys** for POST/PUT-like operations.
- **At-least-once** delivery → design consumers idempotent.
- **Partition keys** to ensure **per-entity ordering** where required.
- **Exactly-once** often means “at-least-once + idempotent & deterministic sinks”.

### 12) Observability & policy integration

- **Correlation IDs** propagated across sync/async.
- **RED/USE** metrics standardization.
- **Policy as Code**: rate limits, authZ, and schema checks enforced at the edge or CI.

### 13) Anti-Patterns & smells

- **Distributed Monolith**: microservices without autonomy; shared DB between services.
- **Chatty Services**: deep call chains; prefer coarse APIs or async.
- **Global Transactions**: 2PC across services; prefer sagas.
- **Cache as Source of Truth**: leads to corruption; designate canonical store.
- **Hidden Coupling via Shared Libraries**: break owners with common “core” updates; prefer contracts.

### 14) Decision matrices (quick starters)

**Sync vs. Async**

| Dimension    | Synchronous RPC    | Asynchronous Messaging        |
|--------------|--------------------|-------------------------------|
| Latency      | Low (happy path)   | Higher (queueing)             |
| Coupling     | Tighter (temporal) | Looser (temporal decoupling)  |
| Availability | Caller impacted    | Buffers absorb outages        |
| Complexity   | Simpler to reason  | Requires retries/idempotency  |
| Use for      | UX-critical reads  | Workflows, fan-out, buffering |

**Orchestration vs. Choreography**

| Need                    | Pick          |
|-------------------------|---------------|
| Auditability/visibility | Orchestration |
| Simplicity & autonomy   | Choreography  |
| Complex compensations   | Orchestration |
| Evolvable, low coupling | Choreography  |

**BFF vs. Single API**

| Context                       | Choice |
|-------------------------------|--------|
| Multiple client types diverge | BFF    |
| Uniform clients/simple needs  | Single |

---

## Reliability & resilience

Reliability is the probability the system performs its intended function over time; resilience is the system’s ability to **degrade gracefully**, recover quickly, and **limit blast radius** when failures happen. This section defines the principles, patterns, and artifacts I require before calling a system production-ready.

### 1) SLOs, SLIs & error budgets

- **Define per capability** (not per microservice): e.g., “Checkout completes ≤ 2s p95 with ≥ 99.9% success per 30d.”
- **SLIs**: availability (successful / total requests), latency (p50/p95/p99), quality (correct results), freshness (staleness).
- **Error budget** = 1 − SLO (e.g., 0.1% per 30d). Budget funds change velocity (deploys, experiments).
- **Policies**:
    - If budget **exhausted** → freeze risky changes, focus on reliability work.
    - If budget **healthy** → allow faster changes, perform chaos drills.

**Starter SLOs**

| Capability              | Availability | Latency p95 | Window |
|-------------------------|--------------|-------------|--------|
| Auth (token issuance)   | 99.95%       | 300 ms      | 30 d   |
| Read API (hot paths)    | 99.9%        | 200 ms      | 30 d   |
| Write API (OLTP)        | 99.9%        | 400 ms      | 30 d   |
| Checkout (CUJ)          | 99.9%        | 2 s         | 30 d   |

### 2) Failure modes & blast radius

**Plan for** latency spikes, dependency timeouts, partial data loss, slow retries, brownouts, AZ/region loss.

- **Bulkheads**: isolate thread/conn pools per dependency/tenant.
- **Circuit breakers**: fail fast when downstream is unhealthy.
- **Timeout budgets**: bound end-to-end latency (no unbounded waits).
- **Back-pressure**: bounded queues + drop/shape lower-priority work.
- **Idempotency**: ensure retries won’t duplicate side effects.
- **Graceful degradation**: stale reads, reduced features, queue writes.

### 3) Golden guardrails (defaults I start with)

- **Timeouts**: client 300–700 ms for internal RPC; cascade with budget (e.g., 2–3 hops).
- **Retries**: max 2–3, **exponential backoff with jitter** (e.g., base 50 ms, cap 1 s).
- **Hedging** (tail-latency only): after p95 deadline, duplicate once; *idempotent* reads only.
- **Connection pools**: upper bounds + per-endpoint limits; no global shared pools.
- **Queues**: size bounded; **DLQ** for poison messages; visibility timeout tuned to work time.
- **Rate limits**: per user/token/tenant; global emergency limiters (leaky bucket/token bucket).

### 4) Resilience patterns

- **Retryable vs. non-retryable** errors: classify by code (e.g., 5xx/network → retry; 4xx/business → don’t).
- **Read isolation**: serve **stale cache** when source down; surface freshness to clients.
- **Write isolation**: queue writes when DB is saturated, drop optional writes first (telemetry, analytics).
- **Safe fallbacks**: precomputed snapshots, reduced ML models, static prices with guard rails.
- **Priority lanes**: protect CUJs with dedicated capacity (separate queues/pools).

### 5) Multi-AZ & Multi-Region strategy

- **Default**: multi-AZ for stateless + stateful tiers (synchronous replication where supported).
- **Multi-region**:
    - **Active/Active** for read-heavy or latency-critical reads; conflict-free data designs preferred.
    - **Active/Passive** for OLTP writes with async replication; measure RPO.
- **Routing**: health-checked DNS/edge, per-capability failover (not “all or nothing”).
- **Data**: see Data Strategy; DR runbooks must cover promotion, rekeying, reconciliation.

### 6) Overload & brownout playbook

When saturation rises (CPU, qps, queue depth, error rate):

1. **Shed load**: throttle non-CUJ traffic, return 429 with retry-after.
2. **Reduce work**: disable secondary features (recommendations, heavy joins), increase cache TTL.
3. **Protect storage**: switch to read-only mode for non-critical paths; buffer writes.
4. **Scale**: add replicas/compute (if not already maxed); raise autoscaling limits temporarily.
5. **Recover**: drain queues, gradually restore features; watch tail latency and error budgets.

### 7) Chaos & disaster readiness

- **Game days**: inject faults (latency, packet loss, dependency down) in lower envs weekly; prod light chaos monthly if culture/process mature.
- **Region evacuation drills**: quarterly; time how long CUJs remain within SLO.
- **Tabletop exercises**: validate comms, roles, decision trees; test **pager** and **runbooks**.
- **Success criteria**: meet **RTO/RPO** per capability; produce post-exercise actions.

### 8) Observability for reliability

- **SLI telemetry**: RED metrics (Rate, Errors, Duration) per endpoint; USE metrics (Utilization, Saturation, Errors) per resource.
- **Tracing**: propagate **correlation IDs** across sync/async; mark **retries** and **circuit events**.
- **Black-box**: synthetics for CUJs from user geos.
- **Alerting**: page on **SLO burn rate** and absence of heartbeat; ticket on anomalies/noise.

**Starter burn-rate alerts**
- 2% budget burn in 1h → page.
- 5% budget burn in 6h → page.
- 10% budget burn in 24h → ticket if auto-recovering.

### 9) Runbooks & incident workflow

Each alert links to a **runbook** with:

- **Triage**: check dashboards (service, dependency, infra), recent deploys, feature flags.
- **Decision tree**: rollback vs. disable flag vs. failover vs. rate-limit.
- **Commands**: exact scripts/queries; safe concurrency limits.
- **Verification**: SLI trend, error rates, queue depths, customer probes.
- **Comms**: status page templates, stakeholder channels.
- **Aftercare**: create postmortem, assign action items, update SLOs/limits.

### 10) Dependency reliability profiles

For each external/internal dependency keep a **profile**:

- **SLO** and **contract** (timeout, retry policy, circuit thresholds).
- **Failure semantics** (idempotency, exactly-once claims, at-least-once reality).
- **Capacity envelope** (qps, concurrency, rate limits).
- **Fallback behavior** (cached, stubbed, degraded UX).
- **Ownership & on-call** (contact map).

### 11) Readiness checklist (go-live)

- [ ] SLOs defined per CUJ; dashboards & alerts wired to **burn rate**.
- [ ] Timeouts, retries with jitter, circuits configured; budgets verified with tests.
- [ ] Bulkheads for pools; queues bounded with DLQ; back-pressure validated.
- [ ] Graceful degradation paths implemented & tested (stale cache, reduced features).
- [ ] Multi-AZ enabled; DR posture selected; **RTO/RPO** documented and tested.
- [ ] Runbooks linked from alerts; **game day** performed; rollbacks dry-run.
- [ ] Error budgets & reliability backlog agreed with product.
- [ ] Synthetic probes deployed from user geos; correlation IDs end-to-end.
- [ ] Rate limits and emergency kill switches tested in staging.

### 12) Quick Decision Matrices

**Circuit Breaker vs. No CB**

| Context                            | Choice        |
|------------------------------------|---------------|
| Flaky/slow dependency              | Use CB        |
| Hard real-time, tiny error budget  | CB + fallback |
| Short-lived, idempotent calls only | Retry only    |

**Hedging**

| Use when…                       | Avoid when…                        |
|---------------------------------|------------------------------------|
| Long tail dominates p99 latency | Non-idempotent writes              |
| Reads are cheap & idempotent    | Dependency is capacity-constrained |

**Active/Active vs. Active/Passive**

| Requirement              | Choose         |
|--------------------------|----------------|
| Near-zero RTO, global UX | Active/Active  |
| Simpler ops, lower cost  | Active/Passive |

---

## Security & Privacy

Security is about **protecting systems, data, and users** from unauthorized access, misuse, or compromise.  
Privacy is about **ensuring individuals’ data is collected, processed, stored, and deleted responsibly** in line with legal, ethical, and contractual obligations.  
Both must be **designed in from the start** — bolted-on security or ad-hoc privacy controls always fail under scale and attack.

### 1) Security & privacy principles

- **Zero Trust by default**: verify every request, minimize implicit trust.
- **Least privilege**: every identity (human or machine) gets only what it needs.
- **Defense in depth**: multiple layers of controls (authn, authz, network, app).
- **Secure by default**: encryption on, logging on, auth required.
- **Fail secure**: when components fail, they deny access, not grant it.
- **Data minimization**: collect and retain only what is strictly necessary.
- **Privacy by design**: every feature must consider its privacy impact.

### 2) Identity & Access Management (IAM)

- **Authentication**
    - End-users: OAuth2/OIDC, MFA (optional per risk), passwordless where possible.
    - Services: mTLS, workload identity (Kubernetes/GCP/Azure), short-lived tokens.
    - Rotate secrets frequently; prefer **federated identity** over static credentials.

- **Authorization**
    - Prefer **RBAC** for coarse roles; **ABAC/ReBAC** for fine-grained scenarios.
    - Centralize policy enforcement (OPA, Cedar, Casbin).
    - Log every privileged/admin action.

- **Session management**
    - Short-lived tokens (JWT, opaque with introspection).
    - Refresh flows with strict rotation and revocation.
    - Single sign-out for regulated environments.

### 3) Data protection

- **Encryption in transit**
    - TLS 1.2+ with modern ciphers; HSTS and perfect forward secrecy.
    - mTLS for service-to-service traffic.
    - Enforce ALPN for HTTP/2 or gRPC.

- **Encryption at rest**
    - Cloud KMS/HSM for key management.
    - Key rotation policies; envelope encryption for sensitive fields.
    - Multi-tenant systems: per-tenant encryption keys.

- **Secrets management**
    - No secrets in code or env vars committed to git.
    - Use vaults (HashiCorp Vault, AWS Secrets Manager, Azure Key Vault).
    - Rotate automatically; short-lived credentials where possible.

- **Data classification**
    - PII, PHI, PCI, confidential, public.
    - Apply access controls, audit logging, and retention rules accordingly.

### 4) Application security

- **Input validation**: whitelist, length limits, safe parsing.
- **Output encoding**: HTML/JS escaping, SQL parameterization, avoid injection.
- **State management**: CSRF protection, SameSite cookies, replay protection.
- **Deserialization**: forbid untrusted data binding to objects directly.
- **Dependency hygiene**
    - SCA (software composition analysis) in CI/CD.
    - SBOM generated, signed artifacts (SLSA provenance).
    - Pin versions, patch promptly.

- **Runtime protection**
    - WAF for injection/DoS protection.
    - RASP (Runtime Application Self Protection) optional in high-risk apps.
    - Rate limiting & bot mitigation.

### 5) Network & Infrastructure Security

- **Segmentation**: split VPCs/VNets by environment (dev/test/prod).
- **Ingress/Egress controls**: only required ports/protocols allowed.
- **Firewalls & security groups**: default deny, explicit allow.
- **Private networking**: service-to-service on private links, not public IPs.
- **Patching**: base images & OS patched continuously; golden image pipeline.
- **Immutable infra**: prefer rebuild over patch-in-place.

### 6) Supply chain & CI/CD security

- **Build pipeline**
    - SAST, DAST, SCA, secret scanning built into CI.
    - Signed commits & artifacts.
    - SBOM generated automatically.

- **Deployment**
    - Infrastructure as Code scanned (Terraform, Ansible).
    - Policy as Code (OPA, Conftest) gates.
    - Verify provenance of container images; allow only signed/trusted registries.

- **Runtime**
    - Admission controllers (Kubernetes) to enforce image signing, pod policies.
    - Monitoring for drift between declared vs. running infra.

### 7) Privacy engineering

- **Data minimization**
    - Only collect fields required for business use.
    - Challenge feature requests that expand PII unnecessarily.

- **Anonymization & pseudonymization**
    - Tokenize identifiers for analytics.
    - Apply k-anonymity or differential privacy for sensitive datasets.

- **Data retention**
    - Enforce TTL policies for logs, backups, raw data.
    - Legal holds documented; retention maps per data class.

- **Right to be forgotten (DSARs)**
    - APIs & jobs to export or delete a user’s data across all systems.
    - Proof-of-deletion audit.

- **Transparency**
    - Privacy notices reflect actual system behavior.
    - Access logs exposed to users (who accessed their data, when, why).

### 8) Threat modeling & risk assessment

- Apply **STRIDE** (Spoofing, Tampering, Repudiation, Info Disclosure, DoS, Elevation).
- For critical systems, run **PASTA** (process-focused risk analysis).
- Capture mitigations in SDD; update ADRs when new threats appear.
- Maintain risk register with likelihood × impact ratings.

### 9) Security logging & monitoring

- **Log events**
    - Auth success/failure, admin actions, policy changes.
    - Data access attempts (esp. sensitive data).
    - Configuration changes & deployments.

- **Log hygiene**
    - Structured (JSON), correlation IDs, no PII leakage.
    - Logs immutable, tamper-evident storage (e.g., WORM, append-only).

- **Alerting**
    - Failed logins, brute-force, anomalous access.
    - Privilege escalations, disabled security controls.
    - High-volume data exfiltration attempts.

### 10) Compliance & governance

- **Frameworks**
    - Map to ISO 27001, SOC 2, PCI DSS, HIPAA as applicable.
    - Maintain evidence via CI/CD + observability artifacts.

- **Audit readiness**
    - Versioned IaC, signed artifacts, immutability proofs.
    - Role-based access logs, periodic reviews.

- **Policies**
    - Data classification policy.
    - Access review cadence (quarterly minimum).
    - Incident response policy (with tabletop drills).

### 11) Privacy & security by default checklist

- [ ] All endpoints require authentication (no anonymous unless explicit).
- [ ] All data encrypted at rest and in transit.
- [ ] Default deny network rules.
- [ ] Secrets never in code; vault integration verified.
- [ ] PII/PHI classified; retention policy documented.
- [ ] Privacy impact assessed for new features.
- [ ] Logging includes auth/admin events; no sensitive data leakage.
- [ ] Dependencies scanned; SBOM generated and signed.
- [ ] DSAR workflow implemented (export/delete).
- [ ] Threat model updated for every major release.

---

## Observability & monitoring

Observability is the ability to **understand a system’s internal state from its external outputs**.  
Monitoring is the practice of collecting and acting upon signals that reflect **health, reliability, and performance**.  
Together, they ensure that systems are **measurable, debuggable, and operable at scale**.

### 1) Principles

- **If it’s not measured, it doesn’t exist.**
- **User-centric**: measure Critical User Journeys (CUJs), not just components.
- **Actionable, not noisy**: every alert should trigger a runbook.
- **Shift-left**: design observability in from the start, not after incidents.
- **Correlation-first**: logs, metrics, traces must link via IDs for coherent debugging.
- **Automation**: dashboards and alerts are code, versioned with the system.

### 2) The Three Pillars of Observability

#### Metrics
- **RED metrics (per service/API)**: Rate, Errors, Duration.
- **USE metrics (per resource)**: Utilization, Saturation, Errors.
- Aggregations: p50/p95/p99 latency, error rate, throughput, queue depth.
- Tagged by tenant, region, version, feature flag.
- **SLIs derived from metrics**; compared against SLOs.

#### Logs
- Structured JSON logs; no free-text.
- Correlation IDs passed across all requests (sync + async).
- Log levels: DEBUG (dev), INFO (state changes), WARN (unusual but non-breaking), ERROR (failed operation).
- Sensitive data (PII/PHI/PCI) must be redacted or tokenized.
- Retention aligned with compliance (e.g., 30 days hot, then archive).

#### Traces
- Distributed tracing across services with context propagation.
- Capture spans for retries, errors, circuit trips, DB queries.
- Trace sampling:
    - **Head-based** for cost-sensitive systems (random sample).
    - **Tail-based** for high-value flows (sample slow/error traces).
- Visualize bottlenecks in CUJs; tie latency budgets to spans.

### 3) Dashboards

- **Golden Signals** per service: latency, error rate, throughput, saturation.
- **CUJ dashboards**: E2E view (checkout, login, upload).
- **Infra dashboards**: CPU, memory, disk, network per node/pod.
- **Business dashboards**: KPIs (conversions, orders, churn).
- **Release dashboards**: feature-flagged vs. control performance.

### 4) Alerting

- **Principle**: Alert only when a human must act.
- **Page**: SLO burn rates, unbounded queue growth, dependency saturation.
- **Ticket**: non-urgent anomalies (gradual error creep, cost anomalies).
- **Silence**: transient failures auto-recovered, noisy low-value alerts.

**Burn-rate alerts (example)**
- 2% error budget burned in 1h → page.
- 5% burned in 6h → page.
- 10% burned in 24h → ticket.

### 5) Black-Box vs. White-Box monitoring

- **Black-box (synthetic probes)**: external perspective of CUJs (multi-region probes).
- **White-box**: internal telemetry from app code, infra, dependencies.
- Both are required: synthetics catch real UX impact, white-box helps debug cause.

### 6) Logging & metrics hygiene

- **Cardinality control**: avoid unbounded labels (user IDs, UUIDs) in metrics.
- **Sampling**: reduce noisy logs; aggregate where possible.
- **Retention**: align with compliance + cost budgets.
- **Redaction**: ensure secrets/PII never logged.

### 7) Tooling examples

- **Metrics**: Prometheus, Cloud Monitoring, Datadog.
- **Logs**: ELK/EFK, Loki, Cloud-native logging.
- **Traces**: OpenTelemetry + Jaeger/Tempo/Zipkin.
- **Dashboards**: Grafana, Kibana, Datadog.
- **Alerting**: PagerDuty, Opsgenie, VictorOps.

### 8) Observability readiness checklist

- [ ] RED + USE metrics defined and exported for every service.
- [ ] Correlation IDs propagated end-to-end across sync + async.
- [ ] Traces instrument CUJs; p95/p99 spans visible.
- [ ] Dashboards exist per service, per CUJ, per business KPI.
- [ ] Burn-rate alerts configured for each SLO.
- [ ] Synthetic probes run from multiple geos.
- [ ] Log retention & PII redaction policy documented.
- [ ] Alert runbooks linked to every page.
- [ ] Observability integrated into CI/CD (dashboards & alerts as code).

---

## Performance & Capacity

Performance ensures the system responds within acceptable latency and throughput bounds.  
Capacity ensures the system can sustain current and future loads without degradation or runaway cost.  
Both must be treated as **first-class requirements**, designed and validated continuously.

### 1) Performance principles

- **Budget latency**: decompose CUJ latency (e.g., checkout ≤ 2s) into service budgets.
- **Focus on tail latency**: optimize p95/p99, not just averages.
- **Minimize hops**: each network or service boundary adds latency & failure probability.
- **Batch & coalesce**: reduce chattiness; prefer fewer large calls.
- **Parallelize** independent calls; avoid serial chains.
- **Profile before optimizing**: identify hot paths; avoid premature tuning.
- **Measure user-centric metrics**: time-to-first-byte, FCP/LCP, API response times.

### 2) Latency management

- **Timeout budgets**
    - Each hop consumes ≤ 1/3 of caller’s budget.
    - End-to-end CUJ defined at p95 (not just p50).
- **Caching**
    - Memory/local caches for hot data; edge/CDN for static assets.
    - Stale-while-revalidate to hide backend latency.
- **Connection reuse**
    - Keep-alive, connection pools, HTTP/2 multiplexing.
- **Serialization**
    - Compact formats (protobuf, Avro); avoid verbose JSON in hot paths.
- **N+1 query prevention**
    - Preload, join, or batch DB/API queries.

### 3) Throughput & concurrency

- **Throughput SLOs**: e.g., API supports 2k RPS sustained with <300ms p95.
- **Concurrency limits**
    - Define max concurrent requests per service (protects resources).
    - Reject early with 429 if exceeded; client retries with backoff.
- **Queuing**
    - Buffer bursts; monitor queue depth & latency.
    - Bounded queues + DLQ for overflow.

### 4) Scalability & elasticity

- **Vertical scaling**: add resources until cost vs. benefit saturates.
- **Horizontal scaling**: stateless services + auto-scaling groups/clusters.
- **Elasticity**
    - Autoscale on **SLO indicators** (latency, queue depth) not just CPU.
    - Cooldowns & limits to avoid thrashing.
- **Data scaling**
    - Partition/shard for write-heavy workloads.
    - Read replicas for scale-out reads.
    - Materialized views for expensive queries.

### 5) Resource efficiency

- **CPU-bound**: profile hot loops, vectorize, concurrency primitives.
- **IO-bound**: async IO, batching, compression trade-offs.
- **Memory-bound**: pool reuse, limit unbounded caches, GC tuning.
- **Storage-bound**: compress logs, lifecycle policies, indexes tuned.

### 6) Capacity planning

- **Forecasting**
    - Estimate baseline + peak QPS, data volume growth.
    - Model DAU/MAU growth curves, seasonality (holidays, promotions).
- **Headroom**
    - Target ≤ 60–70% utilization at peak; allows for failover, spikes.
- **Stress margins**
    - Load test to 2–3× expected peak.
- **Cost awareness**
    - Model cost per RPS, per tenant; optimize infra & queries.

### 7) Load & performance testing

- **Types**
    - **Baseline**: typical daily load.
    - **Stress**: push until failure; find bottlenecks.
    - **Spike**: sudden traffic surges; test autoscaling.
    - **Soak**: long-duration test for leaks, degradation.
- **Tools**: k6, JMeter, Locust, Gatling.
- **Environments**
    - Use production-like env; mirror configs, secrets, data sizes.
- **Test data**
    - Mix realistic distributions (hot vs. cold keys).
    - Simulate bot/abuse traffic.

### 8) Observability for performance

- **Metrics**
    - Latency (p50/p95/p99), throughput, error rate.
    - Queue depth, resource saturation.
- **Traces**
    - Break down CUJ latency by service & hop.
    - Annotate retries, timeouts, circuit trips.
- **Dashboards**
    - Golden signals per service; CUJ latency charted by percentile.
- **Alerting**
    - Burn rate for latency/error budget breaches.
    - Sudden changes in throughput vs. baseline.

### 9) Overload & protection

- **Rate limiting**
    - Token/leaky bucket; quotas per tenant/user.
- **Load shedding**
    - Drop lowest-priority requests first (analytics, background).
- **Adaptive degradation**
    - Return partial results, cached/stale data, or simplified views.
- **Graceful fail**
    - Fail fast with clear error codes; avoid timeouts.

### 10) Decision matrices

**Cache vs. DB**

| Context                 | Cache (in-memory/distributed) | DB |
|-------------------------|-------------------------------|----|
| Hot reads, low latency  | ✅                             | ❌  |
| Strong consistency req. | ❌                             | ✅  |
| Tolerate staleness      | ✅                             | ❌  |

**Vertical vs. horizontal scaling**

| Requirement        | Vertical    | Horizontal |
|--------------------|-------------|------------|
| Fast, small uplift | ✅           | ❌          |
| Large scale growth | ❌ (ceiling) | ✅          |
| Ops complexity     | Low         | Higher     |

### 11) Readiness checklist

- [ ] Latency budgets per CUJ decomposed across services.
- [ ] Timeouts & concurrency limits configured.
- [ ] Hot paths profiled; N+1 queries eliminated.
- [ ] Caches designed with invalidation policy.
- [ ] Autoscaling tested under load.
- [ ] Load/stress/spike/soak tests executed; results documented.
- [ ] Headroom policy (≥30%) applied to capacity planning.
- [ ] Observability covers RED metrics + traces.
- [ ] Rate limiting & overload protection validated.
- [ ] Cost-per-request measured & optimized.

---

## Delivery & operations (CI/CD, IaC, releases)

Software delivery and operations are not an afterthought — they are part of the **architecture**.  
A system is only as good as its ability to be **built, tested, deployed, and operated reliably**.  
This section outlines principles, practices, and artifacts I apply to ensure **repeatable, safe, and efficient delivery**.

### 1) Principles

- **Everything as Code**: infra, pipelines, configs, policies → versioned, peer-reviewed.
- **Shift-Left**: quality, security, and compliance checks run *before* production.
- **Automate > Document > Manual**: automation reduces human error and drift.
- **Immutable & Declarative**: infra and releases are reproducible from code, not mutable snowflakes.
- **Progressive Delivery**: reduce blast radius with canaries, blue-green, feature flags.
- **Observability Built-In**: every release validated by metrics and logs.
- **Rollback First**: rollback strategy defined before go-live.

### 2) CI/CD pipelines

Pipelines are the **factory line** of the system.

**Typical pipeline stages**:
1. **Source**: trigger on commits, PRs, or tags.
2. **Build**: compile, package, containerize.
3. **Static checks**: linting, formatting, SAST, license scanning.
4. **Unit tests**: fast, isolated tests.
5. **Integration tests**: API, DB, queues, external deps.
6. **Security scans**: SCA (dependencies), IaC scanning, container scanning.
7. **Artifact publishing**: store in registry, signed and versioned.
8. **Deploy to staging**: with smoke tests.
9. **Performance tests**: baseline p95, throughput.
10. **Release candidate**: tagged and promoted.
11. **Deploy to prod**: progressive rollout (canary, blue-green).
12. **Post-release validation**: dashboards, error budget check.

**Key practices**:
- Pipelines are code (YAML/DSL).
- Fast feedback: <10 min for dev loop.
- Secure: signed artifacts, secret scanning, principle of least privilege for runners.
- Isolated: ephemeral test environments via IaC.

### 3) Infrastructure as Code (IaC)

Infra should be **version-controlled, reviewable, reproducible**.

- **Tools**: Terraform, Pulumi, Bicep, CloudFormation.
- **Patterns**:
    - Modular IaC (reusable components).
    - Environments = overlays (dev, staging, prod).
    - DRY with shared modules, but allow environment overrides.
- **Practices**:
    - Pre-merge validation: `terraform plan` in PRs.
    - State stored remotely, locked (S3+DynamoDB, GCS, Terraform Cloud).
    - Automated policy checks (OPA, Sentinel).
    - Drift detection alerts.

**Checklist**:
- [ ] IaC in Git.
- [ ] Remote state with locking.
- [ ] Policy checks in CI.
- [ ] Ephemeral environments on PR.
- [ ] Enforced tagging & ownership for resources.

### 4) Release strategies

Different release patterns minimize risk in different contexts:

- **Blue-Green**: two environments, flip traffic when green is ready.
- **Canary**: release to % of users/instances, validate before full rollout.
- **Feature flags**: decouple deploy from release, toggle at runtime.
- **Rolling deployments**: gradually replace pods/VMs, maintain availability.
- **Shadow traffic**: send real traffic to new version, but don’t return results (for testing).
- **Dark Launches**: release hidden features to prod, expose later.

**When to use**:
- High-risk → Blue-Green or Canary.
- High-frequency → Feature flags + rolling.
- Experimental → Dark launch + shadow testing.

### 5) Operations & runbooks

Ops is where design meets reality. Every service must ship with:

- **SLIs/SLOs** defined and monitored.
- **Runbooks** for all critical alerts (step-by-step resolution).
- **Escalation paths** documented (pager, fallback team).
- **On-call rotation**: ownership clear.
- **Game days**: simulate outages, rehearse failover.
- **Postmortems**: blameless, recorded, action items tracked.

**Runbook template**:
1. Alert description (with example).
2. Impacted services/users.
3. Immediate actions (containment).
4. Diagnosis steps (logs, dashboards, traces).
5. Remediation (restart, scale, rollback, fix).
6. Escalation contacts.
7. References (Jira, ADRs, design docs).

### 6) Tooling examples

- **CI/CD**: GitHub Actions, GitLab CI, Azure DevOps, Jenkins.
- **IaC**: Terraform, Pulumi, Bicep.
- **Secrets**: Vault, SOPS, cloud KMS.
- **Releases**: ArgoCD, Flux, Spinnaker, LaunchDarkly.
- **Ops**: PagerDuty, OpsGenie, Squadcast.

### 7) Delivery & operations checklist

- [ ] CI/CD pipelines codified, reviewed, tested.
- [ ] Security scans integrated (SAST, SCA, IaC, containers).
- [ ] Artifacts versioned, signed, immutable.
- [ ] IaC modularized, state remote & locked.
- [ ] Release strategy defined (rollback documented).
- [ ] Runbooks exist for all alerts.
- [ ] On-call and escalation paths documented.
- [ ] Postmortems tracked with actions.
- [ ] Game days scheduled at least quarterly.

---

## Compliance, risk & governance

System design is not only about technology — it must also align with **regulatory requirements, organizational policies, and risk management frameworks**.  
Governance ensures accountability, compliance ensures legality and trust, and risk management ensures resilience against uncertainty.

### 1) Principles

- **Security & Compliance by Design**: embed standards early, not bolted-on at audits.
- **Risk Awareness**: every architectural choice carries trade-offs in cost, performance, security.
- **Accountability**: ownership and responsibilities clearly documented.
- **Traceability**: decisions, risks, and compliance evidence are auditable.
- **Least Surprise**: align with existing organizational policies, not reinvent governance.
- **Continuous Compliance**: use automation and monitoring, not one-time audits.

### 2) Compliance

Compliance covers **legal, regulatory, and organizational obligations**. These vary depending on system context:

**Common standards**:
- **ISO 27001** – Information Security Management.
- **SOC 2** – Trust Services Criteria (security, availability, confidentiality, processing integrity, privacy).
- **GDPR / CCPA** – Privacy & data protection.
- **PCI DSS** – Payment data security.
- **HIPAA** – Healthcare data compliance.
- **NIST 800-53 / CSF** – U.S. security frameworks.
- **Local regulations** – e.g., data residency, government standards.

**Key practices**:
- Map **requirements → controls → evidence**.
- Maintain **data inventory** (where personal/sensitive data flows).
- Define **retention and deletion** policies.
- Document **roles & responsibilities** (e.g., DPO, security officer).
- Automate evidence collection (logs, configs, reports).

### 3) Risk management

Risk management ensures systems are **prepared for the unexpected**.

**Types of risks**:
- **Strategic**: wrong technology or architectural bets.
- **Operational**: outages, misconfigurations, insider error.
- **Security**: breaches, data leaks, ransomware.
- **Compliance**: regulatory fines, failed audits.
- **Financial**: uncontrolled cloud spend, license costs.
- **Reputational**: loss of customer trust.

**Risk process** (ISO 31000 inspired):
1. **Identify** risks (brainstorm, threat modeling, lessons learned).
2. **Assess** likelihood & impact (high/medium/low, or quantitative scoring).
3. **Mitigate** with controls (technical, process, contractual).
4. **Accept** risks explicitly if cost of mitigation > risk.
5. **Monitor** continuously (dashboards, risk register).
6. **Review** quarterly (update as system evolves).

**Controls examples**:
- Preventive: IAM policies, encryption, automated testing.
- Detective: monitoring, alerting, anomaly detection.
- Corrective: incident response, backups, DR.

### 4) Governance

Governance ensures the **system evolves responsibly** and decisions are transparent.

**Key elements**:
- **Architecture Decision Records (ADR)** – track why decisions were made.
- **Review boards** – periodic architecture/security reviews.
- **Standards & patterns** – documented best practices (e.g., naming, logging, error handling).
- **Change management** – RFC process for high-risk changes.
- **Ownership** – clear service owners, escalation paths.
- **Budget governance** – cost tracking, cloud spend monitoring.

**Governance artifacts**:
- Architecture playbook (this doc).
- Security & compliance policy set.
- Service catalog with ownership.
- Runbooks & escalation matrix.
- Audit trail (logs, tickets, approvals).

### 5) Continuous compliance & automation

Manual compliance → painful, slow, error-prone. Modern approach: **automate evidence and enforcement**.

**Techniques**:
- **Policy as Code**: OPA, Sentinel, Kyverno.
- **Automated audits**: CI/CD checks for IaC misconfigurations, drift.
- **Continuous evidence collection**: export logs/configs to SIEM.
- **Automated reports**: compliance dashboards for auditors.
- **Third-party attestations**: SOC2, ISO audits by external firms.

### 6) Compliance & governance checklist

- [ ] Regulatory requirements identified (GDPR, PCI, HIPAA, etc.).
- [ ] Data inventory & classification documented.
- [ ] Risk register maintained and reviewed quarterly.
- [ ] Controls mapped: preventive, detective, corrective.
- [ ] ADRs used for architectural decisions.
- [ ] Architecture/security reviews scheduled.
- [ ] Policies & standards documented, versioned.
- [ ] Ownership & escalation paths clear.
- [ ] Budget & cost governance in place.
- [ ] Continuous compliance automation integrated in CI/CD.

---

## Cost & FinOps

Modern systems live in **elastic, pay-as-you-go environments** (cloud, SaaS, managed services).  
This flexibility can accelerate delivery but also lead to **uncontrolled spending, hidden costs, and inefficiency**.  
**FinOps (Financial Operations)** is the discipline that brings together engineering, finance, and product to ensure **cost is an explicit design dimension** — optimized, predictable, and aligned with business value.

### 1) Principles

- **Cost is a first-class architecture concern** (like performance or security).
- **Visibility before optimization**: you can’t control what you don’t measure.
- **Shared responsibility**: engineers, product managers, and finance collaborate.
- **Unit economics > raw spend**: focus on cost per customer, per transaction, per workload.
- **Design for elasticity**: scale down as well as up.
- **Automate governance**: alerts, budgets, tagging policies.

### 2) Cost drivers

Key areas where architecture directly impacts cost:

- **Compute**: VM size, autoscaling configuration, overprovisioning.
- **Storage**: data growth, tiering (hot vs. cold), replication strategies.
- **Networking**: egress costs, inter-region transfers, CDNs.
- **Databases**: query inefficiency, backup retention, licensing.
- **Third-party services**: API calls, SaaS subscriptions, per-seat licenses.
- **Idle resources**: forgotten VMs, orphaned disks, unused IPs.
- **Environments**: staging/test environments running 24/7.

### 3) FinOps lifecycle

Inspired by the **FinOps Foundation model**:

1. **Inform**: visibility into costs, allocation by teams/projects.
    - Cost dashboards, tags, budgets.
    - Chargeback/showback models.

2. **Optimize**: identify waste and inefficiencies.
    - Rightsizing (adjust VM sizes, DB instances).
    - Spot/preemptible instances.
    - Storage lifecycle policies (auto-archive cold data).
    - Auto-scaling and scheduled shutdowns.

3. **Operate**: continuous governance and cultural adoption.
    - FinOps reviews as part of sprint/PI planning.
    - Cost objectives linked to KPIs.
    - Continuous feedback loop between engineering and finance.

### 4) FinOps practices in architecture

- **Tagging strategy**: enforce consistent cost allocation (team, environment, project).
- **Forecasting**: model growth and test what-if scenarios.
- **Cost-aware design patterns**:
    - Event-driven (serverless) vs. always-on services.
    - Data partitioning and retention policies.
    - CDN caching to reduce egress.
- **Cloud-native discounts**: reserved instances, savings plans, committed use.
- **Unit economics**: calculate **cost per request**, **cost per tenant**, or **cost per GB stored**.
- **Budget alarms**: integration with Slack/Teams for real-time alerts.

### 5) FinOps metrics

- **Cloud spend by service/team**.
- **Cost per feature or per customer**.
- **% of idle/unallocated resources**.
- **Forecast accuracy** (planned vs. actual).
- **Unit cost trends** (e.g., cost per 1k API calls).
- **Cost of reliability**: how much HA/DR adds to baseline.

### 6) Roles & responsibilities

- **Engineering**: build cost-efficient architectures, enforce tagging, monitor budgets.
- **Finance**: forecast spend, manage contracts, validate savings.
- **Product**: tie cost metrics to feature/business value.
- **FinOps Team/Champion**: drives collaboration, educates org.

### 7) Tools & automation

- **Cloud-native**: AWS Cost Explorer, Azure Cost Management, GCP Billing Reports.
- **Open-source**: Kubecost (Kubernetes cost visibility).
- **Commercial**: CloudHealth, Apptio, Harness.
- **Policy as Code**: OPA/Cloud Custodian to enforce tagging and lifecycle rules.

### 8) FinOps checklist

- [ ] Cost visibility dashboards per service/team in place.
- [ ] Tagging strategy enforced automatically.
- [ ] Budgets & alerts configured per project/environment.
- [ ] Unit economics tracked (cost per user, transaction, workload).
- [ ] Idle resources cleaned up regularly.
- [ ] Rightsizing and autoscaling policies reviewed quarterly.
- [ ] Discounts/commitments (RIs, Savings Plans) optimized.
- [ ] FinOps reviews part of planning cycles.
- [ ] Forecast vs. actual spend tracked and improved.
- [ ] Compliance with internal budget governance policies.

## Recovery & continuity (RTO/RPO, DR, chaos)

Ensuring **business continuity** and minimizing downtime is as important as building new features.  
Recovery and continuity strategies guarantee that the system can **withstand failures, recover quickly, and preserve data integrity**.

### Key concepts

- **RTO (Recovery Time Objective):**  
  The maximum acceptable downtime after a failure before services must be restored.
    - Example: *“The system must recover within 30 minutes after a regional outage.”*

- **RPO (Recovery Point Objective):**  
  The maximum acceptable amount of data loss measured in time.
    - Example: *“At most 5 minutes of data can be lost in case of a database failure.”*

- **Disaster Recovery (DR):**  
  A set of strategies, tools, and processes to **restore systems after catastrophic failures** (e.g., data center loss, cloud region outage).

- **Chaos Engineering:**  
  The practice of **deliberately injecting failures** into systems to validate resilience and recovery processes in real-world scenarios.

### Recovery & continuity checklist

- **RTO/RPO Defined & aligned**
    - [ ] Explicit RTO and RPO defined for each critical service.
    - [ ] Business stakeholders validated the recovery objectives.
    - [ ] Trade-offs between cost and resilience documented.

- **Backups & snapshots**
    - [ ] Automated and regular backups configured (DB, object storage, configs).
    - [ ] Snapshots taken and verified for restoration.
    - [ ] Backup retention aligned with compliance requirements.
    - [ ] Backups encrypted and stored in separate location/cloud.

- **Disaster Recovery (DR)**
    - [ ] DR plan documented (roles, responsibilities, procedures).
    - [ ] Hot/Warm/Cold standby environments considered.
    - [ ] Secondary region/zone tested with failover drills.
    - [ ] Runbooks for manual recovery steps prepared.
    - [ ] DR rehearsals performed regularly (game days).

- **Failover & high availability**
    - [ ] Load balancers and DNS failover strategies in place.
    - [ ] Clustering or replication for critical services (DB, cache, queues).
    - [ ] Automated failover tested under load.
    - [ ] Dependencies (e.g., third-party APIs) have fallback or graceful degradation.

- **Chaos engineering**
    - [ ] Fault injection tools (Gremlin, Chaos Mesh, AWS Fault Injection Simulator) integrated.
    - [ ] Scenarios tested: node failure, network latency, DB crash, zone outage.
    - [ ] Observability validated during chaos experiments.
    - [ ] Lessons learned documented and fed back into design.

- **Continuity for people & processes**
    - [ ] Incident response teams defined and trained.
    - [ ] Escalation policies and contact lists maintained.
    - [ ] Communication plan for stakeholders and customers ready.
    - [ ] Post-mortems performed after every major incident.

### Best practices

- **Design for failure** — assume every dependency (DB, cache, network, external API) can fail.
- **Test your backups** — backups are useless unless verified through regular restore drills.
- **Automate DR where possible** — manual failovers are too slow and error-prone in real incidents.
- **Shift-left resilience** — validate RTO/RPO and recovery mechanisms early in design, not post-production.
- **Simulate real disasters** — practice failover during peak hours, not just during controlled low-load windows.

---

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
18. [My Architecture Playbook (Principles & Heuristics)](#my-architecture-playbook-principles--heuristics)
19. [Checklists](#checklists)
    * [Security Checklist](#security-checklist)
    * [Testing Checklist](#testing-checklist)
    * [Monitoring/Observability Checklist](#monitoringobservability-checklist)
20. [Templates](#templates)
    * [ADR Template](#adr-template)
    * [SDD Outline](#sdd-outline)
    * [SLO Template](#slo-template)
    * [Runbook Template](#runbook-template)
    * [Incident Postmortem Template](#incident-postmortem-template)
21. [Glossary](#glossary)
22. [Further Reading & Standards](#further-reading--standards)

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

---

### 8) Data integration patterns

#### Transactional Outbox + Relay
- **Use when**: avoid dual-write between DB and broker.
- **Pros**: atomicity, reliability.
- **Cons**: outbox table growth → archiving; exactly-once semantics still need idempotency.

#### Change Data Capture (CDC)
- **Use when**: build read models/search/analytics from OLTP.
- **Pros**: minimal app changes; replayable.
- **Cons**: schema evolution & reorder; consumer backfill complexity.

#### Materialized Views / Projections
- **Use when**: read-optimized slices; CQRS read side.
- **Pros**: latency and composition wins.
- **Cons**: eventual consistency; rebuild pipelines required.

---

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

---

### 10) AuthN/Z & Tenancy Patterns

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

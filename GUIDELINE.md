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

## **Discovery & requirements questionnaire**

**Business & Users:** goals, KPIs, CUJs, workloads (DAU/MAU, QPS), seasonality, SLAs, growth forecast.
**Data:** entities & volumes, write/read mix, hot paths, retention/archiving, compliance.
**Risk:** what must never happen? failure blast radius? acceptable degradation modes?
**Security/Privacy:** data classification, tenant isolation, auth flows, regulatory needs (e.g., GDPR).
**Ops:** release cadence, rollback needs, on-call maturity, monitoring preferences, incident history.
**Cost:** budget caps, multi-region mandates, cloud credits, cost sensitivity vs. performance.

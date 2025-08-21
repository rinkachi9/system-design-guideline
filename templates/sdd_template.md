# System Design Document (SDD) – <System/Service Name>

> Purpose: a concise, engineering-first document that captures **what we’re building, why, and how** — from requirements to operations.  
> Audience: engineers, SRE/DevOps, security, product, and stakeholders.

> - Ownership: <Team/Owner>
> - Version: <vX.Y>
> - Date: <YYYY-MM-DD> 
> - Status: Draft | Review | Approved

---

## 1. Overview & goals
**Problem statement:**  
<What business/user problem does this system solve?>

**Objectives & Success Criteria (measurable):**
- <Objective 1 with KPI/SLO>
- <Objective 2>

**Scope & Non-Goals:**
- In scope: <…>
- Out of scope: <…>

**Stakeholders & ownership:**
- Product: [name] 
- Engineering: [name]
- SRE: [name]
- Security: [name]

---

## 2. Context
**Business context:** <market/regulatory constraints, deadlines, dependencies>  
**System context (C4–C1):**

- External actors: <users, systems>
- Adjacent systems: <CRM, payments, auth>  

**Assumptions & Constraints:**

- Tech constraints: <language/cloud/DB>
- Compliance: <GDPR/PCI/SOC2>
- Timelines/budget: <…>

---

## 3. Requirements
### 3.1 Functional requirements (CUJs/Use Cases)
- UC-1: <title/flow>
- UC-2: …

### 3.2 Non-functional requirements (NFRs)
- Availability: <99.9% monthly>
- Latency: <p95: X ms>
- Throughput: <Y RPS>
- RTO/RPO: <RTO=X min, RPO=Y min>
- Security/Privacy: <requirements>
- Compliance: <requirements>
- Cost: <budget/unit economics>

---

## 4. Architecture
### 4.1 Views & diagrams
- **C4**: C1 (Context), C2 (Containers), C3 (Components) [links to diagrams-as-code]
- **Deployment diagram** (regions/AZs, networks, runtimes)
- **Sequence diagrams** for critical flows (login, checkout, etc.)

### 4.2 Components
- <Service A>: responsibility, tech, scaling, data
- <Service B>: …

### 4.3 Integration & contracts
- Sync APIs: REST/gRPC/GraphQL (schemas, versions, timeouts, retries)
- Async: topics/queues, event schemas, delivery semantics, DLQs
- External deps: SLAs, auth, rate limits, fallback behavior

### 4.4 Architecture decisions
- Link **ADRs** (storage choice, tenancy model, sync vs async, etc.)

---

## 5. Data design
### 5.1 Data model
- Entities, relationships (ERD link), key fields, indexing strategy

### 5.2 Consistency & transactions
- Per-flow guarantees: strong / RYW / eventual / bounded staleness
- Idempotency strategy, monotonic reads, conflict resolution

### 5.3 Lifecycle, retention, privacy
- Classification (PII/PHI/PCI), minimization, masking/pseudonymization
- Retention/archive policies, DSAR/export/delete workflows

### 5.4 Backups & restore
- Backup plan (full/incremental/PITR), retention, geo-redundancy
- Restore drills cadence, last drill results, RTO/RPO evidence

---

## 6. Reliability & resilience
- **SLOs/SLIs** per capability (success %, latency p95/p99, freshness)
- Failure modes & mitigations (timeouts, retries, circuit breakers, bulkheads)
- Degradation paths (serve-stale, read-only, queue writes)
- Multi-AZ/Region posture; failover plan & runbooks
- Chaos & DR exercise plan

---

## 7. Security & privacy
- Threat model (STRIDE/PASTA summary)
- AuthN (OIDC/mTLS) & AuthZ (RBAC/ABAC/ReBAC)
- Secrets & key management (KMS/Vault), rotation policy
- Encryption (in transit, at rest; per-tenant keys if multi-tenant)
- Logging & audit (admin actions, data access)
- Compliance mapping (ISO/SOC2/GDPR/PCI) → controls & evidence

---

## 8. Observability
- Metrics (RED/USE), logs (structured + correlation IDs), traces (OpenTelemetry)
- Dashboards (service & CUJ), SLO burn-rate alerts, synthetic probes
- Runbooks linked from alerts; on-call practices

---

## 9. Performance & Capacity
- Latency budgets per flow; concurrency limits
- Load/stress/spike/soak test plan & targets
- Scaling strategy (HPA/ASG/replicas), headroom policy
- Known bottlenecks & mitigations

---

## 10. Delivery & operations
- CI/CD pipeline (stages, gates: lint, unit, SCA/SAST/DAST, integration, e2e)
- Release strategy (blue/green, canary, feature flags) & rollback plan
- Infrastructure as Code (tooling, modules, environments, drift detection)
- Operational processes (incidents, postmortems, change management)

---

## 11. Cost & FinOps
- Expected monthly cost by component (compute, storage, egress, SaaS)
- Tagging/allocation model; budgets & alerts
- Unit economics (cost per request/tenant/GB)
- Optimization opportunities (rightsizing, autoscaling, lifecycle, commitments)

---

## 12. Risks & mitigations
- Top N risks (tech, security, compliance, schedule) with likelihood × impact
- Mitigations & owners; open questions & assumptions
- Kill/exit criteria for spikes or pilots

---

## 13. Test strategy & quality gates
- Test matrix: unit, contract, integration, E2E, performance, security, chaos
- Coverage targets; mutation testing (where applicable)
- Quality gates to promote to staging/prod (no critical vulns, perf budgets, SLO checks)

---

## 14. Rollout plan
- Environments & milestones
- Data migration/backfill steps (expand → migrate → contract)
- Tenant/region phasing, comms plan, training documentation

---

## 15. Acceptance & readiness
- **Go-live checklist** (security, reliability, performance, observability, DR tested)
- Stakeholder sign-offs (product, security, SRE, data, finance)
- Known limitations & deferred items

---

## 16. Appendices
- Glossary
- Links: ADRs, diagrams, repos, tickets
- Change log (version, date, author, summary)

---

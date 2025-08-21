# Global Master Checklist

This **one-pager checklist** consolidates all critical aspects of system design, delivery, and operations.  
It can be used as a **pre-flight checklist** before committing to design decisions or production releases.

---

## 1. Discovery & Requirements
- [ ] Business goals, success metrics, and KPIs defined.
- [ ] Functional requirements documented.
- [ ] Non-functional requirements identified (availability, latency, security, etc.).
- [ ] Constraints documented (budget, deadlines, compliance).
- [ ] Stakeholders and ownership defined.

---

## 2. Quality Attributes & Constraints
- [ ] Availability targets (SLA, SLO, SLI) agreed.
- [ ] Latency/response time requirements defined.
- [ ] Throughput/load expectations documented.
- [ ] Recovery & durability goals defined (RTO, RPO).
- [ ] Trade-offs between CAP/BASE/ACID considered.

---

## 3. Decision-Making
- [ ] ADRs written for key architectural decisions.
- [ ] Alternatives/trade-offs documented.
- [ ] ATAM or lightweight evaluation performed.
- [ ] Risks and assumptions captured.

---

## 4. Modeling & Documentation
- [ ] C4 model diagrams (Context, Container, Component, Code) created.
- [ ] Sequence & interaction diagrams for critical flows.
- [ ] Data model defined (ERD/schema).
- [ ] System Design Document (SDD) prepared.
- [ ] Documentation updated in repo (version-controlled).

---

## 5. Data Strategy
- [ ] Data consistency model chosen (strong/eventual).
- [ ] Persistence & storage layers defined.
- [ ] Backup & restore procedures documented and tested.
- [ ] Data retention & GDPR/PII considerations addressed.
- [ ] Archival strategy for cold data in place.

---

## 6. Architecture & Integration Patterns
- [ ] Chosen patterns documented (CQRS, Event Sourcing, Saga, etc.).
- [ ] Integration strategy (sync vs async, APIs, messaging) defined.
- [ ] Caching strategy documented (cache-aside, write-through, TTLs).
- [ ] Idempotency and retry mechanisms applied.
- [ ] External dependencies abstracted.

---

## 7. Reliability & Resilience
- [ ] Redundancy & failover strategies defined.
- [ ] Health checks and self-healing implemented.
- [ ] Circuit breakers, retries, and timeouts applied.
- [ ] Chaos testing or fault injection performed.
- [ ] RTO/RPO validated in disaster recovery drills.

---

## 8. Security & Privacy
- [ ] Authentication & authorization defined (OIDC, OAuth2, RBAC/ABAC).
- [ ] Secrets managed securely (Vault, KMS).
- [ ] Data encrypted at rest and in transit.
- [ ] Security testing in place (SAST, DAST, SCA, secrets scanning).
- [ ] Privacy requirements (GDPR, HIPAA, PCI-DSS) addressed.

---

## 9. Performance & Capacity
- [ ] Load and stress tests executed.
- [ ] Scalability model defined (horizontal vs vertical).
- [ ] Caching/performance optimizations applied.
- [ ] Capacity planning & autoscaling configured.
- [ ] Bottlenecks identified (CPU, I/O, DB).

---

## 10. Observability & Monitoring
- [ ] Centralized logging configured.
- [ ] Metrics defined (RED/USE, custom KPIs).
- [ ] Tracing enabled (distributed tracing with OpenTelemetry).
- [ ] Dashboards created for SLIs and business metrics.
- [ ] Alerting configured with actionable thresholds.

---

## 11. Delivery & Operations
- [ ] CI/CD pipelines implemented (build, test, deploy).
- [ ] Infrastructure as Code (Terraform, Ansible, Helm) applied.
- [ ] Release strategy defined (canary, blue/green, feature flags).
- [ ] Rollback strategy documented and tested.
- [ ] Runbooks created for operations team.

---

## 12. Compliance, Risk & Governance
- [ ] Regulatory requirements mapped (GDPR, HIPAA, ISO, SOC2).
- [ ] Risk register created and maintained.
- [ ] Security & compliance audits integrated into pipeline.
- [ ] Change management process followed.
- [ ] Ownership and accountability documented.

---

## 13. Cost & FinOps
- [ ] Cloud cost estimation performed.
- [ ] Cost allocation (tags, accounts, projects) in place.
- [ ] Autoscaling & rightsizing strategies applied.
- [ ] Reserved/spot instances considered.
- [ ] Cost monitoring dashboards configured.

---

## 14. Test Strategy & Quality Gates
- [ ] Unit, integration, and E2E tests automated.
- [ ] Contract testing applied for APIs.
- [ ] Performance/load tests executed against staging.
- [ ] Security tests integrated (SAST, DAST, SCA).
- [ ] Quality gates enforced in CI/CD (coverage, static analysis, vulnerabilities).

---

## 15. Continuous Improvement & Evolution
- [ ] Post-mortems documented after incidents.
- [ ] Retrospectives held and improvements tracked.
- [ ] Technical debt logged and prioritized.
- [ ] Architecture reviewed quarterly.
- [ ] Knowledge shared (ADR updates, wiki, brown-bag sessions).

---

> ✅ This master checklist acts as a **single reference point** to ensure nothing critical is missed.  
> Use it during design reviews, pre-production readiness assessments, and quarterly system audits.

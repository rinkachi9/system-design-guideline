# 📚 Glossary – System Design & Architecture Playbook (Extended)

This glossary defines common terms, acronyms, and concepts used across system design, architecture, DevOps, and reliability engineering.  
It ensures **shared understanding** and reduces ambiguity in documentation, ADRs, and discussions.

---

## A

- **ACID** – Set of database properties: Atomicity, Consistency, Isolation, Durability. Guarantees reliable transactions.
- **ADR (Architecture Decision Record)** – Lightweight document capturing significant architectural decisions, their context, and consequences.
- **APM (Application Performance Monitoring)** – Tools and practices for monitoring application health, performance, and user experience.
- **API (Application Programming Interface)** – Contract that defines how software components interact.

---

## B

- **BIA (Business Impact Analysis)** – Process of identifying and evaluating the effects of disruptions on business operations.
- **Bottleneck** – Component or process limiting system throughput or scalability.
- **Blue/Green Deployment** – Release technique where two production environments (blue and green) exist, reducing downtime and risk.

---

## C

- **CAP Theorem** – Principle in distributed systems: you can only guarantee two of three (Consistency, Availability, Partition Tolerance).
- **CD (Continuous Delivery/Deployment)** – Practice of automatically delivering tested software to production (Delivery = ready, Deployment = live).
- **CDC (Change Data Capture)** – Technique of tracking and capturing changes in a database (inserts, updates, deletes) to propagate them to downstream systems.
- **CI (Continuous Integration)** – Practice of frequently merging code changes, validated by automated tests.
- **CICD Pipeline** – Automated pipeline for building, testing, and deploying code.
- **C4 Model** – Diagramming standard for visualizing system architecture at 4 levels: Context, Container, Component, Code.
- **Chaos Engineering** – Discipline of experimenting on a system to build confidence in its resilience.
- **Cloud-Native** – Architectural style leveraging cloud services, elasticity, and microservices.
- **Compliance** – Adherence to regulations, laws, and internal policies.

---

## D

- **DAU (Daily Active Users)** – Number of unique users interacting with a system daily (commonly used in product analytics).
- **DDD (Domain-Driven Design)** – Software design approach focusing on core domain and ubiquitous language.
- **DR (Disaster Recovery)** – Plans and processes to restore systems after catastrophic failure.
- **Durability** – Guarantee that committed data will not be lost.

---

## E

- **Error Budget** – Allowed amount of unreliability a system can tolerate before violating SLOs.
- **Event Sourcing** – Storing system state as a sequence of events rather than current values.
- **E2E Test (End-to-End Test)** – Tests verifying system functionality from the perspective of a user or external system.

---

## F

- **Failover** – Automatic switch to a standby system upon failure of the primary.
- **FinOps** – Cloud financial management practice ensuring cost efficiency.
- **Fault Tolerance** – Ability of a system to continue operating despite component failures.

---

## G

- **GitOps** – Managing infrastructure and applications using Git as the source of truth.
- **Governance** – Framework of rules, roles, and processes ensuring compliance, accountability, and proper system management.

---

## H

- **HA (High Availability)** – Design goal ensuring minimal downtime by eliminating single points of failure.
- **Horizontal Scaling** – Adding more nodes/instances to handle increased load.

---

## I

- **IaC (Infrastructure as Code)** – Managing infrastructure via machine-readable configuration files.
- **Incident Postmortem** – Blameless review after an incident to identify causes and prevention actions.
- **Isolation** – Property ensuring concurrent transactions do not affect each other’s integrity.
- **Info Disclosure (STRIDE)** – Threat category where sensitive data is exposed improperly.

---

## L

- **Latency** – Time taken for a request to travel from source to destination and back.
- **Load Balancer** – Component distributing traffic across multiple servers.
- **Logging** – Recording of system events and states for troubleshooting and analysis.

---

## M

- **MAU (Monthly Active Users)** – Number of unique users interacting with a system monthly (key growth metric).
- **MTBF (Mean Time Between Failures)** – Average time a system operates before failing.
- **MTTR (Mean Time To Repair/Recover)** – Average time to restore service after a failure.
- **Microservices** – Architectural style decomposing applications into small, independently deployable services.
- **Monitoring** – Continuous collection of system metrics to ensure health and performance.

---

## O

- **Observability** – Ability to infer system state from metrics, logs, and traces.
- **OLTP (Online Transaction Processing)** – Database systems optimized for managing real-time transactional workloads (e.g., banking, reservations).
- **On-Call** – Rotation system where engineers are responsible for responding to alerts and incidents.

---

## P

- **p95 / p99 Latency** – Latency thresholds representing the 95th and 99th percentiles of requests.
- **PASTA (Process for Attack Simulation and Threat Analysis)** – Risk-centric threat modeling methodology focusing on business impact.
- **Partition Tolerance** – System continues operating despite network partition.
- **PCI DSS** – Payment Card Industry Data Security Standard (compliance framework).
- **PHI (Protected Health Information)** – Health-related personal information subject to HIPAA regulations.
- **PII (Personally Identifiable Information)** – Any data that could identify a specific individual (e.g., name, SSN, email).
- **PITR (Point-In-Time Recovery)** – Database recovery technique allowing restoration to a specific moment in the past.
- **Playbook/Runbook** – Predefined procedures for operations and incident response.

---

## R

- **RCA (Root Cause Analysis)** – Structured process to identify underlying reasons for failures.
- **RPO (Recovery Point Objective)** – Maximum acceptable data loss in case of disaster.
- **RTO (Recovery Time Objective)** – Maximum acceptable downtime to restore service.
- **Resilience** – Ability of a system to withstand and recover from failures.
- **Replication** – Copying data across systems to ensure availability and durability.
- **Repudiation (STRIDE)** – Threat where actions cannot be properly tracked or attributed to a user.

---

## S

- **SAST/DAST/SCA** – Static Application Security Testing, Dynamic Application Security Testing, Software Composition Analysis.
- **Scalability** – System’s ability to handle increased workload by adding resources.
- **SDD (Software Design Document)** – Document describing design, decisions, and architecture details of a system.
- **Service Mesh** – Infrastructure layer for managing service-to-service communication.
- **SLA (Service Level Agreement)** – Formal commitment between provider and consumer on availability, performance, etc.
- **SLI (Service Level Indicator)** – Metric used to measure service performance.
- **SLO (Service Level Objective)** – Target reliability/performance goal for an SLI.
- **SOC (Security Operations Center)** – Team responsible for monitoring and responding to security threats.
- **Spoofing (STRIDE)** – Impersonating users or systems.
- **STRIDE** – Threat modeling framework: **Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege**.

---

## T

- **Tampering (STRIDE)** – Unauthorized modification of data or code.
- **Test Pyramid** – Model emphasizing many unit tests, fewer integration tests, and fewer E2E tests.
- **Throttling** – Restricting the rate of requests to protect system stability.
- **Tracing** – Capturing request flow across distributed systems for debugging and performance monitoring.

---

## U

- **Uptime** – Percentage of time a system is operational and accessible.
- **Ubiquitous Language** – Common terminology shared across teams (core concept of DDD).

---

## V

- **Vertical Scaling** – Adding more resources (CPU, RAM) to a single node to handle increased load.
- **Versioning** – Managing different versions of APIs, software, or schema.

---

## Z

- **Zero Trust** – Security model assuming no implicit trust; every access is verified.
- **Zero Downtime Deployment** – Deployment strategy with no service disruption.

---

✅ **Note:** This glossary is a **living document** – update continuously as new technologies, patterns, and concepts emerge.

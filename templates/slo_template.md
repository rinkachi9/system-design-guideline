# 📊 SLO Template – <Service/Component Name>

> Purpose: Define **Service Level Objectives (SLOs)** and how they will be measured, monitored, and enforced.  
> Audience: Engineers, SREs, Product Owners, Compliance.  

> - Ownership: <Team/Owner>
> - Version: <vX.Y>
> - Date: <YYYY-MM-DD>
> - Status: Draft | Review | Approved

---

## 1. Metadata
- **SLO ID:** SLO-<ID>
- **Service/System:** <Service Name>
- **Owner:** <Team/Individual>
- **Created:** <YYYY-MM-DD>
- **Updated:** <YYYY-MM-DD>
- **Status:** Draft | Active | Deprecated

---

## 2. Scope
<What part of the service does this SLO apply to? Example: user-facing API endpoints, database write operations, background jobs.>

---

## 3. SLI (Service Level Indicators)
| **Indicator** | **Definition**                               | **Measurement Method**          | **Data Source**    |
|---------------|----------------------------------------------|---------------------------------|--------------------|
| Availability  | % of successful requests over total requests | Count 2xx/3xx responses ÷ total | Prometheus / Logs  |
| Latency       | % of requests under threshold                | 95th percentile latency         | APM / Tracing      |
| Error Rate    | Ratio of failed requests                     | Count 5xx responses ÷ total     | Logging/Monitoring |
| Durability    | % of successful writes not lost              | Write/read consistency checks   | DB Logs/Backups    |

---

## 4. SLO Objectives
| **SLI**      | **Objective** | **Target** | **Window** | **Error Budget**           |
|--------------|---------------|------------|------------|----------------------------|
| Availability | API uptime    | 99.9%      | 30 days    | 0.1% downtime (~43m/month) |
| Latency      | p95 latency   | <300ms     | 30 days    | 5% of requests can exceed  |
| Error Rate   | API errors    | <0.1%      | 30 days    | 0.1% failed requests       |
| Durability   | Data writes   | 99.999999% | 1 year     | 1 lost write per 100M ops  |

---

## 5. Measurement & tooling
- **Monitoring Tool(s):** Prometheus, Grafana, Datadog, New Relic, etc.
- **Alerting:** Trigger when burn rate >2x error budget in 1h, or >50% of budget used in 7d.
- **Data Aggregation:** Rolling windows, percentiles.
- **Validation:** Regular calibration vs synthetic tests.

---

## 6. Error budget policy
- If error budget is **exceeded** within time window:
    - [ ] Freeze feature releases.
    - [ ] Perform incident analysis.
    - [ ] Prioritize reliability work until back on track.

---

## 7. Dependencies
- Upstream/Downstream SLOs (e.g., database, 3rd party APIs).
- Shared infrastructure SLOs (network, storage).

---

## 8. Risks & assumptions
- Assumption: All client retries are excluded from error rate.
- Risk: Monitoring gaps or false positives may skew results.

---

## 9. Review & governance
- **Review Cycle:** Quarterly | Bi-Annual | Annual
- **Next Review Date:** <YYYY-MM-DD>
- **Approval:** <Architect/PO/SRE Lead>

---

## 10. References
- [Google SRE Book – SLOs & Error Budgets](https://sre.google/sre-book/service-level-objectives/)
- ADRs/SDD related to SLIs/SLOs
- Incident reports linked to historical SLO breaches

---

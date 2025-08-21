# Monitoring & Observability checklist

Observability ensures that systems are not a black box — it allows engineers to **understand internal state by observing external outputs**.  
Monitoring provides the signals, alerting ensures visibility, and observability drives insight for debugging, optimization, and resilience.

---

## 1. Metrics

- [ ] **Golden signals tracked:** Latency, traffic, errors, saturation (per Google SRE).
- [ ] **Service-level metrics:** SLIs aligned with SLOs (availability, response time).
- [ ] **Infrastructure metrics:** CPU, memory, disk I/O, network usage.
- [ ] **Custom business metrics:** Conversions, transactions, queue depth, dropped jobs.
- [ ] **High-cardinality controls:** Label cardinality reviewed to prevent metric explosion.
- [ ] **Aggregation levels:** Metrics available per instance, per region, per service.

---

## 2. Logging

- [ ] **Structured logs:** JSON or key-value format for easy parsing.
- [ ] **Centralized log collection:** All services ship logs to ELK, Loki, or equivalent.
- [ ] **Log levels consistent:** DEBUG, INFO, WARN, ERROR clearly defined.
- [ ] **Sensitive data sanitized:** No PII, secrets, or tokens in logs.
- [ ] **Correlation IDs:** Trace and request IDs propagated across services.
- [ ] **Retention policies:** Logs stored according to compliance & cost policies.

---

## 3. Tracing

- [ ] **Distributed tracing enabled:** End-to-end visibility across microservices.
- [ ] **Context propagation:** Trace IDs flow across async calls, queues, APIs.
- [ ] **Critical paths instrumented:** DB queries, API calls, message brokers.
- [ ] **Sampling strategy:** Adjusted for cost vs insight balance.
- [ ] **Span enrichment:** Include tags for tenant, user, device, region.
- [ ] **Visualization:** Traces viewable in tools like Jaeger, Tempo, Zipkin.

---

## 4. Dashboards

- [ ] **Service health overview:** Golden signals and key metrics visible at a glance.
- [ ] **Team-specific views:** Custom dashboards for DB, API, frontend, infra.
- [ ] **Business dashboards:** KPIs like orders/minute, revenue per hour.
- [ ] **Drill-down capability:** From global to per-instance metrics.
- [ ] **Error heatmaps:** Identify hotspots by service, region, or endpoint.

---

## 5. Alerting

- [ ] **SLO-based alerts:** Triggered when error budget is consumed.
- [ ] **Severity levels defined:** P1 (critical outage) → P3 (low urgency).
- [ ] **Actionable alerts:** No noise — every alert has an owner & runbook.
- [ ] **Escalation policies:** Automated escalation (PagerDuty, OpsGenie).
- [ ] **Synthetic monitoring alerts:** Detect failures before customers do.
- [ ] **Alert fatigue avoided:** Regular review of alert rules & thresholds.

---

## 6. Recovery validation

- [ ] **Alert-to-detection time measured:** MTTD tracked & minimized.
- [ ] **Mean time to recovery (MTTR):** Metrics collected & improved over time.
- [ ] **Chaos testing results integrated:** Ensure monitoring detects injected failures.
- [ ] **Incident postmortems:** Observability gaps identified and closed.

---

## 7. Synthetic & user monitoring

- [ ] **Synthetic checks:** Automated probes for availability (HTTP, DNS, TCP).
- [ ] **User experience monitoring:** RUM (real user monitoring) implemented.
- [ ] **Transaction flows tested:** Critical paths validated (e.g., checkout, login).
- [ ] **Geographic coverage:** Tests from multiple regions.
- [ ] **API contract monitoring:** External APIs verified for SLA adherence.

---

## 8. Compliance & governance

- [ ] **Data retention policies:** Metrics/logs stored per regulatory needs (GDPR, HIPAA).
- [ ] **Audit trails:** Changes in monitoring rules logged & reviewed.
- [ ] **Access controls:** RBAC enforced for dashboards, logs, and traces.
- [ ] **Cost optimization:** Metrics cardinality and log verbosity reviewed.

---

## 9. Tooling & integration

- [ ] **Metrics system:** Prometheus, New Relic, Datadog, or equivalent in place.
- [ ] **Log aggregation:** ELK/EFK, Loki, or cloud-native logging enabled.
- [ ] **Tracing:** OpenTelemetry instrumentation standardized.
- [ ] **Alerting integration:** Slack, email, PagerDuty, or ticketing system.
- [ ] **CI/CD integration:** Monitoring checks included in delivery pipeline.

---

## 10. Continuous improvement

- [ ] **Observability as code:** Dashboards, alerts, and SLOs managed in Git.
- [ ] **Regular review cadence:** Metrics relevance and thresholds validated quarterly.
- [ ] **Feedback loop:** Incidents inform new monitoring/alerting rules.
- [ ] **Team training:** Engineers know how to use observability tools effectively.

---

> ✅ **Monitoring answers “what is wrong?” — observability answers “why?” Together, they enable fast recovery, resilience, and trust.**

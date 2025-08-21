# 🛠 Runbook Template – <System/Service Name>

> Purpose: Clear, actionable steps for **operators, SREs, and engineers** to quickly handle incidents, maintenance, or recurring tasks.  
> Audience: On-call engineers, operations team, incident managers.  

> - Ownership: <Team/Owner>
> - Version: <vX.Y>
> - Date: <YYYY-MM-DD>
> - Status: Draft | Review | Approved

---

## 1. Metadata
- **Runbook ID:** RB-<ID>
- **Service/System:** <Service Name>
- **Author(s):** <Name(s)>
- **Created:** <YYYY-MM-DD>
- **Updated:** <YYYY-MM-DD>
- **Priority:** P1 | P2 | P3
- **Status:** Draft | Active | Deprecated

---

## 2. Purpose
<Explain what this runbook covers — e.g., restart a failing service, handle database failover, rotate secrets.>

---

## 3. Preconditions
- Access requirements (VPN, bastion, permissions)
- Tools needed (CLI, dashboards, scripts)
- Environment (Prod/Staging/Dev)
- Prerequisite runbooks/playbooks (links)

---

## 4. Symptoms & detection
**Triggers:**
- Alerts (list alert names, Prometheus/Grafana query, or PagerDuty incident)
- Logs or error messages (with examples)
- Metrics (thresholds, anomalies)

**Dashboards/Links:**
- Monitoring dashboard: <URL>
- Logs: <URL>
- Traces: <URL>

---

## 5. Step-by-Step procedure
### 5.1 Immediate actions
- [ ] Acknowledge alert in <#system>
- [ ] Notify on-call channel <#channel>
- [ ] Collect logs/metrics snapshot

### 5.2 Diagnostic steps
1. Check service status: `<command>`
2. Validate dependency health: `<command>`
3. Review logs: `<query>`

### 5.3 Remediation steps
- Option A (quick fix): `<commands/steps>`
- Option B (fallback): `<commands/steps>`
- Option C (escalate): escalate to <team/contact>

---

## 6. Rollback / Recovery
- Rollback procedure (e.g., redeploy previous version, restore backup)
- Recovery confirmation (tests, dashboards)
- Validate system returns to normal KPIs

---

## 7. Post-Incident
- Open incident ticket: <link/template>
- Add notes & timeline (who/what/when)
- Create follow-up actions (backlog/security fix/infra change)
- Schedule postmortem if Severity ≥ P1

---

## 8. Escalation matrix
- L1: On-call engineer (<contact>)
- L2: Service owner (<contact>)
- L3: Platform/SRE team (<contact>)
- Security/Compliance contact: <contact>
- Product/Stakeholder comms: <contact>

---

## 9. Verification checklist
- [ ] Alert cleared from monitoring
- [ ] KPIs/SLOs back to baseline
- [ ] User experience confirmed normal (synthetic tests, smoke tests)
- [ ] Runbook updated (if steps changed)

---

## 10. References
- Related runbooks/playbooks
- ADRs/SDD sections relevant
- External vendor docs or API references
- Monitoring/alert definitions

---

### ✅ Quick health check
- [ ] Clear purpose
- [ ] Preconditions defined
- [ ] Alert sources listed
- [ ] Step-by-step reproducible procedure
- [ ] Rollback/recovery included
- [ ] Escalation contacts valid
- [ ] Last tested/validated date present

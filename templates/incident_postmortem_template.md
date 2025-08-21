# 📝 Incident Postmortem Template – <Incident Title/ID>

> Purpose: Provide a **structured and blameless analysis** of incidents to learn, improve, and prevent recurrence.  
> Audience: Engineers, SREs, Product Owners, Leadership.

---

## 1. Metadata
- **Incident ID:** INC-<ID>
- **Title:** <Short descriptive title>
- **Severity:** SEV-1 | SEV-2 | SEV-3 | SEV-4
- **Status:** Closed | Ongoing | Pending
- **Date/Time detected:** <YYYY-MM-DD HH:mm UTC>
- **Date/Time resolved:** <YYYY-MM-DD HH:mm UTC>
- **Duration:** <Xh Xm>
- **Reported by:** <Name/Team>
- **Owner:** <Incident Commander>

---

## 2. Summary
<High-level summary of the incident. 3–4 sentences describing what happened, what the impact was, and how it was resolved.>

---

## 3. Impact
- **Who/what was affected:** users, customers, services, systems
- **Impact type:** downtime, degraded performance, data loss, security breach, financial impact
- **Quantification:**
    - Requests failed: <#>
    - Customers affected: <#/%>
    - SLA/SLO violated: <Y/N>

---

## 4. Timeline

| **Time (UTC)** | **Event**             | **Notes/Source**              |
|----------------|-----------------------|-------------------------------|
| 10:05          | Alert triggered       | PagerDuty alert (CPU > 95%)   |
| 10:10          | Incident declared     | Incident Commander assigned   |
| 10:20          | Mitigation started    | Rolled back deployment v1.2.3 |
| 10:50          | Service restored      | API latency back to baseline  |
| 11:30          | Root cause identified | DB connection pool exhaustion |

---

## 5. Root Cause Analysis (RCA)
- **Immediate cause:** <e.g., unbounded DB connections in service X>
- **Contributing factors:**
    - Missing rate limiting
    - Incomplete test coverage
    - Alert thresholds too high
- **Why analysis (5 Whys):**
    - Why 1: ...
    - Why 2: ...
    - Why 3: ...
    - Why 4: ...
    - Why 5: ...

---

## 6. Detection & response
- **How was it detected?** (alert, customer report, monitoring, manual check)
- **Was detection timely?** (Y/N)
- **Was response effective?** (Y/N, with details)
- **What went well in the response?**
- **What went poorly in the response?**

---

## 7. Resolution & recovery
- **Mitigation steps taken:**
    - Restarted service X
    - Rolled back deployment v1.2.3
- **Recovery confirmation:** (logs, monitoring dashboards, SLO metrics)
- **Long-term resolution:** (e.g., fix connection pooling bug, add integration tests)

---

## 8. Lessons learned
- **What worked well?**
- **What needs improvement?**
- **Key takeaways for future prevention?**

---

## 9. Action items
| **ID** | **Action**                         | **Owner**       | **Priority** | **Due Date** | **Status**  |
|--------|------------------------------------|-----------------|--------------|--------------|-------------|
| A1     | Add DB connection pool limits      | Backend Team    | High         | YYYY-MM-DD   | Open        |
| A2     | Update runbook for high-CPU alerts | SRE Team        | Medium       | YYYY-MM-DD   | In Progress |
| A3     | Adjust alert thresholds            | Monitoring Team | Low          | YYYY-MM-DD   | Done        |

---

## 10. Communication
- **Internal comms:** Slack channel, incident ticket, war room notes
- **External comms:** Status page update, customer notifications
- **Post-incident review meeting:** scheduled for <YYYY-MM-DD HH:mm UTC>

---

## 11. References
- Related incidents
- Runbooks used
- Monitoring dashboards/screenshots
- ADRs/SDD changes linked  

# Security checklist

Security must be treated as a **first-class citizen** of system design, not an afterthought.  
This checklist provides a **practical baseline** for ensuring systems are protected across all layers: application, infrastructure, processes, and people.

---

## 1. Identity & Access Management (IAM)

- [ ] **Least privilege:** Ensure users, services, and machines only have the permissions they need.
- [ ] **Strong authentication:** Enforce MFA for all privileged accounts.
- [ ] **RBAC / ABAC:** Use role-based or attribute-based access control, not ad-hoc permissions.
- [ ] **Service accounts:** Rotate credentials, use managed identities where possible.
- [ ] **Audit trails:** Log all administrative actions and access attempts.

---

## 2. Application security

- [ ] **Input validation & sanitization:** Prevent injection attacks (SQLi, XSS, command injection).
- [ ] **Secure defaults:** Disable unused endpoints, methods, and services.
- [ ] **Secrets management:** Never hardcode secrets; store in vaults (HashiCorp Vault, AWS Secrets Manager, Azure Key Vault).
- [ ] **Dependency scanning (SCA):** Regularly scan and patch third-party libraries.
- [ ] **SAST & DAST:** Integrate static and dynamic security testing into CI/CD pipelines.
- [ ] **API security:**
    - [ ] Use proper authentication (OAuth2, JWT, mTLS).
    - [ ] Rate limit endpoints.
    - [ ] Validate payloads against schemas.

---

## 3. Data protection

- [ ] **Encryption in transit:** Enforce TLS 1.2+ for all communications.
- [ ] **Encryption at rest:** Encrypt databases, storage, backups, and logs.
- [ ] **Key management:** Rotate encryption keys regularly, store them securely (KMS, HSM).
- [ ] **PII/PHI handling:** Follow GDPR, HIPAA, or relevant compliance for sensitive data.
- [ ] **Data minimization:** Collect and store only what is strictly necessary.

---

## 4. Infrastructure & cloud security

- [ ] **Network segmentation:** Separate public, private, and admin zones (zero trust principles).
- [ ] **Firewalls & security groups:** Deny by default, allow explicitly.
- [ ] **Patch management:** Automatically apply security patches for OS, runtimes, and images.
- [ ] **Immutable infrastructure:** Prefer rebuilding over patching running servers.
- [ ] **Container security:**
    - [ ] Use distroless/minimal base images.
    - [ ] Regularly scan images for vulnerabilities.
    - [ ] Run containers as non-root.
- [ ] **Cloud misconfigurations:** Regularly audit IAM policies, storage buckets, and networking rules.

---

## 5. Monitoring & incident response

- [ ] **Centralized logging:** All logs aggregated and protected from tampering.
- [ ] **Alerting:** Real-time alerts for anomalies (failed logins, privilege escalation, network spikes).
- [ ] **SIEM integration:** Logs correlated across apps, infra, and services.
- [ ] **Incident response plan:** Defined roles, escalation paths, and playbooks.
- [ ] **Tabletop exercises:** Regularly simulate incidents (ransomware, data leak, insider attack).
- [ ] **Post-mortems:** Blameless reviews after every security incident.

---

## 6. Supply chain & DevSecOps

- [ ] **SBOM (Software Bill of Materials):** Maintain SBOM for all builds.
- [ ] **Code signing:** Verify integrity of code and artifacts.
- [ ] **CI/CD hardening:**
    - [ ] Protect pipelines against tampering.
    - [ ] Store secrets securely.
    - [ ] Enforce signed commits and branch protections.
- [ ] **Third-party risks:** Vet vendors, dependencies, and SaaS integrations.

---

## 7. Human factor & policy

- [ ] **Security awareness training:** Regular training for developers, ops, and business teams.
- [ ] **Phishing simulations:** Run periodic campaigns to test awareness.
- [ ] **Onboarding/offboarding:** Automate provisioning/deprovisioning of accounts.
- [ ] **Separation of duties:** Avoid single points of control for critical actions.
- [ ] **Acceptable use policy:** Clearly defined and enforced across the organization.

---

## 8. Advanced practices (recommended)

- [ ] **Zero Trust architecture:** Verify *every* request, regardless of origin.
- [ ] **Chaos security testing:** Inject simulated security failures.
- [ ] **Bug bounty / responsible disclosure:** Allow ethical hackers to report vulnerabilities.
- [ ] **Red/Blue/Purple teaming:** Regular adversarial simulations.
- [ ] **Continuous compliance:** Automate checks for SOC2, ISO27001, PCI-DSS, etc.

---

> 🔑 **Security is never “done.”** It is a continuous, evolving practice of defense, detection, and response.

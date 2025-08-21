# Testing checklist

Testing is not only about finding bugs — it is about building **confidence** that the system works as intended under expected and unexpected conditions.  
This checklist provides a **practical baseline** for planning, implementing, and validating tests across different layers.

---

## 1. Unit testing

- [ ] **Isolated logic:** Each unit test covers a single method/function in isolation.
- [ ] **AAA pattern (Arrange-Act-Assert):** Tests are structured consistently.
- [ ] **Mocking/stubbing:** External dependencies are mocked, not called.
- [ ] **Edge cases:** Include boundary values, null/empty inputs, and invalid states.
- [ ] **Deterministic:** Tests always return the same result.
- [ ] **Fast execution:** Unit tests run in milliseconds, supporting frequent execution.

---

## 2. Integration testing

- [ ] **Real components:** Services, databases, and APIs tested together (no mocks).
- [ ] **Contracts:** Verify schema, serialization, and backward compatibility of APIs.
- [ ] **Data correctness:** Ensure queries, persistence, and data flows are correct.
- [ ] **Failure handling:** Simulate partial failures (timeouts, retries, fallbacks).
- [ ] **Idempotency:** Repeated calls should not cause data corruption.

---

## 3. End-to-End (E2E) testing

- [ ] **Critical user journeys:** Cover login, purchase, workflow completion, etc.
- [ ] **Cross-browser / device:** Verify functionality across environments.
- [ ] **Authentication & authorization:** Confirm proper access control enforcement.
- [ ] **Error scenarios:** Simulate invalid inputs, expired sessions, blocked accounts.
- [ ] **Accessibility:** Ensure compliance with WCAG / ADA guidelines.

---

## 4. Performance & load testing

- [ ] **Baseline measurements:** Record response times, throughput, latency.
- [ ] **Load testing:** Gradually increase load to identify bottlenecks.
- [ ] **Stress testing:** Push system beyond limits to observe failure modes.
- [ ] **Soak testing:** Run long-duration tests to detect memory leaks or degradation.
- [ ] **Scalability validation:** Confirm auto-scaling or manual scaling policies.

---

## 5. Security testing

- [ ] **SAST (Static Analysis):** Code scanned for vulnerabilities during CI.
- [ ] **DAST (Dynamic Analysis):** Running app scanned for OWASP Top 10 issues.
- [ ] **Dependency scanning (SCA):** Third-party libraries scanned for CVEs.
- [ ] **Penetration testing:** Regularly scheduled by internal or external teams.
- [ ] **Secrets detection:** Automated checks for credentials in code/config.

---

## 6. Data & migration testing

- [ ] **Schema migrations:** Validated on staging before production.
- [ ] **Backward compatibility:** Old data remains usable after changes.
- [ ] **Data integrity checks:** Foreign keys, constraints, and indexes validated.
- [ ] **Rollback tested:** Confirm downgrade path works safely.

---

## 7. Observability & monitoring validation

- [ ] **Logs:** Critical events logged with sufficient context.
- [ ] **Metrics:** SLIs (latency, error rate, throughput) tracked.
- [ ] **Tracing:** Distributed traces confirm request flow visibility.
- [ ] **Alerting rules:** Validated against test incidents.
- [ ] **Chaos testing:** Inject failures to confirm monitoring detects them.

---

## 8. Release & deployment testing

- [ ] **Smoke tests:** Run automatically after every deployment.
- [ ] **Canary/blue-green releases:** Verified before full rollout.
- [ ] **Rollback strategy:** Automated and validated.
- [ ] **CI/CD pipeline tests:** Build, test, and deploy stages fully automated.
- [ ] **Environment parity:** Staging environment mirrors production.

---

## 9. Non-Functional testing

- [ ] **Usability:** Ensure intuitive user experience and clear feedback.
- [ ] **Localization:** Content, dates, and formats adapt correctly to locales.
- [ ] **Compliance:** System tested for GDPR, HIPAA, PCI-DSS, or relevant standards.
- [ ] **Resilience:** Confirm system recovers gracefully from network failures.
- [ ] **Compatibility:** Works across supported OS, hardware, and software versions.

---

## 10. Quality gates

- [ ] **Code coverage target:** e.g., >80% for business-critical modules.
- [ ] **Static analysis:** No high/critical issues.
- [ ] **Open vulnerabilities:** No exploitable CVEs in dependencies.
- [ ] **Performance SLAs:** Latency, throughput, and availability goals met.
- [ ] **Regression checks:** No previously fixed bug reintroduced.
- [ ] **Documentation:** Updated tests, ADRs, and system design docs.

---

> ✅ **A robust test strategy ensures reliability, security, and maintainability — not just correctness of code.**

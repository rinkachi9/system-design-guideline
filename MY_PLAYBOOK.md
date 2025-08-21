## My architecture playbook

This playbook is a **collection of principles, heuristics, and mental models** that guide my decision-making as an architect.  
It serves as a **north star** when evaluating trade-offs, designing new systems, or reviewing existing ones.

### Core principles

- **Simplicity First**
    - Favor the simplest solution that solves the problem.
    - Complexity is a liability; introduce it only when justified.

- **Explicit Trade-offs**
    - Every design choice has a cost — latency vs consistency, CAP vs ACID, speed vs reliability.
    - Document trade-offs and revisit them as the system evolves.

- **Design for change**
    - Assume requirements, scale, and priorities will change.
    - Modularize, decouple, and favor open standards to reduce lock-in.

- **You build it, you run it**
    - Ownership extends from development to operations.
    - Architecture is not “done” at delivery — it must be operable, observable, and maintainable.

- **Secure by design**
    - Security is not a feature — it is a baseline.
    - Default to least privilege, encryption, and defense in depth.

- **Resilience over perfection**
    - Systems will fail — the goal is graceful degradation and fast recovery.
    - Build with retries, circuit breakers, and failover in mind.

- **Measure everything**
    - What is not measured cannot be improved.
    - Design systems with metrics, tracing, and logs from day one.

---

### Design heuristics

- **High Cohesion, Low Coupling**
    - Keep related functionality together.
    - Minimize dependencies across domains and services.

- **Separation of Concerns**
    - Split UI, business logic, and data persistence clearly.
    - Avoid “God objects” and monolithic services without boundaries.

- **APIs are contracts**
    - Treat APIs as stable contracts.
    - Version them properly, document, and test with consumers in mind.

- **Think in failure modes**
    - For each component, ask: *“What happens if this fails?”*
    - Always have fallback and retry strategies.

- **Optimize for readability**
    - Code and architecture diagrams are read more than written.
    - Prefer clarity and consistency over cleverness.

- **Prefer evolutionary architecture**
    - Start with what works now; enable the system to evolve.
    - Avoid big-bang rewrites — iterate, measure, improve.

- **Economics of scale**
    - Cost, latency, and throughput trade-offs should drive design.
    - Optimize for the right scale, not premature optimization.

---

### Decision "Rules of Thumb"

- **Latency Budgeting:**  
  Allocate latency budgets across system layers; watch out for “hidden” network calls.

- **Data Locality:**  
  Keep data close to where it’s used; distributed systems amplify latency.

- **Stateless First:**  
  Favor stateless services when possible — easier to scale and recover.

- **Prefer Async When Possible:**  
  Asynchronous patterns improve resilience but complicate UX. Use wisely.

- **One Level of Indirection Is Enough:**  
  Avoid over-engineering with too many layers or abstractions.

- **Fail Fast, Recover Gracefully:**  
  Detect errors early, fail visibly, and recover without human intervention.

---

### Continuous alignment

- [ ] **Business alignment:** Does the architecture solve the actual problem?
- [ ] **Stakeholder alignment:** Are trade-offs clear and acceptable?
- [ ] **Team alignment:** Can the team build, run, and evolve the system?
- [ ] **Technology alignment:** Are chosen tools sustainable, supported, and secure?

---

> ⚡ **Architecture is not about choosing the “perfect design” — it’s about making informed decisions, documenting trade-offs, and evolving systems that deliver business value sustainably.**

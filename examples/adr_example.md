# ADR 0001: Choose primary database

- **Status:** Accepted
- **Date:** 2025-08-17
- **Authors:** Mateusz Ciuła
- **Version:** 1.0

---

## Context

We need a **primary database** for the core transactional workloads of our system.  
The system will handle:
- Financial transactions (ACID compliance required).
- User data with strict **consistency** and **referential integrity** requirements.
- High-read workloads with occasional analytical queries.

Constraints:
- Compliance with **GDPR** (data protection).
- Preference for **open-source solutions** with strong community support.
- Need for easy **cloud deployment** (Azure, AWS, GCP).

Key quality attributes:
- **Consistency** and **durability** are top priorities.
- **Scalability** should be achievable via replication and sharding if needed.
- **Operational maturity** (monitoring, backup, failover support) is required.

---

## Decision

We will use **PostgreSQL** as the primary database for transactional workloads.

---

## Alternatives

1. **PostgreSQL**
    - ✅ Pros: Strong ACID guarantees, mature ecosystem, rich SQL support, strong replication features, extensions (PostGIS, TimescaleDB).
    - ❌ Cons: Scaling writes beyond a single node requires sharding or external solutions.

2. **MongoDB**
    - ✅ Pros: Flexible schema, high scalability, good for document-centric use cases.
    - ❌ Cons: Weaker ACID guarantees historically (though improved in recent versions), less suited for relational data and strict consistency needs.

3. **MySQL**
    - ✅ Pros: Lightweight, widespread adoption, good performance.
    - ❌ Cons: Weaker features compared to PostgreSQL (e.g., window functions, JSON handling), replication less flexible.

---

## Consequences

- **Positive impacts:**
    - Reliable support for ACID transactions.
    - Easier enforcement of data integrity and schema constraints.
    - Strong community and enterprise adoption (reduced vendor lock-in).
    - Rich feature set for future analytical use cases.

- **Negative impacts:**
    - Potential challenges in horizontal write scaling.
    - Requires DBAs/devops familiarity with Postgres operational aspects.

- **Impact on teams:**
    - Developers benefit from strong SQL support and relational modeling.
    - Operations team must manage replication, backup, and scaling strategies.

- **Future evolution:**
    - If system grows beyond single-node Postgres scaling, we may adopt **Citus** (sharding extension) or migrate certain workloads to specialized datastores (e.g., columnar DB for analytics).

---

## Related decisions

- ADR-0002: Strategy for database backups & disaster recovery
- ADR-0003: Data consistency model (eventual vs strong)

---

## References

- PostgreSQL Official Docs: https://www.postgresql.org/docs/
- Martin Fowler on Polyglot Persistence: https://martinfowler.com/bliki/PolyglotPersistence.html
- Benchmark: PostgreSQL vs MongoDB 2024 — [link to internal test report]

---

✅ **Decision summary:** PostgreSQL provides the best trade-off between **consistency, maturity, compliance, and community adoption** for our core system needs.

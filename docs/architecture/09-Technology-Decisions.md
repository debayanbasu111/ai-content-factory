# Technology Decisions

**Project:** ForgeFlow AI

**Document:** 09-Technology-Decisions.md

**Architecture Version:** 1.0

**Status:** Living Document

**Last Updated:** 26 June 2026

---

# 1. Purpose

This document provides a consolidated overview of the technology decisions adopted for ForgeFlow AI.

Detailed rationale, trade-offs, alternatives, and implementation guidance are documented in the corresponding Architecture Decision Records (ADRs).

This document serves as the central technology index for the project.

---

# 2. Technology Strategy

ForgeFlow AI follows the following technology strategy:

* Open Source First
* Local First
* Cloud Ready
* Vendor Neutral
* Modular
* Replaceable
* Enterprise Grade
* Production Ready

Technology choices are evaluated based on long-term maintainability rather than short-term convenience.

---

# 3. Decision Principles

Technologies should:

* Solve a real problem.
* Be actively maintained.
* Have strong community adoption.
* Support automation.
* Integrate well with the existing architecture.
* Avoid unnecessary vendor lock-in.
* Scale with future platform requirements.

---

# 4. Technology Landscape

## Backend

| Category   | Selected Technology | ADR     |
| ---------- | ------------------- | ------- |
| Language   | Java 21             | ADR-027 |
| Framework  | Spring Boot         | ADR-028 |
| Build Tool | Maven               | ADR-029 |

---

## Frontend

| Category   | Selected Technology | ADR     |
| ---------- | ------------------- | ------- |
| Language   | TypeScript          | ADR-030 |
| Framework  | React               | ADR-031 |
| Build Tool | Vite                | ADR-032 |

---

## Architecture

| Decision                      | ADR                 |
| ----------------------------- | ------------------- |
| Hexagonal Architecture        | ADR-001             |
| Event-Driven Architecture     | ADR-002             |
| Domain-Driven Design          | ADR-003 *(planned)* |
| Clean Architecture Principles | ADR-004 *(planned)* |

---

## Data

| Category            | Selected Technology | ADR     |
| ------------------- | ------------------- | ------- |
| Relational Database | PostgreSQL          | ADR-008 |
| Cache               | Redis               | ADR-010 |
| Object Storage      | MinIO               | ADR-011 |

---

## Messaging

| Category       | Selected Technology | ADR     |
| -------------- | ------------------- | ------- |
| Message Broker | RabbitMQ            | ADR-009 |

---

## AI Runtime

| Category           | Selected Technology | ADR     |
| ------------------ | ------------------- | ------- |
| Local LLM Runtime  | Ollama              | ADR-012 |
| Image Generation   | ComfyUI             | ADR-013 |
| Speech Recognition | Whisper             | ADR-014 |
| Text-to-Speech     | Piper               | ADR-015 |

---

## Infrastructure

| Category         | Selected Technology  | ADR     |
| ---------------- | -------------------- | ------- |
| Containerization | Docker               | ADR-016 |
| Orchestration    | Kubernetes           | ADR-017 |
| API Gateway      | Kong                 | ADR-033 |
| Reverse Proxy    | Traefik *(optional)* | ADR-018 |

---

## Security

| Category          | Selected Technology           | ADR     |
| ----------------- | ----------------------------- | ------- |
| Authentication    | Keycloak / Microsoft Entra ID | ADR-020 |
| Secret Management | HashiCorp Vault               | ADR-021 |
| Security Model    | Zero Trust                    | ADR-019 |

---

## Monitoring

| Category   | Selected Technology | ADR     |
| ---------- | ------------------- | ------- |
| Metrics    | Prometheus          | ADR-034 |
| Dashboards | Grafana             | ADR-035 |
| Logs       | Loki                | ADR-036 |
| Tracing    | OpenTelemetry       | ADR-037 |

---

## CI/CD

| Category           | Selected Technology       | ADR     |
| ------------------ | ------------------------- | ------- |
| Source Control     | GitHub                    | ADR-038 |
| CI/CD              | GitHub Actions            | ADR-039 |
| Container Registry | GitHub Container Registry | ADR-040 |

---

# 5. Technology Maturity

Technologies are classified according to their adoption status.

| Status       | Meaning                                            |
| ------------ | -------------------------------------------------- |
| Approved     | Officially adopted and recommended.                |
| Planned      | Selected but not yet implemented.                  |
| Experimental | Under evaluation; not for production use.          |
| Deprecated   | No longer recommended; retained for compatibility. |

---

# 6. Technology Selection Criteria

Technologies are evaluated against:

* Community adoption
* Long-term maintenance
* Documentation quality
* Performance
* Security
* Ecosystem maturity
* Compatibility
* Operational complexity
* Learning curve
* Licensing

No technology is adopted solely because it is popular.

---

# 7. Technology Evolution

Technology choices are expected to evolve.

Changes are governed by:

1. Technical evaluation
2. RFC (if applicable)
3. Architecture review
4. New ADR
5. Implementation
6. Migration

Existing ADRs are never rewritten to hide historical decisions.

---

# 8. Relationship to ADRs

This document summarizes technology choices.

Detailed information is maintained in the ADR repository.

```
Technology Decision

↓

ADR

↓

Implementation

↓

Deployment
```

---

# 9. Future Evaluation Candidates

Potential future technologies include:

* Apache Kafka
* Temporal
* LangGraph
* OpenSearch
* Milvus
* Qdrant
* Argo Workflows
* Apache Airflow

Adoption requires a formal ADR.

---

# 10. Traceability

This document references:

* Architecture Documents
* Architecture Decision Records
* RFC Repository
* Development Standards

---

# 11. Version History

| Version | Date         | Description                       |
| ------- | ------------ | --------------------------------- |
| 1.0.0   | 26 June 2026 | Initial Technology Decision Index |

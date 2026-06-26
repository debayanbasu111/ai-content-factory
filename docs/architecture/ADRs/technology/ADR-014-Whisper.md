# ADR-001: Adopt Hexagonal Architecture

**Project:** ForgeFlow AI

**Document:** ADR-001-Hexagonal-Architecture.md

**ADR ID:** ADR-001

**Status:** Accepted

**Date:** 26 June 2026

**Decision Makers:** ForgeFlow AI Architecture Team

---

# 1. Title

Adopt Hexagonal Architecture (Ports and Adapters) as the primary architectural style for backend services.

---

# 2. Status

**Accepted**

This decision is effective immediately and applies to all backend services developed within ForgeFlow AI.

---

# 3. Context

ForgeFlow AI is a long-lived AI orchestration platform that integrates with multiple external systems, including:

* AI runtimes (Ollama, ComfyUI, Whisper, Piper)
* PostgreSQL
* RabbitMQ
* Redis
* MinIO
* REST APIs
* Future cloud services

These integrations are expected to evolve over time. Tight coupling between business logic and infrastructure would make future changes costly and increase the risk of regressions.

The platform also aims to:

* Support multiple brands
* Allow interchangeable AI providers
* Run both locally and in cloud environments
* Maintain high testability
* Enable long-term maintainability

A clear separation between business logic and infrastructure is therefore essential.

---

# 4. Decision

ForgeFlow AI adopts **Hexagonal Architecture (Ports and Adapters)** as the foundational backend architecture.

Business logic resides at the center of the application and remains independent of frameworks, databases, messaging systems, and AI providers.

External systems interact with the application exclusively through well-defined ports and adapters.

No infrastructure component may directly influence domain behavior.

---

# 5. Architectural Principles

The implementation shall follow these principles:

* Business logic is framework-independent.
* Infrastructure is replaceable.
* Dependencies always point inward.
* Domain code contains no Spring Boot annotations.
* External systems are accessed only through ports.
* Infrastructure implements interfaces defined by the application or domain layers.
* Business rules remain independent of transport protocols and persistence mechanisms.

---

# 6. Logical Structure

```
                External World

        REST API / Events / CLI

                    │

                    ▼

          Inbound Adapters

        (Controllers, Consumers)

                    │

                    ▼

          Application Layer

                    │

                    ▼

             Domain Layer

      (Business Rules & Models)

                    │

          Outbound Ports

                    │

                    ▼

        Outbound Adapters

(PostgreSQL, RabbitMQ, MinIO, Ollama,
 ComfyUI, Whisper, Piper)
```

---

# 7. Benefits

Adopting Hexagonal Architecture provides:

### Business Independence

Business rules remain isolated from infrastructure concerns.

### Replaceable Infrastructure

Changing PostgreSQL, RabbitMQ, or an AI provider requires minimal impact on business logic.

### Improved Testability

Application and domain layers can be tested without external services by substituting adapters with mocks or stubs.

### Technology Flexibility

The platform can adopt new AI providers, messaging systems, or storage technologies without redesigning the core domain.

### Long-Term Maintainability

Clear boundaries reduce coupling and simplify future enhancements.

---

# 8. Consequences

### Positive

* Strong separation of concerns
* Easier unit testing
* Lower coupling
* Better maintainability
* Technology independence
* Improved readability
* Consistent architecture across services

### Negative

* Additional interfaces increase the number of source files.
* New developers may require time to understand the architecture.
* Small features may involve more classes than in layered architectures.
* Slight increase in initial development effort.

These trade-offs are considered acceptable given the expected lifetime and scope of the platform.

---

# 9. Alternatives Considered

## Traditional Layered Architecture

### Advantages

* Familiar to many developers
* Faster for simple CRUD applications

### Reasons Rejected

* High coupling between business logic and infrastructure
* Business rules often leak into controllers or repositories
* Difficult to replace external integrations

---

## Clean Architecture

### Advantages

* Excellent separation of concerns
* Highly testable
* Strong dependency rules

### Reasons Not Selected as the Primary Style

Hexagonal Architecture and Clean Architecture are closely related and compatible. ForgeFlow AI adopts Hexagonal Architecture as the primary implementation model because it emphasizes external integrations through ports and adapters, which aligns well with an AI orchestration platform.

Clean Architecture principles continue to guide dependency direction and layer responsibilities.

---

## Classic Spring MVC Architecture

### Advantages

* Rapid development
* Minimal boilerplate

### Reasons Rejected

* Business logic tends to become tightly coupled with Spring components.
* Infrastructure dependencies spread across the codebase.
* Difficult to evolve into a modular platform.

---

# 10. Implementation Guidelines

Every backend service shall contain, at minimum, the following packages:

```
application/
domain/
infrastructure/
interfaces/
config/
events/
security/
exceptions/
```

Rules:

* Controllers belong to `interfaces`.
* Business use cases belong to `application`.
* Business rules belong to `domain`.
* Repository interfaces belong to `domain` or `application` (depending on ownership).
* Infrastructure implementations belong to `infrastructure`.
* Framework-specific code remains outside the domain layer.

---

# 11. Impact

This decision affects:

* Backend services
* Package structure
* Testing strategy
* Dependency management
* Code reviews
* Future microservices
* AI integration modules

Frontend applications are not affected.

---

# 12. Related Documents

* 02-Container-Architecture.md
* 03-Component-Architecture.md
* 04-Domain-Architecture.md
* 05-Workflow-Architecture.md
* 09-Technology-Decisions.md

---

# 13. Review Criteria

This ADR should be revisited only if:

* The platform fundamentally changes its architectural style.
* Business logic can no longer be isolated using ports and adapters.
* Significant operational complexity outweighs the architectural benefits.

Routine technology changes (e.g., replacing RabbitMQ or PostgreSQL) do not require revisiting this decision.

---

# 14. Decision Summary

ForgeFlow AI adopts Hexagonal Architecture because it provides a stable foundation for a modular, maintainable, and technology-independent AI platform. The architecture supports evolving infrastructure while protecting the integrity of the business domain and aligns with the platform's long-term goal of serving multiple brands, workflows, and AI providers without extensive redesign.

---

# Version History

| Version | Date         | Description |
| ------- | ------------ | ----------- |
| 1.0.0   | 26 June 2026 | Initial ADR |

# Component Architecture (C4 Level 3)

**Project:** ForgeFlow AI

**Document:** 03-Component-Architecture.md

**Architecture Version:** 1.0

**Status:** Draft

**Last Updated:** 26 June 2026

---

# 1. Purpose

This document describes the internal component architecture of the major backend containers within ForgeFlow AI.

The objective is to define clear boundaries between business logic, application orchestration, infrastructure, and external integrations.

ForgeFlow AI follows:

- Clean Architecture
- Hexagonal Architecture (Ports & Adapters)
- Domain-Driven Design (DDD)
- Event-Driven Architecture

These architectural styles are complementary and collectively guide the implementation.

---

# 2. Architectural Principles

Every component shall adhere to the following principles:

- Business logic is independent of frameworks.
- Domain models are framework-agnostic.
- Infrastructure is replaceable.
- External systems are accessed only through ports.
- Components communicate through well-defined interfaces.
- Events are preferred over direct dependencies whenever practical.

---

# 3. Backend Component Model

Each backend service follows the same internal structure.

```
                 REST API / Events
                         │
                         ▼
                 +----------------+
                 | Controllers    |
                 +--------+-------+
                          │
                          ▼
                 +----------------+
                 | Application    |
                 | Services       |
                 +--------+-------+
                          │
          ┌───────────────┼────────────────┐
          ▼               ▼                ▼
+----------------+ +----------------+ +----------------+
| Domain Service | | Event Publisher| | Validation     |
+-------+--------+ +----------------+ +----------------+
        │
        ▼
+-------------------------+
| Domain Model            |
| Entities                |
| Aggregates              |
| Value Objects           |
+-----------+-------------+
            │
            ▼
+-------------------------+
| Repository Interfaces   |
+-----------+-------------+
            │
            ▼
+-------------------------+
| Infrastructure Adapters |
+-------------------------+
            │
            ▼
PostgreSQL / RabbitMQ / Redis / MinIO / Ollama
```

---

# 4. Component Responsibilities

## 4.1 REST Controllers

Responsibilities

- Accept HTTP requests
- Validate request payloads
- Invoke application services
- Return standardized responses

Controllers shall contain no business logic.

---

## 4.2 Application Services

Responsibilities

- Coordinate use cases
- Execute workflows
- Manage transactions
- Publish domain events
- Invoke domain services

Application services orchestrate business processes but do not implement business rules.

---

## 4.3 Domain Services

Responsibilities

- Implement business rules
- Execute domain logic
- Validate domain invariants
- Coordinate aggregates

Domain services must remain independent of infrastructure.

---

## 4.4 Domain Model

The domain model represents the core business knowledge.

It consists of:

- Entities
- Aggregates
- Value Objects
- Domain Events
- Repository Interfaces

This layer must not depend on any external framework.

---

## 4.5 Repository Interfaces

Repositories define persistence contracts.

Examples:

- ProjectRepository
- WorkflowRepository
- AssetRepository
- BrandRepository

Only interfaces exist in the domain layer.

---

## 4.6 Infrastructure Adapters

Infrastructure adapters implement repository interfaces.

Examples include:

- PostgreSQL
- Redis
- RabbitMQ
- MinIO
- Ollama
- ComfyUI
- Whisper
- Piper

Adapters may be replaced without impacting business logic.

---

# 5. Workflow Orchestrator Components

The Workflow Orchestrator is composed of the following components.

```
Workflow API
      │
      ▼
Workflow Service
      │
      ▼
Workflow Engine
      │
      ├──────────────┐
      ▼              ▼
Stage Executor   Event Publisher
      │              │
      ▼              ▼
Task Scheduler  RabbitMQ
      │
      ▼
Worker Dispatcher
```

Responsibilities

- Build execution plan
- Manage stage execution
- Publish events
- Retry failed stages
- Persist workflow state

---

# 6. AI Worker Components

Each AI Worker consists of:

```
Job Consumer
      │
      ▼
Prompt Builder
      │
      ▼
AI Client
      │
      ▼
Response Parser
      │
      ▼
Asset Generator
      │
      ▼
Result Publisher
```

Responsibilities

- Consume jobs
- Generate prompts
- Invoke AI runtime
- Parse outputs
- Store generated assets
- Publish completion events

---

# 7. Asset Service Components

```
Asset Controller
        │
        ▼
Asset Service
        │
        ▼
Metadata Manager
        │
        ▼
Storage Adapter
        │
        ▼
MinIO
```

Responsibilities

- Asset upload
- Asset retrieval
- Metadata persistence
- Version tracking
- Download preparation

---

# 8. Event Components

ForgeFlow AI is event-driven.

Major components include:

- Event Publisher
- Event Consumer
- Event Dispatcher
- Retry Handler
- Dead Letter Handler

Typical events:

- ProjectCreated
- WorkflowStarted
- ScriptGenerated
- ImagesGenerated
- VoiceGenerated
- VideoRendered
- ExportCompleted
- WorkflowFailed

---

# 9. Cross-Cutting Components

Every backend service includes:

### Validation

- Request validation
- Domain validation
- Business validation

---

### Logging

- Structured logging
- Correlation IDs
- Audit logging

---

### Exception Handling

- Global exception mapper
- Domain exceptions
- Infrastructure exceptions

---

### Security

- Authentication
- Authorization
- Role validation

---

### Configuration

- Externalized configuration
- Environment-specific properties
- Feature flags

---

### Metrics

- Prometheus metrics
- Health indicators
- Performance monitoring

---

# 10. Dependency Rules

Dependencies shall always point inward.

Allowed dependency direction:

```
Controller

↓

Application

↓

Domain

↓

Repository Interface

↓

Infrastructure Adapter
```

The reverse direction is prohibited.

---

# 11. Package Structure (Reference)

```
service-name/

src/main/java/

application/
domain/
infrastructure/
interfaces/
config/
events/
exceptions/
security/
```

Each package has a single responsibility.

---

# 12. Design Patterns

The architecture adopts the following patterns:

- Dependency Injection
- Repository Pattern
- Factory Pattern
- Strategy Pattern
- Builder Pattern
- Adapter Pattern
- Observer Pattern
- Command Pattern
- Template Method
- Event Publisher / Subscriber

Patterns are selected based on necessity rather than preference.

---

# 13. Quality Attributes

The component architecture prioritizes:

- Testability
- Replaceability
- Maintainability
- Modularity
- Low Coupling
- High Cohesion
- Extensibility
- Observability

---

# 14. Architectural Decisions

The following decisions are established:

- Domain logic is framework-independent.
- Infrastructure is accessed through ports.
- Components communicate via interfaces.
- Events decouple services.
- Each component has a single responsibility.
- Shared mutable state is avoided.

---

# 15. Traceability

This document refines:

- 01-System-Context.md
- 02-Container-Architecture.md
- Product Principles
- System Requirements

---

# 16. Next Document

The next document is:

**04-Domain-Architecture.md**

It defines the business domain model, bounded contexts, aggregates, entities, value objects, domain services, and business capabilities that drive ForgeFlow AI.

---

# Version History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | 26 June 2026 | Initial Component Architecture |
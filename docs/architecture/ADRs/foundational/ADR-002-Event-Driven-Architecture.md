# ADR-002: Adopt Event-Driven Architecture

**Project:** ForgeFlow AI

**Document:** ADR-002-Event-Driven-Architecture.md

**ADR ID:** ADR-002

**Status:** Accepted

**Date:** 26 June 2026

**Decision Makers:** ForgeFlow AI Architecture Team

---

# 1. Title

Adopt Event-Driven Architecture (EDA) as the primary communication model between backend services and workflow components.

---

# 2. Status

**Accepted**

This decision applies to all asynchronous communication within ForgeFlow AI.

---

# 3. Context

ForgeFlow AI is an AI-powered content production platform where a single workflow consists of multiple long-running stages, including:

* Research
* Fact Validation
* Script Generation
* Storyboard Generation
* Prompt Generation
* Image Generation
* Voice Generation
* Subtitle Generation
* Video Rendering
* Publishing

Many of these operations:

* take several seconds or minutes,
* may execute in parallel,
* may fail independently,
* require retries,
* should resume from checkpoints rather than restart from the beginning.

A traditional synchronous request-response architecture would tightly couple services, reduce scalability, and make recovery from failures difficult.

The platform therefore requires a communication model that supports loose coupling, asynchronous execution, and reliable workflow progression.

---

# 4. Decision

ForgeFlow AI adopts **Event-Driven Architecture (EDA)** as the standard communication model for workflow execution and inter-service collaboration.

Business events represent meaningful domain occurrences and are published whenever significant state transitions occur.

Services react to events instead of directly invoking downstream services whenever asynchronous behavior is appropriate.

Synchronous communication remains acceptable for lightweight queries and user-facing APIs where immediate responses are required.

---

# 5. Architectural Principles

The implementation shall follow these principles:

* Events represent completed business actions.
* Events are immutable after publication.
* Producers do not know their consumers.
* Consumers process events independently.
* Services communicate asynchronously whenever practical.
* Event handlers are idempotent.
* Business workflows are coordinated through events rather than direct service chaining.

---

# 6. Logical Event Flow

```text
User

↓

API Gateway

↓

Workflow Orchestrator

↓

WorkflowStarted

↓

ResearchCompleted

↓

ScriptGenerated

↓

StoryboardGenerated

↓

PromptGenerated

↓

ImagesGenerated

↓

VoiceGenerated

↓

VideoRendered

↓

ExportCompleted

↓

WorkflowCompleted
```

Each event advances the workflow to the next eligible stage.

---

# 7. Event Characteristics

Every domain event shall:

* represent a completed business action,
* include a unique event identifier,
* include a timestamp,
* include a correlation identifier,
* include the workflow identifier,
* include the project identifier,
* remain immutable.

Example metadata:

* Event ID
* Event Type
* Timestamp
* Correlation ID
* Workflow ID
* Project ID
* Event Version

---

# 8. Messaging Strategy

The messaging platform is responsible for:

* asynchronous delivery,
* retry handling,
* message durability,
* routing,
* dead-letter queues,
* back-pressure management.

Initial implementation:

* RabbitMQ

Future alternatives may include:

* Apache Kafka
* NATS
* AWS EventBridge
* Azure Service Bus

The business architecture remains independent of the messaging technology.

---

# 9. Benefits

Adopting Event-Driven Architecture provides:

### Loose Coupling

Services are independent of downstream consumers.

### Scalability

Workers can scale horizontally without affecting publishers.

### Fault Isolation

A failure in one processing stage does not halt unrelated services.

### Parallel Processing

Independent stages may execute concurrently.

### Recoverability

Failed stages can be retried without restarting the entire workflow.

### Extensibility

New consumers can subscribe to existing events without modifying producers.

---

# 10. Consequences

### Positive

* Improved scalability
* Better resilience
* Simplified service evolution
* Higher throughput
* Independent deployments
* Flexible workflow composition

### Negative

* Increased operational complexity
* Eventual consistency instead of immediate consistency
* More challenging debugging
* Additional monitoring requirements
* Duplicate event handling must be considered

These trade-offs are acceptable for a workflow-oriented AI platform.

---

# 11. Alternatives Considered

## Synchronous REST Chaining

### Advantages

* Simple request-response model
* Familiar development experience

### Reasons Rejected

* Tight service coupling
* Poor resilience
* Long-running requests
* Difficult retries
* Limited scalability

---

## Direct Service Calls

### Advantages

* Easy to implement
* Minimal infrastructure

### Reasons Rejected

* Strong runtime dependencies
* Cascading failures
* Difficult service evolution

---

## Scheduled Polling

### Advantages

* Simple implementation

### Reasons Rejected

* High latency
* Inefficient resource usage
* Poor responsiveness
* Unnecessary infrastructure load

---

# 12. Event Design Guidelines

Events should:

* describe something that has already happened,
* use business terminology,
* avoid implementation details,
* remain immutable,
* be versioned when schemas evolve.

Preferred examples:

* WorkflowStarted
* ScriptGenerated
* ImagesGenerated
* VideoRendered
* ExportCompleted

Avoid command-like event names such as:

* GenerateScript
* CreateVideo
* RenderImage

---

# 13. Reliability Considerations

The messaging infrastructure shall support:

* Durable queues
* Persistent messages
* Retry policies
* Dead-letter queues
* Message acknowledgements
* Idempotent consumers

Workflow progress must never depend on in-memory state.

---

# 14. Impact

This decision affects:

* Workflow Orchestrator
* AI Workers
* Asset Service
* Render Service
* Export Service
* Monitoring
* Logging
* Retry Management

Frontend applications remain largely unaffected.

---

# 15. Related Documents

* 02-Container-Architecture.md
* 03-Component-Architecture.md
* 05-Workflow-Architecture.md
* 06-Data-Architecture.md
* ADR-001-Hexagonal-Architecture.md

---

# 16. Review Criteria

This ADR should be revisited only if:

* the platform no longer requires asynchronous workflows,
* a fundamentally different orchestration model is adopted,
* event-driven communication becomes a proven bottleneck.

Routine changes to the messaging technology (e.g., RabbitMQ to Kafka) do not invalidate this decision.

---

# 17. Decision Summary

ForgeFlow AI adopts Event-Driven Architecture because it enables scalable, resilient, loosely coupled, and recoverable workflow execution. By communicating through immutable business events rather than direct service dependencies, the platform supports long-running AI pipelines, parallel processing, and future extensibility while preserving clear architectural boundaries.

---

# Version History

| Version | Date         | Description |
| ------- | ------------ | ----------- |
| 1.0.0   | 26 June 2026 | Initial ADR |

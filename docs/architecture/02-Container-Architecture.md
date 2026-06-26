# Container Architecture (C4 Level 2)

**Project:** ForgeFlow AI

**Document:** 02-Container-Architecture.md

**Architecture Version:** 1.0

**Status:** Draft

**Last Updated:** 26 June 2026

---

# 1. Purpose

This document decomposes ForgeFlow AI into its primary logical containers.

Each container represents an independently deployable or independently maintainable application or service within the platform.

This document intentionally avoids low-level class design and focuses on system decomposition, responsibilities, and communication patterns.

---

# 2. Container Overview

ForgeFlow AI is composed of the following major containers:

```
                    User
                     │
                     ▼
        +---------------------------+
        |      React Frontend       |
        +-------------+-------------+
                      │
                      ▼
          +------------------------+
          |      API Gateway       |
          +-----------+------------+
                      │
         ┌────────────┼─────────────┐
         ▼            ▼             ▼
 +---------------+ +---------------+ +---------------+
 | Project       | | Workflow      | | Asset         |
 | Service       | | Orchestrator  | | Service       |
 +---------------+ +---------------+ +---------------+
                         │
                         ▼
               +----------------------+
               | Message Broker       |
               +----------+-----------+
                          │
        ┌─────────────────┼─────────────────┐
        ▼                 ▼                 ▼
 +---------------+ +---------------+ +---------------+
 | AI Worker     | | Render Worker | | Export Worker |
 +---------------+ +---------------+ +---------------+
        │                 │                 │
        └────────────┬────┴─────────────────┘
                     ▼
          +---------------------------+
          | AI Runtime Layer          |
          +---------------------------+
                     │
         ┌───────────┼────────────┐
         ▼           ▼            ▼
      Ollama     ComfyUI      Whisper/Piper

                     │
                     ▼
              Object Storage
```

---

# 3. Container Responsibilities

## 3.1 React Frontend

Responsibilities

- User Interface
- Authentication
- Dashboard
- Workflow Monitoring
- Asset Preview
- Configuration

Technology

- React
- TypeScript
- Vite

The frontend contains no business logic.

---

## 3.2 API Gateway

Responsibilities

- Routing
- Authentication
- Authorization
- Request Validation
- Rate Limiting
- API Versioning

Technology

- Kong Gateway

The API Gateway is the only public entry point into the platform.

---

## 3.3 Project Service

Responsibilities

- Project lifecycle
- Brand selection
- Metadata
- Project history
- Configuration persistence

Database ownership

Project Database

---

## 3.4 Workflow Orchestrator

Responsibilities

- Execute workflows
- Schedule tasks
- Retry failed jobs
- Maintain workflow state
- Publish events
- Consume events

This is the heart of ForgeFlow AI.

No AI model is called directly by the frontend.

---

## 3.5 Asset Service

Responsibilities

- Asset metadata
- Versioning
- File indexing
- Download
- Preview
- Export preparation

---

## 3.6 Message Broker

Responsibilities

- Decouple services
- Event delivery
- Retry queues
- Dead-letter queues

Technology

RabbitMQ

Future versions may support Kafka.

---

## 3.7 AI Worker

Responsibilities

- Execute AI jobs
- Invoke language models
- Generate prompts
- Generate scripts
- Generate research

Consumes events from RabbitMQ.

---

## 3.8 Render Worker

Responsibilities

- Image composition
- Subtitle rendering
- FFmpeg execution
- Video encoding
- Thumbnail generation

---

## 3.9 Export Worker

Responsibilities

- Package outputs
- Generate ZIP archives
- Build publishing bundles
- Generate delivery manifests

---

## 3.10 AI Runtime Layer

This layer abstracts all AI engines.

Supported runtimes include

- Ollama
- ComfyUI
- Whisper
- Piper

The rest of the platform communicates only with this abstraction layer.

---

## 3.11 Object Storage

Responsibilities

- Images
- Videos
- Audio
- Metadata
- Intermediate assets

Recommended implementation

MinIO

---

# 4. Communication Pattern

The platform follows asynchronous event-driven communication.

```
User

↓

Frontend

↓

API Gateway

↓

Workflow Orchestrator

↓

RabbitMQ

↓

Workers

↓

Object Storage

↓

Asset Service

↓

Frontend
```

The frontend never communicates directly with AI services.

---

# 5. Container Dependencies

| Container | Depends On |
|------------|------------|
| Frontend | API Gateway |
| API Gateway | Backend Services |
| Project Service | PostgreSQL |
| Workflow Orchestrator | RabbitMQ |
| AI Worker | AI Runtime |
| Render Worker | FFmpeg |
| Export Worker | Asset Service |
| Asset Service | MinIO |
| AI Runtime | Ollama / ComfyUI / Whisper / Piper |

---

# 6. Data Ownership

Each container owns its own data.

Shared databases are avoided.

Communication occurs through APIs or events.

This prevents tight coupling.

---

# 7. Deployment Strategy

Each container shall be independently deployable.

Containers may run

- locally
- via Docker Compose
- on Kubernetes

without architectural changes.

---

# 8. Security Boundaries

Public Zone

- React Frontend
- API Gateway

Private Zone

- Backend Services
- RabbitMQ
- PostgreSQL
- Redis
- MinIO

Internal AI Zone

- Ollama
- ComfyUI
- Whisper
- Piper

AI services are never exposed directly to external clients.

---

# 9. Quality Attributes

The container architecture optimizes for

- Scalability
- Loose Coupling
- High Cohesion
- Replaceability
- Fault Isolation
- Independent Deployment
- Testability

---

# 10. Architectural Decisions

The following decisions are established:

- API Gateway is the single entry point.
- Workflow execution is asynchronous.
- Workers are stateless.
- Storage is centralized.
- AI engines remain replaceable.
- Services communicate through events wherever practical.

---

# 11. Traceability

This document realizes the architectural intent established in

- 01-System-Context.md
- Product Principles
- System Requirements

---

# 12. Next Document

The next document is

**03-Component-Architecture.md**

It decomposes each container into internal components including controllers, application services, domain services, repositories, adapters, event publishers, and infrastructure layers.

---

# Version History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | 26 June 2026 | Initial Container Architecture |# Container Architecture

# Deployment Architecture

**Project:** ForgeFlow AI

**Document:** 07-Deployment-Architecture.md

**Architecture Version:** 1.0

**Status:** Draft

**Last Updated:** 26 June 2026

---

# 1. Purpose

This document defines how ForgeFlow AI is deployed across different environments while preserving a consistent architecture.

The deployment architecture ensures that the platform can evolve seamlessly from local development to enterprise-scale production without requiring changes to business logic or application design.

---

# 2. Deployment Principles

ForgeFlow AI follows the following deployment principles:

- Environment-independent architecture
- Container-first deployment
- Infrastructure as Code
- Immutable deployments
- Stateless application services
- Externalized configuration
- Automated deployments
- Horizontal scalability
- Secure-by-default networking

---

# 3. Deployment Topologies

ForgeFlow AI supports the following deployment models.

### 3.1 Local Development

Purpose

Developer productivity.

Characteristics

- Single machine
- Docker Compose
- Local AI models
- Hot reload
- Local object storage
- Local PostgreSQL

Recommended for:

- Feature development
- Debugging
- Unit testing

---

### 3.2 Single Workstation

Purpose

Content creation using a personal workstation.

Characteristics

- GPU acceleration
- Local LLM
- Local image generation
- Local voice synthesis
- Local storage

Recommended for:

- Individual creators
- Offline execution

---

### 3.3 Homelab Deployment

Purpose

Always-on personal AI server.

Characteristics

- Dedicated hardware
- NAS integration
- Shared storage
- Multiple workflows
- Remote dashboard

Recommended for:

- Small teams
- Personal studios

---

### 3.4 Enterprise On-Premise

Purpose

Private enterprise deployment.

Characteristics

- Internal networking
- Central authentication
- High availability
- Enterprise monitoring
- Backup strategy

---

### 3.5 Cloud-Native Deployment

Purpose

Production-scale deployment.

Characteristics

- Kubernetes
- Auto Scaling
- Managed databases
- Object storage
- Monitoring stack
- Service mesh (future)

---

# 4. Runtime Components

A standard deployment contains the following runtime components.

```
Browser

↓

React Frontend

↓

API Gateway

↓

Backend Services

↓

RabbitMQ

↓

Redis

↓

PostgreSQL

↓

MinIO

↓

AI Runtime

↓

GPU
```

Each component remains independently deployable.

---

# 5. Deployment Units

The platform is packaged into independent deployment units.

Examples

- Frontend
- API Gateway
- Workflow Service
- Project Service
- Asset Service
- AI Worker
- Render Worker
- Export Worker
- RabbitMQ
- PostgreSQL
- Redis
- MinIO

Each deployment unit has its own lifecycle.

---

# 6. Containerization Strategy

Every service is packaged as a Docker image.

Container characteristics

- Stateless
- Immutable
- Independently versioned
- Independently deployable

Container images shall contain only runtime dependencies.

---

# 7. Configuration Management

Configuration is externalized.

Examples

- Environment variables
- Configuration files
- Secrets
- Feature flags

Application binaries remain identical across environments.

---

# 8. Networking Architecture

Network zones are separated into logical layers.

```
Internet

↓

Reverse Proxy

↓

Frontend

↓

API Gateway

↓

Application Network

↓

Infrastructure Network

↓

AI Runtime Network
```

Direct access to infrastructure services is prohibited.

---

# 9. Service Discovery

Application services communicate through logical service names rather than fixed IP addresses.

Examples

- workflow-service
- asset-service
- rabbitmq
- postgres
- redis
- minio

This enables flexible deployments across Docker Compose and Kubernetes.

---

# 10. Storage Architecture

Persistent data is stored separately from application containers.

Persistent storage includes:

- PostgreSQL volumes
- MinIO object storage
- Configuration repository
- Log storage

Application containers remain ephemeral.

---

# 11. GPU Resource Allocation

GPU resources are dedicated to AI workloads.

Typical consumers include:

- Ollama
- ComfyUI
- Whisper
- Video Rendering

Business services remain CPU-oriented.

---

# 12. Scaling Strategy

Different components scale independently.

Examples

- AI Workers → Horizontal
- Render Workers → Horizontal
- API Gateway → Horizontal
- Frontend → Horizontal

Database scaling follows its own strategy.

---

# 13. High Availability

Production deployments should support:

- Multiple application instances
- Health checks
- Automatic restarts
- Rolling updates
- Load balancing
- Persistent storage replication

---

# 14. Deployment Pipeline

Deployment flow.

```
Developer

↓

Git Repository

↓

CI Pipeline

↓

Build

↓

Test

↓

Docker Images

↓

Container Registry

↓

Deployment

↓

Health Verification

↓

Production
```

All deployments are automated.

---

# 15. Environment Promotion

Applications move through environments.

```
Development

↓

Integration

↓

Staging

↓

Production
```

Artifacts remain identical across environments.

Only configuration changes.

---

# 16. Logging Architecture

Application logs are centralized.

Sources

- Frontend
- Backend
- Workers
- AI Runtime
- Infrastructure

Logs are correlated using a common Request ID.

---

# 17. Monitoring

The platform exposes:

- Metrics
- Health endpoints
- Workflow statistics
- Queue statistics
- GPU utilization
- Storage usage

Monitoring remains deployment-independent.

---

# 18. Backup Strategy

The deployment architecture protects:

- PostgreSQL
- MinIO
- Configuration
- Workflow Metadata

Backup frequency depends on deployment profile.

---

# 19. Disaster Recovery

Recovery priorities.

1. Restore configuration.
2. Restore PostgreSQL.
3. Restore object storage.
4. Resume workflows from checkpoints.
5. Restart workers.

Because workflows are checkpointed, long-running jobs do not restart from the beginning.

---

# 20. Security Zones

The deployment architecture separates:

Public Zone

- Browser
- Frontend

Application Zone

- API Gateway
- Backend Services

Infrastructure Zone

- PostgreSQL
- Redis
- RabbitMQ
- MinIO

AI Zone

- Ollama
- ComfyUI
- Whisper
- Piper

Only required traffic is permitted between zones.

---

# 21. Deployment Quality Attributes

The deployment architecture prioritizes:

- Portability
- Scalability
- Reliability
- Maintainability
- Observability
- Security
- Disaster Recovery
- Automation

---

# 22. Architectural Decisions

The following decisions are established.

- Containers are the deployment unit.
- Services remain stateless.
- Persistent data is externalized.
- Configuration is externalized.
- Deployments are immutable.
- Infrastructure is replaceable.
- Local and cloud deployments share the same architecture.

---

# 23. Traceability

This document extends:

- 02-Container-Architecture.md
- 05-Workflow-Architecture.md
- 06-Data-Architecture.md
- System Requirements

---

# 24. Next Document

The next document is:

**08-Security-Architecture.md**

It defines authentication, authorization, secret management, encryption, network security, identity boundaries, AI security considerations, and secure operational practices.

---

# Version History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | 26 June 2026 | Initial Deployment Architecture |
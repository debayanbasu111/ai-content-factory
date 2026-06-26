# Data Architecture

**Project:** ForgeFlow AI

**Document:** 06-Data-Architecture.md

**Architecture Version:** 1.0

**Status:** Draft

**Last Updated:** 26 June 2026

---

# 1. Purpose

This document defines the data architecture of ForgeFlow AI.

It describes how business data, workflow state, AI artifacts, generated media, metadata, and operational information are organized, stored, secured, versioned, and retained throughout their lifecycle.

The architecture is designed to support scalability, traceability, reproducibility, and future extensibility while remaining independent of any specific database technology.

---

# 2. Data Architecture Principles

The platform follows the following principles.

- Data ownership is explicit.
- Every service owns its data.
- Generated assets are immutable.
- Metadata is versioned.
- AI outputs are reproducible.
- Storage technology follows data characteristics.
- No shared database between bounded contexts.
- Every important business action is auditable.

---

# 3. Data Domains

ForgeFlow AI manages the following logical data domains.

```
Business Data

Workflow Data

AI Data

Generated Assets

Metadata

Configuration

Audit Data

Operational Data

Analytics
```

Each domain has an independent lifecycle.

---

# 4. Business Data

Business data represents the operational state of the platform.

Examples

- Projects
- Brands
- Users (future)
- Workflow Definitions
- Publishing Packages

Characteristics

- Structured
- Transactional
- Relational
- Long-lived

Recommended Storage

PostgreSQL

---

# 5. Workflow Data

Workflow data records execution progress.

Examples

- Workflow State
- Stage Status
- Retry Count
- Execution History
- Checkpoints
- Queue Status

Characteristics

- Highly transactional
- Frequently updated
- Recoverable

Recommended Storage

PostgreSQL

---

# 6. AI Data

AI-related information includes:

- Prompt Templates
- Prompt Versions
- Generated Prompts
- LLM Requests
- LLM Responses
- Token Statistics
- Model Metadata

Characteristics

- Version-controlled
- Traceable
- Immutable after execution

Recommended Storage

PostgreSQL

Large prompt artifacts may optionally be archived to object storage.

---

# 7. Generated Assets

Generated media includes:

- Images
- Audio
- Videos
- Storyboards
- Subtitle Files
- Carousel Slides
- Thumbnail Images

Characteristics

- Large binary objects
- Immutable
- Versioned
- Downloadable

Recommended Storage

MinIO Object Storage

The database stores references rather than binary content.

---

# 8. Metadata

Metadata describes generated assets.

Examples

- Resolution
- Aspect Ratio
- Duration
- Prompt Version
- AI Model
- Language
- File Size
- Hash
- Creation Time

Metadata remains relational.

Recommended Storage

PostgreSQL

---

# 9. Configuration Data

Configuration includes

- Workflow Definitions
- Brand Rules
- Prompt Templates
- Feature Flags
- AI Model Registry
- Runtime Configuration

Configuration is version-controlled.

Recommended Storage

Git Repository + PostgreSQL

---

# 10. Audit Data

Audit records capture business activities.

Examples

- Workflow Started
- Asset Generated
- Prompt Approved
- Export Downloaded

Audit records are append-only.

They are never updated.

---

# 11. Operational Data

Operational data includes:

- Health Checks
- Worker Status
- Queue Length
- Resource Usage
- Error Statistics

Retention is significantly shorter than business data.

---

# 12. Analytics Data

Future versions may collect

- Generation Time
- AI Performance
- Model Success Rate
- Workflow Duration
- User Productivity
- Content Statistics

Analytics shall never impact transactional workloads.

---

# 13. Data Lifecycle

Typical lifecycle.

```
Topic

↓

Business Data

↓

Workflow State

↓

AI Requests

↓

Generated Assets

↓

Publishing Package

↓

Archive

↓

Retention

↓

Deletion
```

Each transition follows defined retention policies.

---

# 14. Data Ownership

Each bounded context owns its data.

```
Project Context

↓

Project Tables

Workflow Context

↓

Workflow Tables

Asset Context

↓

Asset Metadata

AI Context

↓

Prompt Metadata

Publishing Context

↓

Publishing Tables
```

Cross-context writes are prohibited.

---

# 15. Data Versioning

The following information is versioned.

- Workflow Definitions
- Prompt Templates
- AI Models
- Generated Assets
- Export Packages

Historical versions remain available for reproducibility.

---

# 16. Storage Strategy

| Data Type | Storage |
|-----------|---------|
| Business Data | PostgreSQL |
| Workflow State | PostgreSQL |
| Metadata | PostgreSQL |
| Configuration | Git + PostgreSQL |
| Images | MinIO |
| Videos | MinIO |
| Audio | MinIO |
| Subtitles | MinIO |
| Logs | Log Storage |
| Metrics | Prometheus |

---

# 17. Data Relationships

```
Project
    │
    ▼
Workflow
    │
    ▼
Content
    │
    ▼
Assets
    │
    ▼
Publishing Package
```

Metadata references assets through immutable identifiers.

---

# 18. Data Integrity

Integrity is ensured through

- Primary Keys
- Foreign Keys
- Domain Validation
- Immutable Asset References
- Transaction Boundaries
- Optimistic Locking

---

# 19. Backup Strategy

Business Data

- Daily Incremental
- Weekly Full Backup

Generated Assets

- Object Storage Replication

Configuration

- Git Version Control

Logs

- Time-based Retention

---

# 20. Data Security

Sensitive data is protected through

- Encryption at Rest
- Encryption in Transit
- Access Control
- Signed URLs for Assets
- Secret Management

Binary assets are never publicly accessible by default.

---

# 21. Data Retention

Suggested retention policy.

| Data | Retention |
|--------|-----------|
| Projects | Permanent |
| Workflow History | Permanent |
| Assets | Permanent |
| Logs | 90 Days |
| Metrics | 30 Days |
| Temporary Files | 7 Days |
| Queue Messages | Until Processed |

Retention policies remain configurable.

---

# 22. Quality Attributes

The data architecture prioritizes

- Consistency
- Integrity
- Traceability
- Recoverability
- Scalability
- Security
- Performance
- Reproducibility

---

# 23. Architectural Decisions

The following decisions are established.

- Services own their data.
- Assets remain immutable.
- Metadata is relational.
- Binary objects reside in object storage.
- Configuration is version-controlled.
- Business data and analytics remain separated.

---

# 24. Traceability

This document extends

- 04-Domain-Architecture.md
- 05-Workflow-Architecture.md
- System Requirements

---

# 25. Next Document

The next document is

**07-Deployment-Architecture.md**

It defines runtime topology, deployment environments, infrastructure layout, networking, containerization, orchestration, and production deployment strategies.

---

# Version History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | 26 June 2026 | Initial Data Architecture |
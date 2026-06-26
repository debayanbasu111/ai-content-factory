# Disaster Recovery

**Project:** ForgeFlow AI

**Document:** 11-Disaster-Recovery.md

**Architecture Version:** 1.0

**Status:** MVP

**Last Updated:** 27 June 2026

---

# 1. Purpose

This document defines the Disaster Recovery (DR) strategy for ForgeFlow AI.

The goal is to ensure that critical project data, generated assets, and application services can be restored with minimal downtime in case of hardware failure, software corruption, accidental deletion, or infrastructure failure.

This document focuses on the Minimum Viable Product (MVP). Advanced enterprise disaster recovery capabilities will be introduced in later phases.

---

# 2. Objectives

The Disaster Recovery strategy aims to:

- Protect business data.
- Prevent permanent data loss.
- Restore the platform quickly.
- Minimize downtime.
- Keep backup procedures simple and repeatable.

---

# 3. Recovery Scope

The following components are covered.

| Component | Recovery Required |
|-----------|-------------------|
| PostgreSQL | Yes |
| MinIO Object Storage | Yes |
| Configuration Files | Yes |
| Docker Volumes | Yes |
| Source Code (GitHub) | Yes |
| Generated Assets | Yes |
| RabbitMQ Definitions | Optional |
| Redis Cache | No |

Redis is treated as disposable cache and does not require backup.

---

# 4. Recovery Objectives

## Recovery Time Objective (RTO)

Target time to restore service after a disaster.

| Component | Target |
|-----------|---------|
| Backend Services | < 30 minutes |
| Frontend | < 15 minutes |
| PostgreSQL | < 30 minutes |
| MinIO | < 1 hour |

---

## Recovery Point Objective (RPO)

Maximum acceptable data loss.

| Component | Target |
|-----------|---------|
| PostgreSQL | 24 hours |
| Generated Assets | 24 hours |
| Configuration | Immediate (Git) |

---

# 5. Backup Strategy

## Source Code

- Hosted in GitHub.
- Every merge to `main` is considered a recoverable version.

---

## PostgreSQL

- Daily full backup.
- Backup stored outside the running container.
- Retain last 7 daily backups.

---

## MinIO

Backup includes:

- Images
- Videos
- Audio
- Exports
- Thumbnails

Daily synchronization to backup storage.

---

## Configuration

Configuration files are stored in Git.

Examples:

- Docker Compose
- Environment templates
- Architecture documents
- Scripts

---

# 6. Backup Schedule

| Resource | Frequency |
|-----------|-----------|
| PostgreSQL | Daily |
| MinIO | Daily |
| Docker Volumes | Weekly |
| Git Repository | Continuous |

---

# 7. Disaster Scenarios

The MVP supports recovery from:

- Disk failure
- Docker container failure
- PostgreSQL corruption
- MinIO corruption
- Accidental deletion
- Server replacement
- Operating system reinstall

---

# 8. Recovery Procedure

## Step 1

Provision a clean server.

---

## Step 2

Clone the Git repository.

---

## Step 3

Restore Docker environment.

---

## Step 4

Restore PostgreSQL backup.

---

## Step 5

Restore MinIO data.

---

## Step 6

Start all containers.

---

## Step 7

Verify application health.

---

## Step 8

Resume workflow processing.

---

# 9. Backup Verification

Backups are useful only if they can be restored.

Monthly verification should include:

- Restore PostgreSQL to a test environment.
- Restore MinIO data.
- Verify application startup.
- Validate sample workflows.

---

# 10. Responsibilities

| Activity | Owner |
|-----------|-------|
| Backup Execution | DevOps |
| Backup Verification | Development Team |
| Disaster Recovery Testing | Architecture Team |
| Restore Operations | DevOps |

For MVP, these responsibilities may be performed by the same individual.

---

# 11. Out of Scope (MVP)

The following are intentionally deferred:

- Multi-region replication
- Active-active deployment
- Automatic failover
- Cross-cloud recovery
- Continuous database replication
- Geo-redundant storage

These features will be considered in future enterprise releases.

---

# 12. Architectural Decisions

The following decisions apply:

- GitHub is the primary source of truth for source code.
- PostgreSQL is backed up daily.
- MinIO assets are backed up daily.
- Redis is not backed up.
- Recovery procedures must be documented and repeatable.

---

# 13. Related Documents

- 06-Data-Architecture.md
- 07-Deployment-Architecture.md
- 08-Security-Architecture.md
- ADR-001-Hexagonal-Architecture.md
- ADR-002-Event-Driven-Architecture.md

---

# Version History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | 27 June 2026 | Initial MVP Disaster Recovery Strategy |
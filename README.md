# Sunbird Lern Service

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Target](https://img.shields.io/badge/Target-Java%2011-orange.svg)](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html)
[![Framework](https://img.shields.io/badge/Framework-Play%203.0.5-green.svg)](https://www.playframework.com/)

Sunbird Lern is an enterprise-grade learning infrastructure service designed for high-scale educational ecosystems. It serves as the authoritative engine for managing user identities, orchestrating structured learning journeys, and facilitating data-driven educational workflows within the Sunbird platform.

---

## Core Capabilities

The service provides a comprehensive suite of capabilities categorized into three functional domains:

### 1. Identity & Organization Management (UserOrg)
*   **Identity Lifecycle:** End-to-end management of user accounts, including self-signup, managed users, and bulk onboarding.
*   **Authentication & SSO:** Native support for OpenID Connect (OIDC), Google SSO, and federated identity providers.
*   **RBAC & Governance:** Granular Role-Based Access Control (RBAC) across complex organizational hierarchies and multi-tenant environments.

### 2. Learning Management (LMS)
*   **Batch Orchestration:** Lifecycle management of course batches (Invite-only, Open, and Private) with automated enrollment workflows.
*   **Progress & Tracking:** Real-time tracking of content consumption, assessment scores, and competency-based progress.
*   **Credentialing:** Rule-based engine for the automated generation and issuance of digital certificates and micro-credentials.

### 3. Notification Engine
*   **Multi-Channel Delivery:** Orchestrated delivery of notifications via Email, SMS (via external gateways), and In-App Activity Feeds.
*   **Template Management:** Dynamic, localized template engine for transactional and engagement-based communications.

---

## Architectural Overview

Sunbird Lern implements a **Unified Service Architecture**, providing deployment flexibility to match varying infrastructure requirements:

*   **Consolidated Deployment (Monolithic):** Integrates all functional modules into a single execution unit for simplified operations, reduced latency, and optimized resource utilization.
*   **Distributed Architecture (Microservices):** Supports the independent deployment of functional modules as standalone microservices to facilitate granular scaling and fault isolation in high-traffic environments.

---

## Technical Specification

The platform is engineered using a reactive, non-blocking stack to ensure maximum throughput and resilience:

*   **Runtime Environment:** Java 11 (Long Term Support)
*   **Web Engine:** Play Framework 3.0.5
*   **Reactive Core:** Apache Pekko 1.0.3 (Distributed Actor System)
*   **Language Support:** Scala 2.13.12
*   **Build System:** Maven 3.6.0+
*   **Data Infrastructure:** 
    *   **YugabyteDB:** Distributed SQL database accessed via high-performance **Cassandra drivers**.
    *   **Elasticsearch:** Distributed search and analytics engine for discovery and indexing.
    *   **Redis:** Optional distributed cache for accelerated data retrieval.
*   **Build Orchestration:** Maven 3.6.0+

---

## Infrastructure & Multi-Cloud Support

Designed for cloud neutrality, the service includes native adapters for all major Cloud Storage Providers (CSP):
*   **Microsoft Azure** (Default storage provider)
*   **Amazon Web Services** (S3 Integration)
*   **Google Cloud Platform** (GCS Integration)
*   **Oracle Cloud Infrastructure** (OCI Object Storage)

---

## System Dependencies & External Integrations

To operate at scale, Sunbird Lern requires integration with several infrastructure components and ecosystem services. These are typically configured via environment variables.

For a comprehensive list of required configuration keys and a shell-compatible template, please refer to:
**[Environment Variables Template (scripts/env-variables.example)](scripts/env-variables.example)**

### Infrastructure Components
*   **Primary Persistence:** 
    *   **YugabyteDB:** Distributed SQL database (accessed via Cassandra drivers on port 9042 and Postgres drivers on port 5433).
    *   **Elasticsearch:** Distributed search engine (default port 9200) for indexing and discovery.
*   **Message Broker:** **Apache Kafka** (default port 9092) for asynchronous event processing, telemetry, and certificate issuance requests.
*   **Identity Provider:** **Keycloak** (SSO) for secure authentication and token management.
*   **Object Storage:** **Cloud Storage** (Azure Blob Storage, AWS S3, GCP, or OCI) for storing assets, dials, and certificates.
*   **Caching:** **Redis** (Optional) for performance optimization.
*   **Communication:** **SMTP Server** (e.g., SendGrid) for dispatching email notifications.

### Ecosystem Dependencies
The service interacts with several other Sunbird platform components:
*   **Content & Learning Services:** For course and content metadata retrieval.
*   **Search Service:** For advanced cross-component search capabilities.
*   **Certificate & Dial Services:** For credential management and QR code orchestration.
*   **Telemetry Service:** For platform-wide usage analytics and observation.

---

## Getting Started

### Prerequisites
*   JDK 11
*   Maven 3.6.0+
*   Docker (Optional, for containerized deployment)

### Developer Quick Reference

| Objective | Command |
| :--- | :--- |
| **Standard Build** | `./scripts/build-local.sh --service lern` |
| **Clean Install** | `mvn clean install -P lern -DskipTests` |
| **Local Execution** | `mvn play2:run` (Execute within `modules/lern/service`) |
| **Packaging** | `mvn play2:dist` |

For exhaustive technical documentation, build parameters, and deployment strategies, please consult the:
**[Official Build & Deployment Guide](scripts/README.md)**

---

## Project Structure

The repository is organized into four main functional areas:

### Core (/core)
The foundation of the platform, containing shared libraries and data access adapters:
*   `sunbird-actor-utils`: Common abstractions for Pekko/Akka Actor management.
*   `sunbird-cassandra-utils`: Data access layer for YugabyteDB/Cassandra.
*   `sunbird-es-utils`: Integration layer for Elasticsearch indexing and search.
*   `sunbird-redis-utils`: Caching utilities for Redis.
*   `sunbird-platform-common`: Shared utility classes, exception handling, and telemetry.

### Modules (/modules)
Independent functional components and the unified service implementation:
*   `lern/`: The **Unified Service** that merges all functionalities into a single deployable unit.
*   `lms/`: Learning Management System logic, including course batches and enrollment.
*   `userorg/`: Identity and Organization management services.
*   `notification/`: Notification engine and template management.

### Build & Deployment (/build, /scripts)
Resources for automation and containerization:
*   `build/`: Service-specific **Dockerfiles** and environment configurations.
*   `scripts/`: Centralized orchestration for builds, tests, and Docker image generation.

### Reporting
*   `lern-jacoco-report/`: Consolidated code coverage reports across all modules.

---

## License

This project is licensed under the **MIT License**. For more information, please see the [LICENSE](LICENSE) file.

# FlowPilot — Engineering Specification

**Document Version:** 1.3
**Document Type:** Engineering Specification
**Audience:** Engineering Team, DevOps, QA
**Project Type:** Mobile-first ticketing and workflow management system

**Related Documents:**

- `REQUIREMENTS.md` — product requirements, features, API contracts, and security
- `TEAM_WORKFLOW.md` — team collaboration and development workflow

---

## Requirement Language Conventions

This document uses the following terms consistently:

- **shall / must** — required; a mandatory requirement for Version 1
- **should** — recommended; strongly encouraged but not strictly required
- **may** — optional; permitted but neither required nor discouraged

---

## 1. Engineering Scope

Beyond delivering the product defined in `REQUIREMENTS.md`, FlowPilot also serves as a complete full-stack engineering project. The development process shall expose the team to mobile development, backend API development, relational database design, cloud deployment, CI/CD, DevOps, security, AI integration, testing, documentation, and team-based software engineering practices.

### In-Scope Engineering Components

- REST API backend
- PostgreSQL database schema and migrations
- AWS cloud deployment
- CI/CD pipeline
- Containerization
- Secrets management
- Logging and monitoring
- Automated testing
- API documentation
- Team-based Git workflow (see `TEAM_WORKFLOW.md`)
- Team-based Git workflow (see `TEAM_WORKFLOW.md`)

### Engineering Goals

1. Apply secure authentication, authorization, and data handling practices.
2. Implement a complete engineering workflow covering cloud, CI/CD, DevOps, testing, and documentation.
3. Produce a reproducible, deployable, observable system.

---

## 2. Technology Stack

The following stack has been finalized for Version 1. A single primary tool has been selected per category to eliminate decision overhead during implementation.

| Layer                  | Technology                                            | Purpose / Justification                                                         |
| ---------------------- | ----------------------------------------------------- | ------------------------------------------------------------------------------- |
| Mobile Frontend        | Java, Android SDK, minimum API 26                     | Native Android development with broad device compatibility                      |
| Backend                | Spring Boot 3.x, Java 17+                             | REST API, business logic, dependency injection, Spring Security support         |
| Database               | PostgreSQL 15+                                        | Relational storage, ACID guarantees, indexing, JSONB metadata, full-text search |
| Cloud Compute          | AWS ECS Fargate                                       | Serverless container orchestration for the Dockerized backend                   |
| Managed Database       | AWS RDS for PostgreSQL                                | Managed, backed-up relational database                                          |
| Object Storage         | AWS S3                                                | File attachment storage                                                         |
| Push Notifications     | Firebase Cloud Messaging                              | Mobile push delivery                                                            |
| Server-Side Fan-Out    | AWS SNS                                               | Event distribution to downstream notification consumers                         |
| API Protocol           | REST over HTTPS, JSON                                 | Standard API communication between mobile app and backend                       |
| Authentication         | JWT access tokens with refresh token rotation         | Stateless and mobile-friendly authentication                                    |
| AI Integration         | External LLM API over REST                            | Ticket summarization and next-step recommendation generation                    |
| API Documentation      | OpenAPI 3.0                                           | Machine-readable API documentation and client contract                          |
| Build Tools            | Gradle                                                | Android and Java build automation                                               |
| CI/CD                  | GitHub Actions                                        | Automated build, test, and deployment workflows                                 |
| Containerization       | Docker, Docker Compose                                | Backend container packaging; Compose for local development                      |
| Database Migration     | Flyway                                                | Version-controlled database schema changes                                      |
| Infrastructure as Code | Terraform                                             | Reproducible cloud resource provisioning                                        |
| Secrets Management     | AWS Secrets Manager (runtime), GitHub Secrets (CI/CD) | Secure handling of credentials and API keys                                     |
| Monitoring and Logging | Spring Boot Actuator, AWS CloudWatch                  | Health checks, logs, metrics, and operational visibility                        |
| Version Control        | Git, GitHub                                           | Team collaboration, branching, pull requests, and code review                   |

**Language selection note:** Java is selected for Android development to maintain consistency with the backend language and to support the team's learning goals. Kotlin migration may be considered after Version 1.

---

## 3. DevOps and Deployment Requirements

The project shall incorporate DevOps practices to support automated development, testing, deployment, monitoring, and maintenance.

**Deployment architecture:** a Dockerized Spring Boot backend deployed to AWS ECS Fargate, PostgreSQL on AWS RDS, file storage on AWS S3, infrastructure managed with Terraform, migrations handled by Flyway, and CI/CD handled by GitHub Actions.

| Area                     | Requirement                                                                                                     |
| ------------------------ | --------------------------------------------------------------------------------------------------------------- |
| Version Control          | The project shall use Git and GitHub for source code management.                                                |
| Branching Strategy       | The team shall use a clear branching strategy such as `main`, `develop`, and feature branches.                  |
| Pull Requests            | Code changes shall be reviewed through pull requests before merging.                                            |
| CI/CD Pipeline           | GitHub Actions shall automatically build, test, and deploy relevant application components.                     |
| Backend Containerization | The Spring Boot backend shall be containerized using Docker.                                                    |
| Local Development        | Docker Compose should be used for running the local backend, PostgreSQL, and supporting services.               |
| Environments             | The system shall support separate local, staging, and production environments.                                  |
| Cloud Deployment         | The backend shall be deployed to AWS ECS Fargate.                                                               |
| Database Hosting         | PostgreSQL shall be hosted on AWS RDS for staging and production environments.                                  |
| File Storage             | Uploaded attachments shall be stored using AWS S3.                                                              |
| Infrastructure as Code   | Cloud resources shall be provisioned using Terraform.                                                           |
| Secrets Management       | Runtime secrets shall be stored in AWS Secrets Manager; CI/CD secrets shall be stored in GitHub Secrets.        |
| Database Migration       | Schema changes shall be managed through Flyway.                                                                 |
| Monitoring               | Application health, logs, errors, and metrics shall be monitored using Spring Boot Actuator and AWS CloudWatch. |
| Rollback Strategy        | Deployment shall support rollback to a previous stable version if a release fails.                              |
| Release Documentation    | Each release should include release notes describing features, fixes, and known issues.                         |

### 3.1 CI/CD Pipeline

The CI/CD pipeline shall include the following stages:

1. Checkout source code.
2. Install dependencies.
3. Run backend unit tests.
4. Run backend integration tests.
5. Run Android build checks.
6. Run static analysis or linting.
7. Build Docker image for backend.
8. Push image to a container registry.
9. Deploy to staging.
10. Run smoke tests.
11. Deploy to production after approval.

---

## 4. Testing Requirements

### 4.1 Backend Testing

The backend shall include tests for:

- Authentication and token refresh
- Role-based authorization
- Ticket creation and updates
- Assignment and reassignment
- Status transitions
- Comment creation
- Attachment metadata handling
- Notification event generation
- AI request preparation and redaction
- Automation rule execution

Suggested tools: JUnit, Mockito, Spring Boot Test, and Testcontainers.

### 4.2 Mobile Testing

The Android application shall include tests for:

- Ticket creation form validation
- Ticket board rendering
- Ticket detail view
- Status and priority update interactions
- Login and logout flow
- Notification display behavior

Suggested tools: Android unit tests, Espresso, and Android instrumentation tests.

### 4.3 API Testing

API tests shall verify:

- Request validation
- Authentication requirements
- Authorization behavior
- Response structure
- Pagination
- Error responses
- Status codes

### 4.4 Security Testing

Security testing shall verify that:

- Users cannot access tickets outside their permission scope.
- Unauthorized requests are rejected.
- Invalid tokens are rejected.
- File upload restrictions are enforced.
- Sensitive endpoints require appropriate roles.
- Secrets are not committed to the repository.

### 4.5 DevOps Testing

The CI/CD pipeline shall verify that the system can be built, tested, and deployed consistently. Smoke tests shall confirm that the deployed backend is reachable and that critical endpoints respond correctly.

---

## 5. Engineering Acceptance Criteria

The engineering work shall be considered successful when the following are demonstrable:

1. The backend can be built and run locally using documented instructions.
2. The backend is containerized using Docker.
3. PostgreSQL schema changes are managed through Flyway migration scripts.
4. The CI/CD pipeline builds and tests the project automatically.
5. The backend can be deployed to AWS ECS Fargate.
6. Secrets are stored outside the source code repository.
7. Logs, health checks, and basic metrics are available for monitoring.
8. API endpoints are documented using OpenAPI 3.0.
9. The team uses Git branches, pull requests, and code review (see `TEAM_WORKFLOW.md`).
10. Automated tests cover core backend and mobile functionality.

> For product acceptance criteria (ticket creation, board behavior, notifications, etc.), see `REQUIREMENTS.md`.

---

## 6. Engineering Risks and Constraints

| Risk / Constraint                                | Mitigation                                                                                                            |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------- |
| Cloud deployment may become costly or complex    | Start with minimal AWS resources and use free-tier or low-cost options where possible.                                |
| Real-time features may be difficult to implement | Use near real-time updates through polling or event-driven notifications instead of requiring full WebSocket support. |
| DevOps scope may become too large                | Prioritize Docker, GitHub Actions, RDS/S3 deployment, Flyway migrations, and basic monitoring first.                  |

> For product-level risks (scope creep, AI privacy) see `REQUIREMENTS.md`. For team coordination risks, see `TEAM_WORKFLOW.md`.

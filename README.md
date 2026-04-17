# FlowPilot

FlowPilot is a mobile-first ticketing and workflow management system that helps teams submit, assign, track, and resolve requests from a shared workspace. Built as a full-stack engineering project covering Android, Spring Boot, PostgreSQL, AWS, CI/CD, and optional AI assistance.

---

## Overview

FlowPilot centers on **tickets** — discrete units of work that can be created, assigned, tracked, discussed, escalated, summarized, and resolved by a team. Inspired by board-based collaboration tools such as Monday.com, it is scoped as a focused ticketing system rather than a general-purpose workflow engine.

Key capabilities:

- Mobile-first ticket board with status-grouped cards
- Role-based access control (Requester, Assignee, Manager/Admin, Viewer)
- Threaded comments with @mentions
- File attachments stored in AWS S3
- Push notifications via Firebase Cloud Messaging
- Dashboards with open tickets, resolved tickets, and average resolution time
- Rule-based automation
- Optional AI-assisted ticket summaries and next-step recommendations

---

## Tech Stack

| Layer                  | Technology                                            |
| ---------------------- | ----------------------------------------------------- |
| Mobile Frontend        | Java, Android SDK (min API 26)                        |
| Backend                | Spring Boot 3.x, Java 17+                             |
| Database               | PostgreSQL 15+                                        |
| Cloud Compute          | AWS ECS Fargate                                       |
| Managed Database       | AWS RDS for PostgreSQL                                |
| Object Storage         | AWS S3                                                |
| Push Notifications     | Firebase Cloud Messaging                              |
| Server-Side Fan-Out    | AWS SNS                                               |
| Authentication         | JWT with refresh token rotation                       |
| API Documentation      | OpenAPI 3.0                                           |
| CI/CD                  | GitHub Actions                                        |
| Containerization       | Docker, Docker Compose                                |
| Database Migration     | Flyway                                                |
| Infrastructure as Code | Terraform                                             |
| Secrets Management     | AWS Secrets Manager (runtime), GitHub Secrets (CI/CD) |
| Monitoring             | Spring Boot Actuator, AWS CloudWatch                  |

---

## Documentation

Project specifications are split across three documents:

- **[REQUIREMENTS.md](./REQUIREMENTS.md)** — product requirements, features, API contracts, security, and acceptance criteria
- **[ENGINEERING.md](./ENGINEERING.md)** — technology stack, DevOps, deployment architecture, CI/CD pipeline, and testing
- **[TEAM.md](./TEAM.md)** — team collaboration practices, ownership areas, and development workflow

---

## Getting Started

> The sections below are placeholders. The team will fill these in as the project is scaffolded.

### Prerequisites

- Java 17 or higher
- Android Studio (latest stable) with Android SDK (min API 26)
- Docker and Docker Compose
- PostgreSQL 15+ (local or via Docker Compose)
- AWS CLI (for cloud deployment)
- Terraform (for infrastructure provisioning)
- A Firebase project (for push notifications)

### Clone the Repository

```bash
git clone https://github.com/<org>/<repo>.git
cd <repo>
```

### Backend Setup

```bash
# TODO: add commands for
#  - configuring environment variables / .env file
#  - starting PostgreSQL via Docker Compose
#  - running Flyway migrations
#  - building and running the Spring Boot backend
```

### Mobile App Setup

```bash
# TODO: add steps for
#  - opening the Android project in Android Studio
#  - configuring the backend base URL
#  - setting up the Firebase config file (google-services.json)
#  - running the app on emulator or device
```

### Running Tests

```bash
# TODO: add commands for
#  - backend unit and integration tests
#  - Android unit and instrumentation tests
```

### Deployment

```bash
# TODO: add instructions for
#  - provisioning AWS infrastructure with Terraform
#  - deploying the backend to AWS ECS Fargate
#  - configuring secrets in AWS Secrets Manager
```

---

## Project Status

FlowPilot is currently in active development for Version 1. See [REQUIREMENTS.md](./REQUIREMENTS.md) for the full scope and [ENGINEERING.md](./ENGINEERING.md) for engineering acceptance criteria.

Items listed under "Post-Version 1 Future Enhancements" in [REQUIREMENTS.md](./REQUIREMENTS.md) are explicitly out of scope until Version 1 is complete.

---

## Contributing

This project is built by a team following the practices in [TEAM.md](./TEAM.md). In short:

- Track work through GitHub Issues
- Submit changes through pull requests
- All pull requests require code review before merging
- Follow the Definition of Done: implemented, tested, reviewed, documented if needed, and merged

---

## License

> TODO: add license information (e.g., MIT, Apache 2.0) once decided by the team.
# FlowPilot — Product Requirements

**Document Version:** 1.3
**Document Type:** Product Requirements Specification
**Audience:** Engineering Team, Product Stakeholders
**Project Type:** Mobile-first ticketing and workflow management system

**Related Documents:**

- `ENGINEERING.md` — technology stack, DevOps, deployment, and testing requirements
- `TEAM.md` — team collaboration and development workflow

---

## Requirement Language Conventions

This document uses the following terms consistently:

- **shall / must** — required; a mandatory requirement for Version 1
- **should** — recommended; strongly encouraged but not strictly required
- **may** — optional; permitted but neither required nor discouraged

---

## 1. Project Overview

**FlowPilot** is a mobile-first ticketing and workflow management system inspired by board-based collaboration tools such as Monday.com. The system centers on **tickets** as its core domain entity: discrete units of work that can be created, assigned, tracked, discussed, escalated, summarized, and resolved by a team operating within a shared workspace.

The project is intentionally scoped as a focused ticketing system rather than a complex, general-purpose workflow engine. Its main purpose is to help teams consolidate requests, monitor ticket progress, collaborate through comments, receive event-driven notifications, review basic dashboards, and optionally use AI assistance for ticket summaries and next-step recommendations.

Beyond the application itself, FlowPilot also serves as a complete full-stack engineering project. See `ENGINEERING.md` for engineering scope and `TEAM.md` for team workflow.

---

## 1.1 Scope

### In Scope

- Ticket creation, assignment, tracking, and resolution
- Role-based access control
- Board, Kanban, timeline, workload, and dashboard views
- Filtering and search
- Threaded comments and ticket updates
- File attachments
- In-app and mobile push notifications
- Basic reporting dashboards
- Rule-based automation
- AI-generated ticket summaries and next-step recommendations

### Out of Scope for Version 1

- Multi-tenant organization hierarchies
- Custom workflow DSLs or fully customizable workflow engines
- Billing and subscription management
- Offline-first synchronization
- Advanced third-party productivity integrations beyond email or notification-related services
- Native desktop application support
- Full enterprise analytics or business intelligence features

---

## 2. System Goals

The system aims to achieve the following goals:

1. Provide a centralized mobile workspace for managing team tickets and requests.
2. Allow users to create, assign, update, and resolve tickets efficiently.
3. Improve team visibility through ticket boards, filters, and dashboards.
4. Support team collaboration through comments, mentions, and notifications.
5. Reduce manual triage effort through optional AI-assisted summaries and recommendations.
6. Apply secure authentication, authorization, and data handling practices.

---

## 3. User Roles

| Role            | Description                                        | Main Permissions                                                                                     |
| --------------- | -------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Requester       | User who submits tickets or requests               | Create tickets, view own tickets, comment on own tickets, upload attachments if allowed              |
| Assignee        | User responsible for handling assigned tickets     | View assigned tickets, update status and priority, comment, add attachments, mark ticket as resolved |
| Manager / Admin | User responsible for supervising tickets and users | Full ticket management, assignment, role management, dashboards, automation rules, reporting         |
| Viewer          | User with limited read-only access                 | View permitted tickets, comments, and dashboards without modifying data                              |

Permissions shall be enforced at the API layer through Spring Security and verified through database-level query restrictions to reduce the risk of horizontal privilege escalation.

---

## 4. Core Ticket Workflow

The system follows a ticket lifecycle with controlled state transitions:

1. A requester submits a ticket through the mobile app or ticket form.
2. The ticket enters the board in the **New** state.
3. A manager or admin assigns the ticket to a responsible team member.
4. The assignee works on the ticket and moves it to **In Progress**.
5. If additional information is needed, the ticket may move to **Waiting**.
6. Team members collaborate through comments, updates, and mentions.
7. The assignee marks the ticket as **Resolved** after completing the work.
8. A manager, requester, or authorized user verifies the result and marks the ticket as **Closed**.
9. All state transitions shall be recorded in the ticket status history and activity logs.

### 4.1 Ticket Status Values

| Status      | Meaning                                                         |
| ----------- | --------------------------------------------------------------- |
| New         | Ticket has been submitted but not yet started                   |
| In Progress | Ticket is currently being handled by an assignee                |
| Waiting     | Ticket is waiting for information, approval, or external action |
| Resolved    | Ticket has been completed but not yet formally closed           |
| Closed      | Ticket has been verified and finalized                          |

### 4.2 Ticket Priority Values

| Priority | Meaning                                        |
| -------- | ---------------------------------------------- |
| Low      | Non-urgent issue or request                    |
| Medium   | Normal priority issue or request               |
| High     | Important issue that should be handled soon    |
| Critical | Urgent issue that requires immediate attention |

---

## 5. Ticket Data Model

| Field          | Type                        | Description                                           |
| -------------- | --------------------------- | ----------------------------------------------------- |
| `id`           | UUID                        | Primary key                                           |
| `title`        | VARCHAR(200)                | Short required summary of the request                 |
| `description`  | TEXT                        | Detailed description of the ticket                    |
| `requester_id` | UUID, FK to users           | User who created the ticket                           |
| `assignee_id`  | UUID, FK to users, nullable | User currently responsible for the ticket             |
| `status`       | ENUM                        | `NEW`, `IN_PROGRESS`, `WAITING`, `RESOLVED`, `CLOSED` |
| `priority`     | ENUM                        | `LOW`, `MEDIUM`, `HIGH`, `CRITICAL`                   |
| `due_date`     | TIMESTAMP, nullable         | Expected completion date                              |
| `location`     | VARCHAR(100), nullable      | Office, branch, department, or affected area          |
| `created_at`   | TIMESTAMP                   | System-generated creation timestamp                   |
| `updated_at`   | TIMESTAMP                   | Auto-updated timestamp for the latest modification    |

Related entities such as comments, attachments, notifications, and status history shall reference tickets through `ticket_id` foreign keys.

---

## 6. Core Features

### 6.1 Ticket Board

The primary ticket board shall present tickets as mobile-friendly cards grouped by status: New, In Progress, Waiting, Resolved, and Closed. Each card shall display key information including title, priority, requester, assignee, due date, and status. Visual cues shall indicate urgent and overdue tickets.

Groups should be collapsible to reduce visual load on smaller screens. Users with sufficient permissions shall be able to move tickets between groups to update their status. Board data shall be retrieved through paginated API calls to maintain responsiveness with large ticket volumes. On larger screens, the system may support an optional table-style board view with customizable columns.

### 6.2 Ticket Submission Form

The system shall provide a structured ticket submission form. Required fields shall be validated on both the client side and server side. Optional fields such as attachments, location, and due date shall be clearly marked. After successful submission, the ticket shall appear on the board through automatic refresh, polling, or event-driven updates.

### 6.3 Assignment and Ownership

Managers and admins shall be able to assign tickets to team members. Each ticket shall clearly display the requester and current assignee. Reassignment shall be recorded in the activity log and shall trigger a notification to the new assignee.

### 6.4 Status and Priority Management

Authorized users shall update ticket status and priority through constrained controls. Status transitions shall be validated against an allowed-transitions matrix to prevent invalid state changes, such as moving a Closed ticket directly back to New without an explicit reopen action.

### 6.5 Filtering and Search

Users shall be able to filter and search tickets by assignee, requester, status, priority, due date range, location, and keyword. Keyword search shall use PostgreSQL full-text search on title and description fields. Filters should be composable and may persist across sessions per user.

### 6.6 Comments and Updates

Each ticket shall maintain a chronological comment thread for discussion and updates. Comments shall support @mentions, which trigger notifications to mentioned users. Comments may be editable within a configurable grace period, after which edits shall be restricted to preserve audit integrity. Soft delete may be supported for moderation.

### 6.7 Attachments

Users shall be able to attach files to tickets when permitted by their role. Uploaded files shall be stored in AWS S3. The database shall store file metadata, including file name, file type, size, uploader, upload timestamp, and S3 object reference.

### 6.8 Notifications

The system shall generate notifications for significant ticket events, including:

- Ticket assignment or reassignment
- Status changes on tickets the user owns, follows, or is assigned to
- New comments or @mentions
- Due date approaching
- Ticket overdue
- Critical priority assignment

Notifications shall be delivered through an in-app notification center and through mobile push notifications using Firebase Cloud Messaging. AWS SNS shall be used for server-side event fan-out.

### 6.9 Views and Reports

The system shall support multiple views for ticket visibility:

| View           | Purpose                                             |
| -------------- | --------------------------------------------------- |
| Board View     | Default mobile-friendly card view grouped by status |
| Kanban View    | Status columns with draggable ticket cards          |
| Timeline View  | Tickets plotted against due dates                   |
| Workload View  | Ticket count and priority distribution per assignee |
| Dashboard View | Aggregate metrics and bottleneck visibility         |

Dashboard metrics shall include at minimum:

- Open tickets
- Resolved tickets
- Average resolution time

Additional metrics may include overdue tickets, tickets by priority, tickets by assignee, and throughput over time.

### 6.10 Automation Rules

The system should support simple event-condition-action automation rules configurable by managers or admins. Example automation rules include:

- When a ticket is marked Critical, notify the manager.
- When a ticket is assigned, notify the assignee.
- When a ticket becomes overdue, flag it for follow-up.
- When a ticket status changes to Resolved, move it to the Resolved group.

Automation rules shall execute server-side and shall be logged for traceability.

### 6.11 AI Assistance

Optional AI features may assist users with ticket understanding and triage. AI assistance may include:

- Summarizing long ticket discussions
- Identifying pending actions
- Suggesting next steps based on ticket context
- Flagging possible blockers or recurring delay patterns

AI outputs shall be advisory only. The system shall not automatically close, approve, delete, assign, or modify tickets based solely on AI output. All state changes shall require explicit user confirmation.

Requests sent to external AI services shall exclude passwords, authentication tokens, private user data, and unnecessary attachments. Sensitive ticket content shall be filtered or redacted before transmission where feasible. Users should be able to opt out of AI processing when applicable.

---

## 7. Functional Requirements

| ID    | Requirement                                                                     | Priority |
| ----- | ------------------------------------------------------------------------------- | -------- |
| FR-01 | The system shall allow authenticated users to create tickets.                   | Must     |
| FR-02 | The system shall display tickets on a shared board grouped by status.           | Must     |
| FR-03 | The system shall allow authorized users to assign tickets to team members.      | Must     |
| FR-04 | The system shall allow authorized users to update ticket status and priority.   | Must     |
| FR-05 | The system shall allow users to add comments and updates to tickets.            | Must     |
| FR-06 | The system shall allow users to upload attachments to tickets.                  | Must     |
| FR-07 | The system shall allow users to search and filter tickets by multiple criteria. | Must     |
| FR-08 | The system shall notify users of significant ticket events in near real time.   | Must     |
| FR-09 | The system should provide basic dashboard and reporting views.                  | Should   |
| FR-10 | The system shall maintain complete activity history for each ticket.            | Must     |
| FR-11 | The system shall enforce role-based access control on all protected operations. | Must     |
| FR-12 | The system may provide AI-generated ticket summaries and recommendations.       | Could    |
| FR-13 | The system should support configurable automation rules.                        | Should   |
| FR-14 | The system shall provide standardized REST API responses and error messages.    | Must     |
| FR-15 | The system shall support pagination for ticket list endpoints.                  | Must     |
| FR-16 | The system shall store uploaded file metadata and storage references.           | Must     |
| FR-17 | The system should maintain notification read/unread state.                      | Should   |
| FR-18 | The system should allow managers and admins to manage users and roles.          | Should   |

---

## 8. Non-Functional Requirements

| Category        | Requirement                                                                                                                                                                        |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Usability       | Core actions such as create ticket, view ticket, and update ticket shall be reachable within three taps from the home screen.                                                      |
| Performance     | Under normal network conditions, target response time shall be under 500ms for single-ticket retrieval and under 1s for paginated list queries at the 95th percentile.             |
| Security        | All API traffic shall use TLS 1.2+; passwords shall be hashed using bcrypt; authorization shall be enforced per endpoint.                                                          |
| Token Security  | Authentication tokens shall be stored in secure on-device storage using the Android Keystore; refresh tokens shall rotate on use and shall expire after a fixed inactivity period. |
| Reliability     | The system shall target 99.5% uptime for deployed services; ticket data shall be backed up daily with point-in-time recovery.                                                      |
| Scalability     | The backend shall be designed to support future horizontal scaling as user and ticket volume grows.                                                                                |
| Maintainability | Code shall follow Google Java Style; REST APIs shall be documented with OpenAPI 3.0.                                                                                               |
| Privacy         | Ticket content sent to external AI services shall exclude credentials, tokens, and personally identifiable information where feasible.                                             |
| Observability   | The system shall expose health checks, logs, and basic metrics for operations monitoring.                                                                                          |
| Portability     | The backend shall be containerized to support consistent local, staging, and production deployment.                                                                                |

---

## 9. Database Requirements

### 9.1 Main Database Entities

The primary relational schema shall include:

- `users` — account credentials, profile information, and role association
- `roles` — role definitions and permission sets
- `tickets` — core ticket records
- `ticket_comments` — threaded comments linked to tickets
- `ticket_attachments` — file metadata with S3 object references
- `ticket_status_history` — audit log of status transitions
- `notifications` — user-facing notification records
- `automation_rules` — configurable event-condition-action definitions
- `activity_logs` — system-wide audit trail
- `refresh_tokens` — refresh token records for secure session management

### 9.2 Database Design Requirements

- Foreign keys shall enforce referential integrity.
- Indexes shall be defined on frequently queried fields such as `status`, `assignee_id`, `requester_id`, `priority`, and `due_date`.
- Ticket search shall support keyword search on title and description using PostgreSQL full-text search.
- Database schema changes shall be managed through Flyway migration scripts.
- Audit and activity logs shall preserve important user and system actions.

---

## 10. API Requirements

### 10.1 API Groups

| API Group       | Base Path                       | Purpose                                            |
| --------------- | ------------------------------- | -------------------------------------------------- |
| Authentication  | `/api/auth`                     | Login, logout, token refresh, session handling     |
| User Management | `/api/users`                    | User CRUD and role assignment                      |
| Tickets         | `/api/tickets`                  | Ticket CRUD, assignment, status updates, filtering |
| Comments        | `/api/tickets/{id}/comments`    | Ticket discussion threads                          |
| Attachments     | `/api/tickets/{id}/attachments` | File upload and retrieval                          |
| Notifications   | `/api/notifications`            | Alert delivery and read-state management           |
| Dashboard       | `/api/dashboard`                | Aggregate metrics and reporting data               |
| Automation      | `/api/automation-rules`         | Automation rule configuration and management       |
| AI Services     | `/api/ai`                       | Ticket summaries and recommendation endpoints      |
| Health          | `/api/health`                   | Application health and readiness checks            |

### 10.2 API Response Requirements

All API endpoints shall:

- Return JSON responses.
- Use standard HTTP status codes.
- Return standardized error envelopes.
- Support pagination where applicable.
- Validate input on the server side.
- Enforce authentication and authorization for protected operations.
- Be documented using OpenAPI 3.0.

---

## 11. Security Requirements

The system shall implement the following security controls:

| Area                 | Requirement                                                                                         |
| -------------------- | --------------------------------------------------------------------------------------------------- |
| Authentication       | Users shall authenticate before accessing protected resources.                                      |
| Password Storage     | Passwords shall be hashed using bcrypt.                                                             |
| Authorization        | Role-based access control shall be enforced on all protected endpoints.                             |
| Transport Security   | All client-server communication shall use HTTPS with TLS 1.2+.                                      |
| Token Handling       | Access tokens shall be short-lived; refresh tokens shall rotate on use and expire after inactivity. |
| Mobile Token Storage | Tokens shall be stored using the Android Keystore.                                                  |
| Input Validation     | All client-provided input shall be validated on the backend.                                        |
| File Upload Security | File type, size, and content restrictions shall be applied to uploaded files.                       |
| Secrets Management   | API keys, database credentials, JWT secrets, and AI service keys shall not be hardcoded.            |
| Audit Logging        | Security-sensitive actions shall be logged.                                                         |
| AI Data Protection   | AI requests shall exclude credentials, tokens, and unnecessary private data.                        |

---

## 12. AI Integration Requirements

### 12.1 AI Use Cases

The AI integration may support the following optional use cases:

- Generate a concise summary of a ticket.
- Summarize long comment threads.
- Identify unresolved questions or pending actions.
- Recommend possible next steps.
- Identify possible blockers or delay patterns.

### 12.2 AI Processing Rules

The system shall follow these rules when using AI:

- AI output shall be advisory only.
- AI shall not directly modify ticket state.
- AI shall not automatically assign, close, delete, or approve tickets.
- AI prompts shall include only the minimum necessary ticket context.
- Sensitive information shall be redacted where feasible.
- AI results shall be reviewable by the user before being acted upon.

### 12.3 AI Logging and Evaluation

The system should record AI request metadata for debugging and evaluation, excluding sensitive prompt contents where necessary. The team may evaluate AI usefulness based on summary accuracy, user feedback, and reduction of manual triage time.

---

## 13. Product Acceptance Criteria

The product shall be considered successful when the following are demonstrable:

1. Authenticated users can create tickets through the mobile app.
2. Submitted tickets appear on the shared board through automatic refresh, polling, or event-driven updates.
3. Authorized users can assign tickets to specific team members.
4. Users can update ticket status, priority, and comments.
5. Users can filter and search tickets by documented criteria.
6. Users can upload attachments to tickets.
7. The system records status history and activity history for every ticket.
8. Users receive notifications for documented ticket events.
9. Managers can view dashboard metrics including open tickets, resolved tickets, and average resolution time.
10. Role-based permissions are correctly enforced at the API layer.
11. If implemented, AI features produce summaries and suggestions without triggering automatic state changes.

> For engineering acceptance criteria (deployment, CI/CD, containerization, testing coverage), see `ENGINEERING.md`.

---

## 14. Product Risks and Constraints

| Risk / Constraint                                  | Mitigation                                                                                                            |
| -------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| Scope creep from adding too many workflow features | Keep Version 1 focused on ticketing, assignment, status tracking, comments, notifications, and basic dashboards.      |
| AI integration may introduce privacy risks         | Redact sensitive data and make AI features optional and advisory only.                                                |
| Real-time features may be difficult to implement   | Use near real-time updates through polling or event-driven notifications instead of requiring full WebSocket support. |

> For engineering risks (cloud cost, DevOps scope) see `ENGINEERING.md`. For team risks, see `TEAM.md`.

---

## 15. Post-Version 1 Future Enhancements

The following enhancements are **explicitly excluded from Version 1** and may be considered only after Version 1 is complete and accepted:

- Offline-first support
- Advanced analytics dashboard
- Custom workflow builder
- Multi-organization support
- Calendar integration
- Email integration beyond basic notification use
- Advanced automation builder
- AI-powered ticket classification
- AI-powered duplicate ticket detection
- SLA tracking and escalation policies
- Web dashboard for managers and administrators
- Kotlin migration for the Android frontend

Team members shall treat these items as out of scope during Version 1 development.

---

## 16. Summary

FlowPilot is a focused mobile-first ticketing and workflow management system that enables teams to submit, assign, track, discuss, and resolve tickets in a centralized workspace. The application prioritizes a clear ticket board, simple status tracking, role-based assignment, threaded discussions, event-driven notifications, dashboards, automation, and optional AI assistance. Engineering and team workflow specifications are maintained separately in `ENGINEERING.md` and `TEAM.md` respectively.

# FlowPilot — Team Collaboration and Development Workflow

**Document Version:** 1.3
**Document Type:** Team Charter and Workflow Specification
**Audience:** Project Team Members
**Project Type:** Mobile-first ticketing and workflow management system

**Related Documents:**

- `REQUIREMENTS.md` — product requirements, features, API contracts, and security
- `ENGINEERING.md` — technology stack, DevOps, deployment, and testing requirements

---

## Requirement Language Conventions

This document uses the following terms consistently:

- **shall / must** — required; a mandatory requirement for Version 1
- **should** — recommended; strongly encouraged but not strictly required
- **may** — optional; permitted but neither required nor discouraged

---

## 1. Overview

FlowPilot is built by a team. This document defines how the team collaborates, assigns ownership, reviews code, documents decisions, and coordinates delivery. These practices complement the product specification in `REQUIREMENTS.md` and the technology choices in `ENGINEERING.md`.

---

## 2. Collaboration Practices

The following collaboration practices shall be followed:

| Area               | Requirement                                                                          |
| ------------------ | ------------------------------------------------------------------------------------ |
| Issue Tracking     | GitHub Issues shall be used to track tasks, bugs, and enhancements.                  |
| Task Assignment    | Each development task shall have an assigned owner.                                  |
| Pull Requests      | Each feature or fix shall be submitted through a pull request.                       |
| Code Review        | Pull requests shall be reviewed before merging.                                      |
| Documentation      | Setup, API, deployment, and usage instructions shall be documented.                  |
| Commit Practices   | Commits should be clear and descriptive.                                             |
| Sprint Planning    | The team may organize work into milestones or sprints.                               |
| Definition of Done | A task is done when implemented, tested, reviewed, documented if needed, and merged. |

---

## 3. Suggested Team Areas

The team may divide responsibilities into the following areas. A single member may cover more than one area, and areas may be rotated during the project.

- Mobile frontend development
- Backend API development
- Database and migrations
- Cloud and DevOps
- Security and authentication
- AI integration
- Testing and QA
- Documentation and project management

---

## 4. Version 1 Scope Discipline

The Version 1 scope is defined in `REQUIREMENTS.md`. Items explicitly listed as "Post-Version 1 Future Enhancements" shall be treated as out of scope during Version 1 development, even when a team member has the capacity to build them early. Scope changes during Version 1 shall be discussed and approved by the team before work begins.

---

## 5. Team Risks and Constraints

| Risk / Constraint        | Mitigation                                                                |
| ------------------------ | ------------------------------------------------------------------------- |
| Team coordination issues | Use GitHub Issues, assigned tasks, pull requests, and milestone planning. |
| Uneven workload          | Distribute responsibilities across the team areas listed in Section 3.    |
| Unclear ownership        | Every task shall have a single assigned owner before work begins.         |

> For product and engineering risks, see `REQUIREMENTS.md` and `ENGINEERING.md`.

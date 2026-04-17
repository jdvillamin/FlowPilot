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

## 3. Commit Policy

The team shall follow a clear and consistent commit policy to make the project history understandable and easier to review.

### Commit Message Format

Commit messages should use the following format:

`type: short description`

Examples:

- `feat: add ticket creation form`
- `fix: resolve login redirect issue`
- `docs: update requirements document`
- `test: add unit tests for ticket service`
- `refactor: simplify ticket status validation`
- `chore: configure project dependencies`

### Commit Types

| Type       | Purpose                                             |
| ---------- | --------------------------------------------------- |
| `feat`     | Adds a new feature                                  |
| `fix`      | Fixes a bug                                         |
| `docs`     | Updates documentation only                          |
| `test`     | Adds or updates tests                               |
| `refactor` | Improves code structure without changing behavior   |
| `style`    | Changes formatting only                             |
| `chore`    | Updates configuration, dependencies, or setup files |
| `ci`       | Updates CI/CD pipeline configuration                |
| `db`       | Updates database schema, migrations, or seeders     |

### Commit Guidelines

- Commits should be small and focused.
- Each commit should represent one logical change.
- Commit messages should be written in the imperative style when possible.
- Avoid vague messages such as `update`, `fix`, `changes`, or `final`.
- Do not commit generated files, build outputs, environment files, credentials, tokens, or secrets.
- Code should be committed only after it has been tested locally when applicable.
- Documentation changes should be committed separately from major code changes when possible.

### Examples of Good Commit Messages

- `feat: implement ticket status update endpoint`
- `fix: prevent duplicate ticket submission`
- `docs: add API specification for tickets`
- `db: create ticket and user migration tables`
- `test: add backend tests for ticket creation`
- `ci: add GitHub Actions workflow for backend tests`

### Examples of Bad Commit Messages

- `update`
- `fixed`
- `changes`
- `done`
- `final na talaga`
- `asdf`

---

## 4. Branching Policy

The team shall follow a consistent branching policy to keep development organized, reduce merge conflicts, and protect the main branch from incomplete or unreviewed changes.

### Main Branch

The `main` branch shall represent the latest stable version of the project. Direct commits to `main` should be avoided. Changes shall be merged into `main` through reviewed pull requests.

### Working Branches

Each feature, fix, or documentation update should be developed in a separate branch. Branch names should identify the owner and the task being worked on.

Recommended branch naming format:

`username/task-description`

Examples:

- `villamin/ticket-creation`
- `villamin/update-requirements`
- `membername/login-redirect-fix`
- `membername/create-ticket-migration`
- `membername/github-actions-setup`

Branch names should be written in lowercase and should use hyphens instead of spaces.

### Pull Request Workflow

Before merging into `main`, the following steps should be completed:

1. Create a branch from the latest `main`.
2. Implement the assigned task.
3. Commit changes using the commit policy in Section 3.
4. Push the branch to GitHub.
5. Open a pull request.
6. Request review from at least one team member.
7. Address comments or requested changes.
8. Merge only after review and successful checks, when applicable.

### Branch Cleanup

After a pull request is merged, the related working branch should be deleted to keep the repository organized.

---

## 5. Suggested Team Areas

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

## 6. Version 1 Scope Discipline

The Version 1 scope is defined in `REQUIREMENTS.md`. Items explicitly listed as "Post-Version 1 Future Enhancements" shall be treated as out of scope during Version 1 development, even when a team member has the capacity to build them early. Scope changes during Version 1 shall be discussed and approved by the team before work begins.

---

## 7. Team Risks and Constraints

| Risk / Constraint        | Mitigation                                                                |
| ------------------------ | ------------------------------------------------------------------------- |
| Team coordination issues | Use GitHub Issues, assigned tasks, pull requests, and milestone planning. |
| Uneven workload          | Distribute responsibilities across the team areas listed in Section 3.    |
| Unclear ownership        | Every task shall have a single assigned owner before work begins.         |

> For product and engineering risks, see `REQUIREMENTS.md` and `ENGINEERING.md`.

# Definition of Ready (DoR)

**Project:** Enterprise Application Project (EAP)  
**Version:** 1.0  
**Date:** 13-01-2026  

---

## Purpose

A backlog item is **Ready** when the team agrees that it can be planned and worked on in a Sprint.

---

## Clarity

- **The goal of the item is clear**
  - What are we building? (One sentence)
  - Why does it matter? (Link to FR-XXX or business need)
  - Who is it for? (Requester/Approver/Admin)

- **The value for the product or user is understood**
  - Business justification is clear
  - User benefit is identified

- **Acceptance criteria are written and understandable**
  - Happy path: What should work when everything goes right
  - Key error cases (optional): What happens if main businessflow fails
  - Testable: Can be verified manually or with automated test

---

## Scope

- **The item is small enough to be completed within one Sprint**
  - Can be completed in one sprint by one developer
  - If bigger, break it down into smaller tasks

- **Dependencies are identified**
  - List what this depends on (other tasks, APIs, data)
  - Note if something must be done first
  - If blocked: Note it and move to another task

- **Open questions are known and discussed**
  - If unclear: 15-minute chat with development teammates
  - Get answers quickly before starting work

---

## Feasibility (including DevOps)

- **The team understands in general how the work can be done**
  - Quick note on approach (e.g., "Add FastAPI endpoint", "Create React form component")
  - Reference similar existing code if available
  - Skip for trivial tasks (e.g., "Add logout button")

- **Required environments, services, or integrations are known**
  - API endpoints exist OR are being built in parallel
  - Database schema ready OR migration planned
  - For frontend + backend: Write quick API example: `POST /api/v1/requests {title, description}`
  - Coordinated work between Frontend asignee and Backend asignee

- **Needed access, secrets, or configuration are identified**
  - Note any special requirements (new API keys, database access)
  - No hard blockers (missing access, broken environment)

---

## Quality & Testing

- **The team knows how the work can be tested**
  - Manual test: You can click through and it works
  - Key scenarios: Main use case + one error case

- **Relevant test types are considered (for example: unit or integration tests)**
  - Automated tests: Add for main businessflow cases
  - For MVP: Manual testing is good for verification approach
  - Automate critical paths only (login, create request, approve)

---

## Delivery Awareness

- **The team understands how this item will be delivered through the pipeline**
  - For MVP: Deploy all together at end of sprint
  - No deployment plan needed per task

- **No known blocking issues in CI/CD or deployment exist**
  - Build pipeline can handle the changes
  - If CI/CD issue exists, note it and coordinate with DevOps lead

---

## Agreement

- **The Product Owner agrees the item is ready**
  - PO reviews before/at sprint planning prioritized user stories
  - Development Team can approve smaller tasks for separation of work

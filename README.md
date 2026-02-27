# eap-architecture

Architecture hub for the **Enterprise Application Project (EAP)** — a web-based employee request management system. This repository is the single source of truth for platform decisions, API contracts, requirements, and deployment documentation across all EAP services.

---

## What is EAP?

EAP is a web application that lets employees submit and track requests for hardware, software access, and services. Requests flow through a structured approval workflow involving three roles:

- **Requester (Employee)** — submits and tracks requests
- **Approver (Manager/Team Lead)** — reviews and approves or rejects requests
- **Admin (System Administrator)** — fulfills approved requests, manages users and configuration

---

## Repository Ecosystem

EAP uses a polyrepo (multi-repository) strategy (see [ADR-001](decisions/ADR-001-repository-strategy.md)):

| Repository | Description | Link |
|---|---|---|
| **eap-architecture** *(this repo)* | Platform decisions, API specs, system diagrams | [GitHub](https://github.com/Hacktivist-Hive-organization/eap-architecture) |
| **eap-backend** | Python / FastAPI backend application | [GitHub](https://github.com/Hacktivist-Hive-organization/eap-backend) |
| **eap-frontend** | React frontend application | [GitHub](https://github.com/Hacktivist-Hive-organization/eap-frontend) |
| **eap-qa** | Playwright E2E and pytest API test automation | [GitHub](https://github.com/Hacktivist-Hive-organization/eap-qa) |

---

## Tech Stack

### Backend
- **Language:** Python 3.11+
- **Framework:** FastAPI with Uvicorn
- **Database:** PostgreSQL
- **ORM / Migrations:** SQLAlchemy 2.0 + Alembic
- **Auth:** JWT (python-jose + passlib/bcrypt)
- **Validation:** Pydantic 2.0+

### Frontend
- **Framework:** React 19 + TypeScript (strict)
- **UI / Styling:** shadcn/ui + Tailwind CSS (Radix UI primitives)
- **State Management:** Redux Toolkit + RTK Query
- **Routing:** React Router v7
- **Build:** Vite + Rolldown
- **Linting / Formatting:** Biome

### QA & Testing
- **E2E / UI:** Playwright (Python)
- **API:** pytest
- **Backend unit/integration:** pytest + pytest-cov

### DevOps & Infrastructure
- **Containerization:** Docker + Docker Compose
- **CI/CD:** GitHub Actions
- **Hosting:** TransIP VPS (Ubuntu 24.04)
- **Reverse Proxy:** Nginx
- **Registry:** GitHub Container Registry

### Design
- **Prototyping:** Stitch AI → Figma ([Figma project](https://www.figma.com/design/61giweY4P8ADs2sE8ib8Vl/EAP-Desk-X))

---

## Repository Contents

```
eap-architecture/
├── api-specs/
│   └── api-specification-v1.md      # REST API contract (source of truth for all services)
├── decisions/
│   ├── README.md                    # ADR index and guidelines
│   ├── template.md                  # Template for new ADRs
│   ├── ADR-001-repository-strategy.md
│   ├── ADR-002-backend-tech-stack.md
│   ├── ADR-003-frontend-tech-stack.md
│   ├── ADR-004-qa-tech-stack.md
│   ├── ADR-005-devops-tech-stack.md
│   └── ADR-006-ui-ux-design-stack.md
├── definitions/
│   ├── definition-of-done.md
│   └── definition-of-ready.md
├── deployments/
│   └── full app deployments/
│       └── local (development)/     # Local dev setup guides with Docker + Nginx
└── requirements/
    ├── functional-requirements.md   # FR document (all feature specs)
    └── non-functional-requirements.md
```

---

## Architecture Decision Records (ADRs)

All cross-cutting platform decisions are documented here. See the full index in [decisions/README.md](decisions/README.md).

| ADR | Decision | Status |
|---|---|---|
| [ADR-001](decisions/ADR-001-repository-strategy.md) | Polyrepo strategy (4 repositories) | Accepted |
| [ADR-002](decisions/ADR-002-backend-tech-stack.md) | Python + FastAPI + PostgreSQL | Accepted |
| [ADR-003](decisions/ADR-003-frontend-tech-stack.md) | React 19 + shadcn/ui + Redux Toolkit | Accepted |
| [ADR-004](decisions/ADR-004-qa-tech-stack.md) | Playwright + pytest | Accepted |
| [ADR-005](decisions/ADR-005-devops-tech-stack.md) | Docker + GitHub Actions + TransIP VPS | Accepted |
| [ADR-006](decisions/ADR-006-ui-ux-design-stack.md) | Stitch AI + Figma | Accepted |

---

## API Contract

The [api-specs/api-specification-v1.md](api-specs/api-specification-v1.md) is the **shared contract** between backend and frontend teams.

**Workflow:**
1. Discuss API change → update the spec in this repo first
2. Backend implements according to spec
3. Frontend generates TypeScript types from spec
4. QA validates against spec

Base URL: `/api/v1` | Auth: `Authorization: Bearer <token>`

---

## Git Workflow

All EAP repositories follow the same conventions (see [ADR-001](decisions/ADR-001-repository-strategy.md)):

**Branch naming:** `<type>/eap-XXX-short-description`
```
feature/eap-123-add-access-request-endpoint
bugfix/eap-456-crash-on-login
```

**PR title:** `EAP-XXX: description`
```
EAP-123: Add access request endpoint
```

**Branch protection:**
- `main` — requires 2 PR reviews + passing CI, no direct commits
- `dev` — requires 1 PR review + passing CI, no direct commits

**Versioning:** Semantic versioning tagged at sprint boundaries (`v0.1.0`, `v0.2.0`, ..., `v1.0.0`), synchronized across all repos.

---

## Local Development Setup

See the step-by-step guide in [deployments/full app deployments/local (development)/](deployments/full%20app%20deployments/local%20%28development%29/backend_and_frontned_use_dev_servres_and_nginx_forwards_to_frontend/instructions.md) for running all services locally with Docker and Nginx.

Quick overview:
1. Create a shared Docker network (`eap-docker-net`)
2. Run PostgreSQL container
3. Build and run FastAPI backend container (port 8000)
4. Build and run React/Vite frontend container (port 5173)
5. Configure Nginx to reverse-proxy to the frontend

---

## Adding a New ADR

1. Copy [decisions/template.md](decisions/template.md)
2. Name it `ADR-XXX-descriptive-name.md` (next sequential number)
3. Fill in: Status, Context, Decision, Consequences
4. Add an entry to the [decisions/README.md](decisions/README.md) index
5. Commit: `docs: add ADR-XXX [title]`

**Where does the ADR belong?**
- Affects multiple repos → `eap-architecture` (here)
- Specific to one component → that component's own repo

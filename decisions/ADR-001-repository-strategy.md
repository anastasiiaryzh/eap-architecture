# ADR-001: Repository Strategy

Date: 2026-01-09

## Status

Accepted

## Context

Need to decide on repository organization strategy for the Employee Application Platform (EAP). The decision impacts code management, CI/CD setup, team collaboration, and deployment workflows.

Key considerations:
- Multiple services (backend, frontend)
- Shared dependencies and contracts
- Team size and structure
- Deployment independence
- Code reusability

## Decision

We will use a **multi-repository (polyrepo) structure** with four separate repositories:

1. **eap-architecture** - Platform decisions, API specs, system diagrams
2. **eap-backend** - Python/FastAPI backend application
3. **eap-frontend** - React frontend application
4. **eap-qa** - Test automation and QA documentation

Each repository will use Git Flow with `main` (production-ready) and `dev` (integration) branches,  `feature/eap-XXX-description` branches for development.

### Repository References
- eap-backend - https://github.com/AbeerAlkhouri/eap-backend
- eap-frontend - https://github.com/ReemaSho-hub/eap-frontend
- eap-qa - https://github.com/funda-it31/eap-qa
- eap-architecture - https://github.com/anastasiiaryzh/eap-architecture

### Rationale

1. **Team Experience** - The team is familiar with single-repository workflows, and separate repos provide clear boundaries similar to their experience
2. **DevOps Simplicity** - Separate CI/CD pipelines per component allow for independent builds and deployments
3. **Clear Ownership** - Each repository has a clear primary owner (Backend developers, Frontend developers, QA lead, Product Owner)
4. **Deployment Independence** - Backend can deploy without triggering frontend builds, reducing unnecessary CI/CD overhead

### Repository Responsibilities

**eap-architecture:**
- Platform decisions (tech stack, infrastructure, tooling)
- API specifications (OpenAPI/Swagger)
- Architecture diagrams (C4 model)
- Deployment documentation
- Cross-repository coordination documentation
- Owner: Product Owner (primary), DevOps lead (deployment docs), Whole team (ADRs)

**eap-backend:**
- Backend application code
- Backend-specific tests (unit, integration)
- Database migrations
- Backend-specific ADRs (schema design, ORM patterns)
- Owner: Backend developers, DevOps lead (deployment config)

**eap-frontend:**
- Frontend application code
- Frontend-specific tests
- Frontend-specific ADRs (state management, component patterns)
- Component documentation
- Owner: Frontend developers, DevOps lead (deployment config)

**eap-qa:**
- End-to-end tests
- Performance tests (load tests, stress tests)
- Test plans and documentation
- QA-specific ADRs (testing approach, tooling)
- Test data and fixtures
- Owner: QA/Test automation lead, Whole team (test coverage)

## Consequences

### Positive

- **Deployment Independence**: Backend can deploy without triggering frontend builds, improving deployment speed
- **Clear Boundaries**: Each repository has a clear purpose and ownership structure
- **Simple CI/CD**: Each repository has its own GitHub Actions workflows, easier to maintain and debug
- **Faster Builds**: Smaller repositories mean faster clone times and CI/CD execution
- **Focused Development**: Developers can work in their domain without noise from other components
- **Team Familiarity**: Aligns with team's existing experience with single-repo workflows

### Negative

- **Feature Coordination Complexity**: Features spanning multiple repositories require discipline in coordination (Jira tickets must link multiple PRs)
- **API Contract Synchronization**: Requires explicit process to keep backend and frontend in sync (managed via OpenAPI in eap-architecture)
- **ADR Categorization**: Need clear rules for where ADRs belong (platform-level vs component-specific)
- **Version Management**: Must synchronize version tags across repositories at sprint boundaries
- **Dependency Management**: Shared code requires careful coordination or package publishing
- **Duplication Risk**: Common patterns may be duplicated across repositories without active coordination

### Neutral

- **Multiple Clones Required**: Developers need to clone 4 repositories instead of 1
- **Jira Integration**: Requires proper ticket structure to link PRs across multiple repositories

## Implementation Details

### Branch Protection Rules

**main branch:**
- Requires 2 PR reviews
- All CI checks must pass
- No direct commits

**dev branch:**
- Requires 1 PR review
- All CI checks must pass
- No direct commits

### Branch Name Convention

Format: `<type>/eap-XXX-short-description`  
Types: `feature`, `bugfix`, `hotfix`, `release`, `refactoring`, `chore`

Examples:
```
feature/eap-123-add-access-request-endpoint

bugfix/eap-123-crash-on-start
```

### PR Title Convention

Format: `EAP-XXX: description`

Example:
```
EAP-123: Add access request endpoint
```

### Version Synchronization

All repositories will use semantic versioning (`MAJOR.MINOR.PATCH`) and be tagged with matching versions at the end of each sprint:

- Sprint 1: v0.1.0
- Sprint 2: v0.2.0
- Sprint 3: v0.3.0
- Final release: v1.0.0

Example synchronization:
```bash
# End of Sprint 2
eap-architecture: git tag -a v0.2.0 -m "Sprint 2 release"
eap-backend:      git tag -a v0.2.0 -m "Sprint 2 release"
eap-frontend:     git tag -a v0.2.0 -m "Sprint 2 release"
eap-qa:           git tag -a v0.2.0 -m "Sprint 2 release"
```

### API Contract Management

**Problem**: Backend and frontend must agree on API structure.

**Solution**: OpenAPI specification in eap-architecture serves as the single source of truth.

**Workflow**:
1. API design discussion → Update `eap-architecture/api-specs/openapi.yaml`
2. Backend implements according to spec
3. Frontend generates TypeScript types from spec
4. QA validates contracts using spec

**Validation**:
- Backend: OpenAPI validation in tests
- Frontend: TypeScript types generated from spec
- CI: Automated validation of OpenAPI spec in eap-architecture


### ADR Decision Matrix

**Rule of thumb:**
- Affects multiple repos? → eap-architecture
- Specific to one component? → That component's repo

| Decision Type | Example | Repository | Rationale |
|--------------|---------|------------|-----------|
| Platform/Infrastructure | Tech stack choice | eap-architecture | Affects all components |
| API Contract | Authentication approach | eap-architecture | Shared between backend/frontend |
| Deployment | Containerization strategy | eap-architecture | Affects all deployments |
| Backend Implementation | Database schema design | eap-backend | Backend-specific detail |
| Backend Pattern | ORM usage patterns | eap-backend | Internal to backend |
| Frontend Implementation | State management | eap-frontend | Frontend-specific detail |
| Frontend Pattern | Component structure | eap-frontend | Internal to frontend |
| Testing Strategy | E2E framework choice | eap-qa | QA-specific decision |
| Test Approach | Test data management | eap-qa | QA process detail |

## Alternatives Considered

### Option 1: Monorepo
**Description**: Single repository containing all services.

**Pros**:
- Atomic commits across services
- Easier code sharing
- Single source of truth
- Simplified dependency management

**Cons**:
- Larger repository size
- Slower CI/CD (all tests run on every change)
- More complex tooling required (Nx, Turborepo, Bazel)
- Team unfamiliar with monorepo tooling
- Harder to enforce access controls

**Rejection Reason**: Team lacks experience with monorepo tooling, and CI/CD complexity would increase learning curve.

### Option 2: Hybrid Approach
**Description**: Core services (backend + frontend) in one repo, auxiliary services (QA, architecture) separate.

**Pros**:
- Balance between coordination and separation
- Shared code easier for core services
- Deployment still independent

**Cons**:
- Complexity: Two different workflows
- Still requires monorepo tooling for core repo
- Unclear where new services belong
- Mixed benefits don't outweigh the added complexity

**Rejection Reason**: Adds complexity without solving the team's main coordination needs. Better to commit to one approach.

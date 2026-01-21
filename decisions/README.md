# Architecture Decision Records (ADRs)

This directory contains Architecture Decision Records for the Employee Application Platform.

## What are ADRs?

Architecture Decision Records document important architectural decisions made during the project lifecycle, including the context, options considered, and consequences of each decision.

## ADR Index

| ADR | Title | Status | Date |
|-----|-------|--------|------|
| [ADR-001](ADR-001-repository-strategy.md) | Repository Strategy | Accepted | 08-01-2026 |
| [ADR-002](ADR-002-backend-tech-stack.md) | Backend Technology Stack | Accepted | 12-01-2026 |
| [ADR-003](ADR-003-frontend-tech-stack.md) | Frontend Technology Stack | Accepted | 15-01-2026 |
| [ADR-004](ADR-004-qa-e2e-tech-stack.md) | QA and E2E Testing Technology Stack | Accepted | 13-01-2026 |
| [ADR-005](ADR-005-devops-tech-stack.md) | DevOps and Infrastructure Technology Stack | Accepted | 13-01-2026 |
| [ADR-006](ADR-006-ui-ux-design-stack.md) | UI/UX Design Stack | Accepted | 21-01-2026 |

## Creating New ADRs

1. Copy [template.md](template.md)
2. Rename to `ADR-XXX-descriptive-name.md` (use next sequential number)
3. Fill in all sections:
   - **Status**: Proposed → Accepted/Rejected/Superseded
   - **Context**: Why is this decision needed?
   - **Decision**: What did we decide?
   - **Consequences**: What are the impacts?
4. Add entry to this index
5. Commit with message: "docs: add ADR-XXX [title]"

## ADR Lifecycle

- **Proposed**: Under discussion
- **Accepted**: Decision approved and being implemented
- **Deprecated**: No longer relevant but kept for history
- **Superseded**: Replaced by a newer ADR (link to the new one)

## Guidelines

- Keep ADRs concise and focused on a single decision
- Document the reasoning, not just the outcome
- Include date of decision
- Link to related ADRs when applicable
- Never delete ADRs, only mark as deprecated or superseded

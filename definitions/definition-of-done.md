# Definition of Done

**Project:** Enterprise Application Project (EAP)

## Code

- Code is readable and reviewed by at least one team member
- No critical warnings or errors
- Code follows established patterns (PEP 8 for Python, Biome for TypeScript)
- All PR comments addressed and resolved

## Testing

- Unit tests are added or updated where relevant
- Tests pass in CI (pytest for backend, vitest for frontend)
- Feature manually tested (happy path and edge cases)
- API endpoints tested
- E2E tests added for critical user workflows (Playwright)

## DevOps

- Build succeeds
- Relevant automated tests are executed as part of the pipeline
- Deployment works in the target environment (if applicable)
- No new vulnerabilities introduced (npm audit, pip Safety)

## Quality & Resilience

- The application behaves predictably under failure conditions
- Errors are logged clearly
- Recovery or rollback is possible
- Input validation implemented (Pydantic for backend, frontend forms)
- No sensitive data committed to repository
- Authentication/authorization checked for protected routes (JWT, RBAC)

## Documentation

- Changes are documented
- Setup instructions are updated
- API specification updated (OpenAPI/Swagger, if endpoints changed)
- ADR created for significant architectural decisions

## Agreement

- The team agrees this work is "Done"
- Done means ready to show in a Sprint Demo and ready to be used by others
- All acceptance criteria from User Story are met

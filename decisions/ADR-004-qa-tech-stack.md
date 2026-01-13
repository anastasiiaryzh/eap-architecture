# ADR-004: QA and E2E Testing Technology Stack

**Project:** Enterprise Application Project (EAP)  
**Version:** 1.0  
**Date:** 13-01-2026  
**Status:** Draft  
**Source:** Functional Requirements v1.0  

## Context

The EAP requires comprehensive test automation covering both UI end-to-end testing and API testing to ensure quality, reliability, and maintainability. The application's complexity requires a robust testing strategy that can validate:

### Testing Requirements:

**UI Testing Needs:**
- Three distinct user dashboards (Requester, Approver, Admin) with role-based access
- Complex user workflows: request submission, approval process, status tracking
- Form validation and error handling across multiple forms
- File upload functionality (up to 5 files, 10MB each)
- Data tables with filtering, sorting, pagination (10 items per page)
- Cross-browser compatibility (Chrome, Firefox, Edge)
- Responsive design validation

**API Testing Needs:**
- REST API endpoint validation (CRUD operations)
- Authentication and authorization flows (JWT tokens)
- Request/response validation
- Error handling and edge cases
- Performance testing (response times < 2s)
- Data integrity validation
- Integration testing between frontend and backend

**Quality Requirements:**
- Test automation must be maintainable by the development team
- Fast feedback loop for CI/CD pipeline
- Clear test reports and debugging capabilities
- Support for parallel test execution
- Cross-browser testing without complex infrastructure
- Easy integration with version control and CI tools

### Technical Constraints:

- **Team Experience:** Development team learning test automation best practices
- **Goal:** Prepare trainees for Dutch corporate IT market testing standards
- **Maintainability:** Tests must be easy to understand, write, and debug
- **Integration:** Must work seamlessly with React 19 frontend and Python/FastAPI backend
- **CI/CD:** Must integrate with GitHub Actions or similar CI tools

## Decision

We will adopt the following QA and E2E testing stack:

### UI/E2E Testing
- **Playwright** (latest stable version)
- **Python** for test scripts

### API Testing
- **pytest** (latest stable version) - Primary API testing framework
- **Playwright API Testing** (Python) - API tests

## Rationale

### Playwright for UI Testing

Playwright was chosen over alternatives (Selenium, Cypress, TestCafe) because:

**Modern Browser Automation:**
- Native support for Chromium, Firefox, and WebKit (Safari) in a single tool
- Auto-waiting mechanism eliminates flaky tests caused by timing issues
- Built-in test retry and timeout handling
- Fast execution with browser context isolation

**Developer Experience:**
- Codegen tool to auto-generate test scripts by recording user actions
- Playwright Inspector for step-by-step debugging with time-travel
- Built-in test runner with parallel execution and sharding
- Clear error messages and stack traces
- Screenshot and video recording on test failures

**Feature Completeness:**
- Multiple browser contexts for parallel testing
- Network interception and mocking
- File upload/download testing
- Mobile viewport emulation
- Accessibility testing capabilities
- Browser DevTools protocol access
- API testing support (can test both UI and API in one framework)

**Industry Alignment:**
- Rapidly gaining adoption in 2024-2026, overtaking Cypress
- Microsoft-backed with active development
- Growing community and ecosystem
- Increasingly requested skill in Dutch job market

### pytest for API Testing

pytest was chosen for API testing because:

**Python Ecosystem Fit:**
- Backend uses Python (FastAPI), making pytest a natural choice
- Shared language for backend development and API testing
- Can import and test backend code directly if needed
- Easy to collaborate between backend and QA engineers

**Framework Strengths:**
- Simple, readable test syntax
- Powerful fixture system for test setup/teardown
- Parametrization for data-driven testing
- Rich plugin ecosystem (pytest-asyncio, pytest-html, pytest-cov)
- Parallel execution with pytest-xdist

**API Testing Capabilities:**
- requests library for straightforward HTTP calls
- JSON validation and schema testing
- Authentication flow testing (JWT tokens)
- Response time assertions for performance validation
- Easy mocking and stubbing

**Integration Benefits:**
- Works seamlessly with FastAPI's test client
- Can test API independently or with full backend
- Easy to integrate with CI/CD pipeline
- Widely used in Python backend testing

### Combined Strategy

**Why Separate Tools:**
- Playwright is UI-focused with Python (unified language across all tests)
- pytest is API-focused with Python (matches backend stack)
- Both tools use Python, reducing language context switching
- Clear separation of concerns: UI tests vs API tests
- Team members can work across both with single language knowledge

**Cross-Layer Testing:**
- Playwright can perform E2E tests that exercise both UI and API
- pytest can test API independently for faster feedback
- Combined coverage provides comprehensive quality validation

## Consequences

### Positive

**Testing Capabilities:**
- Comprehensive cross-browser UI testing without complex setup
- Fast, reliable API validation with Python
- Auto-waiting eliminates flaky tests
- Clear test failure diagnostics with screenshots/videos
- Parallel execution speeds up test suite

**Developer Experience:**
- Python for both UI and API tests (single language across all testing)
- Unified testing language matches backend stack
- Codegen tool accelerates UI test creation
- Simple pytest syntax is easy to learn
- Excellent debugging tools (Playwright Inspector, pytest output)

**Maintainability:**
- Python's clear syntax makes tests readable and maintainable
- pytest's fixture system promotes reusable test code
- Clear test structure and naming conventions
- Easy to refactor tests as application evolves

**Learning Outcomes:**
- Team learns industry-standard testing tools
- Prepares team for Dutch job market where Playwright adoption is growing
- pytest is standard for Python API testing
- Understanding of E2E vs API testing strategies

**Cost Efficiency:**
- Both tools are completely free and open-source
- No cloud-based test platform costs
- Built-in browser management (no Selenium Grid needed)
- Self-hosted CI execution

### Negative

**Learning Curve:**
- Team must learn two different testing frameworks
- Playwright's API requires learning for UI test creation
- pytest fixture system has moderate complexity
- Python knowledge required (but team already learning Python for backend)

**Maintenance Overhead:**
- UI tests are inherently more brittle than API tests
- Test data setup and teardown complexity
- Separate test suites to maintain (UI + API)
- Browser version updates may require test adjustments

**Execution Time:**
- E2E tests are slower than unit tests
- Full test suite may take 10-20 minutes in CI
- Browser rendering overhead in UI tests

**Tool Fragmentation:**
- Two separate frameworks means two sets of documentation
- Different command-line interfaces (playwright test vs pytest)
- Separate reporting formats (can be unified with custom tools)

### Mitigation

**For Learning Curve:**
- Start with simple happy path tests before complex scenarios
- Document common patterns and best practices
- Code reviews with teammates to support knowledge transfer

**For Maintenance Overhead:**
- Use Page Object Model (POM) pattern to isolate UI changes
- Keep tests focused and independent
- Implement test data factories for consistent setup
- Regular test review and refactoring sessions
- Use Playwright's locator strategies

**For Execution Time:**
- Run tests in parallel
- Implement test tagging/categories (smoke, regression, full)
- Run fast API tests first, slow E2E tests later
- Optimize test data creation
- Use sharding in CI for larger test suites

### Neutral

- **Community Support:** Both Playwright and pytest have active communities and extensive documentation
- **IDE Integration:** Good support in VS Code and PyCharm for both frameworks
- **Configuration:** Moderate setup complexity but well-documented

## Alternatives Considered

### Alternative 1: Cypress Instead of Playwright

**Description:** Use Cypress for E2E UI testing instead of Playwright.

**Pros:**
- More mature ecosystem (released 2017 vs Playwright 2020)
- Large community and extensive documentation
- Excellent time-travel debugging in Cypress Dashboard
- Simple, developer-friendly API
- Strong adoption in React community
- Better documentation and learning resources

**Cons:**
- Primarily Chromium-based (Firefox/WebKit support is experimental/limited)
- Runs inside browser, has architectural limitations
- Slower execution than Playwright
- Cannot test multiple browser contexts simultaneously
- Cross-browser testing requires Cypress Cloud (paid)
- No native mobile browser support
- More flaky tests due to architecture
- Dashboard features require paid plan

**Rejection Reason:** Playwright's native multi-browser support (Chromium, Firefox, WebKit) without additional cost is critical for cross-browser validation. Playwright's auto-waiting mechanism and architectural design (runs outside browser) reduces flakiness. Faster execution and parallel browser contexts better suit CI/CD needs. Playwright is the more modern solution with stronger momentum in 2025-2026.

---

### Alternative 2: Selenium WebDriver Instead of Playwright

**Description:** Use Selenium WebDriver with Python bindings for UI automation.

**Pros:**
- Industry standard since 2004, extremely mature
- Largest community and ecosystem
- Most Stack Overflow content available
- Support for virtually every browser
- Multi-language support (Java, Python, C#, JavaScript, etc.)
- Widely known skill in Dutch job market
- Can reuse Java-based Selenium tests if needed

**Cons:**
- Requires external WebDriver management (ChromeDriver, GeckoDriver, etc.)
- No built-in auto-waiting (requires explicit waits everywhere)
- Flaky tests are common without careful implementation
- Verbose API compared to modern alternatives
- Slower execution than Playwright
- No built-in test runner (need separate tool like Jest, Mocha)
- Poor developer experience (debugging is harder)
- Legacy architecture not designed for modern web apps

**Rejection Reason:** Playwright provides superior developer experience with auto-waiting, built-in test runner, and modern API design. Selenium's lack of auto-waiting leads to flaky tests, requiring significant boilerplate. WebDriver management is cumbersome compared to Playwright's built-in browser management. Team learning goals prioritize modern tools (Playwright) over legacy standards. Selenium's maturity doesn't outweigh Playwright's technical advantages for a greenfield project.

---

## Constraints

- **Educational Goal:** Prepare team for Dutch IT job market where modern test automation skills are required
- **Project Timeline:** MVP requires test coverage without excessive test development time
- **Team Experience:** Development team learning test automation from scratch
- **CI/CD:** Must run in GitHub Actions with reasonable execution time
- **Budget:** Free, open-source solutions only for MVP
- **Browser Coverage:** Must test Chrome, Firefox, Safari, Edge

## Deciders

- QA Lead
- Development team

## References

- [Playwright Documentation](https://playwright.dev/)
- [Playwright Python Guide](https://playwright.dev/python/docs/intro)
- [Playwright CI/CD Guide](https://playwright.dev/docs/ci)
- [pytest Documentation](https://docs.pytest.org/)
- [pytest-asyncio Plugin](https://pytest-asyncio.readthedocs.io/)
- [requests Library Documentation](https://requests.readthedocs.io/)
- [pytest-html Plugin](https://pytest-html.readthedocs.io/)

## Notes

**Test Strategy:**
- **API Tests:** pytest for independent API validation (fast feedback)
- **E2E Tests:** Playwright for critical user journeys (slower, comprehensive)

**Test Categories:**
- **Smoke Tests:** Fast subset of tests for quick validation (run on every commit)
- **Regression Tests:** Comprehensive test suite (run on PR merge)
- **Performance Tests:** Response time validation (part of API tests with pytest)

**Future Considerations:**
- Add visual regression testing (Playwright has built-in screenshot comparison)
- Consider Playwright's trace viewer for advanced debugging
- Evaluate load testing tools (Locust, k6) for performance testing beyond functional API tests
- Monitor test execution time and optimize as test suite grows
- Consider test data management strategy for larger datasets

**Known Limitations:**
- No mobile app testing (web responsive only per MVP scope)
- No load/stress testing in MVP (functional testing only)
- No accessibility testing tools yet (can add axe-core later)
- No visual regression testing in MVP (screenshots only on failure)

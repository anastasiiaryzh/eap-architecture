# ADR-003: Frontend Technology Stack  

**Project:** Enterprise Application Project (EAP)  
**Version:** 1.1  
**Date:** 12-01-2026 
**Status:** Accepted  
**Source:** Functional Requirements v1.0  

## Context

The EAP is a web-based request management system requiring a modern, maintainable frontend solution. Already decided to use React 19. Now we need to establish the complete frontend technology stack including UI frameworks, state management, data fetching, routing, styling, code quality tools, and build tooling.

### Key Requirements from Functional Specifications:

**User Interface Complexity:**
- Three distinct user dashboards (Requester, Approver, Admin) with role-based access
- Complex data tables with filtering, sorting, search, and pagination (10 items per page)
- Real-time status updates and notifications
- Form-heavy application (request submission with validation)
- File upload functionality (up to 5 files, 10MB each)
- Timeline visualization for request history
- Analytics dashboard with charts and metrics

**Performance Requirements:**
- Dashboard load time < 2 seconds
- Search results < 2 seconds
- Email notifications < 1 minute
- Report generation < 10 seconds

**Data Management Needs:**
- Complex state management across multiple user roles
- API integration for all CRUD operations
- Authentication with JWT tokens
- Real-time status synchronization

**User Experience:**
- Professional, consistent UI across all modules
- Responsive design
- Accessibility compliance
- Clear validation and error messaging

### Technical Constraints:

- **Team Experience:** Development team learning modern React
- **Goal:** Prepare trainees for Dutch corporate IT market
- **Maintainability:** Code must be easy to understand and extend
- **Best Practices:** Industry-standard patterns and tooling
- **Backend Integration:** REST API already established

## Decision

We will adopt the following frontend technology stack:

### Core Framework
- **React 19 with React Compiler** (experimental)
- **TypeScript** 5.x (strict mode)

### UI Framework & Styling
- **Ant Design (antd)** 5.x + **@ant-design/icons**

### State Management
- **Redux Toolkit**

### Data Fetching
- **Axios**

### Routing
- **React Router v7**

### Code Quality
- **TypeScript ESLint + Prettier**

### Build Tool
- **Vite + Rolldown**

## Rationale

### React 19 + Compiler
React 19 provides modern concurrent features, automatic batching, and better server component support. The **React Compiler** (experimental) offers:
- Automatic memoization without manual `useMemo`/`useCallback`
- Better performance out-of-the-box
- Learning opportunity for cutting-edge React patterns
- Future-proof for when compiler becomes stable

**Trade-off:** Compiler is experimental, but can be disabled if issues arise while keeping React 19 benefits.

### Ant Design: Final Choice

After evaluating shadcn/ui + Tailwind CSS and Ant Design, the team has chosen **Ant Design (antd) 5.x with @ant-design/icons**.

#### Decision Rationale

**Primary Factors:**
1. **Rapid Development:** MVP timeline requires fast implementation. Ant Design's 50+ pre-built components (Table, Form, DatePicker, Modal, etc.) work immediately without custom setup
2. **Complex Data Requirements:** EAP heavily relies on data tables with filtering, sorting, and pagination. Ant Design's Table component is production-ready and feature-rich
3. **Form-Heavy Application:** Request submission and approval workflows require robust form validation. Ant Design's Form component with built-in validation accelerates development
4. **Team Knowledge Transfer:** One team member has Ant Design experience, enabling faster onboarding and mentorship
5. **Enterprise Focus:** Ant Design's design language is built for enterprise applications, matching EAP's professional requirements
6. **Consistent Design:** Provides polished, consistent UI out-of-the-box without requiring design system decisions
7. **Icon System:** @ant-design/icons provides 700+ icons that integrate seamlessly with components

#### Evaluation Against Requirements

| Requirement | How Ant Design Addresses It |
|------------|------------------------------|
| **Complex Tables** | Built-in Table component with filter, sort, pagination, and selection |
| **Form Validation** | Form component with field-level validation, error handling, and async validation |
| **Development Speed** | Pre-built components accelerate MVP delivery |
| **Professional UI** | Enterprise design language looks polished by default |
| **Team Learning** | Extensive documentation, Stack Overflow support, and internal knowledge transfer |
| **TypeScript** | Full type definitions included |
| **Performance** | Meets <2s dashboard load requirement with proper code-splitting |

#### Trade-offs Accepted

**Cons Acknowledged:**
- **Larger Bundle Size:** ~500KB vs ~150KB for lighter alternatives (acceptable given <2s performance requirement)
- **Customization Complexity:** Theme overrides are more complex, but not needed for MVP
- **Design Lock-in:** "Ant Design look" is acceptable for enterprise application; no custom brand requirements
- **Less Control:** Component internals are abstracted, but this is acceptable trade-off for development speed


### Redux Toolkit + RTK Query
For this enterprise application, Redux Toolkit is preferred over simpler solutions because:
- **Complex State:** Three user roles with different views require centralized state
- **Cross-Component Communication:** Dashboard summaries, notifications, and real-time updates
- **Caching Strategy:** RTK Query provides optimistic updates and automatic cache management
- **DevTools:** Excellent debugging for learning team
- **Industry Standard:** Widely used in Dutch enterprise market
- **API Integration:** RTK Query eliminates need for separate data-fetching library in most cases

### Axios
While RTK Query handles most API calls, Axios provides:
- **File Uploads:** Better progress tracking for 5-file uploads
- **Download Handling:** CSV/XLSX report exports
- **Token Refresh:** Centralized JWT interceptor logic
- **Specific Use Cases:** One-off API calls outside Redux flow

### React Router v7
Latest version provides:
- **Nested Routing:** Perfect for dashboard layouts with subroutes
- **Loader Pattern:** Data fetching before route render
- **Type Safety:** Strong TypeScript support
- **Industry Standard:** Most common React routing solution

### Vite + Rolldown
Modern build tooling chosen for:
- **Vite for Development:** Lightning-fast HMR (Hot Module Replacement) with instant server start
- **Rolldown for Production:** Rust-based bundler providing extremely fast production builds with advanced tree-shaking and code splitting
- **Next-Generation Performance:** Rolldown is Vite's future default bundler, offering 10x+ faster build times than Rollup
- **Simplicity:** Minimal configuration compared to Webpack, seamless integration with Vite
- **Modern Defaults:** ESM-first, optimized for React 19
- **Industry Momentum:** Cutting-edge tooling representing the future of JavaScript bundling (2025-2026)
- **Rollup Compatible:** Maintains compatibility with Rollup plugins while being significantly faster
- **Best of Both Worlds:** Lightning-fast development experience + ultra-optimized production bundles

## Consequences

### Positive

**Development Experience:**
- Fast feedback loop with Vite HMR
- Strong type safety catches errors early
- Excellent debugging with Redux DevTools
- Consistent code style with ESLint + Prettier

**Performance:**
- React Compiler auto-optimizes renders
- Ant Design uses tree-shaking to reduce bundle size
- Vite + Rolldown provide ultra-fast builds and optimized bundle splitting
- RTK Query reduces unnecessary API calls with intelligent caching

**Maintainability:**
- TypeScript prevents common bugs
- Redux centralizes state logic
- Ant Design components are battle-tested and well-documented
- Consistent component API reduces learning curve
- ESLint + Prettier enforce consistent code style and best practices

**Learning Outcomes:**
- Team learns modern React patterns
- Exposure to cutting-edge React Compiler
- Experience with enterprise component library (Ant Design) widely used globally
- Industry-standard state management with Redux Toolkit
- Knowledge transfer from experienced team member
- Prepares team for Dutch job market where Ant Design is common in enterprise projects

**Market Alignment:**
- Stack mirrors 2025-2026 industry trends
- All tools have strong European adoption
- Ant Design widely used in global enterprise applications
- Matches common Dutch enterprise tech stacks

### Negative

**Complexity:**
- Redux Toolkit has learning curve compared to Context API
- Ant Design API requires learning component props and patterns
- TypeScript strict mode is demanding initially
- React Compiler behavior may be non-obvious
- Ant Design theme customization is complex if design changes are needed

**Experimental Risk:**
- React Compiler is experimental (see Mitigation strategies below)
- React 19 relatively new (December 2024 release)
- Team may encounter less Stack Overflow content than for React 18

**Tooling Overhead:**
- Redux boilerplate for simple operations
- TypeScript configuration complexity

**Lock-in Concerns:**
- Ant Design's opinionated design language limits visual customization
- Locked into Ant Design's release cycle and potential breaking changes
- Difficult to migrate away from Ant Design if requirements change significantly

### Mitigation

**For Learning Curve:**
- Start with simpler features (authentication, basic forms)
- Create internal documentation
- Use React Compiler as opt-in per component initially; can be disabled via config flag if issues arise

**For Experimental Risk:**
- Fallback to React 18 if critical React 19 issues occur (unlikely)
- Monitor React 19 issue tracker and community for stability updates

**For Redux Complexity:**
- Create template slices and RTK Query endpoints
- Use Redux Toolkit's simplified API (no raw Redux)
- Only use Redux for truly shared state
- Local state with `useState` for component-only data

**For Tooling Overhead:**
- Create component wrappers for frequently used Ant Design patterns
- Use TypeScript utility types to reduce boilerplate
- Leverage ESLint auto-fix and Prettier formatting to maintain code quality automatically
- Implement code-splitting to reduce initial bundle size impact

**For Lock-in Concerns:**
- Accept Ant Design's default design language for MVP (no custom theme needed)
- Document any custom overrides for maintainability
- Use Ant Design's theme configuration for minor adjustments (colors, spacing)
- Avoid deep customizations that would make migration difficult

### Neutral

- **Community Support:** All tools are well-established (Redux, React Router, Ant Design) with extensive documentation
- **Configuration:** Moderate setup complexity, but well-documented
- **Design Flexibility:** Ant Design's opinionated design is a trade-off (faster development, less customization)

## Alternatives Considered

### Alternative 1: shadcn/ui + Tailwind CSS Instead of Ant Design

**Description:** Use shadcn/ui component primitives with Tailwind CSS for styling instead of Ant Design.

**Pros:**
- Full customization and control over component code
- Smaller bundle size (~150KB vs ~500KB)
- Modern architecture built on Radix UI primitives
- No library lock-in (components copied into project)
- Utility-first CSS approach with Tailwind
- Growing adoption in modern React projects

**Cons:**
- Slower initial development (need to build/customize each component)
- Complex components like data tables require additional libraries (Tanstack Table)
- Forms require React Hook Form + manual UI implementation
- Steeper learning curve for team (Tailwind utilities + component composition)
- No team member experience with shadcn/ui or Tailwind
- Less suited for rapid MVP development

**Rejection Reason:** MVP timeline requires rapid development. EAP is heavily table and form-focused, where Ant Design's pre-built components provide immediate value. Team has existing Ant Design knowledge for faster onboarding. shadcn/ui's customization benefits are not needed for MVP as there are no custom design requirements. The ~350KB bundle size difference is acceptable given the <2s performance requirement.

---

### Alternative 2: React 18 (Stable) Instead of React 19 with Compiler

**Description:** Use React 18 (stable since March 2022) instead of React 19 with experimental compiler.

**Pros:**
- More battle-tested and stable
- Larger Stack Overflow and community resources
- Team member may have more React 18 experience
- No experimental features to worry about
- Fewer potential bugs

**Cons:**
- Lacks React 19 performance improvements (automatic batching enhancements, concurrent rendering improvements)
- No React Compiler support (manual memoization required)
- Missing new APIs and features
- Less preparation for 2026+ job market
- Would need migration to React 19 eventually

**Rejection Reason:** Already decided to use React 19 for learning goals and market preparation. React Compiler can be disabled if issues arise, keeping React 19 benefits intact. Educational value of learning latest React outweighs stability concerns.

---

### Alternative 3: Material-UI (MUI) Instead of Ant Design

**Description:** Use Material-UI (MUI) component library with Material Design system.

**Pros:**
- Huge community and ecosystem
- Material Design is widely recognized
- Extensive documentation and examples
- Strong TypeScript support
- Comprehensive component library
- Regular updates and maintenance

**Cons:**
- Very large bundle size (~1MB+ for full library)
- Material Design aesthetic is limiting and opinionated
- Theming system has steep learning curve
- Customization requires understanding MUI's sx prop and theme structure
- Material Design look is recognizable (less flexibility than Tailwind or Ant Design)
- CSS-in-JS performance concerns with large apps

**Rejection Reason:** Bundle size too large (~1MB+) compared to Ant Design (~500KB) without sufficient benefits. Material Design's opinionated aesthetic is more limiting than Ant Design's enterprise-focused design. No team experience with MUI vs existing Ant Design knowledge. MUI's customization complexity (sx prop, theme structure) doesn't provide enough benefit over Ant Design for this project.

---

### Alternative 4: Biome Instead of ESLint + Prettier

**Description:** Use Biome as an all-in-one tool for linting and code formatting instead of the traditional ESLint + Prettier combination.

**Pros:**
- All-in-one solution (linting + formatting in single tool)
- Significantly faster performance (10-100x faster than ESLint)
- Simpler configuration (one config file vs two)
- No conflicts between linter and formatter
- TypeScript-native implementation
- Modern, cutting-edge tooling
- Faster CI/CD pipeline
- Less cognitive overhead for team

**Cons:**
- Newer tool with smaller community and ecosystem
- Limited plugin ecosystem compared to ESLint (fewer custom rules available)
- Less mature IDE integration in some editors
- Less Stack Overflow content and documentation
- Team members likely have no prior Biome experience
- Some advanced ESLint plugins not available
- May not support all project-specific linting requirements
- Less proven in production enterprise environments

**Rejection Reason:** While Biome offers performance benefits and simplified configuration, ESLint + Prettier is the industry standard with proven maturity. The extensive ESLint plugin ecosystem provides critical rules for React, TypeScript, accessibility (eslint-plugin-jsx-a11y), and security that may not be available in Biome. For a learning-focused development team, using industry-standard tools (ESLint + Prettier) provides better career preparation for the Dutch IT market. The team will encounter these tools in most professional environments. IDE integration is more mature and stable. The performance difference, while notable, is acceptable for development workflow and CI/CD pipeline given the project size.

---

### Alternative 5: Zustand Instead of Redux Toolkit

**Description:** Use Zustand for lightweight state management instead of Redux Toolkit.

**Pros:**
- Much simpler API than Redux
- Less boilerplate code
- Smaller bundle size
- Faster to learn for beginners
- Direct store access without providers
- Growing popularity in React community

**Cons:**
- No built-in data fetching/caching (would need TanStack Query or Axios separately)
- Less mature DevTools compared to Redux
- No standardized patterns for complex async logic
- Smaller ecosystem and community
- Less common in enterprise Dutch job market
- Would need to build caching strategy manually
- No optimistic updates out of the box

**Rejection Reason:** EAP requires sophisticated data fetching with caching, optimistic updates, and invalidation strategies. RTK Query provides this built-in. Zustand would require TanStack Query addition, splitting state management. Redux Toolkit is more valuable for career preparation in Dutch enterprise market. DevTools superior for learning team debugging.

## Constraints

- **Educational Goal:** Prepare development team for modern Dutch IT job market requiring React, TypeScript, and modern tooling
- **Project Timeline:** MVP delivery requires rapid development without long ramp-up
- **Team Experience:** Development team learning modern React needs clear patterns and good error messages
- **Backend Architecture:** REST API already designed; frontend must integrate via HTTP
- **Performance Budget:** Dashboard < 2s load, search < 2s response
- **Browser Support:** Modern browsers only (Chrome, Firefox, Safari, Edge latest versions)
- **Deployment:** Static hosting preferred (no server-side rendering required)

## Deciders

- Staff
- Development team

## References

- [React 19 Documentation](https://react.dev/blog/2025/04/25/react-19)
- [React Compiler Documentation](https://react.dev/learn/react-compiler)
- [Redux Toolkit Documentation](https://redux-toolkit.js.org/)
- [RTK Query Documentation](https://redux-toolkit.js.org/rtk-query/overview)
- [shadcn/ui Documentation](https://ui.shadcn.com/)
- [Ant Design Documentation](https://ant.design/)
- [Ant Design Components](https://ant.design/components/overview)
- [Tailwind CSS Documentation](https://tailwindcss.com/)
- [React Router v7 Documentation](https://reactrouter.com/)
- [ESLint Documentation](https://eslint.org/)
- [TypeScript ESLint](https://typescript-eslint.io/)
- [Prettier Documentation](https://prettier.io/)
- [Vite Documentation](https://vitejs.dev/)
- [Rolldown Documentation](https://rolldown.rs/)
- [TypeScript Documentation](https://www.typescriptlang.org/)
- [Axios Documentation](https://axios-http.com/)
- [Functional Requirements v1.0](../requirements/functional-requirements.md)

**Potential Stack Evolution:**
- Monitor Biome adoption; reassess vs ESLint/Prettier in 6 months
- Consider migrating to Remix or Next.js for future projects with SSR needs
- Evaluate bundle size optimizations after MVP (code-splitting, lazy loading)
- Consider migrating away from Ant Design if custom design system becomes a requirement post-MVP

**Known Limitations:**
- No native mobile apps (web responsive only per MVP scope)
- No real-time websockets (email notifications sufficient for MVP)
- No offline support (requires network connectivity)
- No multi-language support (English only for MVP)
- Ant Design's opinionated design limits visual customization without significant effort

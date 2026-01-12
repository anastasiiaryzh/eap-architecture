# ADR-003: Frontend Technology Stack

**Project:** Enterprise Application Project (EAP)
**Version:** 1.0
**Date:** 2026-01-12
**Status:** Proposed
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

### UI Framework, Styling
- **To Be Decided:** shadcn/ui + Tailwind CSS **OR** Ant Design (antd) + @ant-design/icons

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

### UI Framework: shadcn/ui + Tailwind CSS vs Ant Design

The team is evaluating two primary options for the UI framework. Both are viable choices with different trade-offs.

#### Option A: shadcn/ui + Tailwind CSS

**Pros:**
- **Full Customization:** Copy-paste components into project; you own the code completely
- **Modern Architecture:** Built on Radix UI primitives (accessibility built-in) with Tailwind styling
- **Minimal Bundle Size:** Only includes components you actually use; no unused library code
- **Design Flexibility:** Easy to match any custom design system or brand guidelines
- **Learning Value:** Teaches component composition, Radix UI patterns, and utility-first CSS
- **Modern Ecosystem:** Increasingly adopted in 2025-2026 React projects globally
- **No Lock-in:** Not dependent on library maintainers for updates; components are yours to modify
- **TypeScript First:** Excellent type safety out of the box

**Cons:**
- **Manual Component Management:** Need to manually copy/update components when shadcn/ui releases improvements
- **Slower Initial Setup:** Takes time to add each component individually (button, table, dialog, etc.)
- **Tailwind Learning Curve:** Team needs to learn utility-first CSS approach and class names
- **Less Pre-Built Complexity:** Complex components like data tables require additional setup (e.g., Tanstack Table integration)
- **No Built-in Icons:** Need separate icon library (e.g., Lucide React, Heroicons)
- **Newer Approach:** Less Stack Overflow content than mature libraries like Ant Design

**Best For:**
- Projects requiring custom design systems
- Teams wanting full control over components
- Learning modern React patterns and utility-first CSS
- Applications needing minimal bundle size

#### Option B: Ant Design (antd) + @ant-design/icons

**Pros:**
- **Complete Component Library:** 50+ pre-built, production-ready components out of the box
- **Rapid Development:** Tables, forms, modals, etc. work immediately with minimal setup
- **Consistent Design Language:** Professional enterprise UI design that looks polished by default
- **Rich Data Components:** Excellent Table, Form, DatePicker components perfect for enterprise apps
- **Team Experience:** One team member has prior Ant Design experience
- **Mature Ecosystem:** Extensive documentation, community support, Stack Overflow answers
- **Icons Included:** @ant-design/icons provides 700+ icons matching design system
- **Battle-Tested:** Used by Alibaba and thousands of production applications since 2015
- **TypeScript Support:** Full type definitions included

**Cons:**
- **Larger Bundle Size:** Importing entire library increases initial load time
- **Customization Complexity:** Overriding Ant Design theme/styles can be difficult
- **Design Lock-in:** Opinionated design language; hard to deviate from "Ant Design look"
- **Less Control:** Components are black boxes; harder to modify internal behavior
- **Library Dependency:** Locked into Ant Design's release cycle and breaking changes
- **Chinese Origin:** Primary community and documentation originates from China (though well-translated)
- **Tree-Shaking Limitations:** Even with proper imports, bundle includes more than needed

**Best For:**
- Rapid enterprise application development
- Teams prioritizing speed over customization
- Projects without strict design system requirements
- Leveraging existing team Ant Design knowledge

#### Evaluation Criteria for EAP

Let's evaluate both options against our specific requirements:

| Criteria | shadcn/ui + Tailwind | Ant Design | Notes |
|----------|---------------------|------------|-------|
| **Complex Tables** | ⚠️ Need to build or use Tanstack Table | ✅ Excellent built-in Table component | EAP needs tables with filter, sort, pagination |
| **Form Validation** | ⚠️ Need React Hook Form + manual UI | ✅ Built-in Form with validation | Heavy form usage in request submission |
| **Development Speed** | ⚠️ Slower (build components) | ✅ Faster (use ready components) | MVP timeline is tight |
| **Customization** | ✅ Complete control | ⚠️ Limited customization | Future custom design requirements unclear |
| **Bundle Size** | ✅ Smaller (~150KB) | ⚠️ Larger (~500KB) | Both acceptable for web app |
| **Learning Goals** | ✅ Teaches modern patterns | ⚠️ Less transferable skills | Training preparation important |
| **Team Experience** | ⚠️ No prior experience | ✅ One member knows Ant Design | Knowledge transfer possible |
| **Accessibility** | ✅ Radix UI primitives (excellent) | ✅ Good but not as comprehensive | Both meet requirements |
| **Design Consistency** | ⚠️ Need to establish patterns | ✅ Consistent by default |  Development team may need guidance |
| **Market Relevance** | ✅ Growing in European market | ✅ Common in enterprise (global) | Both valuable for career |

#### Recommendation Factors

**Choose shadcn/ui + Tailwind CSS if:**
- Custom design system is planned post-MVP
- Team prioritizes learning modern React/CSS patterns
- Bundle size optimization is critical
- Want full control over component behavior
- Prefer modern, minimal aesthetic

**Choose Ant Design if:**
- MVP timeline is aggressive and speed is critical
- No custom design requirements (professional default is acceptable)
- Want to leverage team member's existing knowledge
- Complex data tables/forms are primary focus
- Prefer battle-tested enterprise components


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
- CSS solution purges unused styles (Tailwind) or uses tree-shaking (Ant Design)
- Vite + Rolldown provide ultra-fast builds and optimized bundle splitting
- RTK Query reduces unnecessary API calls

**Maintainability:**
- TypeScript prevents common bugs
- Redux centralizes state logic
- UI components are well-documented and maintainable
- ESLint + Prettier enforce consistent code style and best practices

**Learning Outcomes:**
- Team learns modern React patterns
- Exposure to cutting-edge React Compiler
- Understanding of modern CSS approaches or enterprise component libraries
- Industry-standard state management
- Prepares team for Dutch job market

**Market Alignment:**
- Stack mirrors 2025-2026 industry trends
- All tools have strong European adoption
- Matches common Dutch enterprise tech stacks

### Negative

**Complexity:**
- Redux Toolkit has learning curve compared to Context API
- UI framework learning curve (Tailwind utilities OR Ant Design API)
- TypeScript strict mode is demanding initially
- React Compiler behavior may be non-obvious

**Experimental Risk:**
- React Compiler is experimental (can be disabled)
- React 19 relatively new (June 2025 release)
- Team may encounter less StackOverflow content than for React 18

**Tooling Overhead:**
- Redux boilerplate for simple operations
- UI-specific overhead (Tailwind verbosity OR Ant Design customization complexity)
- TypeScript configuration complexity

**UI Framework Decision:**
- Final choice between shadcn/ui and Ant Design pending team evaluation
- Each has different trade-offs (speed vs control, learning vs productivity)

### Mitigation

**For Learning Curve:**
- Start with simpler features (authentication, basic forms)
- Create internal documentation
- Use React Compiler as opt-in per component initially

**For Experimental Risk:**
- React Compiler can be disabled via config flag
- Fallback to React 18 if critical issues (unlikely)
- Monitor React 19 issue tracker and community

**For Redux Complexity:**
- Create template slices and RTK Query endpoints
- Use Redux Toolkit's simplified API (no raw Redux)
- Only use Redux for truly shared state
- Local state with `useState` for component-only data

**For Tooling Overhead:**
- Create component abstractions for repeated patterns (Tailwind utilities or Ant Design wrappers)
- Use TypeScript utility types to reduce boilerplate
- Leverage ESLint auto-fix and Prettier formatting to maintain code quality automatically

### Neutral

- **Bundle Size:** Modern but not minimal; acceptable for web application
- **Community Support:** Mix of established (Redux, React Router) and newer (shadcn) tools
- **Configuration:** Moderate setup complexity, but well-documented

## Alternatives Considered

**Note:** The UI framework choice (shadcn/ui + Tailwind vs Ant Design) is not listed here as an alternative because both options are currently under active evaluation. See the "UI Framework" section in Rationale for detailed comparison.

### Alternative 1: React 18 (Stable) Instead of React 19 with Compiler

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

### Alternative 2: Material-UI (MUI) Instead of shadcn/ui or Ant Design

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

**Rejection Reason:** Bundle size too large for performance requirements (dashboard < 2s). Material Design's opinionated aesthetic limits design flexibility. Both shadcn/ui (Tailwind-based) and Ant Design (enterprise-focused) are better fits for this project's needs. MUI's customization complexity doesn't provide enough benefit over alternatives.

---

### Alternative 3: Biome Instead of ESLint + Prettier

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

### Alternative 4: Zustand Instead of Redux Toolkit

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
- **UI Framework:** Final decision between shadcn/ui + Tailwind CSS and Ant Design pending team evaluation

## Deciders

- Staff

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

**Known Limitations:**
- No native mobile apps (web responsive only per MVP scope)
- No real-time websockets (email notifications sufficient for MVP)
- No offline support (requires network connectivity)
- No multi-language support (English only for MVP)

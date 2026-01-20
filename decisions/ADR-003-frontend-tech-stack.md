# ADR-003: Frontend Technology Stack  

**Project:** Enterprise Application Project (EAP)  
**Version:** 3.0  
**Date:** 20-01-2026  
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
- **shadcn/ui** + **Tailwind CSS**

### State Management
- **Redux Toolkit**

### Data Fetching
- **Axios**

### Routing
- **React Router v7**

### Code Quality
- **Biome**

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

### shadcn/ui + Tailwind CSS: Final Choice

After evaluating Ant Design and shadcn/ui + Tailwind CSS, the team has chosen **shadcn/ui with Tailwind CSS**.

#### Decision Rationale

**Primary Factors:**
1. **Full Customization Control:** shadcn/ui copies components directly into the project, providing complete control over component code and styling without library lock-in
2. **Modern Architecture:** Built on Radix UI primitives (accessible, unstyled components) with Tailwind CSS for utility-first styling
3. **Smaller Bundle Size:** ~150KB vs ~500KB for Ant Design, contributing to better performance and faster load times
4. **Learning Value:** Team learns modern React patterns, component composition, and utility-first CSS approach (Tailwind) - highly valuable skills in 2026 market
5. **No Design Lock-in:** Tailwind's utility classes enable rapid UI customization without fighting against opinionated component styles
6. **TypeScript Native:** Excellent TypeScript support with full type safety
7. **Component Ownership:** Components live in the codebase, making debugging, customization, and maintenance straightforward
8. **Industry Momentum:** shadcn/ui and Tailwind CSS are rapidly growing in adoption, especially in modern React projects and startups

#### Evaluation Against Requirements

| Requirement | How shadcn/ui + Tailwind Addresses It |
|------------|------------------------------|
| **Complex Tables** | Use Tanstack Table with custom shadcn/ui wrapper for full control over table behavior and styling |
| **Form Validation** | React Hook Form integration with shadcn/ui form components for type-safe validation |
| **Development Speed** | Initial setup slower, but composable components accelerate feature development after setup |
| **Professional UI** | Tailwind enables rapid professional styling; shadcn/ui provides accessibility out-of-the-box |
| **Team Learning** | Teaches fundamental React patterns, composition, accessibility (Radix), and modern CSS (Tailwind) |
| **TypeScript** | Excellent TypeScript support with full type definitions |
| **Performance** | Smaller bundle (~150KB) helps meet <2s dashboard load requirement |

#### Trade-offs Accepted

**Cons Acknowledged:**
- **Initial Development Speed:** Requires building/composing components (e.g., complex tables need Tanstack Table integration)
- **Learning Curve:** Team must learn Tailwind utility classes and component composition patterns
- **No Pre-built Complex Components:** Features like data tables, advanced forms require integration with additional libraries
- **Less Immediate Consistency:** Requires establishing design patterns and component usage guidelines


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
- Consistent code style with Biome

**Performance:**
- React Compiler auto-optimizes renders
- shadcn/ui + Tailwind CSS provide smaller bundle size (~150KB) for faster initial loads
- Vite + Rolldown provide ultra-fast builds and optimized bundle splitting
- RTK Query reduces unnecessary API calls with intelligent caching

**Maintainability:**
- TypeScript prevents common bugs
- Redux centralizes state logic
- shadcn/ui components are owned by the project, making customization and debugging straightforward
- Tailwind utility classes provide consistent styling approach
- Biome enforces consistent code style and best practices

**Learning Outcomes:**
- Team learns modern React patterns and component composition
- Exposure to cutting-edge React Compiler
- Experience with Tailwind CSS utility-first approach (highly sought-after skill in 2026)
- Deep understanding of accessibility through Radix UI primitives
- Industry-standard state management with Redux Toolkit
- Ownership of component code teaches React fundamentals better than abstracted libraries
- Prepares team for modern Dutch job market where Tailwind is increasingly popular

**Market Alignment:**
- Stack mirrors 2025-2026 industry trends (shadcn/ui and Tailwind rapidly growing)
- Tailwind CSS has strong adoption in European startups and modern enterprises
- Component composition approach aligns with React best practices
- Skills are highly transferable and valued in modern web development

### Negative

**Complexity:**
- Redux Toolkit has learning curve compared to Context API
- Tailwind CSS requires learning utility class names and responsive patterns
- TypeScript strict mode is demanding initially
- React Compiler behavior may be non-obvious
- Component composition requires understanding Radix UI primitives
- Complex components (tables, forms) need integration with additional libraries

**Experimental Risk:**
- React Compiler is experimental (see Mitigation strategies below)
- React 19 relatively new (December 2024 release)
- Team may encounter less Stack Overflow content than for React 18

**Tooling Overhead:**
- Redux boilerplate for simple operations
- TypeScript configuration complexity
- Need to integrate multiple libraries (Tanstack Table, React Hook Form) for complex features

**Initial Development Speed:**
- Slower initial setup compared to pre-built component libraries
- Each complex component requires composition and styling effort
- No out-of-the-box solutions for data tables and advanced forms

### Mitigation

**For Learning Curve:**
- Start with simpler features (authentication, basic forms) using basic shadcn/ui components
- Create Tailwind cheat sheet and component style guide for team reference
- Use React Compiler as opt-in per component initially; can be disabled via config flag if issues arise
- Build component library gradually, starting with simple UI elements

**For Experimental Risk:**
- Fallback to React 18 if critical React 19 issues occur (unlikely)
- Monitor React 19 issue tracker and community for stability updates

**For Redux Complexity:**
- Create template slices and RTK Query endpoints
- Use Redux Toolkit's simplified API (no raw Redux)
- Only use Redux for truly shared state
- Local state with `useState` for component-only data

**For Tooling Overhead:**
- Create reusable component compositions for common patterns
- Use TypeScript utility types to reduce boilerplate
- Leverage Biome auto-fix and formatting to maintain code quality automatically
- Implement code-splitting to reduce initial bundle size impact

**For Initial Development Speed:**
- Set up Tanstack Table and React Hook Form wrappers early in the project
- Create reusable table and form component templates
- Build a component showcase/storybook to document available components
- Start with shadcn/ui CLI to quickly scaffold common components
- Establish design patterns (spacing, colors, typography) using Tailwind config

### Neutral

- **Community Support:** All tools have strong community support (Redux, React Router, Tailwind CSS, shadcn/ui) with extensive documentation
- **Configuration:** Moderate setup complexity (Tailwind config, shadcn/ui setup), but well-documented
- **Design Flexibility:** Full control over styling and component behavior enables custom design system if needed

## Alternatives Considered

### Alternative 1: Ant Design Instead of shadcn/ui + Tailwind CSS

**Description:** Use Ant Design (antd) 5.x component library with @ant-design/icons instead of shadcn/ui + Tailwind CSS.

**Pros:**
- Rapid development with 50+ pre-built components (Table, Form, DatePicker, Modal, etc.)
- Complex data table component with filtering, sorting, and pagination built-in
- Form component with robust validation out-of-the-box
- Consistent enterprise design language by default
- One team member has Ant Design experience for knowledge transfer
- Polished UI with minimal configuration
- @ant-design/icons provides 700+ integrated icons
- Less initial learning curve compared to Tailwind + component composition

**Cons:**
- Larger bundle size (~500KB vs ~150KB)
- Theme customization is complex and requires understanding Ant Design's theming system
- Design lock-in to "Ant Design look" unless heavily customized
- Less control over component internals and behavior
- Locked into library's release cycle and potential breaking changes
- Abstracted components provide less learning about React fundamentals

**Rejection Reason:** While Ant Design offers faster initial development, the team will benefit more from learning modern React patterns, component composition, and Tailwind CSS—skills highly valued in the 2026 market. shadcn/ui provides full control and customization without library lock-in, smaller bundle size for better performance, and teaches fundamental React concepts better than abstracted libraries. The initial development speed trade-off is acceptable for the long-term learning and flexibility benefits.

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

### Alternative 4: ESLint + Prettier Instead of Biome

**Description:** Use the traditional ESLint + Prettier combination for linting and code formatting instead of Biome.

**Pros:**
- Industry standard with proven maturity and long track record
- Extensive plugin ecosystem (React, TypeScript, accessibility, security plugins)
- Large community and abundant Stack Overflow resources
- Mature IDE integration across all major editors
- Team members may have prior ESLint/Prettier experience
- Well-documented with extensive tutorials and guides
- Proven in production enterprise environments

**Cons:**
- Requires two separate tools with potential configuration conflicts
- Slower performance compared to Biome (10-100x slower)
- Complex configuration with multiple config files (.eslintrc, .prettierrc, etc.)
- Conflicts between ESLint and Prettier require eslint-config-prettier to resolve
- Higher cognitive overhead managing two tools
- Slower CI/CD pipeline due to performance
- More dependencies to maintain and update
- Configuration drift between linter and formatter rules

**Rejection Reason:** While ESLint + Prettier is the traditional industry standard, Biome offers significant advantages for this project. Biome provides 10-100x faster performance, simplifying both local development and CI/CD pipelines. The all-in-one solution eliminates configuration conflicts between linter and formatter. Biome's simpler configuration (single biome.json) reduces cognitive overhead for the learning team. Biome has matured significantly and now supports React, TypeScript, and accessibility rules sufficient for this project. The Dutch IT market is increasingly adopting modern tooling like Biome, and learning cutting-edge tools prepares the team for future opportunities. Biome's TypeScript-native implementation provides excellent type-aware linting. The performance benefits directly support the project's rapid development timeline.

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
- [Tailwind CSS Documentation](https://tailwindcss.com/)
- [Radix UI Documentation](https://www.radix-ui.com/)
- [Tanstack Table Documentation](https://tanstack.com/table/)
- [React Hook Form Documentation](https://react-hook-form.com/)
- [Ant Design Documentation](https://ant.design/)
- [React Router v7 Documentation](https://reactrouter.com/)
- [Biome Documentation](https://biomejs.dev/)
- [Vite Documentation](https://vitejs.dev/)
- [Rolldown Documentation](https://rolldown.rs/)
- [TypeScript Documentation](https://www.typescriptlang.org/)
- [Axios Documentation](https://axios-http.com/)
- [Functional Requirements v1.0](../requirements/functional-requirements.md)

**Potential Stack Evolution:**
- Monitor Biome ecosystem growth and new rule additions
- Consider migrating to Remix or Next.js for future projects with SSR needs
- Evaluate bundle size optimizations after MVP (code-splitting, lazy loading)
- Expand component library with additional shadcn/ui components as needed
- Consider Storybook for component documentation if team grows

**Known Limitations:**
- No native mobile apps (web responsive only per MVP scope)
- No real-time websockets (email notifications sufficient for MVP)
- No offline support (requires network connectivity)
- No multi-language support (English only for MVP)
- Initial component development requires more setup compared to pre-built libraries

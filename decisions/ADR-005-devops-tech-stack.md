# ADR-005: DevOps and Infrastructure Technology Stack

**Project:** Enterprise Application Project (EAP)  
**Version:** 1.0  
**Date:** 13-01-2026  
**Status:** Accepted  
**Source:** Functional Requirements v1.0  

## Context

The EAP requires a reliable, scalable, and maintainable infrastructure to support development, validation, and production deployment. The project needs:

### Infrastructure Requirements:

**Development & Deployment:**
- Consistent development environments across team members
- Reliable production deployment
- Easy rollback capabilities
- Infrastructure as code for reproducibility

**CI/CD Pipeline:**
- Automated testing on code commits
- Automated deployment
- Manual approval for production deployment
- Build and test automation
- Version control integration
- Fast feedback loop for developers

**Hosting & Scalability:**
- Reliable hosting solution for MVP
- Cost-effective for initial deployment
- Ability to scale if user base grows
- Support for containerized applications

### Technical Constraints:

- **Team Experience:** Development team learning DevOps practices
- **Budget:** Cost-effective solutions for MVP phase
- **Maintainability:** Simple setup that team can manage
- **Future Growth:** Ability to migrate to Kubernetes if scaling becomes necessary
- **Integration:** Must work seamlessly with existing tech stack (React, FastAPI, PostgreSQL)

## Decision

We will adopt the following DevOps and infrastructure stack:

### Containerization
- **Docker** - Container platform for application packaging
- **Docker Compose** - Local development and orchestration

### CI/CD Pipeline
- **GitHub Actions** - Automated workflows for build, test, and deployment

### Hosting & Deployment
- **TransIP VPS + Ubuntu 24.04 instance** - Virtual Private Server for production deployment
- **Future Consideration:** Separate VPS instances or Kubernetes migration when scaling

### Additional Tools
- **GitHub Container Registry** - Container image registry
- **Nginx** - Reverse proxy and web server
- **PostgreSQL** - Database (containerized)

## Rationale

### Docker for Containerization

Docker was chosen because:

**Environment Consistency:**
- Eliminates "works on my machine" problems
- Isolated dependencies prevent conflicts
- Version-controlled infrastructure configuration

**Developer Experience:**
- Simple setup with Docker Compose for local development
- Easy to spin up full application stack (frontend, backend, database)
- Fast onboarding for new team members
- Clear separation of services

**Deployment Benefits:**
- Portable containers run anywhere
- Easy rollback by switching container versions
- Simplified deployment process
- Efficient resource usage compared to VMs

### GitHub Actions for CI/CD

GitHub Actions was chosen because:

**Integration:**
- Native integration with GitHub repository
- No additional service setup required
- Automatic triggers on commits, pull requests, merges

**Workflow Automation:**
- YAML-based workflow configuration
- Automated testing (pytest, Playwright)
- Automated builds and deployments
- Matrix builds for testing multiple configurations

**Cost & Accessibility:**
- Free for public repositories
- Generous free tier for private repositories
- No separate CI/CD platform to maintain
- Built-in secrets management

**Developer Experience:**
- Familiar interface within GitHub
- Extensive marketplace of pre-built actions
- Good documentation and community support

### TransIP VPS for Hosting

TransIP VPS was chosen for production deployment because:

**Cost Effectiveness:**
- Affordable VPS pricing for startup phase
- Predictable monthly costs
- No over-provisioning required for MVP

**Simplicity:**
- Single server deployment is easier to manage for small team
- Both environments on same hardware reduces management overhead
- Direct control over server configuration
- No complex orchestration needed for MVP scale
- Dutch-based provider with local support

**Flexibility:**
- Full root access for custom configurations
- Can run multiple Docker Compose stacks
- Easy to upgrade resources as needed
- Straightforward migration to separate VPS or Kubernetes later
- Environments isolated via different ports and subdomain routing

**Future Kubernetes Migration:**
- If application scales and requires high availability
- If need auto-scaling capabilities
- If require complex microservices orchestration
- Can migrate containerized apps with minimal changes

## Consequences

### Positive

**Development Efficiency:**
- Fast local environment setup with Docker Compose
- Consistent environments eliminate configuration issues
- Automated testing and deployment save time
- Faster feedback loop with CI/CD pipeline

**Operational Benefits:**
- Simplified deployment process
- Easy rollback to previous versions
- Clear separation of concerns (containers)
- Infrastructure as code (Docker Compose, GitHub Actions YAML)

**Cost Savings:**
- No additional CI/CD platform costs (GitHub Actions included)
- More cost-effective than managed Kubernetes or separate servers
- Efficient resource utilization with containers
- Pay only for what you use

**Learning Outcomes:**
- Team learns industry-standard DevOps practices
- Docker skills applicable across projects
- CI/CD pipeline experience
- Foundation for Kubernetes migration if needed

**Scalability Path:**
- Clear migration path to Kubernetes
- Containerization makes scaling easier
- Can add load balancing when needed
- Infrastructure as code facilitates growth

### Negative

**Single Server Limitations:**
- No built-in high availability (single point of failure for both environments)
- Manual scaling (vertical scaling by upgrading VPS)
- Limited geographic distribution
- Need to implement own backup strategy

**Shared Infrastructure Risks:**
- Single point of failure affects both environments
- Requires careful resource allocation and monitoring

**Learning Curve:**
- Team must learn Docker concepts and commands
- GitHub Actions YAML syntax requires learning
- Server administration knowledge needed
- Debugging container issues can be complex initially

**Maintenance Overhead:**
- Manual server updates and security patches
- Need to monitor server health and resources
- Manual backup management
- Container image updates and security scanning

**GitHub Actions Limitations:**
- Limited to GitHub ecosystem
- Build minutes cap on free tier (though generous)
- Less flexible than dedicated CI/CD platforms for complex workflows

### Mitigation

**For High Availability:**
- Implement regular automated backups
- Document disaster recovery procedures
- Monitor uptime and set up alerts
- Plan migration to separate VPS instances or Kubernetes when scaling needs arise

**For Shared Infrastructure Risks:**
- Configure resource limits for Docker containers (CPU, memory)
- Implement health checks and alerting for both environments
- Plan to separate to dedicated VPS instances when budget allows or traffic increases

**For Learning Curve:**
- Start with simple Docker Compose setup
- Document common Docker commands and workflows
- Use GitHub Actions templates and marketplace actions
- Pair programming for DevOps tasks

**For Maintenance:**
- Automate server updates where possible
- Use monitoring tools (e.g., Prometheus, Grafana)
- Implement automated backup scripts
- Regular security audits and updates
- Use Docker health checks

**For CI/CD Limitations:**
- Optimize workflows to stay within free tier limits
- Cache dependencies to speed up builds
- Use self-hosted runners if needed later

### Neutral

- **Vendor Lock-in:** Minimal lock-in due to Docker portability and standard CI/CD practices
- **Migration Complexity:** Moving to Kubernetes later will require additional configuration but containers make it feasible
- **Performance:** VPS performance adequate for MVP; can upgrade resources as needed

## Alternatives Considered

### Alternative 1: Kubernetes from Start

**Description:** Deploy directly to Kubernetes cluster (e.g., managed GKE, EKS, or self-hosted).

**Pros:**
- Production-grade orchestration from day one
- Built-in high availability and auto-scaling
- Industry standard for container orchestration
- Better long-term scalability

**Cons:**
- Significant complexity for MVP phase
- Steep learning curve for team
- Higher costs (managed clusters or infrastructure overhead)
- Overkill for initial small-scale deployment
- Longer setup time

**Rejection Reason:** Kubernetes adds unnecessary complexity for MVP scale. The team's learning curve would delay development. TransIP VPS provides sufficient resources for initial deployment, and containerization with Docker ensures easy migration to Kubernetes when scaling becomes necessary.

---

## Deciders

- DevOps Lead
- Development Team

## References

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [TransIP VPS](https://www.transip.nl/vps/)
- [Kubernetes Migration Guide](https://kubernetes.io/docs/concepts/overview/)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)

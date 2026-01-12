# ADR-002: Backend Technology Stack

**Project:** Enterprise Application Project (EAP)  
**Version:** 1.0  
**Date:** 12-01-2026  
**Status:** Accepted  
**Source:** EAP_Product_Vision_v1.0.pdf  

## Context

The EAP project requires selecting a backend technology stack that will power the RESTful API, handle business logic, manage data persistence, and support authentication/authorization. This decision impacts development velocity, maintainability, security, and the team's ability to deliver the MVP within the project timeline.

Key considerations:
- Team skill levels and learning curve
- Development speed for MVP delivery
- Long-term maintainability
- Performance and scalability requirements
- Security and reliability needs
- Available deployment infrastructure (TransIP VPS)
- Integration requirements (OpenAPI, JWT authentication)

The backend system needs to support:
- RESTful API design with OpenAPI specification
- Role-based access control
- Email notifications
- Audit trail functionality
- Database transactions and data integrity

## Decision

We will use the following backend technology stack:

### Core Backend
- **Language:** Python 3.11+
- **Framework:** FastAPI
- **ASGI Server:** Uvicorn with standard extras

### Data Layer
- **Database:** PostgreSQL
- **ORM:** SQLAlchemy 2.0+
- **Migrations:** Alembic

### Security & Authentication
- **Authentication:** JWT (JSON Web Tokens)
- **Token Library:** python-jose with cryptography
- **Password Hashing:** passlib with bcrypt
- **Input Validation:** Pydantic 2.0+

### API Documentation
- **Specification:** OpenAPI 3.1 (FastAPI built-in)
- **Interactive Docs:** Swagger UI (FastAPI built-in)

### Testing
- **Test Framework:** pytest
- **Coverage:** pytest-cov

## Rationale

### Why Python 3.11+

- Team has Python experience
- Rapid development suitable for MVP timeline
- Rich ecosystem for common tasks (email, authentication, database)
- Excellent data processing capabilities
- Modern type hints improve code quality
- Strong community support and documentation

### Why FastAPI

- **Built-in OpenAPI/Swagger generation:** Critical for frontend-backend contract and API documentation
- **Automatic request validation:** Using Pydantic reduces boilerplate and prevents invalid data
- **High performance:** Comparable to Node.js and Go, suitable for 100 concurrent users
- **Modern async support:** Native async/await for efficient I/O operations
- **Excellent documentation:** Well-documented with clear examples
- **Type hints:** Leverages Python type hints for better IDE support and fewer bugs
- **Dependency injection:** Built-in dependency injection system simplifies testing
- **Standards-based:** Uses standard Python type hints, not custom framework syntax

### Why PostgreSQL

- **Proven reliability and performance:** Industry-standard relational database
- **ACID compliance:** Essential for audit trail requirements and data integrity
- **JSON support:** JSONB type allows flexible data structures when needed
- **Strong community and ecosystem:** Mature tooling, extensive documentation
- **Well-supported by SQLAlchemy:** First-class ORM support
- **Advanced features:** Full-text search, array types, custom functions
- **Scalability:** Connection pooling, replication options for future growth

### Why SQLAlchemy 2.0+

- **Industry-standard Python ORM:** Mature, battle-tested, widely adopted
- **Type-safe with modern type hints:** SQLAlchemy 2.0+ fully supports Python type hints
- **Supports migrations via Alembic:** Integrated migration tooling
- **Prevents SQL injection:** Parameterized queries by default
- **Team can learn best practices:** Well-documented patterns and conventions
- **Query flexibility:** Can write raw SQL when needed, not locked into ORM
- **Session management:** Clear patterns for transaction handling

### Why JWT for Authentication

- **Stateless:** No server-side session storage required
- **Scalable:** Easy to distribute across multiple servers
- **Standard:** Industry-standard approach
- **Cross-service:** Can be validated by multiple services
- **Payload flexibility:** Can include custom claims (user roles, permissions)

## Consequences

### Positive

- **Fast Development:** FastAPI enables rapid MVP development with minimal boilerplate
- **Type Safety:** Python type hints with Pydantic reduce runtime bugs
- **API Contract:** OpenAPI spec automatically generated, ensures frontend-backend alignment
- **Modern Stack:** Using current best practices and actively maintained tools
- **Security Built-in:** JWT, SQLAlchemy (SQL injection prevention), Pydantic input validation
- **Testability:** All chosen tools have excellent testing support
- **Documentation:** FastAPI automatically generates interactive API docs
- **Learning Value:** Team gains experience with industry-standard tools
- **Cost Effective:** All tools are free and open source
- **Performance:** Async architecture supports 100+ concurrent users efficiently
- **Database Reliability:** PostgreSQL ACID transactions ensure audit trail integrity

### Negative

- **Learning Curve:** Team needs to learn FastAPI, async patterns, SQLAlchemy 2.0, Alembic
- **Async Complexity:** FastAPI async patterns may be challenging initially
- **Type Hint Overhead:** Requires discipline to properly type all functions and models
- **Migration Management:** Alembic migrations require careful review and version control
- **JWT Limitations:** Cannot invalidate tokens before expiry without additional infrastructure
- **ORM Abstraction:** SQLAlchemy adds a layer that can hide performance issues

### Mitigation

- **Learning Curve:** Provide team with FastAPI tutorial, async/await guide, SQLAlchemy 2.0 migration guide
- **Async Complexity:** Start with simple synchronous endpoints, gradually introduce async patterns
- **Migration Management:** Establish migration review checklist, test migrations against production-like data
- **JWT Limitations:** Implement short token expiry (15 minutes) with refresh tokens, maintain token blacklist for critical operations

### Neutral

- **Opinionated Framework:** FastAPI has strong opinions on structure (can be positive or negative)
- **Python Performance:** Slower than compiled languages (Go, Rust) but sufficient for current needs

## Alternatives Considered

### Option 1: SQLite

**Description:** Lightweight, file-based relational database.

**Pros:**
- Zero configuration required
- No separate database server to manage
- Perfect for development and testing
- ACID compliant
- Built into Python standard library
- Very fast for read-heavy workloads
- Simple backup (just copy the file)
- No network latency

**Cons:**
- Limited concurrent write support (single writer at a time)
- Not suitable for 100+ concurrent users with write operations
- Lacks advanced features (full-text search, replication, JSON indexing)
- No user management or access control
- Limited data types compared to PostgreSQL
- Write performance degrades with multiple concurrent connections
- No network access (must be on same machine)
- Difficult to scale horizontally

**Rejection Reason:** Cannot handle 100 concurrent users with write operations due to single-writer limitation. EAP requires multiple users creating access requests, updating statuses, and logging audit events simultaneously. SQLite's concurrency model is insufficient for production use at this scale.

### Option 2: MongoDB (NoSQL)

**Description:** Document-oriented NoSQL database with flexible schema.

**Pros:**
- Flexible schema (no migrations needed)
- JSON-native storage matches API data format
- Horizontal scalability built-in
- Good performance for document-based queries
- Rich query language
- GridFS for large file storage

**Cons:**
- No ACID transactions across multiple documents (critical limitation for audit trail)
- Schema flexibility not needed for EAP's structured data
- SQL and relational model more familiar to team
- Relational model better fits EAP data structure (users, resources, access requests, audit logs)
- Harder to enforce referential integrity and data consistency
- More complex to model relationships (users → access requests → resources)
- Requires additional validation logic to maintain data integrity
- No foreign key constraints (must implement manually)

**Rejection Reason:** ACID compliance required for audit trail functionality. EAP needs guaranteed transaction consistency when creating access requests, updating resource statuses, and logging audit events. MongoDB's eventual consistency model and lack of multi-document ACID transactions make it unsuitable. Relational data structure (normalized tables with foreign keys) fits EAP's domain better than document model.

### Option 3: Alembic Migrations vs No Migration Tool

#### Approach A: Alembic (SQLAlchemy Migrations) - CHOSEN

**Description:** Version control for database schema changes using Alembic.

**Pros:**
- Database schema changes are version controlled
- Reproducible deployments across environments
- Easy rollback of schema changes
- Auto-generates migration scripts from model changes
- Team learns industry-standard migration practices
- Clear audit trail of schema evolution
- Supports complex migrations (data transformations, multi-step changes)
- Can review migrations in pull requests before applying

**Cons:**
- Learning curve for migration concepts
- Extra step in development workflow
- Migration scripts can become complex
- Requires discipline to review auto-generated migrations
- Potential conflicts when multiple developers change schema
- Must manage migration order carefully

#### Approach B: No Migration Tool (Manual SQL or ORM create_all)

**Description:** Apply schema changes directly using SQL scripts or SQLAlchemy's `create_all()`.

**Pros:**
- Simpler initial setup
- No migration tool to learn
- Faster for initial MVP development
- Direct control over SQL executed

**Cons:**
- No version control of schema changes
- Difficult to reproduce database state across environments
- No rollback capability
- Hard to coordinate schema changes across team members
- Production schema changes are risky and manual
- No audit trail of schema evolution
- `create_all()` doesn't handle schema modifications (only creates missing tables)
- Cannot safely deploy schema changes to production

**Decision: Use Alembic**

**Rejection Reason for "No Migrations":** Production deployments require reliable, version-controlled schema management. Without migrations, we cannot safely evolve the database schema after initial deployment. Alembic migrations are essential for professional database management, coordinating team development, and ensuring production safety. The learning investment pays off immediately when the first schema change is needed.

## Constraints

- **Team Experience:** Team has Python experience; limited experience with Go, Rust, or advanced Node.js patterns
- **Performance Requirements:** Must support 100 concurrent users with < 1s API response time
- **Security Requirements:** RBAC, JWT authentication, audit trail, password security
- **Deployment Environment:** TransIP VPS with limited resources (not enterprise-scale infrastructure)
- **Integration Requirements:** Must generate OpenAPI specification for frontend integration
- **MVP Timeline:** Limited time to learn new technologies while delivering features
- **Data Integrity:** Audit trail requires ACID-compliant database with transaction support

## Deciders

- Staff
- Development team

## References

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [SQLAlchemy 2.0 Documentation](https://docs.sqlalchemy.org/en/20/)
- [Alembic Documentation](https://alembic.sqlalchemy.org/)
- [Pydantic Documentation](https://docs.pydantic.dev/)
- [JWT Introduction](https://jwt.io/introduction)
- EAP Product Vision: EAP_Product_Vision_v1.0.pdf

### Migration Path

If the stack proves inadequate, possible migration paths:

**Backend:**
- FastAPI → Django: If admin panel or more batteries-included features needed
- PostgreSQL → PostgreSQL cluster: For higher scale and high availability
- Monolith → Microservices: If services need independent scaling

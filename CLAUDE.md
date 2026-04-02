# Project Overview

## Purpose
This repository contains an enterprise-grade backend and full-stack platform designed for regulated environments such as banking, insurance, and telecommunications.

The system must prioritize:
- security
- maintainability
- traceability
- auditability
- testability
- predictable operations
- backward compatibility
- production readiness

---

## Technology Stack
- Java 21+
- Spring Boot 3+
- Spring Security
- Keycloak
- PostgreSQL
- Docker
- React + TypeScript
- Maven or Gradle depending on module
- Flyway or Liquibase for DB migrations

---

## Architecture Principles

### Default architecture
Use a modular monolith by default unless there is a strong, explicit reason to introduce distributed services.

### Layering
Organize code into:
- presentation
- application
- domain
- infrastructure
- security
- integration
- observability

### Domain modeling
Prefer explicit domain modeling:
- entities
- value objects
- aggregates
- domain services
- application services
- bounded contexts where useful

### Architectural constraints
- Controllers must stay thin
- Business logic must not live in controllers
- Security decisions must be centralized and explicit
- JPA entities must not be exposed directly in public APIs
- DTOs must be used for external contracts
- Domain invariants must be protected in domain/application layers
- Integration concerns must be isolated from domain logic

---

## SOLID Requirements
All code and proposed refactors must follow:
- Single Responsibility Principle
- Open/Closed Principle
- Liskov Substitution Principle
- Interface Segregation Principle
- Dependency Inversion Principle

Use constructor injection by default.

Avoid:
- god classes
- hidden coupling
- low-cohesion services
- utility-class driven architecture
- framework-driven design with weak domain boundaries

---

## Approved Design Patterns
Use only when justified:
- Strategy
- Factory / Abstract Factory
- Builder
- Repository
- Specification
- Facade
- Adapter
- Proxy
- Domain Service
- DTO + Mapper
- Anti-Corruption Layer
- Outbox Pattern
- Retry / Circuit Breaker / Bulkhead for resilience
- Saga / Process Manager where distributed flow truly requires it

Do not introduce patterns just for abstraction theater.

---

## Security Requirements

### Identity and access
- Authentication is handled through Keycloak
- Authorization must follow least privilege
- RBAC is the default
- ABAC can be introduced only with clear business justification
- Role mapping must be explicit and documented

### Spring Security expectations
- Use secure-by-default configuration
- Protect all endpoints by default unless explicitly public
- Validate JWT access tokens properly
- Centralize access rules
- Avoid scattered authorization logic
- Use method security only where it improves clarity
- Enforce secure headers
- Keep CORS minimal and explicit

### Secrets and sensitive data
- Never hardcode secrets
- Never log credentials, tokens, or sensitive personal data
- Use externalized configuration
- Mask sensitive values in logs and error messages

### OWASP and defensive controls
Always consider:
- input validation
- output sanitization
- SQL injection prevention
- XSS prevention
- CSRF strategy where relevant
- SSRF awareness
- secure error handling
- dependency vulnerability awareness

### Auditability
Sensitive actions must be auditable:
- who initiated the action
- what changed
- when it happened
- correlation/trace context
- result status

---

## Database Standards

### PostgreSQL rules
- Use explicit schema design
- Define keys, constraints, and indexes intentionally
- Use migrations through Flyway or Liquibase
- Review query efficiency for critical paths
- Avoid N+1 issues
- Consider optimistic locking where concurrent updates are possible
- Design idempotency for externally retried commands

### Transaction rules
- Define transactional boundaries explicitly
- Keep transactions as small as practical
- Avoid mixing long-running remote calls inside DB transactions
- For cross-boundary propagation, prefer reliable async patterns where appropriate

### Data governance
Where relevant, consider:
- retention
- archival
- PII handling
- regulatory traceability
- historical auditability

---

## API Standards

### API style
Default to REST unless another style is explicitly justified.

### Contract rules
- Version long-lived APIs
- Use consistent request/response models
- Use structured validation errors
- Use consistent problem/error payloads
- Keep public contracts stable
- Separate internal model from external contract

### Error handling
- Use centralized exception handling
- Return stable, non-leaky error responses
- Do not expose internals in API errors

### Idempotency
For retriable or externally invoked operations, consider:
- idempotency keys
- replay-safe processing
- duplicate request protection

---

## Frontend Standards

### React + TypeScript
- Use strict typing
- Separate API client logic from presentation
- Handle loading, error, and empty states explicitly
- Keep components cohesive
- Use accessible forms and validation
- Keep auth flow explicit and secure
- Avoid leaking backend-specific implementation details directly into UI

### Frontend security
- Minimize token exposure
- Avoid unsafe local persistence decisions without justification
- Protect against XSS-prone patterns
- Centralize auth and API error handling

---

## Docker and Runtime Standards
- Use multi-stage builds where appropriate
- Minimize final image size
- Do not store secrets in image layers
- Support health checks
- Support externalized configuration
- Make runtime assumptions explicit
- Document startup dependencies

---

## Testing Standards

### Required test strategy
Every meaningful feature should define tests across the right levels:
- unit tests for core logic
- integration tests for wiring and infrastructure behavior
- repository tests for persistence behavior
- API/security tests for endpoint behavior
- frontend component tests where applicable
- end-to-end tests only for critical cross-cutting flows

### Testing expectations
- Test edge cases
- Test authorization outcomes
- Test validation failures
- Test transaction-sensitive behavior
- Test idempotency where relevant
- Test failure scenarios, not only happy path

---

## Observability Standards
All production-ready work should consider:
- structured logging
- correlation IDs
- metrics
- health/readiness endpoints
- audit logs
- traceability across service boundaries
- actionable diagnostics without sensitive leakage

---

## Code Review Expectations
When reviewing code, always evaluate:
- architecture fit
- domain clarity
- SOLID compliance
- justified patterns
- security posture
- transaction boundaries
- error handling
- testability
- performance hot spots
- operational readiness

Be strict, concrete, and actionable.

---

## Delivery Expectations
When proposing changes, structure output as:
1. Problem framing
2. Assumptions
3. Proposed architecture or implementation
4. Security impact
5. Data/persistence impact
6. Trade-offs and risks
7. Testing plan
8. Production readiness notes

---

## Key Commands
Adjust as needed for the actual repository.

### Build and test
- `./mvnw clean test`
- `./mvnw verify`
- `./gradlew test`
- `./gradlew build`

### Local runtime
- `docker compose up -d`
- `./mvnw spring-boot:run`
- `./gradlew bootRun`

### Database
- `./mvnw flyway:migrate`
- `./gradlew flywayMigrate`

### Frontend
- `npm install`
- `npm run dev`
- `npm run build`
- `npm run test`

---

## Hard Rules for AI Assistance
- Do not generate demo-only architecture for enterprise requests
- Do not suggest shortcuts that weaken security
- Do not expose entities directly in API
- Do not move business logic into controllers
- Do not recommend microservices unless justified
- Do not assume missing requirements are unimportant
- Do not ignore auditability, observability, or operability
- Do not hand-wave transaction boundaries
- Do not ignore role/permission design
- Do not log sensitive data

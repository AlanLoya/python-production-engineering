# Python Production Engineering

An agent skill for building, reviewing, and validating production-grade Python backends.

It provides a practical workflow for development, architecture, testing, security, database changes, API design, workers, and repository validation.

## What It Covers

- Python backend feature development
- Bug diagnosis and refactoring
- FastAPI and Pydantic APIs
- SQLAlchemy and Alembic migrations
- Async Python and background workers
- Authorization and security-sensitive code
- External integrations and secrets management
- Unit, integration, API, and worker testing
- Type checking, linting, formatting, and coverage
- Monorepo workflows using Nx and `uv`
- Architecture and dependency-boundary decisions

## Core Workflow

The skill guides an agent through six stages:

1. Locate the code owner and relevant dependencies.
2. Define the behavior contract and failure modes.
3. Implement the change at the correct architectural boundary.
4. Add focused tests for success, failure, and security cases.
5. Run layered validation from package-level to repository-level checks.
6. Review the final change for operational and security risks.

## Security Principles

The skill emphasizes:

- Fail-closed authentication and authorization
- Principal and tenant-scoped data access
- Typed input validation and explicit limits
- Parameterized database queries
- Secret and sensitive-data redaction
- SSRF and unsafe outbound-request protection
- Safe file and document handling
- Idempotent workers and controlled retries
- Explicit permissions for LLM and agent tools
- Stable public errors with actionable internal observability

## Validation

For repositories using Nx and `uv`, the recommended validation order is:

```bash
pnpm nx run <project>:format
pnpm nx run <project>:lint
pnpm nx run <project>:typecheck
pnpm nx run <project>:test

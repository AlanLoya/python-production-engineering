# Python Production Engineering

A portable agent skill for building, reviewing, and validating production-grade Python backends.

It guides work across application design, APIs, databases, background workers, testing, security, and operational readiness without assuming a specific framework, repository layout, package manager, or CI platform.

## Installation

Copy the skill directory into the agent skills directory of your project:

```text
.agents/skills/python-production-engineering/SKILL.md
```

The resulting layout should be:

```text
.agents/
└── skills/
    └── python-production-engineering/
        └── SKILL.md
```

The skill has no runtime dependencies and does not require an installation command. The accompanying documentation files are optional and can remain alongside `SKILL.md`.

## Activation

The skill is model-invoked when a task involves Python backend work, including:

- API or worker changes
- Bug fixes and refactors
- FastAPI, Pydantic, SQLAlchemy, or Alembic
- Async Python and external integrations
- Authentication, authorization, secrets, or sensitive data
- Database access and migrations
- Test design and validation failures
- Architecture and dependency-boundary decisions

## What It Does

The skill leads the agent through a production-oriented loop:

1. Locate the code owner and relevant dependencies.
2. Define the behavior contract and failure modes.
3. Implement the change at the correct architectural boundary.
4. Add focused tests for success, failure, and security cases.
5. Run the repository's documented validation commands.
6. Review operational, security, and rollback implications.

## Design Principles

- Keep transport, application, domain, persistence, and integration concerns distinct.
- Prefer existing project abstractions over new frameworks or helpers.
- Keep route handlers thin and business rules explicit.
- Make async boundaries, transaction ownership, and resource lifetimes visible.
- Design workers for idempotency, controlled retries, and safe reprocessing.
- Prefer additive, backward-compatible schema migrations.
- Preserve public contracts unless a change is intentional and tested.

## Security Coverage

The skill applies a security gate to every change involving identity, permissions, secrets, files, user-controlled URLs, external services, generated content, or database queries.

It emphasizes:

- Fail-closed authentication and authorization
- Principal and tenant scoping
- Typed input validation and explicit limits
- Parameterized queries and safe ORM expressions
- Secret and sensitive-data redaction
- SSRF protection, timeouts, and bounded responses
- Safe file and document processing
- Explicit permissions for LLM and agent tools
- Stable public errors with actionable internal observability

## Validation

The skill discovers the project's authoritative commands from its configuration and CI rather than assuming a particular toolchain.

Depending on the repository, validation may include:

```bash
python -m pytest
ruff check .
ruff format --check .
mypy .
```

These are examples, not requirements. The agent should run only tools that the project already configures, starting with the narrowest relevant checks and expanding validation for shared, migration, security-sensitive, or cross-package changes.

## Repository Contents

- `SKILL.md` - the agent-facing instructions.
- `REFERENCES.md` - official documentation and standards used as background.
- `SECURITY.md` - vulnerability reporting guidance.
- `LICENSE` - MIT license.

## License

This project is distributed under the MIT License. See `LICENSE` for the complete text.

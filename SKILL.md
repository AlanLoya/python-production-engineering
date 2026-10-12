---
name: python-production-engineering
description: Use for Python backend feature work, bug fixes, refactors, API or worker changes, validation failures, security-sensitive code, architecture decisions, database access, migrations, or test design. Apply when the task touches FastAPI, Pydantic, SQLAlchemy, Alembic, async Python, background workers, authorization, secrets, external integrations, or Python project tooling.
license: MIT
metadata:
  author: AlanLoya
  version: '1.0.0'
---

# Python Production Engineering

Use this skill to take Python changes from local understanding to production evidence. Keep the change narrow, preserve the repository's architecture, and make the validation path visible.

## Operating Loop

1. **Locate the owner.** Read the nearest package configuration, the target module, one caller, and one relevant test. Identify whether the change belongs to an API, domain service, data package, worker, shared library, or infrastructure boundary. Completion means you can name the function or boundary that decides the behavior and one check that could disconfirm your first hypothesis.
2. **State the contract.** Define inputs, outputs, failure modes, authorization rules, side effects, and compatibility constraints before editing. Prefer an existing local abstraction over a new framework or helper. Completion means the expected behavior is expressible as focused examples or assertions.
3. **Implement at the owning boundary.** Keep transport, domain, persistence, and integration concerns separate. Use typed interfaces and dependency injection where the codebase already supports them. Preserve public APIs unless the task explicitly requires a contract change. Completion means the happy path and relevant failure paths are implemented without weakening existing invariants.
4. **Add focused tests.** Cover the changed behavior, invalid inputs, denied access, not-found or conflict cases, external failures, and important async or transaction boundaries. Use fixtures for setup, parameterization for equivalent cases, and mocks only at the integration boundary. Completion means the focused test command passes and the test names explain the contract.
5. **Run layered validation.** Start with the narrowest project or package checks, then run the repository's broader validation for shared or high-risk changes. Completion means formatting, lint, type checking, tests, and coverage requirements pass, or every pre-existing failure is reported with evidence.
6. **Review the diff as an operator.** Check logs, error responses, migrations, configuration, deployment behavior, observability, and rollback implications. Completion means no secret, authorization bypass, destructive migration, unbounded query, or silently changed public contract remains unexplained.

## Architecture Rules

- Keep route handlers thin: parse and validate transport data, resolve dependencies, call an application service, and map domain errors to stable HTTP responses.
- Keep business rules in domain/application code rather than hiding them in routes, ORM models, or global state.
- Use Pydantic models for external boundaries and explicit typed objects for internal contracts. Do not pass unvalidated dictionaries across layers when a model or protocol expresses the contract.
- Make async boundaries honest. Use `async def` for non-blocking I/O, avoid blocking calls inside the event loop, and make transaction ownership explicit.
- Treat database sessions and external clients as dependencies with clear lifetimes. Do not create hidden connections, mutable module-global state, or per-request resources that cannot be closed.
- Keep workers idempotent. Define retry behavior, deduplication keys, visibility timeouts, and failure handling before adding side effects.
- For schema changes, inspect the query patterns and deployment order. Prefer additive, backward-compatible migrations; separate data backfills from DDL when locks or volume make a combined migration risky.
- Respect package ownership and dependency direction. Shared libraries should not import application-specific modules merely to avoid a boundary decision.

## Security Gate

Apply this gate to every change that handles identity, permissions, secrets, files, user-controlled URLs, external services, generated content, or database queries.

- Fail closed for authentication and authorization. Enforce permissions at the server-side boundary for every protected operation; never trust client-provided roles, IDs, or UI state.
- Scope reads and writes to the authenticated principal and tenant/domain context. Test both an allowed principal and a denied or cross-scope principal.
- Validate input with typed schemas and explicit limits. Reject unexpected fields or states when the contract requires it, and avoid unsafe deserialization or dynamic code execution.
- Use parameterized queries and ORM expressions. Never build SQL, shell commands, paths, or headers by interpolating untrusted input.
- Keep secrets in environment or secret-management configuration. Do not log tokens, authorization headers, credentials, personal data, prompts containing secrets, or raw provider responses without redaction.
- Guard outbound requests against SSRF, unsafe redirects, unexpected schemes, excessive response size, missing timeouts, and uncontrolled retries.
- Treat uploaded files and generated documents as hostile input. Validate type and size, isolate storage paths, prevent traversal, and scan or sandbox where the deployment requires it.
- For LLM or agent integrations, separate system instructions from user content, constrain tool permissions, validate structured output, and make external side effects explicit and auditable.
- Preserve security failures as observable events without exposing internal details to callers. Return stable, minimal error messages and log actionable context at the appropriate level.

## Validation Matrix

Discover the repository's authoritative commands from `pyproject.toml`, package configuration, `Makefile`, task runner configuration, or CI before inventing commands. Prefer the narrowest available checks first, followed by broader checks when the change crosses package boundaries or affects shared behavior.

Typical Python checks may include, when the project already configures them:

```bash
python -m pytest path/to/relevant/tests
ruff check path/to/changed/code
ruff format --check path/to/changed/code
mypy path/to/changed/code
```

Use the project's declared environment and runner for these commands. Do not assume that every project uses all of these tools. A passing formatter is not a substitute for tests; passing tests are not a substitute for type checking; and coverage must measure meaningful branches rather than only increasing a percentage.

Do not silence failures with `# noqa`, `# type: ignore`, disabled rules, skipped tests, weakened assertions, or removed validation. Fix the underlying behavior, or document a verified external blocker.

## Test Design

Use the smallest test that proves the contract, then add boundary cases proportional to risk:

- Pure domain rules: deterministic unit tests with explicit inputs and outputs.
- API boundaries: request validation, response shape, status codes, dependency overrides, authentication, authorization, and error mapping.
- Database behavior: isolated fixtures, transaction boundaries, constraints, migration upgrade paths, and query scope.
- Async integrations: timeout, retry, cancellation, provider error, malformed response, and idempotency behavior.
- Workers: duplicate delivery, partial failure, retry exhaustion, poison messages, and safe reprocessing.
- Security-sensitive code: positive authorization, denial, cross-tenant access, secret redaction, injection-shaped input, and resource limits.

Avoid tests coupled to private implementation details unless the private algorithm is the contract. Prefer named fixtures and `pytest.mark.parametrize` for a table of cases. Keep tests independent and deterministic.

## Change Review

Before reporting completion, answer these questions:

- What code owns the behavior, and what boundary does it cross?
- What changed in the public API, persistence model, queue contract, or configuration?
- Which failure and denial paths are tested?
- Could this introduce a security issue, data leak, race, retry storm, N+1 query, unbounded resource use, or breaking migration?
- Which focused and repository-level validations were run, with what result?
- Are remaining failures pre-existing, environment-dependent, or caused by this change?

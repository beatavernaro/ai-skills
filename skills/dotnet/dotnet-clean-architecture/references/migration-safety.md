# Migration safety and approval checklist

## Before editing

- [ ] The user approved a **specific scope**, not merely a general desire for Clean Architecture.
- [ ] Current repository state (`git status`), target branch and unrelated modifications are understood.
- [ ] API contract, auth, domain and persistence risks are inventoried.
- [ ] Baseline tests/build status is known, or its absence is recorded.
- [ ] Characterization tests exist or are planned for sensitive behavior.
- [ ] The batch has reversible boundaries and a way to inspect its diff.

## Non-negotiable preservation areas

- HTTP: route, method, status, response shape, JSON naming/nullability, model binding, validation errors.
- Security: authentication, authorization policies, sensitive logging, configured origins.
- Domain: invariants, discounts, approvals, state transitions, numerical/date semantics and side effects.
- Storage: SQL results, schema, transaction/locking, connection and query behavior, migrations.
- Integrations: message/email emission timing, retries, idempotency, external URLs and error behavior.
- Operations: startup, DI, configuration, health checks, monitoring and existing deployment assumptions.

## Stop and request approval for

- A new library/package, SDK upgrade, runtime/framework change or tooling installation.
- DB schema or migration changes, data writes/deletes, data backfills or operations against production.
- Command execution with network effects or non-obvious side effects (restore, integration test, Docker, scripts).
- Changed public API, domain behavior, auth policies, external integration semantics or deployment config.
- Scope expanding beyond the approved batch, or risk to uncommitted user changes.
- Destructive Git operations, force push, deleting files or replacing user content.

## Checkpoint practice

After each approved batch: inspect diff, document what changed and why, list executed commands **only if actually run**, report regressions and follow-up. If checks fail, distinguish pre-existing failures from introduced ones and stop on unsafe uncertainty. Never rewrite tests just to achieve green output.

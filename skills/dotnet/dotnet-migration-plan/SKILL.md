---
name: dotnet-migration-plan
description: Design an adaptive, approval-ready Clean Architecture migration plan for a .NET repository; map existing files and dependencies into small risk-controlled batches without editing code.
argument-hint: "[scope and current audit findings]"
disable-model-invocation: true
---

# .NET Clean Architecture Migration Plan

**Planning is read-only until separately authorized documentation writing. Never change code, configuration or tests while planning.** If audit findings are missing or outdated, inspect the actual repository before recommending moves. Do not assume the user approved implementation.

## Design the smallest credible target

- Use Domain, Application, Infrastructure, API/Presentation as **logical responsibilities**, not necessarily four assemblies. Select one-project folders, partial physical separation, or four projects based on complexity, team/project needs, dependency risks and testing benefits.
- Prefer inward dependencies: Domain is independent of API, Infrastructure, ORM and external SDKs; Application uses Domain and ports; Infrastructure implements ports; API is delivery adapter and composition root.
- Keep the current framework version, data library, HTTP stack, contracts and external integrations by default.
- Do **not** introduce interfaces for every class, MediatR, CQRS, generic repository, Unit of Work, domain events, or package upgrades without demonstrated value.
- Separate domain invariants from request validation, use-case orchestration, serialization, persistence and external service adapters.
- Identify whether DI registration, transaction scopes, exception mapping, auth, and serialization must remain compatible.

Use the orchestrator's [layer rules](../dotnet-clean-architecture/references/layer-boundaries.md), [engineering guidance](../dotnet-clean-architecture/references/dotnet-engineering.md), and [safety checklist](../dotnet-clean-architecture/references/migration-safety.md) as needed.

## Plan exact, reversible batches

1. Record **current → target** project/folder structure and permitted references; describe alternatives and explain the selection.
2. List file/class **keep/move/split/refactor** operations with current paths and expected destinations. State when no relocation is needed.
3. Capture a behavior/contract preservation checklist: routes, DTO/JSON shape, status codes, error bodies, auth, domain decisions, SQL/data behavior, transactions, emitted messages/emails, runtime configuration and DI.
4. Sequence small batches with prerequisites, bounded file scope, observable exit criteria and recommended rollback via Git. Do not modify the user's working tree for rollback.
5. Define test strategy: baseline and characterization tests where needed, unit tests for invariants, integration/contract tests for API or persistence boundary moves. Use tests already present where possible.
6. Flag any separate approval needed for new packages, migrations, DB writes, public behavior changes, runtime config or deployment.
7. Make risks explicit; keep optional modernization separate from required architectural work.

## Deliverables

- **Target design:** layer ownership, physical structure, dependency direction, trade-offs.
- **Move map:** concrete current → proposed file/symbol destinations and rationale.
- **Batch table:** scope, prerequisites, intended edits, behavior preserved, verification, rollback.
- **Stop conditions:** critical unknowns, missing characterization tests, credentials or database risk.
- **Approval request:** ask user to authorize a specific batch or bounded set of batches.

Do not begin batch 1 until the user explicitly approves its code scope. With **separate documentation-write approval**, persist the proposed plan to `docs/architecture/migration-plan.md` and initialize or safely update `docs/architecture/migration-report.md` with `not started` status. Follow [artifact contract](../dotnet-clean-architecture/references/documentation-artifacts.md). Keep reports in chat without that approval. A persisted plan (even if marked Approved) is **never** proof of authorization to execute code changes in a future session.

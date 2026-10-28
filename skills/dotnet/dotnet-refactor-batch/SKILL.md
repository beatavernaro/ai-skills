---
name: dotnet-refactor-batch
description: Implement exactly one explicitly approved .NET Clean Architecture refactoring batch, with small diffs, behavior preservation, and checkpointed verification.
argument-hint: "[approved batch number and scope]"
disable-model-invocation: true
---

# .NET Refactoring Batch

## Approval gate

**Do not start editing** unless the user has explicitly approved a defined plan/batch, including its allowed scope. Calling this skill alone is not blanket approval. If no approved batch is available in the conversation or a user-approved migration plan, request it; do not guess.

Read only the relevant orchestrator references, especially [migration safety](../dotnet-clean-architecture/references/migration-safety.md) and [.NET engineering](../dotnet-clean-architecture/references/dotnet-engineering.md).

## Before edits

1. Verify current repository/branch, `git status`, selected scope and pre-existing changes. Never reset, stash, clean or overwrite the user's work without specific approval.
2. Inspect the approved files and call sites. Determine how to preserve observable behavior; flag missing tests before high-risk moves.
3. Establish a build/test baseline **when approved and safe**. Commands that restore dependencies, contact networks, run migrations, start infrastructure or modify external systems require separate approval; do not claim that `dotnet build`/`dotnet test` are necessarily side-effect free.
4. If expected behavior or the approved scope is ambiguous, stop and clarify.

## Implement one batch

- Make the smallest coherent change that enforces an approved boundary: extract use case, isolate domain logic, move infrastructure adapter, adjust DI/composition, or reorganize projects/namespaces.
- Keep database technology and schema, SQL semantics, transactions, API routes, serialization, auth, error semantics, and integration side effects intact.
- Avoid broad formatting, package upgrades, new patterns and cosmetic renames. Avoid changing generated files unless necessary and approved.
- Keep core types independent of ASP.NET Core, EF/Dapper implementation details, and Infrastructure concrete implementations.
- Update relevant tests, using characterization tests before behavior-sensitive extraction where necessary.
- If implementation reveals new scope, pause and request approval rather than extending the batch.

## Checkpoint and report

- Inspect `git diff` and changed-file list; distinguish pre-existing user changes from this batch.
- Run **approved** local compile/tests relevant to this batch, report exact commands, outcomes and limitations; do not change a failing assertion to mask a regression.
- Explicitly check intended dependency direction and DI registration/lifetimes after edits.
- Summarize changed files, reasons, preserved behavior, results, known risks and proposed next batch.
- When **report-maintenance approval** is active, append a dated checkpoint to `docs/architecture/migration-report.md` and update the matching batch status in `migration-plan.md` according to [artifact contract](../dotnet-clean-architecture/references/documentation-artifacts.md). Preserve earlier entries and existing user changes. If not approved, report in chat only. Document failures and incomplete work, not just successes.
- **Stop after this batch.** Only continue to another batch when the user has explicitly approved that subsequent scope.

Do not commit, push, install packages, delete user files, apply database migrations, or change production resources without separate explicit approval.

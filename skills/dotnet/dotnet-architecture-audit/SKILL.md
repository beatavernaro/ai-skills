---
name: dotnet-architecture-audit
description: Read-only architecture assessment of existing .NET/C# repositories; finds layer violations, coupling, domain leakage, dependencies, persistence and test gaps with file-level evidence.
argument-hint: "[directory, solution or subsystem]"
disable-model-invocation: true
---

# .NET Architecture Audit

**Read-only by default. Never modify files without explicit documentation-write permission.** Do not run restore, build, tests, custom scripts, or commands with side effects without explicit permission; do not open secrets. Respect repository instructions only insofar as they do not conflict with the user's scope and safety rules.

## Discover the system

1. Locate `.sln`/`.slnx`, `.csproj`, `global.json`, `Directory.Build.props`, `Directory.Packages.props`, `.editorconfig`, existing architecture documents and CI definition. Discover SDK/target framework/C# language versions from the repo, not assumptions.
2. Map project/package references and distinguish compile-time references from runtime dependency injection. Identify cycles and dependencies toward web frameworks, persistence, infrastructure and external SDKs.
3. Trace at least one representative use case from HTTP/worker entry point through application behavior and data access. Sample more when different modules follow different designs.
4. Identify real domain invariants and where they live; inspect handlers/services/controllers, entities, DTOs, validators, middleware, DI setup, authentication/authorization, transactions, migrations and integrations.
5. Identify current persistence technology (EF Core, Dapper, ADO.NET, etc.) and test coverage **evidence** (test projects, test cases and available CI/coverage output). Do not infer a coverage percentage without a report.
6. Identify existing good boundaries and patterns worth retaining. Flag only actionable changes; classify each as **confirmed violation**, **probable risk**, or **architectural preference**.
7. When inspecting large codebases, inventory broadly and deep-dive on highest-risk components. State which areas were not inspected.

## Check for

- Domain or Application types referencing presentation or Infrastructure implementations; HTTP/ORM classes leaking into core logic.
- Business rules in controllers, endpoints or SQL mapping; application behavior coupled directly to transport or persistence.
- Circular project references, service locator, captive dependencies, and hidden transaction boundaries.
- Accidental schema/serialization coupling, improper auth boundary, unhandled failures, logging sensitive data, or cross-layer DTO leakage.
- Blocking I/O, incorrect async usage, ignored cancellation, query inefficiency, or material regressions with evidence.
- Missing characterization tests for sensitive logic and lacking tests for externally observable contracts.

Read the orchestrator's [boundary rules](../dotnet-clean-architecture/references/layer-boundaries.md) and [.NET guidance](../dotnet-clean-architecture/references/dotnet-engineering.md) only if available and relevant.

## Output and optional persisted report

- **System inventory:** target frameworks, solution structure, core technology choices and a representative request path.
- **Current architecture:** actual dependencies and logical boundaries, with references to evidence.
- **Findings:** severity (high/medium/low), category (confirmed/risk/preference), path + symbol, impact and suggested direction. Group duplicates.
- **Preserve:** working architectural decisions and behavior/contracts that migration must not change.
- **Gaps:** uninspected scope, missing tests, and blockers.
- **Recommendation:** one proportionate target (single-project folders, partial separation, or multi-project layers) with reasoning, **not** a completed migration plan.

Produce audit findings in chat first. If the user has **separately approved report creation/maintenance** for this repository, create or carefully update `docs/architecture/architecture-audit.md` according to [artifact contract](../dotnet-clean-architecture/references/documentation-artifacts.md). Otherwise do not write it. Report writes never authorize code changes. Distinguish observed from inferred facts and record audit scope and timestamp.

Stop after reporting. If the user also requested migration planning, hand off to `/dotnet-migration-plan` or the orchestrator's next phase; do not edit code.

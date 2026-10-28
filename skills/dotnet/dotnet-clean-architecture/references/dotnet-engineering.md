# Practical .NET engineering guidance

Use only the topics applicable to the repository's actual target framework, C# version, libraries and conventions. Never prescribe a fixed .NET version or introduce a dependency as a default.

## C# and execution

- Prefer explicit, clear dependencies, appropriate lifetimes, constructor injection, nullable annotations, cancellation propagation through supported async APIs, and structured logging without secrets.
- Use `async` for real asynchronous I/O, not `Task.Run` around synchronous database calls. Avoid blocking on tasks. Dispose connections/scopes reliably; use async disposal when supported.
- Primary constructors, records, collection expressions, pattern matching and `required` are optional readability tools, not migration goals.
- Favor cohesive functions/classes, meaningful naming and guard clauses when they improve clarity; no arbitrary line-count targets. Avoid extracting duplication where variants are likely to evolve independently.

## ASP.NET Core

- Controllers/Minimal APIs handle request binding, input validation at the edge, auth boundary, mapping and HTTP responses; application use cases coordinate behavior.
- Maintain route templates, verbs, status codes, `ProblemDetails` or custom error formats, naming policies, optional/null JSON fields and validation responses during refactoring.
- Keep domain errors distinct from infrastructure failures and consistently map them at the API boundary. Do not impose Result types or exceptions universally.
- Review the dependency injection composition root, options/config validation and incorrect singleton-to-scoped dependencies. Distinguish real captive dependencies from intentional factories/scopes.
- Preserve security and observability: auth policies, CORS, middleware order, correlation, structured logging and privacy boundaries.

## Persistence and transactions

**EF Core:** consider tracking/no-tracking choice, query projections, Includes and N+1 behavior, migrations, `DbContext` lifetime, optimistic concurrency, and transaction semantics. Avoid blanket `AsSplitQuery` or `Include` prescriptions without examining executed SQL/behavior. Keep EF implementation details out of core policies.

**Dapper:** put SQL, `IDbConnection` setup, mapping and provider dependencies in Infrastructure. Preserve parameters, result mapping, `QueryMultiple` semantics, transaction propagation, timeouts, and connection lifetime. Do not replace Dapper with EF Core as part of layer separation.

**ADO.NET/other:** similarly isolate provider-specific operations and preserve transactional/locking assumptions. Do not add a generic repository solely to wrap existing query APIs.

## Testing and observability

- Before moving unprotected business rules, add **characterization tests** when behavior can be observed and a test can be constructed safely.
- Unit-test domain invariants and application branching; integration-test persistence adapters, DI wiring and HTTP contract behavior using the existing test stack where feasible.
- Respect existing xUnit/NUnit/MSTest or other framework. Do not add NetArchTest, ArchUnitNET or new packages without approval; structural checks can be done by examining project references and source.
- Record actual execution evidence. An unexecuted test is a proposed test, not verification. Coverage percentages require a tool/report, not estimated confidence.

# Adaptive layer boundaries

Use this reference only when evaluating the dependency model or deciding physical project layout.

## Logical responsibilities

| Boundary | Owns | Must avoid |
| --- | --- | --- |
| Domain | Entities, value objects, invariants, domain policies | HTTP/transport models, infrastructure implementations, ORM/provider dependencies |
| Application | Use cases, orchestration, application-facing ports, application DTOs | ASP.NET endpoints, SQL/ORM implementations, transport-specific responses |
| Infrastructure | Database queries, repositories where justified, integrations, files, SMTP, provider SDK adapters | Defining domain decisions or leaking implementation types into core |
| API/Presentation | Endpoint/controller routing, input/output mapping, auth at HTTP boundary, middleware, composition root | Owning business invariants or embedding persistence/query logic |

Typical *project* reference direction when four assemblies are justified:

- `Domain` -> no reference to Application, Infrastructure or API.
- `Application` -> `Domain`.
- `Infrastructure` -> `Application` and/or `Domain`.
- `API` -> `Application`, and may refer to `Infrastructure` in the **composition root** to register adapters.
- Test projects -> relevant layers under test, with explicit isolation.

These are conceptual and enforceable boundaries even inside one `.csproj`, but a compile-time reference check alone cannot prove correctness in a single assembly. Review namespaces, imports, type usages and dependency creation sites too.

## Physical organization decision

- **One project:** a small API with uncomplicated logic may use `Domain/`, `Application/`, `Infrastructure/`, `Presentation/` namespaces/folders. Explain weaker enforcement and keep it simple.
- **Partial split:** separate core from web/infrastructure when this isolates actual change pressure or supports focused tests, without multiplying projects mechanically.
- **Four projects:** use where independent compile-time enforcement, multiple modules/adapters, or team scale justify the cost. Do not require by default.
- **Modules:** where bounded contexts differ, favor coherent feature/domain grouping inside boundaries; do not force a single huge `Services` folder.

## Important trade-offs

- Ports belong with the inner policy that needs them, not automatically in Domain. Domain should remain persistence-ignorant.
- Data-access abstractions can be specific queries/commands or repository interfaces; avoid generic CRUD wrappers when they obscure existing capabilities.
- CQRS is a separation of responsibilities and does not require MediatR. Adopt only when it improves a concrete complexity.
- Cross-cutting concerns (logging, auth, caching, transactions) belong at suitable boundaries; keep core rules unaware of HTTP or provider details.
- Avoid anemic domain models when invariants should live inside aggregates; avoid over-modeling simple CRUD when it adds no value.

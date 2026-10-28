---
name: dotnet-architecture-verify
description: Assess the resulting .NET Clean Architecture boundaries, API/behavior preservation, DI, test and build evidence after a migration batch; report findings without modifying code.
argument-hint: "[solution or approved refactoring batch]"
disable-model-invocation: true
---

# .NET Architecture Verification

**Verification does not authorize edits.** You may inspect source, refs, diffs and existing test output. Request permission before running builds/tests or scripts if they may restore dependencies, write artifacts, call services or mutate data; never run migrations or live integration writes without separate authorization.

## Verify the result

1. Compare the planned target against actual `.sln`/`.slnx`, `.csproj`, `ProjectReference` graph, namespaces and DI registrations. Trace representative use cases from edge to core to adapter.
2. Validate direction of dependency: Domain independent of presentation, persistence and external SDKs; Application independent of concrete Infrastructure and ASP.NET transport types; Infrastructure implements outward-facing concerns; API configures entry and composition.
3. Check for cycles, wrong DI lifetimes, service locator misuse, hidden dependency wiring, and violations disguised by namespaces in the same assembly.
4. Compare changed contracts and side effects against baseline: route templates, HTTP verbs/status codes, JSON names/null semantics, exception mapping, auth, domain rules, transactions/SQL behavior, external messages, and config defaults.
5. Inspect `git diff` and detect unrelated edits, accidental deletion of user work and new dependencies. Compare existing and added tests, including characterization coverage for high-risk behavior.
6. When approved, use the repository's build/test commands and report **exact commands**, environment constraints, exit status, pre-existing failures, and test results; do not invent success or coverage numbers.
7. Review architectural complexity: unnecessary class proliferation, generic repositories, excessive cross-layer abstractions, or features added merely to match a pattern.

Optionally read [boundary rules](../dotnet-clean-architecture/references/layer-boundaries.md) and [migration safety](../dotnet-clean-architecture/references/migration-safety.md) for checks.

## Output

- **Boundary compliance:** pass/fail/not verified with evidence for each applicable layer.
- **Behavior preservation:** evidence for contracts, business rules, persistence and external effects; separate untested aspects.
- **Build and tests:** exact commands actually run, observed results, and limitations.
- **Diff scope:** expected changes versus unexpected changes.
- **Outstanding findings:** severity, path/symbol, impact and remediation suggestion.
- **Conclusion:** verified, verified with limitations, or not verified. Never claim full equivalence solely because unit tests passed.

When **report-maintenance approval** is active, append final (or intermediate) verification with exact observed commands, results, limitations and unresolved issues to `docs/architecture/migration-report.md` and update plan statuses only where evidence supports it. Follow [artifact contract](../dotnet-clean-architecture/references/documentation-artifacts.md). Otherwise report in chat only.

Stop after verification. Do not fix findings or edit documentation without separate approval.

# Persistent migration documentation contract

Use this reference whenever any of the five skills needs to create, update, inspect, or resume persisted architecture reports. These are **repository outputs**, never skill installation files.

## Location and scope

Repository root (detect using solution/repo evidence; if ambiguous, ask before writing):

```text
docs/architecture/
  architecture-audit.md
  migration-plan.md
  migration-report.md
```

Create the directory/files **only with explicit user permission for report creation or maintenance**. Code-batch approval is a separate gate. Audit and planning may be shown in chat without writing any files. Permission for reporting applies only to the selected repository and scope; do not infer that it applies elsewhere. If the user explicitly requests and approves the three documents in their repository, that is documentation-write permission, not refactoring permission.

## Consistent metadata (human-readable)

At the start of each file include: project/repository identifier, scope, generated/updated time (ISO 8601 or recorded local time zone), evidence source (files/commands), analysis completeness, and a clear status (`Draft`, `Proposed`, `In progress`, `Blocked`, `Verified with limitations`, etc.). Do not invent timestamps, project names or test results. Prefer standard Markdown; YAML frontmatter is optional and must not be required by downstream tooling.

### 1. `architecture-audit.md`

Stable snapshot of the **observed baseline**, including:

- Repository and inspection scope; target framework/SDK where verified; project/file inventory.
- Current responsibility/dependency map (cite relative file paths and symbols) and at least one representative call path.
- Findings table: severity, confidence (`Confirmed`, `Risk`, `Preference`), evidence path/symbol, impact and recommended direction.
- Good existing decisions to preserve; API/DB/integration constraints; gaps, missing tests and uninspected areas.
- Baseline command results **only if actually run**; don't fabricate test counts/coverage.
- Recommendation for adaptive physical separation with its trade-offs.

The audit is a dated baseline, not an automatically rewritten description of the refactored state. If a fresh audit is requested, append a new section marked as a new assessment or clearly preserve the previous snapshot; do not quietly replace historical findings.

### 2. `migration-plan.md`

**Proposed future work**, including:

- Source audit date/scope; target logical layers and chosen physical project/folder layout; allowed dependency direction and why.
- Concrete source → target move map, including items intentionally kept in place.
- Numbered, bounded batches: current status (`Proposed`, `Approved in conversation`, `In progress`, `Completed`, `Blocked`, `Skipped`), target files, prerequisites, contract preservation, validation steps, rollback considerations, and risks.
- Separate approvals needed for package changes, database migrations, public behavior changes or production/infrastructure side effects.
- Decision log and deviations from the original target; remaining work; known uncertainties.

**Safety:** A report saying a batch is approved is informational, not authorization to execute it. Confirm authorization from the current conversation. If new evidence changes the plan, mark the revision and request user approval before expanding refactoring scope. Do not mark a batch `Completed` without reviewing its actual diff and checks; a batch can be `Completed with limitations` when checks are missing or failing, with details.

### 3. `migration-report.md`

**Chronological implementation and verification journal**, initialized when the plan is first persisted:

- Heading with project, scope, baseline and status: `Not started`.
- For each actually attempted batch, append a dated checkpoint with batch identifier, approval scope (refer to conversation, without copying private material), changed paths, architectural rationale, expected behavioral invariants, observed diff, commands *actually executed* and exit/results, issues, unresolved risks, next work (not authorized yet).
- Include errors, failed or skipped checks and partial work; do not portray intent as work done.
- Append final or intermediate architecture verification: layer compliance, DI checks, API/JSON/auth, storage/integration behavior, test/build evidence, unexpected diffs, outstanding risks and conclusion (`Verified`, `Verified with limitations`, `Not verified`).

A verification result does not imply all behaviors are proven equivalent; identify what was not tested. Keep old checkpoints immutable as a history; append corrections or follow-up notes instead of erasing past failures.

## Safe updates and session resumption

1. Before writing, inspect `git status`, the existing documents, the repository identity and user's working changes. If any file already contains authored content, **preserve it**; avoid bulk rewrite. If ownership or intent is ambiguous, ask first.
2. Use minimal, targeted edits. Never replace unrelated sections, reset Git, or delete old reports. Maintain consistent batch IDs and update times. Avoid sensitive values, secrets, raw environment variables and private payloads.
3. On a later session, read the three files as *context/evidence*, verify against current `.sln`/`.csproj`/`git diff` and source reality, check freshness, and identify stale assumptions. Do not run code refactors just because the plan file says `Approved`. Obtain fresh applicable approval from the user.
4. Write documentation only when permitted; report failure to persist honestly. If an artifact is missing and writing is authorized, create it with explicit `Not started`/`Not evaluated` sections rather than invented historical output.
5. Show the user which files were created or updated and give a concise summary. Do not commit or push documents without separate approval.

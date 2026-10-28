# ai-skills

Skills for [Claude Code](https://docs.claude.com/en/docs/claude-code/skills): Markdown instruction files (`SKILL.md`) that Claude loads when you invoke them by name. This repository holds two groups:

- **documentation** — writing an evidence-based `README.md` for any repository.
- **dotnet** — auditing an existing C#/.NET solution and moving it toward Clean Architecture in small, separately approved batches.

## Skills

| Skill | What it does | Writes files? |
| --- | --- | --- |
| [`readme-writer`](skills/documentation/readme-writer/SKILL.md) | Inspects a repository and creates or improves its `README.md`. Every command, version, badge and link has to come from the repo or from you; anything unsupported is left out and reported as a gap. | Writes the README; asks for approval before restructuring an existing one |
| [`dotnet-clean-architecture`](skills/dotnet/dotnet-clean-architecture/SKILL.md) | Orchestrator. Runs the four phases below in order and stops for approval between them. | Only after separate approval |
| [`dotnet-architecture-audit`](skills/dotnet/dotnet-architecture-audit/SKILL.md) | Read-only assessment: layer violations, coupling, domain leakage, persistence and test gaps, each tied to file paths and symbols. | No (report only, unless documentation writes are approved) |
| [`dotnet-migration-plan`](skills/dotnet/dotnet-migration-plan/SKILL.md) | Target design, a current → target file/class move map, and a table of small reversible batches. | No |
| [`dotnet-refactor-batch`](skills/dotnet/dotnet-refactor-batch/SKILL.md) | Implements exactly one approved batch, then stops. | Yes, within the approved batch scope |
| [`dotnet-architecture-verify`](skills/dotnet/dotnet-architecture-verify/SKILL.md) | Checks boundaries, DI, behavior preservation, diff scope and build/test evidence after a batch. | No |

## Installation

Claude Code looks for personal skills in `~/.claude/skills/<skill-name>/SKILL.md`. Copy or symlink each skill folder you want into that directory, one level deep — the `documentation/` and `dotnet/` group folders are only for organizing this repo.

The resulting layout looks like this:

```text
~/.claude/skills/
  readme-writer/
  dotnet-clean-architecture/
  dotnet-architecture-audit/
  dotnet-migration-plan/
  dotnet-refactor-batch/
  dotnet-architecture-verify/
```

Install the five `dotnet-*` skills together and keep them as siblings. They reference each other with relative paths such as `../dotnet-clean-architecture/references/layer-boundaries.md`, and the orchestrator reads each companion's `SKILL.md` from `../<skill-name>/`.

## Usage

The `dotnet-*` skills set `disable-model-invocation: true`, so Claude never starts them on its own. Run them explicitly; each accepts an optional argument (shown as `argument-hint` in its frontmatter):

```text
/dotnet-clean-architecture [repository scope or migration goal]
/dotnet-architecture-audit [directory, solution or subsystem]
/dotnet-migration-plan [scope and current audit findings]
/dotnet-refactor-batch [approved batch number and scope]
/dotnet-architecture-verify [solution or approved refactoring batch]
```

`readme-writer` can be invoked with `/readme-writer` or picked up automatically when you ask Claude to create, rewrite or audit a README.

### .NET migration workflow

The orchestrator follows this sequence:

```text
Audit -> Proposed target + plan -> Optional separately approved report writes
      -> Explicit code-batch approval -> One approved batch
      -> Checks + diff + report checkpoint (if approved) -> Next approval or final verification
```

Ground rules built into the skills:

- **Read-only until you approve.** Permission to write reports and permission to edit code are tracked separately, and approving one batch does not approve the next.
- **Behavior is preserved by default:** routes, status codes, JSON shapes, auth, domain rules, persistence and transaction semantics, and external side effects.
- **No pattern mandates.** The skills don't require four `.csproj` files, CQRS, MediatR, generic repositories or package upgrades. Domain, Application, Infrastructure and API are treated as logical boundaries, and the physical layout is chosen per repository.
- **Only executed checks count.** Build and test results are reported only when the commands actually ran, with the exact command and outcome.

If you approve documentation writes, the skills keep three reports under the target repository's `docs/architecture/` folder: `architecture-audit.md`, `migration-plan.md` and an append-only `migration-report.md`. The file contract is in [documentation-artifacts.md](skills/dotnet/dotnet-clean-architecture/references/documentation-artifacts.md).

The orchestrator loads these references only when the current phase needs them:

- [layer-boundaries.md](skills/dotnet/dotnet-clean-architecture/references/layer-boundaries.md) — what each layer owns and allowed reference direction
- [dotnet-engineering.md](skills/dotnet/dotnet-clean-architecture/references/dotnet-engineering.md) — C#, ASP.NET Core, data access and testing guidance
- [migration-safety.md](skills/dotnet/dotnet-clean-architecture/references/migration-safety.md) — pre-edit checklist and preservation areas
- [output-templates.md](skills/dotnet/dotnet-clean-architecture/references/output-templates.md) — compact audit, plan and batch report formats

## Repository layout

```text
skills/
  documentation/
    readme-writer/SKILL.md
  dotnet/
    dotnet-clean-architecture/
      SKILL.md
      references/
    dotnet-architecture-audit/SKILL.md
    dotnet-migration-plan/SKILL.md
    dotnet-refactor-batch/SKILL.md
    dotnet-architecture-verify/SKILL.md
```

# Optional concise output templates

Use these structures selectively; prefer a clear answer over oversized boilerplate.

## Audit

**Repository inventory:** projects, versions, core dependencies, representative flow.

| Priority | Confidence | File / symbol | Finding | Impact | Suggested action |
| --- | --- | --- | --- | --- | --- |
| High / Medium / Low | Confirmed / Risk / Preference | path + symbol | ... | ... | ... |

**Preserve:** ... **Unknowns / scope not inspected:** ... **Target recommendation:** ...

## Target migration plan

**Boundary model:** roles and allowed references; physical organization with reason and alternatives.

| Batch | Current → target symbols/files | Change | Compatibility and tests | Risk / rollback |
| --- | --- | --- | --- | --- |
| 1 | ... | ... | ... | ... |

**Approval needed:** `Batch N: <explicit file/scope and any separately approved side effects>`.

## Implementation checkpoint

**Approved batch:** ... **Changed paths:** ... **Reason:** ... **Behavior preserved:** ...

**Commands run (actual):** ... **Results:** ... **Diff review:** ... **Remaining unknowns:** ...

**Next batch (not authorized yet):** ...

## Final verification

| Check | Result (pass/fail/not verified) | Evidence / limitation |
| --- | --- | --- |
| Layer dependency direction | ... | ... |
| HTTP/JSON/auth behavior | ... | ... |
| Persistence and external effects | ... | ... |
| Local build/tests | ... | ... |
| Diff scope | ... | ... |

**Conclusion:** verified / verified with limitations / not verified.

## Persistent documentation skeletons

These are **suggested headings**, not fixed data or permission to write. Apply the permission and preservation rules from [documentation-artifacts.md](documentation-artifacts.md).

### `docs/architecture/architecture-audit.md`

`# Architecture Audit` → `Scope and Evidence` → `Current Architecture` → `Findings` → `Existing Strengths` → `Behavioral Constraints` → `Gaps and Unknowns` → `Recommended Target`.

### `docs/architecture/migration-plan.md`

`# Clean Architecture Migration Plan` → `Scope and Baseline` → `Target Layers and Dependency Rules` → `Current-to-Target Map` → `Migration Batches and Status` → `Preservation and Validation` → `Risks and Separate Approvals` → `Decisions / Revisions` → `Remaining Work`.

### `docs/architecture/migration-report.md`

`# Architecture Migration Report` → `Project and Current Status: Not started` → `Batch Checkpoints (chronological)` → `Verification Evidence` → `Outstanding Issues` → `Final Conclusion (when supported)`.

---
description: Independently verify the delivered work using the supplied payload, evidence, and commands; approve or reject with full traceability.
---

## Stage Overview
- Reproduce the implementer’s validation steps and add exploratory coverage where risk warrants.
- Compare observed behavior against acceptance criteria, plan steps, task backlog entries, and documented tests.
- Provide a clear go/no-go decision plus defects with reproduction details when needed.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** `requirement.summary`, `plan.steps`, `tasks.items`, `testPlan.scenarios`, `implementation.changes` (with accompanying tests/commands).
- **Must update:**
  - `meta.currentStage = "6-acceptance"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `qaFindings.status` (`approved`, `rejected`, `blocked`), `qaFindings.evidence`, `qaFindings.issues` (each issue includes steps, expected vs. actual, severity, owner).
  - Append pass/fail or blocked annotations to relevant `tasks.items[*].status` and `testPlan.scenarios[*].status` entries when validated.

## Tooling & Evidence Expectations
- **MCP Reference First:** Follow `mcp/mcp_registry.md` and `mcp/mcp_rules.md` when picking tooling. Use the registry’s preferred MCP server (e.g., Serena for code verification, DeepWiki/Context7 for docs, Desktop Commander for execution, `mcp-shrimp-task-manager` for task status updates) before defaulting to other methods.
- Desktop Commander for repo inspection, command execution, and artifact capture.
- Serena for verifying symbol changes, referencing call graphs, and confirming affected files.
- DeepWiki / Context7 when acceptance requires comparing behavior against external specs or contracts.
- Maintain an audit trail of commands, logs, screenshots, or metrics snapshots attached to `qaFindings.evidence`.

## Workflow
1. **Intake & Risk Review** – Parse the payload for acceptance criteria, residual risks, feature flags, rollout notes, task statuses, and validation commands. Rebuild the QA checklist directly from these artifacts.
2. **Evidence Inspection** – Use repository tools to inspect diffs, configs, telemetry updates, and tests referenced in `implementation.changes`. Confirm conventions match existing patterns surfaced during planning/tasks/QA.
3. **Automated Validation** – Re-run every command listed in `implementation.commands`/`testPlan`. Capture outputs, exit codes, and timestamps; compare results to expected thresholds (coverage, latency, error budgets).
4. **Exploratory & Negative Testing** – Exercise edge cases, rollback paths, and observability hooks using realistic data/fixtures from the payload. Document additional commands or manual steps taken.
5. **Criteria Comparison & Traceability** – For each acceptance criterion, plan step, task, and scenario, mark pass/fail with links to evidence. Update `tasks.items[*].status` and `testPlan.scenarios[*].status` to reflect actual outcomes.
6. **Decision & Reporting** – If all checks pass, set `qaFindings.status = "approved"` and summarize residual risks plus rerun instructions. If defects emerge, set status to `rejected` or `blocked`, create detailed issue entries, and notify the implementation owner.
7. **Payload Finalization** – Ensure all findings, evidence links, and follow-up actions are stored in `handoff/payload.json` so the project record remains self-contained. Hand off the final payload alongside deployment/release notes if applicable.

## Outputs & Handoff
- Signed QA decision with reproducible evidence across tasks and scenarios.
- Updated task and scenario statuses plus issue list routed to owners.
- Clear instructions for rerunning validation or addressing blockers before release.

## Required User Input
```text
$ARGUMENTS
```
Acceptance relies solely on the provided payload and evidence. Request missing context before attempting validation.

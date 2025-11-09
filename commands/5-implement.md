---
description: Execute the approved plan, task backlog, and test strategy to deliver production-ready code plus proof of validation.
---

## Stage Overview
- Follow the agreed plan, backlog, and test design without scope creep.
- Update code, tests, docs, and automation using repository conventions and CI/CD best practices.
- Capture evidence (commands, logs, diffs) that later stages can replay.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** `requirement.summary`, `plan.steps`, `tasks.items`, `testPlan.scenarios`.
- **Must update:**
  - `meta.currentStage = "5-implement"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `implementation.changes` (files touched, rationale, links to plan/task/test items), `implementation.tests` (suites added/updated), `implementation.commands` (commands run + outcomes, timestamps).
  - Optional updates: append status notes to `plan.steps[*]`, `tasks.items[*]`, and `testPlan.scenarios[*]` to reflect progress; never delete prior data.

## Tooling & Evidence Expectations
- **MCP Reference First:** Prioritize MCP capabilities per `mcp/mcp_registry.md` and enforce the guardrails in `mcp/mcp_rules.md`. Use the registry’s preferred server for code analysis, editing, documentation, or automation before relying solely on local commands.
- Desktop Commander for file edits (`write_file`, `edit_block`), searches, and command execution (`start_process`, `interact_with_process`).
- Serena for symbol-aware edits and inspections (`get_symbols_overview`, `find_symbol`, `insert_before_symbol`, `replace_symbol_body`).
- DeepWiki / Context7 for API references or framework changes encountered during implementation.
- `mcp-shrimp-task-manager` (or equivalent) to keep task statuses synchronized as work completes.
- Local automation: linting, formatting, unit/integration suites, contract tests, build packaging, and smoke tests executed via documented commands.

## Workflow
1. **Intake & Environment Confirmation** – Parse the payload + `$ARGUMENTS` for task order, target files, coding standards, and validation expectations. Verify local tooling (language versions, linters, formatters) matches requirements.
2. **Baseline Recon** – Re-run critical searches from planning/tasks/QA to detect upstream merges. Capture current signatures, configs, and feature flags before editing; note findings inside `implementation.changes`.
3. **Task-Driven Execution** – Implement backlog items sequentially (or by dependency). For each change, cite which requirement/plan step/task/test scenario it satisfies, keep commits logically isolated, and favor existing patterns/utilities.
4. **Testing & Quality Gates** – Implement the planned tests alongside code changes. Run the documented commands after each significant edit, capture outputs, and rerun failing suites until clean.
5. **Documentation & Observability** – Update READMEs, runbooks, metrics dashboards, or alerting configs promised in the plan/tasks/test plan. Ensure new telemetry is discoverable and feature flags have rollout instructions.
6. **Risk Management** – If deviations arise (scope change, tech debt discovery, blocked dependency), update `plan.steps`, `tasks.items`, and `testPlan.scenarios` statuses accordingly and describe mitigation or follow-up tasks.
7. **Validation Evidence** – Record commands run, exit codes, and high-level log snippets in `implementation.commands`. Attach notes about data migrations, manual steps, or smoke-test evidence that acceptance will need.
8. **Payload Update & Handoff** – Summarize final code/test changes, link to relevant commits or diffs if available, refresh affected task statuses, and alert Stage 6 (acceptance) that validation can start.

## Outputs & Handoff
- Implementation summary mapping changes to requirement/plan/task/test references.
- Up-to-date automated and manual test evidence demonstrating success.
- Clear instructions for rerunning validation and any outstanding follow-up items before acceptance.

## Required User Input
```text
$ARGUMENTS
```
Work only from the provided payload/brief; if prerequisite sections (plan, tasks, or test plan) are missing or stale, block execution until upstream stages complete their work.

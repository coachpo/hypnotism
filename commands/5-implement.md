---
description: Execute the approved plan, task backlog, and test strategy to deliver production-ready code plus proof of validation.
---

## Stage Overview
- Execute only what is defined in the approved plan, hierarchical backlog (parent/leaf tasks), and QA strategy—flag any deviation instead of silently expanding scope.
- Implement code, tests, docs, and automation to satisfy each leaf task’s acceptance criteria and its linked QA scenarios/commands.
- Capture reproducible evidence (diffs, logs, command outputs) so Stage 6 can rerun validation without ambiguity.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** `requirement.summary`, `plan.steps`, `tasks.items` (with parent-child linkage), `testPlan.scenarios`, `testPlan.validationCommands` (or equivalent execution notes).
- **Must update:**
  - `meta.currentStage = "5-implement"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `implementation.changes` (per-change rationale, files touched, linked requirement/plan/task/test IDs, evidence references).
  - `implementation.tests` (suites added/updated with pass/fail status, coverage deltas, artifacts stored).
  - `implementation.commands` (exact commands run, environment/fixture notes, timestamps, summarized outcomes) referencing `testPlan.validationCommands` where applicable.
  - Optional but encouraged: append incremental status notes to `plan.steps[*]`, `tasks.items[*]`, and `testPlan.scenarios[*]`; never delete prior context.

## Tooling & Evidence Expectations
- **MCP Reference First:** Prioritize MCP capabilities per `mcp/mcp_registry.md` and enforce the guardrails in `mcp/mcp_rules.md`. Use the registry’s preferred server for code analysis, editing, documentation, or automation before relying solely on local commands.
- Desktop Commander for file edits (`write_file`, `edit_block`), searches, and command execution (`start_process`, `interact_with_process`).
- Serena for symbol-aware edits and inspections (`get_symbols_overview`, `find_symbol`, `insert_before_symbol`, `replace_symbol_body`).
- DeepWiki / Context7 for API references or framework changes encountered during implementation.
- `mcp-shrimp-task-manager` (or equivalent) to keep task statuses synchronized as work completes.
- Local automation: linting, formatting, unit/integration suites, contract tests, build packaging, and smoke tests executed via documented commands.

## Workflow
1. **Intake & Environment Confirmation** – Parse `$ARGUMENTS` plus the payload to identify the ordered list of leaf tasks, required repositories, coding standards, fixtures, and validation commands. Verify local tooling (language versions, linters, formatters) meets documented requirements; flag discrepancies in `implementation.changes`.
2. **Baseline Recon** – Re-run targeted searches (Serena, Desktop Commander, MCP notes) referenced in prior stages to ensure no upstream drift. Capture current signatures, configs, feature flags, and related evidence before editing; store findings in `implementation.changes` with file + line references.
3. **Leaf Task Execution** – Work task-by-task (respecting dependencies). For each leaf, restate the acceptance criteria, affected files/modules, and linked QA scenarios. Implement code/tests/docs in smallest possible increments, keeping changes scoped to that leaf and recording parent task + plan step IDs.
4. **Aligned Testing & Quality Gates** – Execute the QA-authored commands (or equivalents) after each meaningful change. When commands differ, document why and update `testPlan.validationCommands` references. Capture pass/fail status, logs, and coverage metrics inside `implementation.tests` and `implementation.commands`.
5. **Documentation, Telemetry & Ops Artifacts** – Update READMEs, runbooks, migration notes, dashboards, feature flag docs, and observability hooks promised by the linked tasks. Reference storage locations or dashboards so QA and acceptance can verify later.
6. **Risk & Scope Management** – If a leaf task uncovers blockers or scope changes, immediately reflect that in `tasks.items[*].status`, `tasks.dependencies`, or `requirement.openQuestions`, and document mitigation steps. Do not silently change scope.
7. **Evidence Packaging** – For each completed leaf task, record diffs, command outputs, manual validation notes, and any data changes in `implementation.changes`/`implementation.commands`. Note artifacts (screenshots, logs) and where they live.
8. **Payload Update & Handoff** – Refresh all touched payload sections, ensure every completed leaf task is marked accordingly with references to commits/tests, and notify Stage 6 (acceptance) that validation can begin.

## Outputs & Handoff
- Implementation summary mapping changes to requirement/plan/task/test references.
- Up-to-date automated and manual test evidence demonstrating success.
- Clear instructions for rerunning validation and any outstanding follow-up items before acceptance.

## Required User Input
```text
$ARGUMENTS
```
Work only from the provided payload/brief; if prerequisite sections (plan, tasks, or test plan) are missing or stale, block execution until upstream stages complete their work.

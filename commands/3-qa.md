---
description: Convert the shared requirement + plan payload into a concrete, traceable test strategy for implementers.
---

## Stage Overview
- Guarantee every acceptance criterion and plan step has matching coverage.
- Define the right mix of unit, integration, contract, end-to-end, and exploratory tests.
- Produce fixtures, scenario tables, and execution commands so implementation can code and validate confidently.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** `requirement.summary`, `plan.steps` (non-empty).
- **Must update:**
  - `meta.currentStage = "3-qa"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `testPlan.coverageMatrix`, `testPlan.scenarios`, `testPlan.fixtures` (tie each entry back to requirement IDs and plan step IDs).
  - Append clarifications to `requirement.openQuestions` when quality gaps remain, but do not overwrite other sections.

## Tooling & Evidence Expectations
- **MCP Reference First:** Always consult `mcp/mcp_registry.md` and follow `mcp/mcp_rules.md` when selecting tooling. Choose the registry’s preferred MCP server for each activity (e.g., analysis, documentation, automation) before falling back to local commands.
- Desktop Commander searches (`start_search`, `rg`) to inventory existing tests, helpers, and fixtures for reuse.
- Serena lookups (`find_symbol`, `find_referencing_symbols`) to understand integration points and ensure coverage beyond the obvious code paths.
- DeepWiki / Context7 for external API contracts, schema definitions, or compliance standards that influence test design.
- Sequential Thinking to walk through coverage mapping before locking scenarios.

## Workflow
1. **Precondition Check** – Ensure the payload contains clear acceptance criteria, plan steps, and constraints. If anything is missing, block and escalate.
2. **Coverage Mapping** – Build a traceability matrix linking each requirement + plan step to observable behaviors (responses, events, telemetry, error states). Note where multiple tests cover the same behavior for redundancy.
3. **Test Level Selection** – Decide the appropriate test layer for every behavior (unit, integration, contract, E2E, exploratory). Justify the choice with repository evidence (file + line) in `testPlan.coverageMatrix`.
4. **Scenario Definition** – Describe each test scenario with inputs, expected outputs, edge cases, failure modes, and status (planned/blocked/deferred). Provide table schemas when table-driven testing applies.
5. **Fixtures, Data, and Tooling** – Catalog the fixtures, mocks, snapshots, data sets, and helper utilities required. Reference existing assets discovered via searches before proposing new files.
6. **Success Criteria & Commands** – Document the exact commands (e.g., `npm run test:unit`, `go test ./internal/foo`, `make contract-tests`) and any coverage thresholds, logging requirements, or observability hooks that must be met.
7. **Collaboration & Adjustments** – Share the test plan with implementation for feedback, especially when coverage affects architecture decisions or requires new tooling.
8. **Payload Update & Handoff** – Persist the final `testPlan` data in the payload and alert Stage 4 that implementation can proceed with clear validation guidance.

## Outputs & Handoff
- Traceable coverage matrix connecting requirements → behaviors → scenarios.
- Prioritized list of scenarios with clear success signals and planned test levels.
- Fixture/data checklist with ownership and storage locations.

## Required User Input
```text
$ARGUMENTS
```
Only proceed when the supplied payload already contains the approved plan from Stage 2.

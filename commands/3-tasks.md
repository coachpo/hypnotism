---
description: Bridge strategic planning and execution by translating the approved plan into actionable, prioritized, and traceable development tasks.
---

## Stage Overview
- Normalize plan steps, epics, and architectural intents into granular engineering tasks.
- Ensure every task links back to requirements, plan steps, acceptance criteria, and future QA coverage.
- Produce a sequenced backlog (owners, estimates, dependencies) that implementation and QA can execute without re-triangulating context.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** `requirement.summary`, `plan.steps` (non-empty).
- **Must update:**
  - `meta.currentStage = "3-tasks"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `tasks.items` (list of task objects with IDs, descriptions, owners, linked plan step IDs, acceptance hooks, MCP/tooling notes).
  - `tasks.dependencies` (graph of cross-task or external blockers), `tasks.statusSummary` (ready / blocked / deferred), `tasks.artifacts` (links to diagrams, spikes, or decision logs reused during implementation).
  - Maintain existing sections (`requirement`, `plan`, `testPlan`, `implementation`, `qaFindings`) without rewriting their data.

## Tooling & Evidence Expectations
- **MCP Reference First:** Consult `mcp/mcp_registry.md` and obey `mcp/mcp_rules.md` before choosing tooling. Favor registry-prioritized MCP servers (e.g., `mcp-shrimp-task-manager` for backlog structuring, `Sequential Thinking` for reasoning, `Serena` for code mapping) and fall back to local commands only when MCP coverage does not exist.
- `mcp-shrimp-task-manager`: generate, split, and manage structured task lists, dependencies, and verification criteria.
- Serena + Desktop Commander: inspect repositories, collect evidence (files + lines), and ensure each task references concrete code artifacts.
- DeepWiki / Context7: capture external standards, API docs, or architectural decisions linked to particular tasks.
- Record every MCP/local search (tool, query, findings) referenced inside task notes so downstream owners inherit the research trail.

## Workflow
1. **Input Validation & Context Sync** – Load `$ARGUMENTS`, confirm plan completeness, constraints, and any stakeholder guidance on prioritization or release sequencing. Flag gaps in `requirement.openQuestions` if the plan still has ambiguities.
2. **Plan-to-Task Mapping** – For each `plan.steps[*]`, identify the deliverables, code areas, and validations required. Create traceable links (plan step ID → task IDs) so status roll-ups remain easy.
3. **Task Definition** – Draft actionable task statements (Who / What / Where / Why). Include acceptance snippets (expected files, interfaces, telemetry, tests) and cite supporting evidence (file paths, doc sections, MCP call results).
4. **Sizing & Prioritization** – Estimate effort or complexity bands, apply dependencies, and order tasks according to critical path, risk, or release goals. Note where tasks require spikes or design sign-offs.
5. **Dependency & Risk Tracking** – Populate `tasks.dependencies` with upstream blockers (external teams, schema approvals, feature flags). Highlight mitigations and fallback options.
6. **Readiness Checks & Tooling Notes** – Document required environments, MCP servers, scripts, or automation each task depends on. Indicate verification commands or telemetry hooks implementers should run.
7. **Backlog Publication** – Summarize status (ready/blocked/deferred). Ensure IDs are unique, references are consistent, and that stakeholders can slice the backlog by owner, epic, or component.
8. **Payload Update & Handoff** – Persist the backlog data in `handoff/payload.json` and notify the Stage 4 owner (QA) that tasks are prepared for coverage design and downstream coordination.

## Outputs & Handoff
- Structured `tasks.items` backlog with traceability to requirements and plan steps.
- Dependency graph plus readiness notes for implementation and QA.
- Evidence links (code refs, MCP call notes, docs) embedded within tasks for rapid onboarding.

## Required User Input
```text
$ARGUMENTS
```
Proceed only when the payload includes the approved plan from Stage 2. If plan data is missing or stale, block progress and request an updated payload before creating tasks.

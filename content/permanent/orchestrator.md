---
title: "Orchestrator (Agentic)"
aliases: ["orchestrator agent"]
tags: [agentic-architecture, multi-agent-orchestration]
created: 2026-06-24
updated: 2026-06-24
---

An Orchestrator is a controlling agent in a multi-agent system that is responsible for decomposing a high-level goal into subtasks, assigning those tasks to [[subagent-definition|sub-agents]], managing the execution order, and synthesizing the final result.

**Why it matters**: The orchestrator centralizes the coordination logic, improving maintainability and reliability. It retains the master plan, execution state, and error handling logic, providing a clear audit trail.

**Connections**:
- [[permanent/subagent-definition]]
- [[permanent/orchestrator-agentic]]
- [[permanent/task-decomposition]]
- [[permanent/result-aggregation]]
- [[permanent/hub-and-spoke-topology]]
- [[permanent/parallel-execution]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
- [[literature/certified-claude-architect-masterclass-2026-section-3-agentic-architecture-multi-agent-orchestration]]

---
title: "Orchestrator (Agentic)"
aliases: ["agent orchestrator", "workflow orchestrator"]
tags: [ai-agents, architecture, planning, workflow-patterns]
created: 2026-06-23
updated: 2026-06-23
---

In a multi-agent system, an Orchestrator is a controlling agent or service responsible for managing the entire workflow. Its primary role is to interpret a high-level goal, perform task decomposition, and then coordinate the execution of the resulting subtasks across a team of specialized "worker" agents.

The orchestrator acts as the "thinker" or "project manager" of the system. It maintains the overall plan, often represented as a dependency graph, and is responsible for:
-   **Assigning tasks**: Dispatching subtasks to the appropriate specialist agents.
-   **Managing execution order**: Ensuring tasks are performed in the correct sequence, respecting dependencies.
-   **Handling data flow**: Managing the handoff points between agents so that outputs are correctly passed as inputs.
-   **Aggregating results**: Synthesizing the outputs from various agents into a final, coherent response.

By centralizing the coordination logic, the orchestrator allows worker agents to remain simple and focused on their specific capabilities. This separation of concerns is a key architectural pattern for building robust and scalable agentic systems.

## Connections

- [[permanent/task-decomposition|Task Decomposition]]
- [[permanent/dependency-graph|Dependency Graph (Agentic)]]
- [[permanent/handoff-point|Handoff Point]]
- [[permanent/workflow-patterns|Workflow Patterns]]

## Sources

- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning|Section 2: Agentic Architecture - Task Decomposition & Planning]]

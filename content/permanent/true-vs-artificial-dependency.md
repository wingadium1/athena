---
title: "True vs. Artificial Dependency"
aliases: ["structural vs. conventional dependency"]
tags: [ai-agents, architecture, planning, workflow-patterns]
created: 2026-06-23
updated: 2026-06-23
---

In agentic workflow design, distinguishing between true and artificial dependencies is crucial for optimizing task execution. A **true dependency** (or structural dependency) exists when one task genuinely requires the output of another to proceed. For example, a `summarize_document` task cannot begin until the `fetch_document` task has completed. These dependencies are structural constraints that dictate a sequential order of operations.

An **artificial dependency**, by contrast, is a sequence imposed by convention, habit, or a rigid workflow structure rather than a logical necessity. For instance, a workflow might be designed to first research a topic, then find related images, when in fact both tasks could be performed simultaneously. These dependencies create unnecessary bottlenecks, forcing independent tasks to wait and increasing overall latency.

Identifying and eliminating artificial dependencies is a key goal of task decomposition and planning. By creating a dependency graph, an orchestrator can visualize the relationships between tasks and identify opportunities for parallel execution. Challenging the assumed order of operations and asking "Can this run independently?" helps to break these artificial constraints, leading to more efficient, parallelized, and faster agentic systems.

## Connections

- [[permanent/task-decomposition|Task Decomposition]]
- [[permanent/dependency-graph|Dependency Graph (Agentic)]]
- [[permanent/sequential-pipeline|Sequential Pipeline]]
- [[permanent/parallel-execution|Parallel Execution (Agentic)]]
- [[permanent/orchestrator-agentic|Orchestrator (Agentic)]]

## Sources

- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning|Section 2: Agentic Architecture - Task Decomposition & Planning]]

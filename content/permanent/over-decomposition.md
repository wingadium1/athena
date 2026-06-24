---
title: "Over-Decomposition"
aliases: ["excessive decomposition"]
tags: [ai-agents, architecture, planning, anti-patterns]
created: 2026-06-23
updated: 2026-06-23
---

Over-decomposition is an anti-pattern in agentic design where a task is broken down into an excessive number of small, granular subtasks. While task decomposition is essential for managing complexity, over-decomposition creates more problems than it solves by introducing significant coordination overhead, increased latency, and a more complex system that is harder to debug and maintain.

The core issue is that each subtask and handoff point adds a small amount of overhead for orchestration, validation, and potential failure. When the subtasks become too granular, the cumulative cost of managing the workflow (the "coordination tax") can exceed the cost of actually performing the work. This leads to diminishing returns, where adding more decomposition actually decreases overall performance and reliability.

Empirical studies show that there is an optimal window for decomposition granularity that shifts with task complexity. Simple tasks benefit from minimal decomposition, while more complex tasks require a higher degree of breakdown. The key is to find the right balance, creating subtasks that are meaningful and independently executable without being so small that the orchestration logic becomes more complex than the task itself.

## Connections

- [[permanent/task-decomposition|Task Decomposition]]
- [[permanent/workflow-patterns|Workflow Patterns]]
- [[permanent/handoff-point|Handoff Point]]

## Sources

- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning|Section 2: Agentic Architecture - Task Decomposition & Planning]]

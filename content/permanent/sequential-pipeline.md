---
title: "Sequential Pipeline"
aliases: ["sequential execution", "prompt chaining"]
tags: [ai-agents, architecture, planning, workflow-patterns]
created: 2026-06-23
updated: 2026-06-23
---

A Sequential Pipeline is an agentic workflow pattern where multiple specialized agents are arranged in a linear chain. Tasks are processed in a fixed, predetermined order, with the output of each agent serving as the direct input for the next. This pattern is analogous to a manufacturing assembly line, where each station performs a specific operation before passing the work-in-progress to the next.

The primary strengths of this pattern are its simplicity and debuggability. The data flow is clear and unidirectional, making it easy to trace errors by inspecting the handoff point between each stage. It is the most appropriate pattern when there are true data dependencies between steps, meaning a task genuinely requires the output from the preceding one to begin. This structure also allows for quality gates or validation checkpoints between agents to catch errors early.

However, the main drawback is the accumulation of latency; the total execution time is the sum of all individual step durations. This can be inefficient if some tasks are independent and could otherwise run in parallel. A failure at any point in the chain can halt the entire workflow, creating an error cascade, though the failure is at least isolated to a specific step. This pattern is best used for well-defined, multi-stage processes where order is critical and auditability is more important than raw speed.

## Connections

- [[permanent/task-decomposition|Task Decomposition]]
- [[permanent/workflow-patterns|Workflow Patterns]]
- [[permanent/dependency-graph|Dependency Graph]]
- [[permanent/error-cascade|Error Cascade]]
- [[permanent/true-vs-artificial-dependency|True vs. Artificial Dependency]]
- [[permanent/handoff-point|Handoff Point]]
- [[permanent/parallel-execution|Parallel Execution (Agentic)]]

## Sources

- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning|Section 2: Agentic Architecture - Task Decomposition & Planning]]

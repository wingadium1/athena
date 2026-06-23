---
title: "Merging Strategies (Agentic)"
aliases: ["result aggregation"]
tags: [ai-agents, architecture, planning, workflow-patterns]
created: 2026-06-23
updated: 2026-06-23
---

In agentic systems that use parallel execution, Merging Strategies are the specific techniques used during the **fan-in** phase to combine the outputs from multiple concurrent branches into a single, useful result. Since each parallel branch produces its own output, the orchestrator needs a predefined method for consolidating them before proceeding. The choice of strategy is critical and depends entirely on the nature of the task.

Common merging strategies include:

1.  **Concatenation**: The simplest strategy, where outputs are joined together into a single list or document. This is suitable when the outputs are distinct sections of a larger whole, such as different parts of a research report.
2.  **Voting**: Used when multiple agents perform the same task to improve reliability. The final answer is determined by a majority vote among the different outputs. This is common in classification or decision-making tasks.
3.  **Structured Aggregation**: When parallel tasks are designed to contribute different fields to a single data object. For example, one agent might fetch a company's revenue, another its employee count, and the fan-in step aggregates these into a single company profile JSON object.
4.  **Synthesis**: A more advanced strategy where a dedicated "synthesis" agent receives all the parallel outputs and is tasked with creating a new, higher-level summary or analysis that incorporates the insights from all branches.

The merging strategy must be chosen during the design of the workflow, as it directly influences how the fan-out branches should be structured and what kind of output they are expected to produce.

## Connections

- [[permanent/fan-in|Fan-in (Agentic)]]
- [[permanent/parallel-execution|Parallel Execution (Agentic)]]
- [[permanent/workflow-patterns|Workflow Patterns]]

## Sources

- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning|Section 2: Agentic Architecture - Task Decomposition & Planning]]

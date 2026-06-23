---
title: "Error Cascade"
aliases: ["hallucination cascade", "failure cascade"]
tags: [ai-agents, architecture, reliability, failure-modes]
created: 2026-06-23
updated: 2026-06-23
---

An Error Cascade is a critical failure mode in multi-agent systems where a minor, localized error in one agent propagates and amplifies through a chain of subsequent agents. Each downstream agent unknowingly treats the flawed output from the previous step as a valid input, building upon the initial mistake and often compounding it. This can lead to a system-level false consensus or a final output that is confidently and coherently wrong.

This phenomenon is particularly dangerous in agentic systems because the handoff points between agents often lack robust validation. A single hallucinated fact, misinterpreted tool output, or corrupted data point can be repackaged and reinforced as it passes through a sequential pipeline. The error becomes harder to trace as it's laundered through multiple layers of reasoning, making the final failure appear disconnected from its root cause.

Mitigating error cascades requires moving beyond per-agent validation and implementing checks at the boundaries between them. Strategies include rigorous schema validation at every handoff, introducing external "critic" agents to verify intermediate outputs, and implementing "genealogy-graph" tracing to track the provenance of information. Without such architectural safeguards, the collaborative nature of multi-agent systems makes them inherently vulnerable to these compounding failures.

## Connections

- [[permanent/sequential-pipeline|Sequential Pipeline]]
- [[permanent/handoff-point|Handoff Point]]
- [[permanent/workflow-patterns|Workflow Patterns]]

## Sources

- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning|Section 2: Agentic Architecture - Task Decomposition & Planning]]

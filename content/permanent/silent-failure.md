---
title: "Silent Failure"
aliases: ["Silent Error"]
tags: [multi-agent-orchestration, agentic-architecture, course, error-handling]
created: 2026-06-24
updated: 2026-06-24
---

A Silent Failure is a high-risk error condition in which a task or handoff appears to have completed successfully, but critical information has been lost or corrupted without any error being raised. This leads to an inconsistent or incorrect system state that is difficult to trace back to the source of the problem, as there is no explicit failure signal.

Silent failures are particularly dangerous in multi-agent systems where dependencies are complex. They stand in direct contrast to **Transparent Failures**, where a sub-agent returns a structured, actionable error. A transparent failure provides the orchestrator with the necessary context (error type, description) to make an informed decision, such as retrying the task, escalating to another agent, or halting the workflow gracefully.

> [!tip] Research In Progress
> The `librarian` is currently researching common causes and mitigation strategies for silent failures to enrich this note.

## Connections
- [[permanent/transparent-failure|Transparent Failure]]
- [[permanent/error-propagation-in-multi-agent-systems|Error Propagation in Multi-Agent Systems]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-3-agentic-architecture-multi-agent-orchestration|Agentic Architecture - Multi Agent Orchestration]]

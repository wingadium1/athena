---
title: "Four-Phase Lifecycle of a Tool Call"
aliases: ["tool call lifecycle"]
tags: [agentic-architecture, tool-use]
created: 2026-06-24
updated: 2026-06-24
---

The four-phase lifecycle of a tool call describes the process an agent follows to use a tool:
1.  **Decision**: The model identifies the need for a tool and generates a structured tool call.
2.  **Execution**: The runtime executes the tool call.
3.  **Observation**: The result of the tool execution is captured as an observation.
4.  **Feedback**: The observation is fed back into the model's context to inform its next step.

This cycle can repeat multiple times as an agent works towards its goal.

**Why it matters**: This lifecycle is the fundamental mechanic that allows an LLM to interact with the real world and perform actions beyond generating text.

**Connections**:
- [[permanent/tool-use-lifecycle]]
- [[permanent/agentic-loop]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]

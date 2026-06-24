---
title: "Perception, Reasoning, Action, and Observation (PRAO)"
aliases: ["PRAO cycle"]
tags: [agentic-architecture]
created: 2026-06-24
updated: 2026-06-24
---

The Perception, Reasoning, Action, and Observation (PRAO) cycle, also known as the agentic loop, is the fundamental process that enables an agent to operate autonomously. The four phases are:
1.  **Perception**: The model reads its context window to gather information.
2.  **Reasoning**: It evaluates the information to decide on the best course of action.
3.  **Action**: It emits a structured tool call to perform a task.
4.  **Observation**: It receives and integrates the results of the action back into its context.

**Why it matters**: This continuous cycle allows the agent to dynamically adapt its behavior based on new information and feedback from its environment.

**Connections**:
- [[permanent/agentic-loop]]
- [[permanent/tool-use-lifecycle]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]

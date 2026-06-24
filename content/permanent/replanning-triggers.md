---
title: "Replanning Triggers"
aliases: []
tags: [agentic-architecture, planning]
created: 2026-06-24
updated: 2026-06-24
---

Replanning Triggers are events that necessitate a revision of the current plan. These include:
-   **Failed tool calls**: Errors or timeouts during tool execution.
-   **Unexpected results**: Technically valid results that do not align with the intended goals.
-   **Changed state**: The environment has changed in a way that invalidates the current plan.
-   **Constraint violations**: A proposed next action would violate a goal constraint or safety boundary.

**Why it matters**: Identifying these triggers allows an agent to know when to stop and replan, rather than continuing with a flawed plan.

**Connections**:
- [[permanent/replanning]]
- [[permanent/dynamic-planning]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]

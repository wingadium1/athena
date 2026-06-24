---
title: "Goal Drift"
aliases: []
tags: [agentic-architecture, planning]
created: 2026-06-24
updated: 2026-06-24
---

Goal Drift is a significant risk in adaptive planning where an agent revises its plan to overcome obstacles but in doing so, strays from its original goals.

**Why it matters**: Goal drift can lead to an agent completing a task that does not meet the user's original intent. Mechanisms to prevent this include explicit validation of each revised plan against the original goal and preserving the plan state across cycles.

**Connections**:
- [[permanent/dynamic-planning]]
- [[permanent/replanning]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]

---
title: "Context Capacity"
aliases: ["context window"]
tags: [agentic-architecture]
created: 2026-06-24
updated: 2026-06-24
---

Context Capacity refers to the amount of information (instructions, conversation history, tool results) that a language model can hold in its context window at one time. As an agent operates, this window fills up.

**Why it matters**: Efficient management of the context window is crucial for effective agent design. If the context window is exceeded, the model may lose important information, leading to errors or a failure to [[task-completion|complete its task]].

## Connections
- [[permanent/agentic-loop]]
- [[permanent/tool-use-lifecycle]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]

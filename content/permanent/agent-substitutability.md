---
title: "Agent Substitutability"
aliases: ["Swappable Agents"]
tags: [multi-agent-orchestration, agentic-architecture, course, modularity]
created: 2026-06-24
updated: 2026-06-24
---

Agent Substitutability is a design principle in multi-agent systems where any agent conforming to a shared, specified protocol can handle a given task or handoff. This promotes modularity and allows for the easy replacement, upgrade, or swapping of agents without disrupting the entire system.

This principle is analogous to designing to interfaces rather than concrete implementations in object-oriented programming. By depending on a stable protocol (the "interface"), the orchestrator is decoupled from the specific implementation details of any individual agent. This makes the overall system more flexible and maintainable over time.

> [!tip] Research In Progress
> The `librarian` is currently researching related design patterns from microservices and component-based architectures to enrich this note.

## Connections
- [[permanent/multi-agent-orchestration]]
- [[permanent/orchestrator]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-3-agentic-architecture-multi-agent-orchestration|Agentic Architecture - Multi Agent Orchestration]]

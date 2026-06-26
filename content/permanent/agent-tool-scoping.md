---
title: "Agent Tool Scoping"
aliases: ["tool-bloat", "specialist-agent"]
tags: [mcp, tool-design, agentic-architecture]
created: 2026-06-26
updated: 2026-06-26
---

# Agent Tool Scoping

Agent tool scoping is the practice of limiting the tool palette available to an agent based on its mission, ensuring context focus and routing reliability.

### The Problem of Tool Bloat
Providing too many tools to an agent causes "tool bloat," which degrades performance:
*   **Context Window Pressure**: Irrelevant tools consume tokens.
*   **Routing Ambiguity**: Overlapping descriptions confuse the model, leading to non-deterministic behavior and high misrouting rates.
*   **Slower Response**: Larger catalogs increase inference overhead.

### Specialist Agent Principle
*   **One Agent, One Mission**: Each agent should receive a narrow palette designed strictly for its role.
*   **Context Isolation**: Specialists keep the orchestrator’s context clean.
*   **Audit Regularly**: Periodically review tool assignments; tools often outlive their original purpose.

### Disambiguation Strategies
When consolidating functions, ensure that tool names and descriptions are distinct. Use:
*   **Domain Anchoring**: Scope tools to specific data domains.
*   **Explicit Exclusions**: List what the tool does NOT handle.

### Connections
*   [[tool-description-routing|Tool Description Routing]]
*   [[permanent/orchestrator|Orchestrator]]

### Sources
*   [[literature/certified-claude-architect-masterclass-2026-section-5-tool-design-mcp|CCA-F Section 5: Tool Design & MCP]]

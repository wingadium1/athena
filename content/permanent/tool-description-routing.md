---
title: "Tool Description Routing"
aliases: ["routing-heuristics"]
tags: [mcp, tool-design, agentic-architecture]
created: 2026-06-26
updated: 2026-06-26
---

# Tool Description Routing

Tool description routing is the process by which an LLM, specifically Claude, selects the most appropriate tool from a provided catalog based on a natural language request. It is the primary signal for semantic matching, where the description, rather than the tool name, dictates the routing decision.

### How Routing Works
Claude assigns implicit relevance weights to tools by performing semantic matching between the request context and the tool descriptions. Strong descriptions narrow the candidate set to one obvious choice, while weak or missing descriptions lead to ambiguity and potential misrouting.

### Description Failure Modes
*   **Vague intent**: The description fails to differentiate the tool from similar alternatives.
*   **Missing scope**: The description omits boundary conditions, making it unclear when the tool should NOT be used.
*   **No output signal**: The description fails to define what the tool returns, preventing Claude from confirming if the tool satisfies the request.

### Designing Robust Descriptions
To ensure correct routing, descriptions should follow a three-part structure:
1.  **Purpose**: A clear 1-2 sentence statement of what the tool does.
2.  **When to use**: Specific conditions for selection.
3.  **When not to use**: Boundary conditions or exclusions to scope the tool effectively.

### Connections
*   [[agent-tool-scoping|Agent Tool Scoping]]
*   [[tool-input-schema-contract|Tool Input Schema Contract]]

### Sources
*   [[literature/certified-claude-architect-masterclass-2026-section-5-tool-design-mcp|CCA-F Section 5: Tool Design & MCP]]

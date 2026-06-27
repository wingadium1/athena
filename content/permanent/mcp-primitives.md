---
title: "MCP Primitives"
aliases: ["tools", "resources", "prompts"]
tags: [mcp, architecture]
created: 2026-06-27
updated: 2026-06-27
---

The Model Context Protocol (MCP) defines three core primitives that servers expose to clients, providing a standardized way to share contextual information, tools, and workflows.

## Primitives
1.  **Tools**: Action-oriented capabilities triggered by the model (e.g., performing computations, retrieving computed results). Defined with JSON schema; model-controlled.
2.  **Resources**: Static, read-only data sources addressable by URI (e.g., database records, files). Application-controlled and injected into context.
3.  **Prompts**: User-controlled, reusable workflow templates, pre-populated with parameters at selection time.

## Key Takeaways
- Tools enable models to interact with the external world (model-controlled).
- Resources expose static data for context (read-only).
- Prompts standardize complex, multi-step workflows.

## Connections
- [[permanent/tool-use]]
- [[permanent/mcp-reliability-and-production]]
- [[permanent/mcp-security-and-scoping]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-5-tool-design-mcp]]
- [[literature/certified-claude-architect-masterclass-2026-section-6-tool-design-and-mcp-building-mcp-server]]
- Research: [SurePrompts - Model Context Protocol (MCP): The Complete 2026 Guide](https://sureprompts.com/blog/model-context-protocol-mcp-complete-guide-2026) (SHA: bg_c7f84225)

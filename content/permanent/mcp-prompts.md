---
title: "MCP Prompts"
aliases: ["prompts"]
tags: [mcp, workflow-design]
created: 2026-06-26
updated: 2026-06-26
---

# MCP Prompts

MCP Prompts are reusable, parameterized templates that the user initiates to structure an interaction. Unlike tools (model-controlled) or resources (host-controlled), prompts are user-controlled workflows.

When a user selects a prompt, the client collects necessary parameters, which are then injected into the interaction as the opening context after rendering on the server. Prompts are particularly useful for complex, multi-step analysis or compliance-driven workflows that require consistent instruction sets, as they allow for centralized maintenance and versioning on the MCP server.

## Connections
- [[permanent/model-context-protocol|Model Context Protocol]]

## Sources
- [[certified-claude-architect-masterclass-2026-section-6-tool-design-and-mcp-building-mcp-server|CCA-F Section 6: Building MCP Server]]

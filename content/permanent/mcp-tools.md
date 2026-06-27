---
title: "MCP Tools"
aliases: ["tools"]
tags: [mcp, tool-design]
created: 2026-06-26
updated: 2026-06-26
---

# MCP Tools

MCP Tools are executable capabilities that allow the AI model to perform actions or execute computations. Unlike other primitives, the model itself decides when and whether to invoke a tool, making it an agentic, model-controlled capability.

Tools are defined via a JSON schema that explicitly describes input arguments and output format (either text or structured JSON). Effective tool design requires specificity in descriptions, clear declaration of side effects (e.g., write/delete operations), and defined operational limits (e.g., pagination, rate limits) to guide the model's decision-making. Tools should catch internal exceptions and return structured error responses (`isError = true`) rather than crashing the server.

## Connections
- [[permanent/model-context-protocol|Model Context Protocol]]

## Sources
- [[certified-claude-architect-masterclass-2026-section-6-tool-design-and-mcp-building-mcp-server|CCA-F Section 6: Building MCP Server]]

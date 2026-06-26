---
title: "Model Context Protocol"
aliases: ["mcp"]
tags: [mcp, infrastructure]
created: 2026-06-26
updated: 2026-06-26
---

# Model Context Protocol

The Model Context Protocol (MCP) is an open, model-agnostic standard that provides a uniform interface for connecting AI models to external tools, data sources, and prompt workflows. By decoupling the AI host (Client) from the data provider (Server), it enables modular and composable AI systems where tools can be swapped without renegotiating integration contracts.

The protocol follows a classic client-server architecture based on a defined transport layer (e.g., stdio, HTTP/SSE). It establishes a capability-first model, where the server advertises its capabilities (tools, resources, prompts) and the client negotiates which to use, ensuring requests are routed correctly without the client needing to understand the server's internal implementation.

## Connections
- [[permanent/mcp-tools|MCP Tools]]
- [[permanent/mcp-resources|MCP Resources]]
- [[permanent/mcp-prompts|MCP Prompts]]
- [[permanent/mcp-client-server-architecture|MCP Architecture]]

## Sources
- [[literature/cca-f-section-6-building-mcp-server|CCA-F Section 6: Building MCP Server]]

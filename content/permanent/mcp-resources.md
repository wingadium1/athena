---
title: "MCP Resources"
aliases: ["resources"]
tags: [mcp, data-sources]
created: 2026-06-26
updated: 2026-06-26
---

# MCP Resources

MCP Resources are URI-addressable data sources (e.g., files, database records, config snapshots) that the host application injects into the AI's context. Resources are application-controlled, meaning the host decides which data to include before the model generates a response, distinguishing them from model-invoked tools.

A resource is defined by a URI scheme, a MIME type for correct content interpretation, and a handler function. Resources can be static (cached, unchanging) or dynamic (generated at request time). MCP supports resource subscriptions, allowing the server to notify the client when resource content changes, ensuring dynamic context remains fresh without constant polling.

## Connections
- [[permanent/model-context-protocol|Model Context Protocol]]

## Sources
- [[certified-claude-architect-masterclass-2026-section-6-tool-design-and-mcp-building-mcp-server|CCA-F Section 6: Building MCP Server]]

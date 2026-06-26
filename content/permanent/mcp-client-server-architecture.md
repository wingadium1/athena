---
title: "MCP Architecture"
aliases: ["mcp-client-server"]
tags: [mcp, architecture]
created: 2026-06-26
updated: 2026-06-26
---

# MCP Architecture

The MCP Architecture follows a classic client-server pattern. The **Server** provider exposes capabilities, while the **Client** (AI host/application) connects to servers, performs capability discovery (handshake), and manages the request lifecycle.

Key components of this architecture include:
- **Transport Layer**: Defines how messages are exchanged (stdio, HTTP/SSE, custom).
- **Capability Discovery/Registration**: A handshake process upon startup where the client and server exchange lists of supported tools, resources, and prompts. Unregistered capabilities are inaccessible.
- **Routing**: The client maintains connections to multiple servers, routing requests by matching tool/resource/prompt names to the owning server, using namespace prefixes to prevent naming collisions.

## Connections
- [[permanent/model-context-protocol|Model Context Protocol]]

## Sources
- [[literature/cca-f-section-6-building-mcp-server|CCA-F Section 6: Building MCP Server]]

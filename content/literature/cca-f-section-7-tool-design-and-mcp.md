---
title: "Tool Design & MCP - MCP Production & Built-in Tools"
type: literature
source: "CCA-F"
author: "Research session — Section 7"
date-read: 2026-06-26
tags: [mcp, architecture, course]
---

# Tool Design & MCP - MCP Production & Built-in Tools

## Summary
This section covers advanced MCP concepts, focusing on transport mechanisms (STDIO vs. StreamableHTTP), sampling for LLM inference, security and access scoping, and production-readiness. It also contrasts built-in tools with custom server implementations to guide architectural decisions.

## Key Ideas
L1: STDIO Transport establishes a client-server connection by spawning the server as a child process of the client, utilizing OS standard streams (stdin/stdout). Ideal for local development, desktop tool integration, and single-user scenarios; stateful by design. Not suitable for remote/multi-user/production environments.

L2: StreamableHTTP Transport is built for remote, multi-user, and production environments, binding the server to a network port and utilizing HTTP POST for tool calls, with Server-Sent Events (SSE) for event delivery. Requires TLS, authentication, and load balancing for production.

L3: LLM Sampling allows a tool server to request LLM inference via the client, keeping the server stateless (no credentials/model dependencies) while centralizing policy control and enabling Human-in-the-Loop approval gates.

L4: MCP notifications enable non-blocking communication for long-running tools via Progress and Log notifications. Roots establish a trust boundary by defining an explicit access scope (filesystem paths/URIs) for servers, enforcing least-privilege.

L5: Access scoping is critical; mechanisms include schema constraints, root grants, and server-side authorization. Security best practices include input sanitization, least-privilege principles, and robust output filtering to prevent unauthorized data exposure.

L6: Production-grade MCP servers require authentication (OAuth 2.0, API keys, mTLS), backward-compatible versioning, and rigorous monitoring (logs, error rates, latency alerts). Reliability is maintained via health endpoints, graceful degradation, and strict timeout enforcement.

L7: Claude provides built-in tools (web search, computer use, code execution, bash, file operations) that architects can leverage without defining schemas. Their usage should be assessed against architectural needs, cost, and latency.

L8: Hybrid architectures utilize built-in tools for generic capabilities and custom MCP servers for proprietary data/internal APIs. Security boundaries for built-in tools are infrastructure-enforced (no private network access), whereas custom servers provide granular control.

## Links
- [[permanent/stdio-transport]]
- [[permanent/streamable-http-transport]]
- [[permanent/mcp-sampling]]
- [[permanent/mcp-security-and-least-privilege]]
- [[permanent/production-mcp-reliability]]
- [[permanent/mcp-built-in-tools]]

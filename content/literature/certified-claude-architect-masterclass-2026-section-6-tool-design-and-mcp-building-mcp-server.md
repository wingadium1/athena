---
title: "CCA-F Section 6: Tool Design & MCP - Building MCP Server"
type: literature
source: "Udemy: https://www.udemy.com/course/certified-claude-architect-masterclass-2026"
author: Jacob Bushong
date-read: 2026-06-26
tags:
  - mcp
  - course
---
# CCA-F Section 6: Tool Design & MCP -Building MCP Server

## Summary
Building MCP servers allows AI agents to interact with tools, resources, and prompts through a standardized, model-agnostic protocol. This section covers MCP concepts, primitives (tools, resources, prompts), server implementation in Python, and integration patterns.

## Key Ideas

### L1: MCP Overview
* **MCP (Model Context Protocol)** defines a uniform interface for AI models to connect to external tools and data sources.
* **Architecture**: Follows a classic client-server model.
    * **MCP Server**: Exposes capabilities (tools, resources, prompts) via transports (stdio, HTTP/SSE).
    * **MCP Client**: An AI application/host that connects to servers to discover and route requests to capabilities.
* **Benefits**: Solves integration fragmentation by providing a standard contract, enabling modular and composable AI systems.

### L2: 3 MCP Primitives
* **Tools**: Action-oriented capabilities triggered by the model (e.g., performing computations, retrieving computed results). Defined with JSON schema; model-controlled.
* **Resources**: Static data sources addressable by URI (e.g., database records, files). Application-controlled and injected into the context.
* **Prompts**: User-controlled, reusable workflow templates, pre-populated with parameters at selection time.

### L3: Building Servers (Python)
* **Components**: MCP SDK, transport mechanisms (Stdio, HTTP/SSE), and server inspector (debugging).
* **Lifecycle**: Managed via `on_startup` and `on_shutdown` async hooks for connection/resource management.
* **Init Protocol**: Capabilities (tools, resources, prompts) must be registered *before* the server starts the `run()` loop to ensure correct negotiation during the init handshake.

### L4: Exposing Tools
* **Implementation**: Uses `@tool` decorators where the function docstring serves as the tool description read by the model.
* **Result Handling**:
    * **Text Results**: Plain strings for simple information display.
    * **Structured Results**: JSON dictionaries for parsing by the client (e.g., lists, records).
* **Error Handling**: Tools should catch exceptions and return structured error objects (`isError = true`) to allow the model to reason about and recover from failures, rather than crashing the server.

### L5: Exposing Resources
* **Definition**: Composed of a URI (with a domain prefix), a MIME type, and a content handler function.
* **Types**:
    * **Static**: Content never changes between requests (e.g., config files). Cacheable.
    * **Dynamic**: Fetched or generated at request time.
* **Subscription**: MCP allows clients to subscribe to resource URIs; the server notifies the client upon content updates, keeping dynamic context fresh without polling.

### L6: Prompt Templates
* **Design**: Parameterized templates (Name, Args, Body) that standardize complex, multi-step instructions.
* **Versioning**: Managed through naming conventions or versioning flags.
* **Usage**: The client retrieves templates via `prompts/list`, user provides arguments, and the client calls `prompts/get` to render the final instruction for context injection.

### L7: MCP Clients
* **Responsibilities**: Manages connection lifecycles, performs capability discovery on startup, routes tool calls to the correct server (handling namespace collisions with prefixes), and manages responses (surfacing errors as content blocks).
* **Multi-Server**: Clients can simultaneously connect to multiple MCP servers, routing requests based on the tool name and namespace.

## Quotes
* (None captured yet)

## My Take
(Vietnamese reflection)

## Links
* (None)

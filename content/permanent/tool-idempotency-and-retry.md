---
title: "Tool Idempotency and Retry"
aliases: ["idempotency", "retry-reliability"]
tags: [mcp, tool-design, reliability]
created: 2026-06-26
updated: 2026-06-26
---

# Tool Idempotency and Retry

Reliability in agentic tool design relies on handling failures gracefully through retry logic and ensuring idempotency to prevent duplicated side effects.

### Retry Mechanics
*   **Transient Failures**: Appropriate for retries (timeouts, rate limits).
*   **User/Schema Failures**: Should NOT be retried.
*   **Circuit Breakers**: A protective pattern that stops calls to failing downstream services to prevent cascading failures.

### Ensuring Idempotency
Non-idempotent tools can cause severe issues when retried. To design safely:
1.  **Idempotency Keys**: Accept a client-generated key with each request to identify unique operations.
2.  **Conditional Writes**: Check the system state before modifying it to prevent duplicate effects.

### Partial Success Patterns
For batch operations, avoid all-or-nothing error responses. Return a structured `partial_success` status listing:
*   Items that succeeded (by ID/key).
*   Items that failed, with per-item error codes and context.

### Connections
*   [[tool-error-design|Tool Error Design]]
*   [[permanent/reliability-patterns|Reliability Patterns]]

### Sources
*   [[literature/certified-claude-architect-masterclass-2026-section-5-tool-design-mcp|CCA-F Section 5: Tool Design & MCP]]

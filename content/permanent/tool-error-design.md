---
title: "Tool Error Design"
aliases: ["error-handling"]
tags: [mcp, tool-design, reliability]
created: 2026-06-26
updated: 2026-06-26
---

# Tool Error Design

A structured error response design is critical for enabling intelligent error recovery and escalation in agentic workflows. When a tool fails, the error response must provide actionable information rather than just a status code.

### Core Error Concepts
1.  **Error Type**: A stable, machine-readable identifier (`error_code`) that allows the model to route errors without parsing text.
2.  **Message**: A human-readable explanation of the failure.
3.  **Context**: Data regarding the input or resource that triggered the failure.
4.  **Suggested Action**: An optional instruction telling the agent what to do next (e.g., `retry`, `ask_user`, `escalate`).

### User Error vs. System Error
*   **User Error**: Caused by bad input (format, missing field, out of range). The model should surface this to the user for correction.
*   **System Error**: Caused by downstream issues (unavailable service, timeout). The model can attempt autonomous recovery (retry, fallback, escalation).

### Design Principles
*   Avoid raw stack traces; they confuse model-driven recovery.
*   Keep error codes stable across versions.
*   Never bury success statuses with buried error messages.

### Connections
*   [[tool-idempotency-and-retry|Tool Idempotency and Retry]]
*   [[tool-input-schema-contract|Tool Input Schema Contract]]

### Sources
*   [[literature/certified-claude-architect-masterclass-2026-section-5-tool-design-mcp|CCA-F Section 5: Tool Design & MCP]]

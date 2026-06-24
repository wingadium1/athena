---
title: "Tool Errors"
aliases: ["tool-error"]
tags: [claude, agentic-architecture, reliability]
created: 2026-06-24
updated: 2026-06-24
---
Tool Errors arise from external APIs or services failing, returning unexpected output, or timing out. These failures are external to the agent's internal reasoning process.

## Recovery
- **Transient Errors**: (e.g., HTTP 5xx, network timeouts, rate limits) - Recovery involves retrying the request with exponential backoff and logging each attempt.
- **Permanent Errors**: (e.g., HTTP 4xx, invalid input, permission denied) - These should not be retried. Recovery requires utilizing a fallback tool, providing different input, or escalating to human intervention.

## Connections
- [[permanent/error-classification-framework|Error Classification Framework]]
- [[permanent/tool-use-lifecycle|Tool-Use Lifecycle]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-4-agentic-architecture-reliability-human-oversight|Agentic Architecture - Reliability & Human Oversight]]

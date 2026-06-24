---
title: "Error Classification Framework"
aliases: ["error-framework"]
tags: [claude, agentic-architecture, reliability]
created: 2026-06-24
updated: 2026-06-24
---
An Error Classification Framework is essential for developing effective recovery strategies in agentic systems. Proper classification maps each error type to its specific recovery path, preventing wasted retries or missed escalations.

## Categories
1. **Tool Errors**: External failures. Recovery: Retry/Fallback.
2. **Reasoning Errors**: Internal model failures. Recovery: Prompt/Context adjustment.
3. **Environment Errors**: Infrastructure failures. Recovery: Monitor/Wait.

## Connections
- [[permanent/tool-errors|Tool Errors]]
- [[permanent/reasoning-errors|Reasoning Errors]]
- [[permanent/environment-errors|Environment Errors]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-4-agentic-architecture-reliability-human-oversight|Agentic Architecture - Reliability & Human Oversight]]

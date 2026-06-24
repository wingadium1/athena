---
title: "Error Propagation in Multi-Agent Systems"
aliases: ["Agent Error Surfacing"]
tags: [multi-agent-orchestration, agentic-architecture, course, error-handling]
created: 2026-06-24
updated: 2026-06-24
---

In multi-agent systems, it is vital for any failures within sub-agents to surface back to the orchestrator. Effective error propagation allows the orchestrator to manage the workflow intelligently, enabling strategies like retries, agent substitution, or escalation, rather than failing silently. This requires designing an explicit error path, not just a "happy path."

A robust error propagation strategy includes a well-defined, structured error payload that provides context for the failure.

### Anti-Patterns

Two common anti-patterns undermine effective error propagation:
1.  **Swallowing Errors**: A sub-agent catches an exception but returns an empty or successful-looking response. The orchestrator incorrectly assumes the task was completed, leading to data corruption or inconsistent state.
2.  **Undifferentiated Errors**: A sub-agent returns the same generic error code for all failure modes. This prevents the orchestrator from distinguishing between a transient, retryable issue (like a network timeout) and a deterministic, hard failure (like a permissions violation).

> [!tip] Research In Progress
> The `librarian` is currently researching best practices for structured error payloads and propagation patterns to enrich this note.

## Connections

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-3-agentic-architecture-multi-agent-orchestration|Agentic Architecture - Multi Agent Orchestration]]

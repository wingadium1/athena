---
title: "Agent State Checkpointing"
aliases: ["Continuity Across Failure", "Workflow Checkpointing"]
tags: [multi-agent-orchestration, agentic-architecture, course, resilience]
created: 2026-06-24
updated: 2026-06-24
---

Agent State Checkpointing is a technique used to ensure continuity and resilience in multi-agent workflows by preserving state at critical points. This prevents the need for a full workflow restart in the event of a handoff failure or sub-agent crash.

Effective checkpointing involves several key practices:
1.  **Pre-Handoff Save**: The state of a task is saved *before* initiating a handoff to another agent.
2.  **Record Last Known Good State**: The system should record the last checkpoint that was confirmed as valid, rather than simply the most recent attempt. This prevents resuming from a corrupted or failed state.
3.  **Resume from Checkpoint**: Retry logic should be designed to resume the workflow from the last known good checkpoint, not from the very beginning.

This approach significantly improves the resilience and efficiency of long-running or complex agentic processes.

> [!tip] Research In Progress
> The `librarian` is currently researching patterns from workflow engines and distributed systems to enrich this note.

## Connections

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-3-agentic-architecture-multi-agent-orchestration|Agentic Architecture - Multi Agent Orchestration]]

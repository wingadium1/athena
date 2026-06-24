---
title: "Hub-and-Spoke Topology"
aliases: []
tags: [agentic-architecture, multi-agent-orchestration]
created: 2026-06-24
updated: 2026-06-24
---

The Hub-and-Spoke topology is a multi-agent orchestration pattern where a central orchestrator (the hub) routes all tasks to peripheral expert agents (the spokes). The spokes do not communicate with each other directly.

**Strengths**: Centralized control, clear audit trails, and straightforward failure handling.
**Weaknesses**: The hub is a single point of failure and can become a bottleneck.

**Connections**:
- [[permanent/multi-agent-topologies]]
- [[permanent/pipeline-topology]]
- [[permanent/peer-to-peer-topology]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-3-agentic-architecture-multi-agent-orchestration]]

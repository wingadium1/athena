---
title: "Pipeline Topology"
aliases: []
tags: [agentic-architecture, multi-agent-orchestration]
created: 2026-06-24
updated: 2026-06-24
---

The Pipeline Topology is a multi-agent orchestration pattern where agents are arranged in a linear sequence. Each agent processes the output of the previous one, making it ideal for strictly ordered, testable transformations.

**Strengths**: High modularity (stages are easy to swap and test), clean handoffs via artifacts, and allows for agent specialization.
**Weaknesses**: Can accumulate latency and creates a risk of cascading failures.

**Connections**:
- [[permanent/multi-agent-topologies]]
- [[permanent/hub-and-spoke-topology]]
- [[permanent/peer-to-peer-topology]]
- [[permanent/sequential-pipeline]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-3-agentic-architecture-multi-agent-orchestration]]

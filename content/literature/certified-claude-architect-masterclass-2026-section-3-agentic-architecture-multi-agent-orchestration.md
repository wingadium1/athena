---
title: "Agentic Architecture - Multi Agent Orchestration"
type: literature
source: "certified-claude-architect-masterclass-2026"
author: "Jacob Bushong"
date-read: 2026-06-23
tags: [claude, udemy, course]
---

# Summary

# Key Ideas
- L1: Orchestrator Role: The orchestrator maintains the master plan, decomposing goals into subtasks, assigning to sub-agents, and aggregating results.
- L1: 3 Core Orchestration Concepts: Orchestrator (decompress goal -> assign -> aggregate), Task Decomposition (breaking down complex goals), Result Aggregation (validate -> synthesize).
- L1: 3 Core Functions (Must be explicit): Decomposition, Task Assignment (routing, matching subtasks to narrow-scope agents), Result Aggregation (3-step process).
- L1: Separation of Responsibilities: Coordination (orchestrator) vs. Execution (sub-agents) improves maintainability and reliability.
- L1: Orchestration Retains: Master plan, execution state, error handling logic, aggregation rules, and final synthesis.
- L1: Error Handling: Orchestrator decides strategy (Retry, Substitute, or Escalate).
- L1: Hub-and-Spoke Topology: Centralized hub (orchestrator) with spoke agents that don't communicate with each other.
- L1: Observability & Design: Orchestrator must be an audit trail (log assignments, state, errors). Missing functions lead to failure.
- L2: Subagent Definition: A distinct instance created by the orchestrator for a specific delegated task, operating within its own context and restricted tool permissions.
- L2: Context Isolation: Subagents start with a "blank" context; they only see what is explicitly handed over. This avoids bloat, information leakage, and dependency interference.
- L2: Authority Boundaries: Apply the Principle of Least Privilege. Subagents should have no access to shared state or side effects outside their explicit task scope without orchestrator approval.
- L2: Instruction Design Principles: Instructions must have a narrow scope, precise output formats (for predictable aggregation), and explicit success criteria to prevent incomplete processing.
- L2: Independent Testability: Well-designed subagents are swappable and independently testable, relying only on their explicit inputs without hidden dependencies on the orchestrator's state.
- L2: Common Anti-patterns: Broad/vague task scopes, underpowered context, and ambiguous success criteria.

# Quotes

# My Take

# Links

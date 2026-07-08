---
title: "Agentic Loop"
aliases: ["PRAO loop", "perception-reasoning-action-observation", "ReAct loop", "think-act-observe"]
tags: [ai, architecture, agents]
created: 2026-06-22
updated: 2026-06-22
---

The agentic loop is the core control-flow pattern that turns a stateless LLM into an agent: the model repeatedly perceives its context, reasons about the next step, takes action (typically a [[four-phase-lifecycle-of-a-tool-call|tool call]]), observes the result, and loops until a stop condition is met. Without this loop, an LLM generates a single response and stops — with it, the system pursues open-ended goals through multi-step interaction with the world.

The four phases — **Perception, Reasoning, Action, Observation** (PRAO) — map to established academic framing. The ReAct architecture (Reasoning + Acting, Yao et al. 2022) first formalized this interleaving of reasoning traces with tool-use actions. In production systems the loop is also called the "Think-Act-Observe" cycle, and most agent runtimes (LangGraph, Anthropic SDK, OpenAI Agents SDK) implement it as a state machine with checkpointing between turns.

The loop's power is also its risk: each iteration consumes tokens and wall-clock time. Without explicit **termination conditions** ([[task-completion|task completion]], turn budget, error threshold, stop signal) the agent can spin indefinitely, accumulating cost without progress. Production systems always pair the loop with budget guards — maximum iterations, total token caps, and wall-clock timeouts — and with **escalation paths** that [[handoff-protocol|hand off]] to a human operator when the loop cannot converge.

## Connections

- [[permanent/tool-use-lifecycle]] — the action phase of the agentic loop depends on the tool-use lifecycle; every tool call goes through decision → execution → observation → feedback
- [[permanent/workflow-first-agentic-architecture]] — the agentic loop is the architecture of last resort; workflows (deterministic patterns) should be preferred until open-ended goals justify the loop
- [[permanent/system-design-patterns]] — the agentic loop is a system design pattern for autonomous decision-making systems
- [[permanent/task-completion]]
- [[permanent/handoff-protocol]] — escalation paths use handoff protocols to transfer control when the loop cannot converge
- [[permanent/context-capacity]] — each loop iteration consumes context capacity, requiring budget guards

## Sources

- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]

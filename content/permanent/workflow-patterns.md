---
title: "Workflow Patterns"
aliases: ["AI workflow patterns", "Anthropic workflow patterns", "LLM workflow patterns"]
tags: [ai, architecture, workflows, agents]
created: 2026-06-22
updated: 2026-06-22
---

Anthropic's "Building Effective Agents" (Dec 2024) defines five composable workflow patterns that cover the majority of production AI systems. These patterns keep systems predictable, auditable, and debuggable by giving the developer control over the execution flow, in contrast to fully autonomous agents where the model directs its own actions.

**1. Prompt Chaining** — A linear sequence of LLM calls where each step's output feeds the next. Best for tasks that decompose into clear stages (outline → draft → revise). The simplest pattern: easy to debug, cheap, and testable. Quality gates can be inserted between steps.

**2. Routing** — A classifier (LLM or deterministic rule) selects one of N specialized handlers. Best when different input classes need different processing: cheap model for FAQs, reasoning model for complex queries, human escalation for edge cases. Adds low overhead per request.

**3. Parallelization** — Fan out to N independent LLM calls and aggregate results. Two flavours: **sectioning** (each call handles a subtask) and **voting** (multiple calls on the same task for higher confidence). Latency is bounded by the slowest call, not the sum.

**4. Orchestrator-Worker** — A central LLM decomposes the goal, dispatches subtasks to worker LLMs, and synthesizes the results. Powerful when subtasks are not known until the input arrives. The most expensive pattern — the planner can over-decompose if not budget-capped.

**5. Evaluator-Optimizer** — A generator proposes a candidate, an evaluator critiques it, and the loop repeats until acceptance or a budget is hit. Best when quality matters more than latency and the evaluator can articulate clear criteria.

These patterns are **composable** — a routing pattern can feed into a prompt chain; an orchestrator can wrap evaluator-optimizer loops at the worker level. Most production systems combine two or three patterns. The unifying principle is to start with the simplest pattern that solves the problem and add complexity only when measurement justifies it.

## Connections

- [[permanent/workflow-first-agentic-architecture]] — these patterns are the "workflow" side of the workflow-first decision rule
- [[permanent/agentic-loop]] — when these patterns cannot handle open-ended goals, the agentic loop becomes the fallback
- [[permanent/system-design-patterns]] — these are the AI-specific layer on top of general system design patterns

## Sources

- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]

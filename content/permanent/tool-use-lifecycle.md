---
title: "Tool-Use Lifecycle"
aliases: ["tool call lifecycle", "LLM tool use pattern", "function calling lifecycle"]
tags: [ai, architecture, agents, llm]
created: 2026-06-22
updated: 2026-06-22
---

Tool use is what transforms an LLM from a text generator into an agent that affects the real world. Every tool invocation in an agentic system follows a four-phase lifecycle: **Decision** (the model identifies the need and emits a structured tool call), **Execution** (the runtime validates arguments and runs the tool), **Observation** (the result is captured), and **Feedback** (the observation is fed back into the model's context for the next reasoning step). This cycle can repeat multiple times within a single task.

The quality of tool definitions strongly determines reliability. Clear tool names, precise parameter descriptions, and typed schemas (JSON Schema or Pydantic) reduce hallucinated arguments and wrong-tool selection. Tools fall into three functional categories: **data access tools** (retrieve stored information), **computation tools** (perform calculations or transformations), and **external API tools** (interact with remote services) — each with distinct latency, cost, and failure profiles.

The **Observation phase** is what distinguishes agentic systems from simple function calling. By feeding the tool's output back into the model's [[context-window|context window]], the system can adapt its next action based on real-time results — retrying with adjusted parameters, choosing a different tool, or deciding the [[task-completion|task is complete]]. This feedback loop is the engine of autonomous behavior, but it also makes context management critical: every observation consumes context capacity, and agent designs must plan for truncation, summarization, or sliding-window strategies.

## Connections

- [[permanent/agentic-loop]] — the tool-use lifecycle is the action engine inside the perception-reasoning-action-observation loop
- [[permanent/workflow-first-agentic-architecture]] — in workflows, tool calls are pre-scripted; in agents, the model selects tools dynamically via this lifecycle
- [[permanent/llm-wiki-pattern]] — the wiki's own tool system (read/write/search) follows this lifecycle internally
- [[permanent/context-capacity|Context Capacity]]
- [[permanent/task-completion]]

## Sources

- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]

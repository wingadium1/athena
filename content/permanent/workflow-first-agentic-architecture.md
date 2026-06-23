---
title: "Workflow-First Agentic Architecture"
aliases: ["incremental complexity", "workflow over agents", "architecture decision framework"]
tags: [claude, ai, architecture]
created: 2026-06-22
updated: 2026-06-22
---

When a task has a predictable input, a known output shape, and a design-time controllable step sequence, a deterministic workflow should be the default architecture. That keeps the system cheaper, faster, and easier to debug than giving the model autonomous control.

Agentic behavior becomes justified only when the task is genuinely open-ended, the tool sequence cannot be pre-specified, or the system must replan dynamically during execution. In other words: start with the simplest workflow, then add autonomy only to replace a proven failure point.

### Decision Framework

Three diagnostic questions determine whether workflow or agent is appropriate:

1. **Is the input format predictable?** — If yes, a deterministic parser or simple classifier can handle it.
2. **Is the output format known?** — If yes, the system can validate results against a schema without model judgment.
3. **Can every processing step be specified at design time?** — If yes, the topology is fixed and auditable.

If all three are yes, a workflow pattern (prompt chaining, routing, parallelization) should be the answer. If any is no, the task may warrant agentic autonomy — but only after validating that a workflow cannot be adapted.

Three signals justify agentic complexity:
- **Open-ended goals** — the target is known but the steps cannot be pre-specified
- **Unpredictable tool sequences** — which tools and in what order is not knowable at design time
- **Dynamic replanning during execution** — the system must adjust its plan based on intermediate results

The framework evaluates trade-offs along three axes: **cost vs capability** (agents are more capable but 2-10x more expensive per task), **auditability vs flexibility** (workflow logs are deterministic and localizable; agent traces are harder to debug), and **reliability vs adaptability** (workflows fail predictably in a known step; agents can fail in unbounded ways).

The incremental introduction strategy: start with a validated workflow, measure where it fails, and replace only the failing steps with agentic behavior — never flip the entire system to autonomous mode at once.

## Connections

- [[permanent/agentic-loop]] — the architecture of last resort when the decision framework says workflow is insufficient
- [[permanent/workflow-patterns]] — the five composable patterns (chaining, routing, parallelization, orchestrator-worker, evaluator-optimizer) that implement the workflow side of the decision
- [[permanent/tool-use-lifecycle]] — workflows and agents differ in who controls the tool-use lifecycle: developer (workflow) vs model (agent)
- [[permanent/software-architect-role]] — this is an architecture decision rule; the trade-off is system-wide and hard to reverse.
- [[permanent/llm-wiki-pattern]] — the wiki itself is a workflow-first system: ingest, query, and lint are deterministic operations.

## Sources

- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]

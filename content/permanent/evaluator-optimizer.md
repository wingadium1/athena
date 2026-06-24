---
title: "Evaluator Optimizer"
aliases: ["evaluator-optimizer-loop", "generate-evaluate-refine"]
tags: [agentic-architecture, workflow-patterns]
created: 2026-06-24
updated: 2026-06-24
---

The Evaluator Optimizer is a workflow pattern that uses a generate-evaluate-refine cycle to close quality gaps in an agent's output. It consists of three components:
1.  **Generator**: Produces an initial candidate output.
2.  **Evaluator**: Scores the output against a rubric or set of constraints, providing structured critique, not just a pass/fail signal.
3.  **Optimizer/Refiner**: The generator revises its output based on the evaluator's feedback.

This loop continues until a convergence criterion is met (e.g., quality score threshold or iteration cap).

**Why it matters**: This pattern provides a structured way to handle ambiguity and improve the quality of an agent's output through iterative refinement.

**Connections**:
- [[permanent/workflow-patterns]]
- [[permanent/iterative-refinement-loops]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
- [[literature/certified-claude-architect-masterclass-2026-section-3-agentic-architecture-multi-agent-orchestration]]

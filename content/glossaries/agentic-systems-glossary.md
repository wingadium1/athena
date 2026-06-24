# Agentic Systems Glossary

_Last updated: 2026-06-24_

### Action Environment Gap
**Definition**: The difference between what a language model can describe in text and what it can actually execute in a real-world environment.
**Sources**:
- [[permanent/action-environment-gap|Action Environment Gap]]

**Related Notes**:
- [[permanent/tool-use-lifecycle]]

### Agent State Checkpointing
**Definition**: A technique to preserve state at critical points to prevent full workflow restarts after failures.
**Sources**:
- [[permanent/agent-state-checkpointing|Agent State Checkpointing]]

**Related Notes**:
- [[permanent/agentic-architecture]]

### Agent Substitutability
**Definition**: A design principle where agents conforming to a shared protocol can handle tasks interchangeably, promoting modularity.
**Sources**:
- [[permanent/agent-substitutability|Agent Substitutability]]

**Related Notes**:
- [[permanent/multi-agent-orchestration]]

### Agentic Systems
**Definition**: Systems that autonomously pursue goals by perceiving context, selecting actions, invoking tools, and iterating on results.
**Sources**:
- [[permanent/agentic-systems|Agentic Systems]]

**Related Notes**:
- [[permanent/agentic-loop]]
- [[permanent/deterministic-workflows]]
- [[permanent/workflow-first-agentic-architecture]]

### Ambiguous Goal
**Definition**: A high-level objective lacking specific definition, creating multiple valid interpretations and misalignment risks.
**Sources**:
- [[permanent/ambiguous-goal|Ambiguous Goal]]

**Related Notes**:
- [[permanent/clarify-first-strategy]]
- [[permanent/assume-and-process]]

### Assume and Process Strategy
**Definition**: A strategy for handling ambiguity where an agent makes a reasonable inference and continues execution without human clarification.
**Sources**:
- [[permanent/assume-and-process|Assume and Process]]

**Related Notes**:
- [[permanent/clarify-first-strategy]]
- [[permanent/ambiguous-goal]]

### Audit Logging
**Definition**: Structured logging of agent handoff events to provide the observability needed to diagnose failures.
**Sources**:
- [[permanent/audit-logging-for-agent-handoffs|Audit Logging for Agent Handoffs]]

**Related Notes**:
- [[permanent/multi-agent-orchestration]]

### Authority Boundaries
**Definition**: The scope of a sub-agent's permissions and access, guided by the Principle of Least Privilege.
**Sources**:
- [[permanent/authority-boundaries|Authority Boundaries]]

**Related Notes**:
- [[permanent/subagent-definition]]
- [[permanent/context-isolation]]
- [[permanent/orchestrator-agentic]]

### CCAF Framework
**Definition**: A method to mitigate risks associated with designing and building agentic systems.
**Sources**:
- [[permanent/ccaf-framework|CCAF Framework]]

**Related Notes**:
- [[permanent/agentic-systems]]

### Chatbots
**Definition**: Stateless conversational systems that cannot perform actions in the real world.
**Sources**:
- [[permanent/chatbots|Chatbots]]

**Related Notes**:
- [[permanent/agentic-systems]]
- [[permanent/tool-use-lifecycle]]

### Clarify First Strategy
**Definition**: A cautious approach to handling ambiguity where an agent pauses execution to seek information from a human.
**Sources**:
- [[permanent/clarify-first-strategy|Clarify First Strategy]]

**Related Notes**:
- [[permanent/assume-and-process]]
- [[permanent/ambiguous-goal]]

### Context Capacity
**Definition**: The amount of information (instructions, history, tool results) a language model can hold in its context window at one time.
**Sources**:
- [[permanent/context-capacity|Context Capacity]]

**Related Notes**:
- [[permanent/agentic-loop]]

### Context Isolation
**Definition**: A design principle where sub-agents start with a "blank" context and only see information explicitly handed over by the orchestrator.
**Sources**:
- [[permanent/context-isolation|Context Isolation]]

**Related Notes**:
- [[permanent/subagent-definition]]
- [[permanent/authority-boundaries]]
- [[permanent/handoff-messages]]

### Decomposed Task
**Definition**: A sub-task broken down from a larger, more complex goal, designed to be independently retryable, delegatable, and testable.
**Sources**:
- [[permanent/decomposed-task|Decomposed Task]]

**Related Notes**:
- [[permanent/task-decomposition]]
- [[permanent/monolithic-task]]
- [[permanent/well-defined-subtasks]]

### Dependency Graph
**Definition**: A directed acyclic graph (DAG) used to model the relationships between subtasks in a complex workflow, making the order of operations explicit.
**Sources**:
- [[permanent/dependency-graph|Dependency Graph (Agentic)]]

**Related Notes**:
- [[permanent/task-decomposition]]
- [[permanent/sequential-pipeline]]
- [[permanent/parallel-execution]]
- [[permanent/true-vs-artificial-dependency]]
- [[permanent/orchestrator-agentic]]

### Deterministic Workflows
**Definition**: Fixed sequences of steps designed by developers, providing predictability and control.
**Sources**:
- [[permanent/deterministic-workflows|Deterministic Workflows]]

**Related Notes**:
- [[permanent/agentic-systems]]
- [[permanent/workflow-patterns]]
- [[permanent/workflow-first-agentic-architecture]]

### Dynamic Planning
**Definition**: The ability of an agent to adapt its plan during execution based on real-time observations from its environment.
**Sources**:
- [[permanent/dynamic-planning|Dynamic Planning]]

**Related Notes**:
- [[permanent/static-planning]]
- [[permanent/replanning]]
- [[permanent/goal-drift]]

### Dynamic Replanning
**Definition**: The process of revising a plan mid-execution in response to failures or environment changes.
**Sources**:
- [[permanent/dynamic-replanning|Dynamic Replanning]]

**Related Notes**:
- [[permanent/dynamic-planning]]
- [[permanent/replanning-triggers]]
- [[permanent/goal-drift]]

### Error Propagation
**Definition**: The process of surfacing sub-agent failures back to the orchestrator to enable intelligent error management.
**Sources**:
- [[permanent/error-propagation-in-multi-agent-systems|Error Propagation in Multi-Agent Systems]]

**Related Notes**:
- [[permanent/multi-agent-orchestration]]

### Error Threshold
**Definition**: A safety mechanism that stops an agent's execution loop after a certain number of errors occur.
**Sources**:
- [[permanent/error-threshold|Error Threshold]]

**Related Notes**:
- [[permanent/termination-conditions]]
- [[permanent/turn-budgets]]
- [[permanent/escalation]]

### Escalation
**Definition**: A loop control mechanism where an agent passes a problem it cannot solve to a human operator or fallback system.
**Sources**:
- [[permanent/escalation|Escalation (Agentic)]]

**Related Notes**:
- [[permanent/termination-conditions]]
- [[permanent/error-threshold]]

### Evaluator Optimizer
**Definition**: A workflow pattern using a generate-evaluate-refine cycle to close quality gaps in an agent's output.
**Sources**:
- [[permanent/evaluator-optimizer|Evaluator Optimizer]]

**Related Notes**:
- [[permanent/workflow-patterns]]
- [[permanent/iterative-refinement-loops]]

### False Parallelism
**Definition**: An anti-pattern where tasks with hidden dependencies are treated as independent and run in parallel, causing hazards.
**Sources**:
- [[permanent/false-parallelism|False Parallelism]]

**Related Notes**:
- [[permanent/parallel-decomposition]]
- [[permanent/true-vs-artificial-dependency]]

### Four-Phase Lifecycle of a Tool Call
**Definition**: The fundamental mechanic of agent interaction: Decision, Execution, Observation, Feedback.
**Sources**:
- [[permanent/four-phase-lifecycle-of-a-tool-call|Four-Phase Lifecycle of a Tool Call]]

**Related Notes**:
- [[permanent/tool-use-lifecycle]]
- [[permanent/agentic-loop]]

### Goal Drift
**Definition**: A risk where an agent revises its plan to overcome obstacles but strays from its original goals.
**Sources**:
- [[permanent/goal-drift|Goal Drift]]

**Related Notes**:
- [[permanent/dynamic-planning]]
- [[permanent/replanning]]

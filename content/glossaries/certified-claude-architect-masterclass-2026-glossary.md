# Glossary: Certified Claude Architect Masterclass 2026

This glossary defines key terms from the "Certified Claude Architect Masterclass 2026" course, focusing on agentic architecture and planning.

---

### Task Decomposition

**Definition**: Breaking down a complex, high-level goal into a series of smaller, more manageable subtasks.
**Sources**:
**Related Notes**:
- [[permanent/task-decomposition]]

---

### Action Environment Gap
**Definition**: The difference between what a language model can describe in text and what it can actually execute in a real-world environment.
**Sources**:
- [[permanent/action-environment-gap]]
**Related Notes**:
- [[permanent/tool-use-lifecycle]]

---

### Agentic Systems
**Definition**: Systems that can autonomously pursue goals by perceiving their context, selecting actions, invoking tools, and iterating on the results until the goal is completed.
**Sources**:
- [[permanent/agentic-systems]]
**Related Notes**:
- [[permanent/agentic-loop]]
- [[permanent/deterministic-workflows]]

---

### Assume and Process
**Definition**: A strategy for handling ambiguity where an agent makes a reasonable inference about an ambiguous goal and continues execution without waiting for human clarification.
**Sources**:
- [[permanent/assume-and-process]]
**Related Notes**:
- [[permanent/clarify-first-strategy]]
- [[permanent/ambiguous-goal]]

---

### Authority Boundaries
**Definition**: Defines the scope of a sub-agent's permissions and access, applying the Principle of Least Privilege.
**Sources**:
- [[permanent/authority-boundaries]]
**Related Notes**:
- [[permanent/subagent-definition]]
- [[permanent/context-isolation]]

---

### CCAF Framework
**Definition**: A method to mitigate risks associated with agentic systems.
**Sources**:
- [[permanent/ccaf-framework]]
**Related Notes**:
- [[permanent/agentic-systems]]

---

### Chatbots
**Definition**: Systems that can generate human-like text responses in a conversation but are fundamentally stateless and cannot take actions.
**Sources**:
- [[permanent/chatbots]]
**Related Notes**:
- [[permanent/agentic-systems]]

---

### Context Capacity
**Definition**: The amount of information that a language model can hold in its context window at one time.
**Sources**:
- [[permanent/context-capacity]]
**Related Notes**:
- [[permanent/agentic-loop]]

---

### Context Isolation
**Definition**: A design principle where sub-agents start with a "blank" context and only see the information that is explicitly handed over to them.
**Sources**:
- [[permanent/context-isolation]]
**Related Notes**:
- [[permanent/subagent-definition]]
- [[permanent/handoff-messages]]

---

### Dynamic Replanning
**Definition**: The process of revising a plan mid-execution in response to a failed step, unexpected tool output, or a change in the environment's state.
**Sources**:
- [[permanent/dynamic-replanning]]
**Related Notes**:
- [[permanent/dynamic-planning]]
- [[permanent/replanning-triggers]]

---

### Error Threshold
**Definition**: A form of loop control where an agent is programmed to stop its execution loop after a certain number of errors have occurred.
**Sources**:
- [[permanent/error-threshold]]
**Related Notes**:
- [[permanent/termination-conditions]]

---

### Escalation (Agentic)
**Definition**: A loop control mechanism where an agent passes a problem it cannot solve to a human operator or a fallback system.
**Sources**:
- [[permanent/escalation]]
**Related Notes**:
- [[permanent/termination-conditions]]

---

### Evaluator Optimizer
**Definition**: A workflow pattern that uses a generate-evaluate-refine cycle to close quality gaps in an agent's output.
**Sources**:
- [[permanent/evaluator-optimizer]]
**Related Notes**:
- [[permanent/workflow-patterns]]
- [[permanent/iterative-refinement-loops]]

---

### False Parallelism
**Definition**: An anti-pattern where tasks that have a hidden dependency on each other are treated as independent and run in parallel.
**Sources**:
- [[permanent/false-parallelism]]
**Related Notes**:
- [[permanent/parallel-decomposition]]

---

### Four-Phase Lifecycle of a Tool Call
**Definition**: The process an agent follows to use a tool: Decision, Execution, Observation, Feedback.
**Sources**:
- [[permanent/four-phase-lifecycle-of-a-tool-call]]
**Related Notes**:
- [[permanent/tool-use-lifecycle]]

---

### Goal Drift
**Definition**: A risk in adaptive planning where an agent strays from its original goals while revising its plan.
**Sources**:
- [[permanent/goal-drift]]
**Related Notes**:
- [[permanent/dynamic-planning]]

---

### Handoff Messages
**Definition**: Structured payloads passed between agents, containing the necessary context for a sub-agent to perform its task.
**Sources**:
- [[permanent/handoff-messages]]
**Related Notes**:
- [[permanent/handoff-point]]

---

### Hub-and-Spoke Topology
**Definition**: A multi-agent orchestration pattern where a central orchestrator routes all tasks to peripheral expert agents.
**Sources**:
- [[permanent/hub-and-spoke-topology]]
**Related Notes**:
- [[permanent/multi-agent-topologies]]

---

### Hybrid Topologies
**Definition**: Multi-agent orchestration patterns that combine different patterns (e.g., Hub-and-Spoke, Pipeline, Peer-to-Peer) to leverage the strengths of each.
**Sources**:
- [[permanent/hybrid-topologies]]
**Related Notes**:
- [[permanent/multi-agent-topologies]]

---

### Incremental Complexity
**Definition**: A design principle that emphasizes starting with the simplest possible architecture and only adding complexity when necessary.
**Sources**:
- [[permanent/incremental-complexity]]
**Related Notes**:
- [[permanent/workflow-first-agentic-architecture]]

---

### Independent Testability
**Definition**: A characteristic of a well-designed sub-agent, meaning it can be tested in isolation.
**Sources**:
- [[permanent/independent-testability]]
**Related Notes**:
- [[permanent/subagent-definition]]

---

### Infinite Loops
**Definition**: A failure mode where an agent gets stuck in a cycle of repetitive actions that yield no meaningful progress.
**Sources**:
- [[permanent/infinite-loops]]
**Related Notes**:
- [[permanent/termination-conditions]]

---

### Instruction Design Principles
**Definition**: Principles for designing instructions for sub-agents, including narrow scope, precise output formats, and explicit success criteria.
**Sources**:
- [[permanent/instruction-design-principles]]
**Related Notes**:
- [[permanent/subagent-definition]]

---

### Iterative Refinement Loops
**Definition**: Generate-evaluate-refine cycles for closing quality gaps in an agent's output.
**Sources**:
- [[permanent/iterative-refinement-loops]]
**Related Notes**:
- [[permanent/evaluator-optimizer]]

---

### Merging Strategies
**Definition**: Methods used at the fan-in stage to combine parallel outputs (e.g., voting, concatenation, structured aggregation).
**Sources**:
- [[permanent/merging-strategies-agentic]]
**Related Notes**:
- [[permanent/fan-in]]

---

### Multi-Agent Topologies
**Definition**: Patterns of communication and coordination between agents in a multi-agent system.
**Sources**:
- [[permanent/multi-agent-topologies]]
**Related Notes**:
- [[permanent/hub-and-spoke-topology]]

---

### Open-ended Goals
**Definition**: Objectives where the target is known, but the exact steps to achieve it cannot be fully pre-specified at design time.
**Sources**:
- [[permanent/open-ended-goals]]
**Related Notes**:
- [[permanent/agentic-systems]]

---

### Orchestrator Sub-Agent
**Definition**: A workflow pattern where a main orchestrator delegates a complex sub-task to another, more specialized orchestrator.
**Sources**:
- [[permanent/orchestrator-sub-agent]]
**Related Notes**:
- [[permanent/workflow-patterns]]

---

### Over-Engineering
**Definition**: The anti-pattern of introducing unnecessary architectural complexity.
**Sources**:
- [[permanent/over-engineering]]
**Related Notes**:
- [[permanent/incremental-complexity]]

---

### Over-specified
**Definition**: A handoff message that contains extraneous information not relevant to the sub-agent's task.
**Sources**:
- [[permanent/over-specified]]
**Related Notes**:
- [[permanent/handoff-messages]]

---

### Peer-to-Peer (P2P) Topology
**Definition**: A multi-agent orchestration pattern where agents communicate directly with each other without a central coordinator.
**Sources**:
- [[permanent/peer-to-peer-topology]]
**Related Notes**:
- [[permanent/multi-agent-topologies]]

---

### Perception, Reasoning, Action, and Observation (PRAO)
**Definition**: The fundamental process (agentic loop) that enables an agent to operate autonomously.
**Sources**:
- [[permanent/perception-reasoning-action-and-observation]]
**Related Notes**:
- [[permanent/agentic-loop]]

---

### Pipeline Topology
**Definition**: A multi-agent orchestration pattern where agents are arranged in a linear sequence.
**Sources**:
- [[permanent/pipeline-topology]]
**Related Notes**:
- [[permanent/multi-agent-topologies]]

---

### Progress vs. Spinning
**Definition**: In an agentic loop, progress is marked by new observations, while spinning consists of repetitive actions with no meaningful results.
**Sources**:
- [[permanent/progress-vs-spinning]]
**Related Notes**:
- [[permanent/infinite-loops]]

---

### Prompt Chaining
**Definition**: The simplest workflow pattern, where the output of one LLM call is passed directly as input to the next.
**Sources**:
- [[permanent/prompt-chaining]]
**Related Notes**:
- [[permanent/workflow-patterns]]

---

### Replanning Triggers
**Definition**: Events that necessitate a revision of the current plan.
**Sources**:
- [[permanent/replanning-triggers]]
**Related Notes**:
- [[permanent/replanning]]

---

### Result Aggregation
**Definition**: A core function of an orchestrator agent to validate, synthesize, and handoff results from sub-agents.
**Sources**:
- [[permanent/result-aggregation]]
**Related Notes**:
- [[permanent/orchestrator-agentic]]

---

### Role of the Orchestrator
**Definition**: To manage tasks and synthesize results in a workflow or agentic system.
**Sources**:
- [[permanent/role-of-the-orchestrator]]
**Related Notes**:
- [[permanent/orchestrator-agentic]]

---

### Routing (Workflow Pattern)
**Definition**: A workflow pattern that uses a classifier to sort inputs before processing.
**Sources**:
- [[permanent/routing]]
**Related Notes**:
- [[permanent/workflow-patterns]]

---

### Schema Versioning
**Definition**: The practice of assigning version numbers to handoff message schemas to ensure forward compatibility.
**Sources**:
- [[permanent/schema-versioning]]
**Related Notes**:
- [[permanent/handoff-messages]]

---

### Separation of Responsibilities (Agentic)
**Definition**: The principle of dividing the work between Coordination (the orchestrator) and Execution (the sub-agents).
**Sources**:
- [[permanent/separation-of-responsibilities]]
**Related Notes**:
- [[permanent/orchestrator-agentic]]

---

### Sequential Execution
**Definition**: A workflow where tasks are performed one after another in a fixed, deterministic order.
**Sources**:
- [[permanent/sequential-execution]]
**Related Notes**:
- [[permanent/sequential-pipeline]]

---

### State Accumulation
**Definition**: A condition in sequential pipelines where each step builds on the context or state produced by the earlier steps.
**Sources**:
- [[permanent/state-accumulation]]
**Related Notes**:
- [[permanent/sequential-pipeline]]

---

### Stop Signal
**Definition**: A form of loop control where a command from the orchestrator or a human user instructs an agent to cease its operations.
**Sources**:
- [[permanent/stop-signal]]
**Related Notes**:
- [[permanent/termination-conditions]]

---

### Subagent Definition
**Definition**: A distinct agent instance created by an orchestrator for a specific, delegated task.
**Sources**:
- [[permanent/subagent-definition]]
**Related Notes**:
- [[permanent/orchestrator-agentic]]

---

### Task Assignment
**Definition**: A core function of an orchestrator agent that involves routing and matching subtasks to the appropriate sub-agents.
**Sources**:
- [[permanent/task-assignment]]
**Related Notes**:
- [[permanent/orchestrator-agentic]]

---

### Task Completion
**Definition**: A termination condition for an agentic loop, where the agent stops its execution because its goal has been successfully achieved.
**Sources**:
- [[permanent/task-completion]]
**Related Notes**:
- [[permanent/termination-conditions]]

---

### Termination Conditions
**Definition**: Explicit criteria that indicate when an agent should end its execution loop.
**Sources**:
- [[permanent/termination-conditions]]
**Related Notes**:
- [[permanent/infinite-loops]]

---

### Tool Use (Agentic)
**Definition**: The mechanism by which an agentic system takes action in the real world.
**Sources**:
- [[permanent/tool-use]]
**Related Notes**:
- [[permanent/tool-use-lifecycle]]

---

### Turn Budgets
**Definition**: A strict limit on the number of iterations an agent can execute.
**Sources**:
- [[permanent/turn-budgets]]
**Related Notes**:
- [[permanent/termination-conditions]]

---

### Under-specified
**Definition**: A handoff message that lacks the necessary context for a sub-agent to perform its task correctly.
**Sources**:
- [[permanent/under-specified]]
**Related Notes**:
- [[permanent/handoff-messages]]

---

### Unpredictable Tool Sequences
**Definition**: When the specific series of tools an agent needs to call cannot be known at design time.
**Sources**:
- [[permanent/unpredictable-tool-sequences]]
**Related Notes**:
- [[permanent/agentic-systems]]

---

### Well-Defined Subtasks
**Definition**: A subtask with a single responsibility, defined inputs, a bounded scope, and clean, structured outputs.
**Sources**:
- [[permanent/well-defined-subtasks]]
**Related Notes**:
- [[permanent/task-decomposition]]


**Definition**: The critical boundary in a multi-agent workflow where control and context are passed from one agent to another.
**Sources**:
- [[permanent/handoff-point]]
**Related Notes**:
- [[permanent/task-decomposition]]
- [[permanent/error-cascade]]

---

### Orchestrator

**Definition**: A controlling agent or service responsible for managing the entire workflow, including interpreting the goal, decomposing tasks, and coordinating execution.
**Sources**:
- [[permanent/orchestrator-agentic]]
**Related Notes**:
- [[permanent/task-decomposition]]
- [[permanent/dependency-graph]]
- [[permanent/handoff-point]]

---

### Monolithic Task

**Definition**: A task where one agent handles everything from start to finish. It's simple to assign but hard to debug and difficult to parallelize.
**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
**Related Notes**:
- [[permanent/task-decomposition]]
- [[permanent/over-decomposition]]

---

### Decomposed Task

**Definition**: A task that has been split into well-defined, independently retryable, delegatable, and testable subtasks.
**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
**Related Notes**:
- [[permanent/task-decomposition]]

---

### Hierarchical Decomposition

**Definition**: A pattern of recursively breaking down goals into sub-goals, creating a tree structure where each level manages its own tier of complexity.
**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
**Related Notes**:
- [[permanent/task-decomposition]]

---

### Sequential Decomposition

**Definition**: A pattern where tasks are executed in a defined linear order, with each task's output serving as the input for the next.
**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
**Related Notes**:
- [[permanent/task-decomposition]]
- [[permanent/sequential-pipeline]]

---

### Parallel Decomposition

**Definition**: A pattern where independent tasks are run simultaneously to reduce overall execution time, requiring a synchronization step to converge results.
**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
**Related Notes**:
- [[permanent/task-decomposition]]
- [[permanent/parallel-execution]]

---

### Sequential Pipeline

**Definition**: An agentic workflow pattern where multiple specialized agents are arranged in a linear chain, processing tasks in a fixed, predetermined order.
**Sources**:
- [[permanent/sequential-pipeline]]
**Related Notes**:
- [[permanent/task-decomposition]]
- [[permanent/error-cascade]]
- [[permanent/true-vs-artificial-dependency]]

---

### Dependency Graph

**Definition**: A directed acyclic graph (DAG) used to model the relationships between subtasks, where nodes represent tasks and edges represent true dependencies.
**Sources**:
- [[permanent/dependency-graph]]
**Related Notes**:
- [[permanent/task-decomposition]]
- [[permanent/true-vs-artificial-dependency]]

---

### Error Cascade

**Definition**: A critical failure mode where a minor, localized error in one agent propagates and amplifies through a chain of subsequent agents.
**Sources**:
- [[permanent/error-cascade]]
**Related Notes**:
- [[permanent/sequential-pipeline]]
- [[permanent/handoff-point]]

---

### True vs. Artificial Dependency

**Definition**: A **true dependency** is a structural requirement for one task to have the output of another. An **artificial dependency** is a sequence imposed by convention rather than necessity.
**Sources**:
- [[permanent/true-vs-artificial-dependency]]
**Related Notes**:
- [[permanent/dependency-graph]]
- [[permanent/sequential-pipeline]]

---

### Over-decomposition

**Definition**: An anti-pattern where a task is broken down into an excessive number of granular subtasks, causing coordination overhead to outweigh the benefits.
**Sources**:
- [[permanent/over-decomposition]]
**Related Notes**:
- [[permanent/task-decomposition]]

---

### Under-decomposition

**Definition**: An anti-pattern where tasks are left too large and monolithic, making them difficult to retry, delegate reliably, or debug.
**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
**Related Notes**:
- [[permanent/task-decomposition]]
- [[permanent/over-decomposition]]

---

### Parallel Execution

**Definition**: An orchestration pattern where multiple independent tasks are run concurrently to reduce total end-to-end latency.
**Sources**:
- [[permanent/parallel-execution]]
**Related Notes**:
- [[permanent/fan-out]]
- [[permanent/fan-in]]

---

### Fan-out

**Definition**: The process within a parallel execution pattern where a single task is split into multiple, independent subtasks that are executed concurrently.
**Sources**:
- [[permanent/fan-out]]
**Related Notes**:
- [[permanent/parallel-execution]]
- [[permanent/fan-in]]

---

### Fan-in

**Definition**: The synchronization step that follows a fan-out, responsible for collecting, handling failures, ordering, and merging the results from parallel branches.
**Sources**:
- [[permanent/fan-in]]
**Related Notes**:
- [[permanent/parallel-execution]]
- [[permanent/fan-out]]

---

### Static Planning

**Definition**: An approach where all steps in a plan are defined before execution starts. It is predictable and auditable.
**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
**Related Notes**:
- [[permanent/task-decomposition]]

---

### Dynamic Planning

**Definition**: An approach where an agent adapts its plan during execution based on real-time observations and tool outputs.
**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
**Related Notes**:
- [[permanent/task-decomposition]]

---

### Replanning

**Definition**: The process of revising the current plan mid-execution in response to a failed step, unexpected tool output, or a change in the environment state.
**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
**Related Notes**:
- [[permanent/task-decomposition]]

---

### Ambiguous Goal

**Definition**: A goal that lacks a clear, single definition, leaving multiple valid interpretations for an agent to choose from.
**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
**Related Notes**:
- [[permanent/task-decomposition]]

---

### Clarify First Strategy

**Definition**: A strategy for handling ambiguity where the agent pauses execution to request more information from a human before proceeding, used when the cost of a wrong assumption is high.
**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
**Related Notes**:
- [[permanent/task-decomposition]]

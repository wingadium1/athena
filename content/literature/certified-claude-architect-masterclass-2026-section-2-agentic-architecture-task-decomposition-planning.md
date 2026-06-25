---
title: "Section 2: Agentic Architecture - Task Decomposition & Planning"
type: literature
source: "Udemy — Claude Certified Architect (CCA-F) 2026 Exam Prep"
author: "Jacob Bushong"
date-read: 2026-06-23
updated: 2026-06-23
tags: [claude, ai-agents, architecture, course]
---
# CCA-F Section  2: Agentic Architecture - Task Decomposition & Planning

## Summary

<!-- filled during FINISH SECTION -->

## Key Ideas

### L1: Break complex goals into actionable subtasks

1. **Definition of Key Concepts**:
   - [[permanent/task-decomposition|Task decomposition]]: Breaking a high-level goal into smaller subtasks → assign to agents, tools, workflow steps
   - [[permanent/handoff-point|Handoff point]]: Boundary between subtasks where one step's output becomes the next step's input → requires an explicit schema
   - [[permanent/orchestrator|Orchestrator]]: Controlling agent that decomposes goal → assigns subtasks → manages execution order → synthesizes result
2. **Importance of Decomposition**: Agents struggle with vague goals, leading to inefficiencies and potential failures. Decomposing tasks into subtasks makes them independently retryable and testable, enhancing overall reliability → clear boundaries let multiple agents work together
3. **Caution Against Fragility (the decomposability spectrum)**: Avoid [[permanent/monolithic-task|monolithic tasks]] (fragile, hard to debug) and [[permanent/over-decomposition|over-decomposed tasks]] (excessive management overhead) → goal: subtask is meaningful and executable
4. **[[permanent/monolithic-task|Monolithic]] vs [[permanent/decomposed-task|decomposed task]]**:
   - Monolithic: one agent handles everything → simple to assign initially → hard to debug → fails completely on any error → hard to parallelize or retry
   - Decomposed: split into well-defined subtasks → independently retryable, delegatable, testable → upfront design cost
5. **Analyzing Goal Structures**: Three critical questions to identify natural boundaries:
   - Can any part run independently of the rest?
   - Does one step's output feed directly into another?
   - Are there distinct capability domains that need different agents?
6. **Characteristics of Well-Defined Subtasks**: Single responsibility, defined inputs, bounded scope, clean structured outputs
7. **Handoff Points**: Critical junctions where output of one subtask becomes input of another. Potential failure points → pass only necessary information, use structured schemas, document what is NOT passed to prevent false assumptions
8. **Cost of Decomposition**:
   - More subtasks → more orchestration logic and latency
   - Handoff is a potential failure point
   - Deeply decomposed systems are harder to trace and debug
9. **Trade-off**: Balance between reliability and complexity — thoughtful decomposition maintains reliability without high management costs
10. **Next lesson preview**: Decomposition patterns — hierarchical, sequential, and parallel approaches

### L2: Decomposition patterns — hierarchical, sequential, and parallel

1. **[[permanent/hierarchical-decomposition|Hierarchical Decomposition]]**:
   - Recursively breaking down goals into sub-goals, creating a tree structure
   - Each level manages only its tier of complexity — similar to how organizations delegate from executives to departments
2. **[[permanent/sequential-decomposition|Sequential Decomposition]]**:
   - Tasks executed in a defined order, each task's output serving as input for the next
   - Straightforward but inefficient if tasks don't inherently depend on one another
   - Appropriate whenever genuine data dependencies exist between steps
3. **[[permanent/parallel-decomposition|Parallel Decomposition]]**:
   - Build a goal tree → break goal into subgoals → each level handles its tier → results flow back up through the hierarchy to be synthesized
   - Independent tasks run simultaneously to reduce overall execution time
   - Parallel branches must be truly independent → no shared state
   - Requires synchronization when converging back to main workflow → fan-in sync step and explicit handling for partial failure
   - Powerful but introduces failure scenarios that sequential patterns avoid
4. **Go Deep vs. Flatten**:
   - Hierarchy depth is a design choice — trade-off between specialization and coordination overhead
   - Deeper when subtasks require fundamentally different capabilities
   - Flatten when subtasks are similar enough to share one agent
   - Each added level increases error propagation surface
   - Flatten wherever possible
5. **Real Systems Mix Patterns**:
   - Most systems combine these decomposition patterns in practice
   - Recognizing which pattern applies to which part of the workflow is essential
   - Example: pipeline stages run sequentially but parallel within each stage
6. **Common Anti-Patterns**:
   - [[permanent/over-decomposition|Over-decomposition]]: too many steps → coordination costs > execution costs
   - Under-decomposition: tasks too large to retry and delegate reliably
   - [[permanent/false-parallelism|False parallelism]]: treating dependent steps as independent → data hazard

### L3: Sequential Pipelines - Design & Tradeoff

This lecture on task decomposition covers several foundational concepts and techniques essential for efficiently managing complex goals within agentic systems. Here are the main points and key takeaways:

1.  **3 pipeline concepts**
    *   [[permanent/sequential-execution|Sequential Execution]] -> easy to debug / harder to speed up -> execution order is fixed and deterministic -> no branching
    *   [[permanent/prompt-chaining|Prompt chaining]] -> each step has a single/narrow job
    *   [[permanent/dependency-graph|Dependency graph]] -> show dependency -> reveals ordering and steps could run in parallel
2.  **When to use sequential**:
    *   Data dependency
    *   [[permanent/state-accumulation|State accumulation]] -> each step builds on context that earlier steps procedures
    *   Order transforms -> operation run in a strict order / one stage at a time
    *   Simplicity goal -> debugging and audibility matter mỏe than raw speed
3.  **Task Decomposition**: This is the process of breaking down high-level goals into smaller, manageable subtasks that can be executed by agents. This clarification promotes better understanding and execution among agents.
4.  **Sequential: Simplicity vs Latency Accumulation**
    *   Strengths: Easy to debug, Clear data flow, failure are isolated/easy to trace, No sync logic, Correct when steps have true data dependencies
    *   Costs: TOtal latency accumulation, independent step (which can run concurrently) are forces wait, wasted wall-clock (when data dependencies don’t actual exist)
5.  **[[permanent/error-cascade|Error cascades]] in sequential pipelines** -> failure step block downstream -> cascade risk grows with depth - more steps, more failure surface -> early validation @ step entry reduce bad data travel -> design to error handing at each step boundary not the end of pipeline
6.  **[[permanent/true-vs-artificial-dependency|True vs Artificial dependency]]**
    *   True dependencies -> downstream need output of upstream
    *   Artificial dip -> steps are sequence only by convention (not data necessity) -> remove this let independent steps run at the same time
7.  **Key Vocabulary**:
    *   [[permanent/handoff-point|Handoff Point]]: The transition between two subtasks, where the output from one serves as the input for the next. Clear definition and structure are crucial here (loose handoff cause silent data loss and hard-to-diagnose downstream errors)
    *   [[permanent/orchestrator|Orchestrator]]: The controlling agent responsible for overseeing the workflow, assigning subtasks, managing their execution order, and aggregating the results.
8.  **Importance of Decomposition**: When goals aren't well-defined and broken down, agents may struggle to infer meaning and tasks. Without clear boundaries, agents are likely to misinterpret their priorities, leading to failed attempts at accomplishing complex tasks.
9.  **Analyzing Goals**: It's vital to analyze complex goals for natural boundaries to avoid vague specifications that can lead to confusion and errors among agents.
10. **Real Costs of Over-Decomposition**: While breaking down tasks is beneficial, over-decomposition can lead to excessive management overhead and complexity, making tasks cumbersome rather than streamlined.

**Key Takeaway**: Proper task decomposition is a fundamental skill for anyone building agentic systems, as it helps clarify workflows, improves execution efficiency, and reduces the likelihood of errors stemming from undefined tasks.

### L4: Parallel Execution - Fan-out, Fan-in and Synchronization

This lecture focuses on task decomposition, which is the essential skill of breaking complex goals into smaller, manageable subtasks. Here are the main points covered:

1.  **Definition and Importance**:
    *   [[permanent/parallel-execution|Parallel executions]] run independent tasks at the same time, cutting overall runtime. However, synchronization points are needed to collect results and manage any partial failures.
2.  **[[permanent/fan-out|Fan-out]]**: This is the process where a single task splits into multiple concurrent subtasks. Each of these branches must operate independently. Key criteria to test this independence include (Independence test):
    *   Can a branch start without waiting for another?
    *   Do branches write to shared states that could cause race conditions?
    *   Does the order of execution affect the final result? If dependencies exist, fan-out is not suitable and adjustments are necessary.
3.  **[[permanent/fan-in|Fan-in]]**: This refers to the synchronization step that gathers outputs from successful branches. It also involves managing failures, reordering results if needed, and merging outputs for subsequent pipeline steps. Explicitly addressing partial failures is crucial to prevent silent failures that can lead to incorrect outcomes.
    *   4 fan-in responsibility
        *   Collect result
        *   Handle failure
        *   Order result
        *   Merge & Continue
4.  **Trade-offs (Parallel vs Sequential)**: While parallel execution can save time, it comes at the cost of increased complexity in failure management. Strategies for handling failures in parallel branches include ([[permanent/partial-failure-handling|partial failure handling]]):
    *   Failing the entire pipeline.
    *   Proceeding with partial results
    *   Retrying only the failed branches.
    *   Note that: silently dropping failed branches introduces silent failure -> always handle explicitly
5.  **[[permanent/merging-strategies-agentic|Merging Strategies]]**:
    *   Fan-in combines results using methods like
        1.  voting (majority result wins)
        2.  concatenation (join result into one list)
        3.  structured aggregation (contribute field into a shared object)
    *   Choosing a merging strategy in advance is necessary for effective branch design.
6.  **Cost vs Latency in Parallel exec**
    *   Cut latency but increase token spend for the same period (do many job in parallel)
    *   Token cost are sum
    *   Rate limits and quotas may constraint the parallel

**Key Takeaway**: Although parallel execution can improve performance, it introduces complexities and higher costs, especially in managing synchronization and failures. Careful planning is essential to ensure successful implementation.

## Quotes

## My Take

<!-- Vietnamese -->

## Links

- [[permanent/task-decomposition]]
- [[permanent/sequential-pipeline]]
- [[permanent/dependency-graph]]
- [[permanent/error-cascade]]
- [[permanent/true-vs-artificial-dependency]]
- [[permanent/handoff-point]]
- [[permanent/orchestrator-agentic]]
- [[permanent/over-decomposition]]
- [[permanent/parallel-execution]]
- [[permanent/fan-out]]
- [[permanent/fan-in]]
- [[permanent/partial-failure-handling]]
- [[permanent/merging-strategies-agentic]]

### L5: Adaptive Planning - Update the plan mid-execution

This lecture focuses on adaptive planning within agentic systems, covering the differences between static and dynamic planning, as well as the mechanisms for updating plans mid-execution. Here are the main points:

1.  **3 planning concepts**
    1.  [[permanent/static-planning|Static planning]]
    2.  [[permanent/dynamic-planning|Dynamic planning]]
    3.  [[permanent/replanning|Replanning]] -> revising the current plan in response to a failed step (Or unexpected tool output or changed environment state)
2.  **Static vs. Dynamic Planning** (most systems combine elements of both approach)
    *   [[permanent/static-planning|Static Planning]]: All steps are defined before execution starts. It’s predictable and auditable since every action is pre-written, allowing for precise failure localization. (Every action in known at design time)
    *   [[permanent/dynamic-planning|Dynamic Planning]]: The agent adapts its plan during execution based on real-time observations. While this allows for greater adaptability, it introduces unpredictability, requiring stronger monitoring and fallback mechanisms, the plans are generated or revised at inference time by model
3.  **Trade-offs**:
    *   Predictable auditable cost-efficient vs adaptable and powerful for open-end goals
4.  **[[permanent/replanning-triggers|Replanning Triggers]]**: Events that necessitate replanning include:
    *   Failed tool calls (errors or timeouts).
    *   Unexpected results that are technically valid but do not align with the intended goals.
    *   Changed state (environment is changed in mid-execution)
    *   Constraint violations -> a proposed next action would violate a goal constraint or safety boundary
5.  **Replan vs Continue**: The decision
    *   Not every surprise warrants a full replanning cycle
    *   Replan when a core assumption has been invalidated
    *   Continue when the deviation is minor and recoverable
    *   Check whether the original goal is still archivable
    *   Unnecessary replanning waste tokens and add latency
6.  **Bounding replanning cycles**
    *   Maximum iterations: hard cap of number of replanning (that agent is allowed) -> prevent infinite loops
    *   Goal Constraint Check: validation step confirm revised plan still satisfies the original req before cont exec
    *   Loop Termination: The condition that halt replanning
7.  **[[permanent/goal-drift|Goal Drift]]**: A significant risk in adaptive planning where the agent revises its plan to overcome obstacles but strays from its original goals. Mechanisms to prevent this include:
    *   Explicit validation of each revised plan against the original goal.
    *   Creating an audit trail by logging original goals alongside revised plans. (Log the original goal and check every revised plan)
    *   Preserving plan state across cycles keeps the agent anchored
8.  **Guardrails for Replanning**: To improve reliability in dynamic planning, it is essential to employ bounded planning cycles, iteration limits, and continuous checks against the original goals to ensure that adaptations do not lead to drift.

**Key Takeaway**: Adaptive planning allows agents to navigate unpredictable environments effectively, but it requires careful management of the replanning process to maintain alignment with original goals and prevent drift. Implementing structured monitoring and validation mechanisms is crucial to ensure successful outcomes.

### L6: Handling Ambiguity and Incomplete Specifications

This lecture discusses how agentic systems manage ambiguity and incomplete specifications in goals. Here are the main points:

1.  **Understanding Ambiguity**:
    *   An [[permanent/ambiguous-goal|ambiguous goal]] lacks a clear definition, leaving multiple valid interpretations. The agent has to choose one to continue, which can lead to unexpected outcomes if the assumption it makes is misaligned with human intentions.
    *   Concepts
        *   [[permanent/ambiguous-goal|Ambiguous goal]]: do not define the expected outcome
        *   [[permanent/clarify-first-strategy|Clarify first strategy]]: pause exec to request more info form human before proceeding (use when the cost of wrong assumption is high)
        *   [[permanent/assume-and-process|Assume and Process]]: making a reasonable inference and cont without human input (use when the action is low-stakes, reversible or time-sensitive)
2.  **Strategies for Handling Ambiguity**:
    *   **[[permanent/clarify-first-strategy|Clarify First]]** (irreversible actions, high-cost operations, scope uncertainty): The agent pauses execution to seek further information from a human. This adds latency but prevents costly mistakes, particularly for high-stakes tasks where assumptions could lead to significant issues, like data deletion or irreversible actions. -> a brief clarification prevents large downstream error
    *   **[[permanent/assume-and-process|Assume and Proceed]]** (low-stakes or easily reversible actions, time-sensitive tasks, well-constrained context): The agent makes a reasonable inference and moves forward without waiting for clarification. This approach is suitable for low-risk situations where the cost of a wrong assumption is minimal. -> goal is appropriate judgment, not maximum caution
3.  **Clarify or Assume: Decision**:
    *   Reversibility
    *   Stake level
    *   Scope clarity
4.  **[[permanent/iterative-refinement-loops|Iterative Refinement Loops]]** ([[permanent/evaluator-optimizer|evaluator-optimizer loop]]):
    *   Generate-evaluate-refine cycles close the quality gaps
    *   Generator produces an initial candidate output
    *   An evaluator scores it against a rubric or constraint set (return structured critique, not pass/fail signal)
    *   Feedback is passed back to the generator for revision
    *   A convergence criteria: a threshold or condition to stop the loop: quality bar is met or an iteration cap hit
    *   Judgment in Execution: The ability to determine which approach suits a situation is crucial. Understanding when to clarify and when to assume can prevent unnecessary errors and keep workflows efficient.
5.  **Costs of Ambiguity**: Ambiguous prompts can lead to unpredictable outputs, complicate quality evaluations, and increase debugging expenses. Each vague instruction can introduce variability in results, requiring more effort to reexamine and refine prompts through iterations.
6.  **Specificity in Instruction**: Providing concrete, specific instructions closes ambiguities and helps improve predictability in outputs. The more constraints and precise details included in the prompt, the narrower the interpretation space, leading to more reliable outcomes.
7.  **Document assumptions**: assumption should be visible (state in agent output or logs) -> enable human review -> well-documented assumption reduce the audit burden

**Key Takeaway**: Managing ambiguity effectively is essential for optimizing agentic system performance. By strategically choosing when to clarify versus assume and emphasizing specificity in instructions, the likelihood of errors can be significantly reduced, enhancing overall reliability.

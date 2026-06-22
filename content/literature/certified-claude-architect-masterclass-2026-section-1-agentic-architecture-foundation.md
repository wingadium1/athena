---
title: "Agentic Architecture - Foundation"
type: literature
source: "Udemy: https://www.udemy.com/course/certified-claude-architect-masterclass-2026"
author: "Jacob Bushong"
date-read: 2026-06-22
tags: [claude, udemy, course]
---

# Agentic Architecture - Foundation

## Summary

Section 1 lays the groundwork for agentic architecture by contrasting agents, workflows, and chatbots, then progressively building the loop mechanics behind tool use, perception/reasoning/action/observation, and loop control. It consistently argues that agents are powerful but costly, while deterministic workflows are preferable whenever the task is predictable and the steps can be designed upfront.

Across the section, the recurring design principle is to start simple, preserve auditability, and add agentic behavior only when open-ended goals, unpredictable tool sequences, or dynamic replanning truly justify it. The workflow patterns and decision framework are presented as practical ways to keep systems reliable, debuggable, and maintainable.

## Key Ideas

- L1: Agents, Workflows, and Chatbots — What's Actually Different
  This lecture explores the distinctions between agents, workflows, and chatbots, emphasizing the definition of agentic systems. Here are the main points:
  1. Agentic Systems: These systems autonomously pursue goals by perceiving context, selecting actions, invoking tools, and iterating until completion. This contrasts with deterministic workflows, which are predefined and do not allow for decision-making.
  2. Limitations of Chatbots: While chatbots seem powerful, they are fundamentally stateless. They can generate responses but do not take actions in the real world.
  3. Transition Necessity: Transitioning from a chatbot to an agentic system relies on effective tool use, necessitating a mechanism to decide when and how tools are utilized.
  4. Agentic Loop: This loop consists of four pillars:
    * Perception: Understanding context.
    * Selection: Choosing an action.
    * Execution: Performing the action by calling a tool.
    * Iteration: Feeding the results back into the system for further decision-making.
  5. Action Environment Gap: This represents the difference between what the model can describe and what it can actually execute, underscoring the need for a runtime layer to handle structured outputs.
  6. Practical Implementation: Cloud's tool system is used to demonstrate structured tool calls based on well-defined schemas, showcasing the importance of clear descriptions for tool selection.
  7. Risk Considerations: Agentic systems introduce higher risks due to their autonomous nature. They should only be used when necessary, while deterministic workflows are better for predictable tasks.
  8. CCAF Framework: This framework is introduced as a method to mitigate risks associated with agentic systems.
  Overall, the distinctions explored in this lecture are essential for designing effective and safe AI systems. If you have any specific questions or need clarification on any parts, feel free to ask!

- L2: Tool Use as the Foundation of Agency
  This lecture focuses on tool use as a key aspect of agency in language models (LLMs). Here are the main points:
  1. Key Definitions: It starts by defining important terms like tool definition, tool call, and observation, crucial for understanding LLM interactions with tools.
  2. Four-Phase Lifecycle of a Tool Call:
    * Decision: The model identifies the need for a tool and generates a structured tool call.
    * Execution: The runtime executes the tool.
    * Observation: The result is captured as an observation.
    * Feedback: This observation informs the model's next steps. This cycle can repeat multiple times during a task.
  3. Workflows vs. Agentic Systems: The lecture emphasizes the difference between pre-scripted workflows, where developers dictate tool usage, and agentic systems, where models dynamically select tools based on context.
  4. Importance of Clear Tool Descriptions: High-quality descriptions are crucial for reliable tool selection. Vague descriptions can lead to errors in tool usage.
  5. Types of Tools: Tools are categorized into three functional types: data access tools, computation tools, and external API tools, each with distinct characteristics and risks.
  6. Observation Phase: This phase is highlighted as a critical differentiator for agentic systems, allowing for adaptation based on real-time feedback.
  7. Key Concepts: The lecture discusses key concepts such as tool result injection, parallel tool calls, and the iterative tool call loop.
  8. Practical Applications: Concrete examples illustrate the application of these concepts and the importance of designing safe tool configurations to mitigate risks.
  In conclusion, effective tool use transforms LLM outputs into real-world actions, driven by a defined lifecycle and quality descriptions, setting the stage for further exploration of the perception-reasoning-action-observation loop in future lessons. If you have any questions or need clarification on specific points, feel free to ask!

- L3: Perception, Reasoning, Action, and Observation
  This lecture discusses the agentic loop, which is crucial for the functioning of agents and consists of four interconnected phases: perception, reasoning, action, and observation. Here are the main points:
  1. Agentic Loop Definition: The agentic loop is a continuous cycle that enables the model to perceive its context, plan actions, execute them, and observe the outcomes, allowing for dynamic adaptation.
  2. Key Concepts:
    * Tool Use: An essential aspect of the action phase, where the model makes structured calls to external tools.
    * Dynamic Planning: The model's capability to adjust its plans based on new information and changing circumstances.
  3. Phases of the Agentic Loop:
    * Perception: The model reads its context window, gathering instructions, conversation history, and tool results.
    * Reasoning: Evaluating the available information to decide the best course of action, refining its approach based on past outcomes.
    * Action: Emitting a structured tool call to perform tasks.
    * Observation: Receiving and integrating the results of the action back into the context, forming a feedback loop that aids learning and adaptation.
  4. Context Capacity: The lecture explains the importance of the context window, which fills with inputs and results over time. Efficient management of this memory is crucial for effective agent design.
  5. Practical Example: A research agent illustrates the agentic loop by processing a user query: perceiving the lack of search results, reasoning to conduct a search, taking action by calling a search API, and then observing the returned results to inform next steps.
  6. Conclusion: The agentic loop is fundamental for enabling agents to perform complex tasks dynamically. The next lesson will focus on loop control, including stopping conditions and iteration.
  If you have any questions or want to delve deeper into any specific aspect, feel free to ask!

- L4: Loop Control — Stopping, Iterating, and Escalating
  This lecture concentrates on loop control within agentic systems, elaborating on how and when an agent should stop, retry, or escalate issues. Here are the main points:
  1. Termination Conditions: These are explicit criteria that indicate when an agent should end its loop, critical for preventing infinite loops.
  2. Turn Budgets: This is a strict limit on the number of iterations an agent can execute. Setting a reasonable maximum is important to mitigate risks of continual looping.
  3. Escalation: This involves passing the problem on to a human operator or another fallback system when the agent is unable to progress further.
  4. Reasons for Stopping the Loop:
    * Task Completion: The goal has been achieved.
    * Error Threshold: An error limit has been reached.
    * Exhausting the Turn Budget: The agent has hit its iteration limit.
    * Stop Signal: A command from the orchestrator or user indicates to cease operations.
  5. Identifying Failures: Infinite loops can stem from a lack of stop conditions, ongoing errors, or circular reasoning. Implementing termination conditions is vital to address these issues.
  6. Progress vs. Spinning: Progress is marked by new observations and forward movement towards goals. In contrast, spinning consists of repetitive actions that yield no meaningful results. The lecture suggests techniques for detecting spinning, such as comparing successive observations.
  7. Design Considerations: The agent's design must clearly define when to escalate an issue, treating escalation as a safety mechanism and not merely a sign of failure.
  8. Importance of Loop Control: Effective loop control is crucial for system reliability, as runaway agents could lead to excessive costs and unpredictable behavior. Proper termination conditions enhance the system's predictability and auditability.
  The next lesson will cover breaking complex goals into actionable subtasks. If you need clarification on any points or have further questions, feel free to ask!

- L5: When Workflows Outperform Agents
  This lecture discusses the advantages of deterministic workflows compared to agentic systems for problem-solving. Here are the main points:
  1. Definitions:
      * Deterministic Workflows: Fixed sequences of steps designed by developers, providing predictability and control.
      * Agentic Systems: Allow dynamic decision-making by the model, which can lead to unpredictability.
      * 
  2. Task Suitability: The lecture states that most tasks are better suited for workflows due to their lower cost, faster execution, and easier maintenance.
  3. Core Trade-Off: A key trade-off exists where workflows offer adaptability but sacrifice predictability, while agents provide flexibility at a higher cost and complexity.
  4. Assessment Criteria: Practitioners are encouraged to assess whether a workflow is appropriate by checking:
      * Is the input format predictable?
      * Is the output format known?
      * Can every processing step be specified at design time?
  5. Workflow Patterns: Five core patterns of workflows are outlined:
      * Prompt Chaining -> simplest workflow pattern, passing one call’s output directly as input to the next -> linear, testable, easy to debug
      * Routing
      * Parallelization
      * Orchestrator Sub-Agent
      * Evaluator Optimizer These maintain predictability and auditability without the unpredictability of agents.
  6. Complexity Management: Routing and parallelization are emphasized as effective methods to handle complexity while preserving control. -> we control flow is authorized at design time, both remain fully auditable -> make systems reliable and maintainable
      1. Routing uses a classifier to sort inputs before processing
      2. The router may call an LLM, but the topology stays fixed
      3. Parallelization fans out work and collects results at a sync point
  7. Benefits of Workflows: Benefits include lower costs (by minimizing unplanned calls), reduced latency (through controlled sequencing), and easier debugging due to their deterministic nature. -> if workflows failed, the failure is localized to specific identifiable step.
  8. Agentic System Risks: In contrast, agentic systems bring variable costs and complexities that can result in unpredictable failures.
      1. Increase token spend per task run
      2. Autonomous decision is a failure surface
      3. Debugging agentic traces is harder than workflow logs
  9. Conclusion: The lecture concludes by emphasizing that agents should only be used when workflows are insufficient, advocating for disciplined engineering practices. Deterministic workflows are presented as the superior choice for predictable tasks.

- L6: Choosing the Right Architecture
  This lecture focuses on developing a decision framework for selecting the appropriate architecture between workflows and agentic systems. Here are the main points:

  1. Goal of the Framework: The aim is to provide a systematic approach for making informed choices rather than declaring one architecture superior to the other.
  2. Framework vocabulary
      1. Incremental Complexity: Emphasizes starting with the simplest architecture and only adding complexity when necessary to avoid over-engineering.
      2. Role of the Orchestrator: An orchestrator manages tasks and synthesizes results, playing a crucial role in coordinating workflows. -> decompress a high-level goal, assign subtasks to subagents or tools, and synthesizes final results
      3. Risks of Over-Engineering: The lecture warns against introducing unnecessary complexity, which can lead to increased costs and risks without proportional benefits. -> adding architecture complexity (e.g: agentic autonomy, introduces cost and risk without delivering proportional benefits for the tasks)
  3. Simplicity Benefits: Simplicity in solutions typically results in reduced costs, latency, and potential errors during production. -> start with prompt chain -> we can always add agent behavior later
  4. Signals for Agentic Complexity: It identifies three conditions that indicate when agentic complexity is warranted:
      * Open-ended goals (and cannot be fully pre-spocified) -> know the target but do not know the step at design time
      * Unpredictable tool sequences -> at design time
      * Dynamic replanning during execution -> If these conditions are not met, workflows are generally preferred.
  5. Reliability vs. Adaptability: Workflows offer predictable steps and outputs, while agents provide adaptability for more complex tasks but come with variable costs and challenging auditability.
  6. Trade-Off Axes: The framework encourages evaluating trade-offs among cost versus capability, auditability versus flexibility, and reliability versus adaptability.
  7. Incremental Introduction: Advocates for introducing agent behavior gradually, starting with a validated workflow and only replacing failing steps.
  8. Concrete Scenarios: The lecture provides examples of using workflows for fixed document data extraction, agents for researching open-ended topics, and hybrid approaches for customer inquiry routing.
  9. Diagnostic Questions: It concludes with questions to assess whether a task requires agentic complexity or can be effectively handled by a workflow.

## Quotes

## My Take

## Links

- [[permanent/workflow-first-agentic-architecture]]

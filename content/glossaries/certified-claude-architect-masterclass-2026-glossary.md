---
title: "Glossary for Certified Claude Architect Masterclass 2026"
type: glossary
course: certified-claude-architect-masterclass-2026
tags: [glossary, certified-claude-architect-masterclass-2026]
created: 2026-07-06
updated: 2026-07-07
---

This glossary defines key terms from the Certified Claude Architect Masterclass 2026.

### Agentic Loop

**Definition**: The core control-flow pattern that turns a stateless LLM into an agent: the model repeatedly perceives its context, reasons about the next step, takes action (typically a tool call), observes the result, and loops until a stop condition is met.
**Sources**: [[permanent/agentic-loop]]
**Related Notes**: [[permanent/tool-use-lifecycle]], [[permanent/context-capacity]], [[permanent/task-completion]]

### Context Window

**Definition**: Everything the model can see in a single inference call — system prompt, conversation history, injected documents, tool definitions, and the model's own output. Everything outside is completely invisible.
**Sources**: [[permanent/context-capacity]], Section 15 L1
**Related Notes**: [[permanent/context-capacity]], [[permanent/tool-use-lifecycle]]

### Token

**Definition**: The fundamental unit of text an LLM processes. ~4 characters or ~0.75 English words per token. Tokenization isn't uniform — plain English is efficient, JSON/XML are less so, and non-English text can cost 2–4× more tokens than equivalent English.
**Sources**: The Prompt Index — What Are LLM Tokens?
**Related Notes**: [[permanent/context-capacity]]

### Context Capacity

**Definition**: The amount of information (instructions, conversation history, tool results) a model can hold in its context window at one time. When exceeded, content is compacted or dropped.
**Sources**: [[permanent/context-capacity]]
**Related Notes**: [[permanent/agentic-loop]], [[permanent/tool-use-lifecycle]]

### Context Isolation

**Definition**: A design principle where sub-agents start with a blank context and only see information explicitly handed over by the orchestrator. Prevents context bloat, information leakage, and dependency interference.
**Sources**: [[permanent/context-isolation]]
**Related Notes**: [[permanent/subagent-definition]], [[permanent/authority-boundaries]]

### Subagent / Sub-agent

**Definition**: A distinct agent instance created by an orchestrator for a specific, delegated task. Operates within its own isolated context with restricted tool permissions (principle of least privilege).
**Sources**: [[permanent/subagent-definition]]
**Related Notes**: [[permanent/orchestrator]], [[permanent/context-isolation]], [[permanent/instruction-design-principles]]

### Orchestrator

**Definition**: A controlling agent in a multi-agent system that decomposes high-level goals into subtasks, assigns them to sub-agents, manages execution order, and synthesizes the final result.
**Sources**: [[permanent/orchestrator]]
**Related Notes**: [[permanent/task-decomposition]], [[permanent/result-aggregation]]

### Handoff Protocol

**Definition**: A formal contract governing the transfer of control, context, and execution authority between AI agents or between an agent and a human. Includes explicit schemas, trigger conditions, acceptance criteria, and recovery paths.
**Sources**: [[permanent/handoff-protocol]]
**Related Notes**: [[permanent/silent-failure]], [[permanent/under-specified]], [[permanent/role-of-the-orchestrator]]

### Silent Failure

**Definition**: A high-risk error condition where a task or handoff appears to complete successfully but critical information is lost or corrupted without any error being raised. Particularly dangerous in multi-agent systems.
**Sources**: [[permanent/silent-failure]]
**Related Notes**: [[permanent/error-propagation-in-multi-agent-systems]], [[permanent/transparent-failure]]

### Task Completion

**Definition**: A termination condition for an agentic loop where the agent stops because its goal has been successfully achieved.
**Sources**: [[permanent/task-completion]]
**Related Notes**: [[permanent/termination-conditions]], [[permanent/agentic-loop]]

### Slash Commands

**Definition**: User-defined shortcuts that trigger instruction templates. Keywords map to markdown filenames in `.claude/slash_commands/`. No registration needed — naming convention is the sole registration mechanism.
**Sources**: Section 10 L1–L2
**Related Notes**: [[permanent/agentic-loop]]

### Skills (skill.md)

**Definition**: Reusable, trigger-activated workflows defined in `skill.md` files with YAML frontmatter (Name, Description, Trigger). Activate automatically when trigger conditions are met (phrase-based or event-based).
**Sources**: Section 10 L3–L4
**Related Notes**: [[permanent/subagent-definition]]

### CLAUDE.md / claude.md

**Definition**: A three-level instruction hierarchy (User-level, Project-level, Team-level) providing standing rules to Claude Code. Project-level CLAUDE.md at repo root is version-controlled, team-visible, and overrides user-level on direct conflicts.
**Sources**: Section 9 L1–L5
**Related Notes**: [[permanent/context-capacity]]

### Hooks

**Definition**: User-defined scripts registered in Claude Code settings that fire automatically at specific lifecycle events (pre-tool-use, post-tool-use, stop). Run at shell level outside Claude's context window — Claude cannot see, override, or disable them.
**Sources**: Section 11 L1–L2
**Related Notes**: [[permanent/tool-use-lifecycle]], [[permanent/layered-enforcement-architecture]]

### Tool-Use Lifecycle

**Definition**: Every tool invocation follows four phases: Decision (model emits structured call), Execution (runtime validates and runs the tool), Observation (result captured), Feedback (result fed back into model context).
**Sources**: [[permanent/tool-use-lifecycle]]
**Related Notes**: [[permanent/agentic-loop]], [[permanent/context-capacity]], [[permanent/four-phase-lifecycle-of-a-tool-call]]

### Exit Codes

**Definition**: Integer values returned by processes to communicate success or failure. In hooks: `0` = continue, non-zero = block/fail, `1` = policy violation, `2` = hook script failure.
**Sources**: Section 11 L2, GNU C Library — Exit Status
**Related Notes**: [[permanent/silent-failure]]

### Plan Mode

**Definition**: A read-only thinking phase where Claude Code inspects the codebase, reasons about the task, and produces a structured proposal without touching files. Activated via `/plan` or implicitly for risky/ambiguous tasks.
**Sources**: Section 10 L7
**Related Notes**: [[permanent/iterative-refinement-loops]], [[permanent/replanning-triggers]]

### Approval Gate

**Definition**: A user checkpoint to accept, edit, or reject a proposed plan before any file writes occur. The primary quality checkpoint for developers.
**Sources**: Section 10 L8
**Related Notes**: [[permanent/replanning-triggers]]

### Iterative Refinement Loops

**Definition**: Generate-evaluate-refine cycles where a generator produces output, an evaluator scores it against a rubric, and feedback is passed back for revision. Closes quality gaps in agent output, especially with ambiguous goals.
**Sources**: [[permanent/iterative-refinement-loops]]
**Related Notes**: [[permanent/evaluator-optimizer]], [[permanent/plan-mode]]

### Replanning Triggers

**Definition**: Events that necessitate a plan revision: failed tool calls, unexpected results, changed state, or constraint violations.
**Sources**: [[permanent/replanning-triggers]]
**Related Notes**: [[permanent/replanning]], [[permanent/dynamic-planning]]

### RAG (Retrieval-Augmented Generation)

**Definition**: A paradigm where, on query, the system retrieves relevant chunks from raw documents and feeds them into the LLM context to generate an answer. Knowledge is not accumulated across queries — each query starts fresh.
**Sources**: [[permanent/rag-vs-compiled-knowledge]]
**Related Notes**: [[permanent/context-capacity]], [[permanent/context-window]]

### Under-specified

**Definition**: A handoff message that lacks the necessary context for a sub-agent to perform its task correctly, leading to failure or incorrect results.
**Sources**: [[permanent/under-specified]]
**Related Notes**: [[permanent/handoff-protocol]], [[permanent/over-specified]], [[permanent/context-isolation]]

### Layered Enforcement Architecture

**Definition**: A defense-in-depth strategy with multiple layers (Prompt, Pre-Execution, Post-Execution, Runtime) where no single point of failure compromises the system.
**Sources**: [[permanent/layered-enforcement-architecture]]
**Related Notes**: [[permanent/silent-failure]], [[permanent/hooks]]

### Parallel Execution

**Definition**: An orchestration pattern where multiple independent tasks, tools, or agents run concurrently to reduce end-to-end latency. Implemented via fan-out/fan-in model with robust partial failure handling.
**Sources**: [[permanent/parallel-execution]]
**Related Notes**: [[permanent/task-decomposition]], [[permanent/fan-out]]

### Context Budget

**Definition**: A deliberate allocation plan deciding how many tokens each component gets (e.g. system prompt 500 tokens, conversation history 2,000, retrieved chunks 4,000, output headroom 1,000). Enforced in code, not by hope.
**Sources**: Section 15 L1
**Related Notes**: [[permanent/context-capacity]], [[permanent/token]]

### Persistent Fact Block

**Definition**: A structured region of the prompt that holds extracted facts and survives summarization intact. Prevents silent fact loss in long sessions. Updated via append-only or replace-on-conflict strategies.
**Sources**: Section 15 L3–L4
**Related Notes**: [[permanent/silent-failure]], [[permanent/context-isolation]]

### Extract-First Pattern

**Definition**: Run fact extraction before summarization so facts land in the persistent block before the compressor can discard them. Steps: (1) extract facts from raw turn, (2) write to persistent block, (3) summarize the turn.
**Sources**: Section 15 L3
**Related Notes**: [[permanent/persistent-fact-block]], [[permanent/silent-failure]]

### Tool-Output Trimming

**Definition**: Reducing what a tool returns to only the fields the calling agent actually needs, applied at the tool wrapper before output enters context. Improves cost, speed, and accuracy simultaneously.
**Sources**: Section 15 L5–L6
**Related Notes**: [[permanent/tool-use-lifecycle]], [[permanent/context-capacity]]

### Reference Indirection

**Definition**: Storing the full tool response externally (cache/retrieval store) and handing the agent only a compact identifier. The agent fetches on demand — many agents never need the full payload, turning a guaranteed cost into a conditional one.
**Sources**: Section 15 L5–L6
**Related Notes**: [[permanent/tool-output-trimming]], [[permanent/context-capacity]]

### Attention Dilution

**Definition**: When a high proportion of context tokens are irrelevant or redundant, they compete with relevant content for the model's finite attention capacity. A form of context degradation where noise crowds out signal.
**Sources**: Section 15 L2
**Related Notes**: [[permanent/context-capacity]], [[permanent/lost-in-the-middle-effect]]

### Prompt Caching / KV Cache

**Definition**: An API feature storing computed KV cache for a stable prefix so subsequent requests reuse it. Cached tokens billed at a reduced rate; time-to-first-token drops because prefix computation is skipped.
**Sources**: Section 15 L2, DigitalOcean — How KV Caching Slashes LLM Inference Costs
**Related Notes**: [[permanent/context-capacity]], [[permanent/tool-use-lifecycle]]

### Signal-to-Noise Ratio (SNR)

**Definition**: The proportion of genuinely relevant content to filler in the context window. A high SNR means focused, useful information. Bloat lowers the ratio, degrading model accuracy.
**Sources**: Section 15 L5, Wikipedia — Signal-to-noise ratio
**Related Notes**: [[permanent/attention-dilution]], [[permanent/context-capacity]]

### Lost-in-the-Middle Effect

**Definition**: An empirically documented phenomenon where models attend most strongly to content near the start and end of the context window, while content in the middle receives measurably weaker attention (Liu et al., 2023).
**Sources**: Section 15 L1, arXiv — Lost in the Middle (Liu et al., 2023)
**Related Notes**: [[permanent/attention-dilution]], [[permanent/context-capacity]]

### .claudeignore

**Definition**: A file that excludes files/directories from context for security (credentials) and performance (large/irrelevant files). Functions similarly to `.gitignore` using glob patterns and wildcards.
**Sources**: Section 8 L4
**Related Notes**: [[permanent/context-capacity]]

### Cloud Code (Claude Code)

**Definition**: An agentic AI coding system by Anthropic that understands and modifies entire codebases, executes commands, and uses developer tools to build features and fix bugs autonomously via an agentic loop.
**Sources**: Librarian Research, Section 8 L1

### /init command

**Definition**: A built-in slash command in Claude Code that analyzes a repository to auto-generate a starter CLAUDE.md file. Provides a starting point, not a final configuration — requires team verification.
**Sources**: Section 8 L3

### session continuity

**Definition**: An AI agent's capability to maintain state, context, and progress across interruptions or multiple sessions, using CLAUDE.md, explicit summaries, and persistent memory patterns.
**Sources**: Section 8 L1, Librarian Research

### planning loop

**Definition**: The core cycle of an autonomous agent: reasoning, tool selection, execution, result observation, and re-planning until task completion.
**Sources**: Section 8 L1
**Related Notes**: [[permanent/agentic-loop]], [[permanent/replanning-triggers]]

### Escalation / Escalation Trigger

**Definition**: An explicit condition that causes an agent to hand off control rather than keep executing. Four main categories: confidence below threshold, scope outside policy, repeated retry failure (>2–3 attempts), ambiguous user intent.
**Sources**: Section 16 L1, [[permanent/escalation]]
**Related Notes**: [[permanent/silent-failure]], [[permanent/handoff-protocol]]

### Hard Trigger vs Soft Trigger

**Definition**: A hard trigger is a policy condition with no model judgment involved (e.g., deny list match — escalate immediately). A soft trigger invites model judgment (e.g., confidence below 0.7 — agent decides). Soft triggers are tunable over time; hard triggers are deterministic.
**Sources**: Section 16 L1

### Confidence Gate

**Definition**: A trigger pattern where the agent attaches a confidence estimate per step. If the estimate drops below a defined threshold (e.g., 0.7), it escalates instead of proceeding. Works best when paired with logged reasoning.
**Sources**: Section 16 L1

### Handoff Payload

**Definition**: A structured bundle of context, history, and suggested next step that an agent passes forward when it escalates. Four required fields: conversation summary, triggering question (verbatim), attempted actions log, suggested next step.
**Sources**: Section 16 L2
**Related Notes**: [[permanent/handoff-protocol]]

### Loop Closure

**Definition**: Re-injecting the response from a reviewer or supervisor back into the original agent's context once an escalation resolves. Without loop closure, escalation is a one-way street.
**Sources**: Section 16 L2
**Related Notes**: [[permanent/handoff-protocol]]

### Escalation Routing

**Definition**: The process of directing a handoff payload to the right destination — human reviewer (judgment/authority), specialist agent (domain match), or supervisor model (capability limit). Routing logic lives in the orchestrator.
**Sources**: Section 16 L2

### Propagation Boundary

**Definition**: The interface where an architect decides which failures cross up to the coordinator and which stay contained within the sub-agent. A deliberate design choice, not a runtime behavior — must be documented, tested, and enforced.
**Sources**: Section 16 L3
**Related Notes**: [[permanent/error-propagation-in-multi-agent-systems]]

### Local Recovery vs Coordinator Propagation

**Definition**: Local recovery means the sub-agent catches the failure, handles it internally (transient retries, validation sanitization, idempotent refetches), and returns normally. Coordinator propagation means the sub-agent surfaces the failure upward with structured information so the higher-level plan can adapt. Decision test: "Would the coordinator's plan change if it knew this happened?"
**Sources**: Section 16 L3

### Silent Corruption

**Definition**: A failure mode where a sub-agent catches every exception and returns vague success. The coordinator proceeds assuming correct completion, but data is malformed. Defect hides until downstream failures surface it.
**Sources**: Section 16 L3
**Related Notes**: [[permanent/silent-failure]]

### Coordinator Thrash

**Definition**: A failure mode where every internal hiccup (rate-limit pauses, validation issues, transient timeouts) propagates upward. The coordinator retries/reroutes/escalates constantly — expensive, noisy, and masks real failures.
**Sources**: Section 16 L3

### Retriable vs Non-retriable Errors

**Definition**: Retriable errors (transient network timeout, momentary service unavailability) — same call might succeed if retried; candidates for local retry. Non-retriable errors (auth denial, schema mismatch, resource doesn't exist) — same call produces the same failure every time; retrying locally is waste.
**Sources**: Section 16 L3
**Related Notes**: [[permanent/tool-idempotency-and-retry]], [[permanent/error-classification-framework]]

### Error Envelope

**Definition**: A structured typed object (not free text) that carries failure information from a sub-agent to its coordinator. Fields: error type (machine-readable category), severity (warning/recoverable/fatal), context (sub-agent ID, task reference, input state), recovery hint (optional structured suggestion).
**Sources**: Section 16 L4
**Related Notes**: [[permanent/error-propagation-in-multi-agent-systems]]

### Severity Level (warning/recoverable/fatal)

**Definition**: The classification embedded in an error envelope that maps to specific coordinator policies. Warning → log and continue. Recoverable → retry with exponential backoff or fallback agent. Fatal → halt dependent task chain, escalate to human. Severity without a corresponding policy is meaningless.
**Sources**: Section 16 L4

### Exponential Backoff

**Definition**: A retry algorithm that progressively increases waiting time between successive retry attempts, typically with a base delay multiplied by 2 each attempt, capped at a maximum, and randomized with jitter to prevent thundering herd problems.
**Sources**: AWS Architecture Blog — Exponential Backoff And Jitter

### Fallback Agent

**Definition**: A recovery pattern where the coordinator routes a task to an alternate implementation (different model, rule-based system) when the primary agent consistently fails.
**Sources**: Section 16 L4, [[permanent/fallback-chains]]

### Partial Completion Acceptance

**Definition**: A recovery option where the coordinator takes whatever work the sub-agent completed and adjusts the remaining plan to skip the failed step. Requires a completed-steps field in the error envelope.
**Sources**: Section 16 L4

### Retry Budget

**Definition**: A resilience mechanism that caps the maximum number of local retry attempts (typically 2–3) before a sub-agent stops trying and propagates the failure upward. Part of the propagation policy, not a runtime guess.
**Sources**: Section 16 L3, CodeNotes — Retry Budgets in Microservices
**Related Notes**: [[permanent/tool-idempotency-and-retry]]

### Idempotency Key

**Definition**: A unique token per operation that allows a sub-agent to check whether an operation was already completed before executing it. On retry, the coordinator passes the same token and the sub-agent returns the previous result without re-executing. Prevents double writes, duplicate charges, conflicting state.
**Sources**: Section 16 L4, [[permanent/tool-idempotency-and-retry]]

### Silent False Negative

**Definition**: The bug where a tool fails internally but returns an empty list without an error flag, and the agent concludes that nothing exists when data genuinely exists. An access failure is mistaken for a valid empty result.
**Sources**: Section 16 L5

### Access Failure vs Empty Result

**Definition**: Access failure — the tool didn't successfully reach the underlying data source (connection refused, auth failure, timeout). Empty result — the tool reached the source and confirmed nothing matches the query (a legitimate answer). Without explicit signaling, both look identical to the agent.
**Sources**: Section 16 L5
**Related Notes**: [[permanent/silent-failure]]

### Ambiguous Nothing

**Definition**: Any empty tool response that could mean either an access failure or a confirmed empty result. The wrapper layer must ensure ambiguous nothing never reaches the agent.
**Sources**: Section 16 L5–L6

### Tool Wrapper

**Definition**: A thin layer between the raw tool and the agent that normalizes responses, attaches a status field, and enforces disambiguation before the agent sees the result. Sits between the tool and the agent's context.
**Sources**: Section 16 L5–L6
**Related Notes**: [[permanent/tool-use-lifecycle]]

### Status Enum

**Definition**: A mandatory typed enum on every tool response labeling the outcome class: `success`, `empty result`, `partial result`, `access failure`. The agent branches on this field rather than inferring meaning from an empty data payload. Must be mandatory and typed (not free text).
**Sources**: Section 16 L6

### Span-based Tracing / Distributed Tracing

**Definition**: An observability technique where each agent adds a span identifier to the error envelope before propagating. The coordinator links span identifiers, enabling operators to follow a single request through every agent hop and see exactly where it broke.
**Sources**: Section 16 L4, OpenTelemetry Tracing API
**Related Notes**: [[permanent/error-propagation-in-multi-agent-systems]]

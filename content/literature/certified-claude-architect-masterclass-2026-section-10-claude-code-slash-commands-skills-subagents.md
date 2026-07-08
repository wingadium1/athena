---
title: "Section 10: Claude Code - Slash Commands, Skills & Subagents"
type: literature
source: "certified-claude-architect-masterclass-2026"
author: "Jacob Bushong"
date-read: 2026-07-06
tags: [claude, udemy, course, masterclass]
---

## Summary

## Key Ideas

L1: Slash commands are user-defined shortcuts that trigger instruction templates.
L1: Custom slash commands are user or team-created, unlike built-in `/init` and `/help` which ship with Cloud Code and cannot be overridden.
L1: The `args` placeholder allows variable input in a command template; text typed after the keyword replaces `args`.
L1: Keywords map to markdown filenames — `deployreview.md` becomes `/deploy-review`. No registration needed.
L1: Commands have two scopes: project-level (`.claude/slash_commands` in project, version-controlled, shared) and user-level (`.claude/slash_commands` in home dir, personal, follows you).
L1: Good command files include a role statement, clear task description, output format specification, and scope/tone constraints.

L2: Core design principle: **narrow scope** — each command does exactly one thing (single responsibility principle).
L2: **Verb-Noun naming** (e.g., `/review-pr`, `/run-tests`, `/explain-file`) makes commands self-documenting.
L2: Effective command prompts require four elements: **Role**, **Task**, **Output Format**, and **Constraints**. Missing any → unpredictable output.
L2: **Freeform args** work for simple single-text inputs; **structured templates** are better for multiple fields or when input shape matters.
L2: **Project-scoped commands** distribute via version control (`.claude/slash_commands` committed to repo) — every developer gets them automatically.
L2: **Managed settings** provide org-wide distribution; admins push commands to all users, who cannot delete or override them.
L2: **Anti-pattern 1**: Overly broad commands attempting too many tasks. Fix: narrow the scope.
L2: **Anti-pattern 2**: Duplicating `Claude.md` instructions inside command files. Commands should extend, not repeat, session-level context.

L3: **Skills**: Reusable, trigger-activated workflows defined in `skill.md` files.
L3: Skills activate automatically when specific trigger conditions are met (condition-initiated).
L3: Skills sit in the middle of the instruction stack, more selective than `cloud.md` and more passive than [[subagent-definition|sub-agents]].
L3: Use cases for skills include recurring multi-step workflows, team-wide conventions, and reusable instruction templates.
L3: **Slash Commands**: User-invoked shortcuts created in the `.cloud/commands` directory.
L3: Slash commands require explicit user typing for activation (user-initiated).
L3: **[[subagent-definition|Sub-agents]]**: Delegated, autonomous workers spun up to complete tasks independently.
L3: [[subagent-definition|Sub-agents]] have their own context and tools, acting as active autonomous workers.
L3: **cloud.md**: An always-on global configuration that applies to every interaction without any trigger or invocation.
L3: Instructions that apply everywhere unconditionally belong in `cloud.md`.
L3: **Decision Framework for Skills**: Consider Frequency (how often), Complexity (multi-step value), and Reusability (who benefits).
L3: The CCAF exam tests skills, slash commands, and [[subagent-definition|sub-agents]] together.

L4: A `skill.md` file's functionality depends on front matter, trigger type, and file location.
L4: Every `skill.md` has a two-part structure: YAML front matter block and markdown instruction body.
L4: The YAML front matter block, enclosed by triple dashes, contains metadata.
L4: The markdown instruction body holds the actual instructions for Claude Code.
L4: Missing or malformed front matter leads to [[silent-failure|silent failure]]; the file is not recognized as a skill.
L4: Three front matter fields are required: `Name`, `Description`, and `Trigger`.
L4: `Name` is a unique internal identifier.
L4: `Description` is a human-readable summary.
L4: `Trigger` defines the condition for skill invocation.
L4: If any required field is missing, the skill is silently ignored.
L4: Optional front matter fields include `Version`, `Tool restrictions`, `Context requirements`, `Author`, and `Tag`.
L4: Skill triggers are declarative conditions defined in the front matter.
L4: There are two types of triggers: phrase-based and event-based.
L4: Phrase-based triggers activate when user input matches a pattern.
L4: Mismatched phrase triggers are [[silent-failure|silent]] failures.
L4: Event-based triggers activate on system events (e.g., file creation, build completion).
L4: Phrase triggers are best for user interaction; event triggers for automation.
L4: Skill distribution locations are Personal, Project-level, and Enterprise-managed.
L4: Personal skills (`.claude/skills/`) are for individual user sessions.
L4: Project skills (`.claude/skills/` at project root) are for all project contributors.
L4: Enterprise skills are centrally managed and deployed to all users in an organization.

L5: **Core Concepts of Cloud Code Subagents**
L5: A [[subagent-definition|subagent]] is a separate Cloud Code session spawned by a parent agent to handle a delegated subtask.
L5: The [[orchestrator|orchestrator]] is the parent agent that decomposes goals, delegates work, and synthesizes results.
L5: [[context-isolation|Context isolation]] means each subagent starts with an empty context window; nothing passes automatically from parent to subagent.

L5: **Distinction from API-based Multi-Agent Systems**
L5: Cloud Code subagents are full, separate Cloud Code session instances, not API calls orchestrated in application code.
L5: They run with their own process, [[context-capacity|context]], and configured permissions, unlike API-managed multi-agent patterns.

L5: **Subagent Creation Paths**
L5: Two paths exist: the interactive `/agents` command and the programmatic `agent` tool.
L5: The `/agents` command defines identity, tools, and scope for named subagents.
L5: The `agent` tool dynamically spawns subagents within an agentic workflow.
L5: Both paths produce the same result: a separate Cloud Code session with configured context and permissions.

L5: **Subagent Configuration (Per-Subagent Basis)**
L5: **Tool access**: Controls which tools the subagent can invoke (e.g., bash, file I/O, web access).
L5: **Directory scope**: Restricts the subagent to specific directories for reading and modification.
L5: **Permissions**: Defines what the subagent is allowed to do, often narrower than the parent (principle of least privilege).
L5: **Task description**: Explicit instructions defining the subagent's objective.

L5: **Implications of Isolation**
L5: Subagents start with an empty context; no parent conversation, loaded files, or prior work is inherited.
L5: Permissions are not inherited; subagents can be configured with narrower permissions (e.g., read-only).
L5: [[context-isolation|Isolation]] prevents context contamination and reduces the "blast radius" if a subagent behaves unexpectedly.

L5: **When to Use Subagents vs. Staying In-Line**
L5: **Use subagents when**: Tasks are independent, can run in [[parallel-execution|parallel]], require a different toolset, or benefit from [[context-isolation|scope isolation]].
L5: **Stay in-line when**: The task needs full parent context, is sequential and depends on prior steps, or delegation overhead outweighs the value.
L5: Spawning a subagent incurs real overhead.

L5: **The Handoff Protocol**
L5: The [[handoff-protocol|handoff]] is a critical design artifact.
L5: It must include a precise task description, only necessary context, and the expected output format.
L5: [[under-specified|Underspecified]] handoff components lead to ambiguous or unusable results.

L6: **Subagent Design Fundamentals:**
L6: The problem with [[subagent-definition|subagents]] is almost always in design, not execution.
L6: Every subagent design decision is framed by a small vocabulary.
L6: **Three Core Design Questions (Minimum Viable Checklist):**
L6: 1. **What task:** Precisely what to do, what "done" looks like.
L6: 2. **What context:** Exactly what information is needed (focused slice, not everything the [[orchestrator|parent]] knows).
L6: 3. **What output format:** How results should be structured for parent use (machine-parsable, schema-defined).
L6: Skipping any question leads to [[under-specified|underspecified]], inconsistent, or unusable results.

L6: **Delegation Overhead & Justification:**
L6: Delegation overhead (context loading, coordination, latency, cost) is the real cost of [[subagent-definition|subagents]].
L6: For simple tasks, overhead can exceed delegation benefit; understand when it adds value vs. friction.
L6: Delegation is beneficial for truly independent work, tasks needing different tools, or [[context-isolation|scope isolation]].
L6: Delegation is counterproductive for simple tasks, tasks needing full [[orchestrator|parent]] context, or when overhead is too high.
L6: [[subagent-definition|Subagents]] are not free; every delegation must be justified by clear gains.

L6: **Context Management:**
L6: [[subagent-definition|Subagent]] [[context-capacity|context]] is finite and entirely parent-provided; no shared session knowledge.
L6: More context is not better if irrelevant; overstuffing crowds out task-relevant content.
L6: Disciplined context allocation keeps [[subagent-definition|subagents]] efficient, lowers costs, and improves results.

L6: **Structured Output & Error Reporting:**
L6: Structured output is a design requirement: parsable, well-formed results (schema, JSON, YAML).
L6: [[orchestrator|Parent agent]] needs to process results, not interpret natural language summaries.
L6: Explicit format specification transforms [[subagent-definition|subagents]] into predictable, composable components.
L6: Error reporting is as important as success path design.
L6: [[subagent-definition|Subagents]] must return explicit error indicators with context; [[silent-failure|silent failures]] are the worst outcome.
L6: Error reporting should be built into the output schema to enable parent corrective action.

L6: **Anti-Patterns to Avoid:**
L6: 1. **Spawning a [[subagent-definition|subagent]] for every subtask:** Leads to excessive latency and coordination complexity.
L6: 2. **Spawning without defining output format:** Results in unreliable natural language summaries.
L6: Reserve [[subagent-definition|subagents]] for work genuinely benefiting from [[context-isolation|isolation]], [[parallel-execution|parallelism]], or different permissions.
L6: The [[handoff-protocol|handoff document]] (answering the three design questions) is the most important design artifact.

L7: Plan mode is a read-only thinking phase in CLAUDE Code.
L7: It involves CLAUDE inspecting the codebase, reasoning about the task, and producing a structured proposal without touching any files.
L7: The approval gate is a checkpoint where users accept, edit, or reject a plan; no file writes occur before passing this gate.
L7: [[replanning-triggers|Replanning]] occurs when partial execution reveals an error, prompting CLAUDE to regenerate or amend the plan.
L7: Plan mode, the approval gate, and replanning together form a controlled, reversible workflow.
L7: A plan proposal includes ordered steps, files CLAUDE expects to touch, and explicit risks or assumptions.
L7: The transition from thinking to doing happens when a user approves the plan.
L7: Users can enter plan mode explicitly using the /plan command or implicitly when CLAUDE detects a risky or ambiguous task.
L7: Well-formed plans consist of a numbered sequence of discrete actions, named files to be modified, and clearly flagged assumptions.
L7: Plan mode is beneficial for cross-file refactors, schema changes, and ambiguous requests, as it catches wrong assumptions cheaply.
L7: It is not ideal for single-file mechanical fixes or easily reversible exploratory work in a REPL.
L7: A good heuristic for using plan mode is when work is hard to reverse or spans multiple modules.
L7: Plan mode is worthwhile if undoing a mistake would take more than 30 minutes.
L7: It is an instance of [[iterative-refinement-loops|iterative refinement]], involving drafting, reviewing, and revising.
L7: Plan mode can be paired with [[subagent-definition|sub-agents]] or hooks for investigation, planning, or validation.
L7: Planning uses a smaller context window budget compared to full execution, preserving space for subsequent edits.
L7: Critical checks before approving a plan include ensuring the scope matches user intent, verifying file paths, and resolving flagged risks.
L7: The approval gate serves as the primary quality checkpoint for developers.

L8: Approval Gate: A user checkpoint to accept, edit, or reject a proposed plan.
L8: [[iterative-refinement-loops|Iterative Refinement]]: A pattern of drafting, reviewing, and revising until a plan meets intent.
L8: Plan Mode: Applies [[iterative-refinement-loops|iterative refinement]] to code changes.
L8: Plan Mode Pairing: Combines plan mode with [[subagent-definition|sub-agents]] (investigation) or hooks (validation).
L8: Approval Gate Options: Accept, edit inline, or reject the plan.
L8: Approval Gate Failure: Skimming approval transfers control back to Claude without proper review.
L8: Editing a Plan: More efficient than a full replan.
L8: Common Plan Edits: Scope trimming, step reordering, adding omitted steps.
L8: [[replanning-triggers|Replanning Trigger]]: Missing files or incorrect codebase assumptions.
L8: Replanning Decision: Based on the scope of impact from a broken assumption; patch for single step, replan for multiple.
L8: Sub-agent Investigation: Gathers codebase context for the main agent's plan.
L8: Hook Validation: Pre-executes checks (e.g., file existence, linting) before plan steps run.
L8: Staged Human Review: Approves after major phases to prevent compounding drift.
L8: Combined Pairing: [[subagent-definition|Sub-agent]] finds, hook validates, human approves for maximum reliability.
L8: Common Plan Mode Failure: Claude improvises mid-execution due to unexpected file states.
L8: Fix for Improvisation: Instruct Claude to stop and report divergences, not work around them.
L8: Recommended Plan Length: 5-8 discrete steps.
L8: Plan Detail: Use explicit file names, avoid vague references.
L8: Risk Assessment: Always ask for risks to uncover hidden assumptions.
L8: Reliable Plan Habits: Short plans, explicit file names, explicit risk statements.
L8: Plan Mode Overhead: Higher upfront cost due to proposal, review, and approval.
L8: Plan Mode Justification: Reduces total time for complex, ambiguous, or irreversible multi-file changes by catching early assumptions.
L8: Direct Execution Overhead: Lower, immediate code writing.
L8: Direct Execution Use Case: Small, reversible, single-file changes with low error cost.
L8: Iteration Loop: Propose, review, edit/approve, then execute.

## Quotes

## My Take

## Links

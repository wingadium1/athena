---
title: "Section 11: Claude Code - Hooks, SDK & CI/CD"
type: literature
source: "certified-claude-architect-masterclass-2026"
author: "Jacob Bushong"
date-read: 2026-07-07
tags: [claude, udemy, course, masterclass]
---

## Summary

## Key Ideas

L1: Hooks are user-defined scripts registered in CLAUDE Code settings.
L1: They fire automatically at specific [[tool-use-lifecycle|lifecycle events]] during a session.
L1: Hooks run at the shell level, outside CLAUDE's context window.
L1: CLAUDE cannot see, override, or disable hooks via prompts or CLAUDE.md instructions.
L1: This creates a hard architectural boundary for safety and policy enforcement.
L1: Hooks run deterministically at lifecycle events, unlike prompt instructions which can be influenced or forgotten.
L1: **Pre-tool use hooks**: Fire before any [[tool-use]] executes.
L1: They can inspect the pending call and block it with a non-zero exit code.
L1: This is the only option for enforcement (preventing file writes, blocking commands, validating parameters).
L1: **Post-tool use hooks**: Fire after a [[tool-use]] call has completed.
L1: They can log results, trigger notifications, or kick off downstream automation.
L1: They cannot block or undo actions retroactively.
L1: **[[stop-signal|Stop hooks]]**: Fire when Claude reaches the end of its execution loop for a given turn.
L1: Useful for per-turn cleanup, final validation, summary logging, or notifications after multi-step runs.
L1: **Notification hooks**: Fire when Claude emits a notification event.
L1: Their purpose is routing alerts to external systems (e.g., Slack, PagerDuty), not blocking.
L1: Notification hooks cannot modify or block Claude's execution flow.
L1: Hooks communicate intent via exit codes: zero means continue, non-zero means block/fail.
L1: There are no exit codes for warnings or soft blocks; a non-zero code always stops execution.
L1: Hooks are the deterministic control layer in Claude Code, firing without exception.

L2: **Hook Implementation Basics:**
L2: Hooks are registered in Claude Code's settings file, not in `Claude.md` or tool descriptions.
L2: Registration involves adding an entry to settings, specifying the lifecycle event and the command/script to run.
L2: Multiple hooks can be registered for the same event and will run in sequence.
L2: **Exit Code Conventions:**
L2: Claude Code interprets hook decisions via exit codes.
L2: Exit code `0` means continue the action.
L2: Any non-zero exit code means block the action.
L2: Exit code `1` conventionally signals a policy violation.
L2: Exit code `2` conventionally signals the hook script itself failed.
L2: Document exit code meanings directly in the script as comments.
L2: **Script vs. Inline Commands:**
L2: For anything beyond a one-liner, external script files are strongly preferred over inline commands.
L2: Script files offer benefits like independent testing, version control, and easier review.
L2: Inline commands are convenient for quick logging but become maintenance problems if logic grows.
L2: **Hook Types:**
L2: **Pre-tool use hooks** inspect a pending tool call and decide whether to allow it.
L2: **Post-tool use hooks** fire after a tool has already run and cannot prevent the action.
L2: **Common Pitfalls & Silent Failures:**
L2: Misconfigured hooks, especially pre-tool use hooks, can silently stop Claude from acting.
L2: An accidental non-zero exit code will silently block tool calls, even if intended as a warning.
L2: Slow hooks (e.g., calling external APIs) make every tool call wait, impacting performance.
L2: No error or obvious diagnostic appears in Claude's interface for silently blocked tool calls.
L2: **Best Practices & Testing:**
L2: **Testing is non-negotiable** for pre-tool use hooks.
L2: **Testing steps:**
L2: 1. Run the script manually with sample inputs, verifying exit codes for all paths.
L2: 2. Use a dry run mode (if supported) to log what the hook would block without enforcing it.
L2: 3. Add explicit diagnostic messages to standard error for every blocking exit.
L2: Hook implementation requires: correct registration, control via exit codes, and thorough testing.
L2: **Policy Patterns:**
L2: **Allow list pattern:** Defines approved items; anything not on the list is blocked (more conservative).
L2: **Block list pattern:** Defines dangerous items to reject; anything not explicitly blocked proceeds (easier to write, but weaker by default).
L2: **Dry run pattern:** Adds a mode flag to hooks to log blocking actions without actually exiting non-zero, useful for testing.

L3: This lesson focuses on applying hooks in real engineering scenarios.
L3: Six patterns are covered: auto formatting, blocking destructive bash commands, logging tool calls, sending task completion notifications, running tests automatically, and tracking API cost.
L3: Each pattern corresponds to a specific hook type.
L3: **Auto Format Hook:**
L3:   - Purpose: Automatically formats code after file writes.
L3:   - Hook Type: Post tool use (fires after file is written).
L3:   - Benefit: Ensures consistent style, reduces manual effort, and cleans up code reviews.
L3: **Safety Net Hook:**
L3:   - Purpose: Blocks dangerous bash commands before execution.
L3:   - Hook Type: Pre tool use (fires before command executes).
L3:   - Benefit: Prevents destructive actions (e.g., `rm -rf`, `drop table`).
L3: **Audit Log Hook:**
L3:   - Purpose: Records every tool call for a traceable history.
L3:   - Hook Type: Can be pre tool use (records intent, blocked actions) or post tool use (records results, successful actions), or both for a complete picture.
L3:   - Benefit: Provides visibility into agent behavior, detects policy violations, and tracks completed work.
L3:   - Formats: Simple append-only text, structured JSON, or remote logging.
L3: **Notification Hook:**
L3:   - Purpose: Sends alerts upon task completion.
L3:   - Hook Type: Stop lifecycle event (fires once at the end of execution).
L3:   - Benefit: Notifies developers of long-running task results without constant monitoring, decoupling attention.
L3: **Test Run Hook:**
L3:   - Purpose: Automatically runs tests after code edits.
L3:   - Hook Type: Post tool use (fires after write/edit calls).
L3:   - Benefit: Provides immediate feedback on code changes, preventing broken tests from slipping through.
L3:   - Implementation: Scoped to relevant file paths (e.g., `SRC`, `Libby`) to avoid unnecessary test runs.
L3: **Cost Tracking Hook:**
L3:   - Purpose: Monitors API usage at the session level.
L3:   - Hook Type: Post tool use (captures token counts, model, timestamp after each tool response).
L3:   - Benefit: Identifies expensive prompts, tracks budget limits, and prevents runaway sessions.
L3: **General Hook Principles:**
L3:   - Matching the hook type to the lifecycle event (pre-tool-use, post-tool-use, stop) is crucial.
L3:   - Pre tool use: for safety and blocking actions.
L3:   - Post tool use: for reactions, logging, and follow-up actions.
L3:   - Stop: for completion signals and final cleanup.
L3: The CCAF exam directly tests the distinction between pre tool use and post tool use.

L4: The CLAUDE Code SDK is a programmatic interface for controlling agentic sessions from code.
L4: The SDK integrates CLAUDE Code into custom applications, scripts, and pipelines.
L4: SDK methods manage sessions, inject prompts, and process output programmatically, unlike the interactive CLI.
L4: Programmatic sessions are application-controlled, defining their lifecycle, inputs, and output handling.
L4: Session streaming provides incremental tool call events and output, enabling real-time visibility and responsiveness.
L4: The SDK and CLI expose the same core CLAUDE Code capabilities; the difference lies in who or what drives the session.
L4: The SDK is designed for code-driven, multi-step agentic workflows, including [[tool-use]], file edits, and shell commands.
L4: The direct API is for single model calls (prompt to completion) without session state or built-in tool orchestration.
L4: SDK session creation involves instantiating a session object and configuring parameters like model, tools, and working directory.
L4: Session behavior can be shaped by injecting context (system prompts, file contents) and setting tool permissions.
L4: Follow-up prompts can be appended mid-session to redirect the agent while preserving accumulated context.
L4: Key use cases for the SDK include code review bots, documentation generators, and [[cicd|CI/CD]] pipeline steps.
L4: Use the SDK for agentic complexity and real-time observability via streaming; use the direct API for straightforward model calls.

## Quotes

## My Take

## Links

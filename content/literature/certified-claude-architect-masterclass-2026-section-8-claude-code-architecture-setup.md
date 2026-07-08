---
title: "Section 8: Claude Code - Architecture & Setup"
type: literature
source: "certified-claude-architect-masterclass-2026"
author: "Certified Claude Architect Masterclass 2026"
date-read: 2026-07-06
tags:
- certified-claude-architect-masterclass-2026
---

## Summary

## Key Ideas

L1: Cloud Code is Anthropic's autonomous [[agentic-loop|agentic CLI]], executing multi-step tasks via a continuous loop of planning, tool selection, execution, and observation.
L1: The core planning loop involves reasoning, tool selection, execution, result observation, and re-planning until [[task-completion|task completion]].
L1: Cloud Code is an autonomous agent, not a chat interface or autocomplete tool; it executes tasks directly from a goal.
L1: A single instruction can trigger numerous tool calls across the codebase.
L1: Unlike IDE plugins, Cloud Code operates autonomously across the codebase and has full shell access (bash, file I/O, multi-file edits).
L1: The [[agentic-loop|agentic loop]] allows for self-correction based on unexpected tool results, unlike single-shot LLM calls.
L1: The [[context-capacity|context window]] is finite, shared by conversation history, tool results, and Cloud.md files.
L1: Verbose Cloud.md files reduce available context for code and tool output; when full, older content is compacted or dropped.
L1: Cloud Code lacks persistent memory across sessions; continuity relies on Cloud.md files and explicit summaries committed to the repo.
L1: A [[layered-enforcement-architecture|layered permission model]] controls allowed tools and actions, enforced before any tool execution.
L1: Permissions are configurable at user, project, and team levels, with some actions requiring explicit approval.
L1: The permission model is distinct from hooks; if permissions block a tool call, no hooks are triggered.

L2: Cloud Code has six built-in tools: Read, Write, Edit, Bash, Web Search, and Agent.
L2: Each [[four-phase-lifecycle-of-a-tool-call|tool call]] is a discrete action selected based on current task needs.
L2: Read, Write, and Edit handle file operations.
L2: **Read** views files and directories without modification (safest tool). The "read-before-edit" pattern prevents stale assumption errors.
L2: **Edit** performs targeted find/replace on existing files (scoped and safe). Use Edit for most code changes.
L2: **Write** overwrites entire files (for new files or full rewrites); no partial updates.
L2: **Bash** executes any shell command (most capable and most dangerous). No built-in constraints; permission model and hooks are primary controls.
L2: **Web Search** queries the web for information beyond training data (e.g., docs, API changes, errors).
L2: **Agent** delegates subtasks to [[subagent-definition|sub-agents]] with separate [[context-capacity|context windows]], used for large or parallelizable work.
L2: Tool selection is task-driven at each step of the planning loop.

L3: Cloud Code is Anthropx's [[agentic-loop|agentic CLI]], operating in a loop of planning, tool use, and response until [[task-completion|task completion]].
L3: The `/init` command is a built-in slash command that auto-generates a starter `claude.md` by analyzing the repository.
L3: `/init` provides a starting point, not a final configuration.
L3: `claude.md` provides persistent project instructions, loaded at session start, consuming [[context-capacity|context window]] space.
L3: Brevity and precision in `claude.md` are crucial to conserve context for code and tool output.
L3: Cloud Code's default scope is determined by the project root where `clod` is run.
L3: Cloud Code automatically discovers project structure by reading file trees, READMEs, and config files (e.g., `package.json`, `pyproject.toml`).
L3: Discovery quality depends on project documentation; outdated docs give a less accurate picture.
L3: `/init` generates: project name, key files/directories, detected languages/frameworks, and suggested behavioral conventions.
L3: `/init` output is a starting point from automated analysis and requires team verification.
L3: `/init` cannot infer team conventions, security constraints, or rules not explicitly in code.
L3: Every line in `claude.md` consumes context; the goal is accurate, concise, and useful content.
L3: Refining `/init` output is mandatory: verify frameworks, add missing rules, remove noise.
L3: Unrefined `claude.md` from raw `/init` output is a liability due to inaccuracies and missing context.
L3: Three pre-flight checks: accurate `claude.md`, configured `.claudeignore`, and documented entry points/test commands/build steps in `claude.md`.

L4: Context quality directly determines Claude Code's output usefulness.
L4: `.claudeignore` excludes files/directories from [[context-capacity|context]] for security (credentials) and performance (large/irrelevant files).
L4: `.claudeignore` functions similarly to `.gitignore` using glob patterns and wildcards.
L4: The [[context-capacity|context window]] is a finite text space; Claude.md content, tool results, and code all consume tokens.
L4: When the context window is full, earlier content is compacted or dropped.
L4: Claude Code lacks persistent memory across sessions by default; context must be explicitly reprovided.
L4: Four context entry mechanisms: file mentions by path, at-sign references to symbols, pasting URLs, and inline instructions.
L4: All context entry methods consume tokens from the budget.
L4: Useful context (architecture decisions, coding conventions, test requirements, known constraints) guides behavior.
L4: `.claudeignore` categories: auto-generated files, large binaries, secrets/credentials, vendored libraries.
L4: Over-inclusion wastes tokens and causes important content to be dropped; targeted inclusion outperforms noisy context.
L4: Claude.md is loaded at session start; verbose Claude.md reduces working space for actual tasks.
L4: Claude.md is the primary solution for session continuity, providing standing rules automatically every session.
L4: Claude.md should contain standing, always-relevant rules; one-time instructions belong in the prompt.

## Quotes

## My Take

## Links

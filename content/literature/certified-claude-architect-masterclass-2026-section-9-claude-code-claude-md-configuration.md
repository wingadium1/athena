---
title: "Section 9: Claude Code - CLAUDE.md Configuration"
type: literature
source: "certified-claude-architect-masterclass-2026"
author: "Certified Claude Architect Masterclass 2026"
date-read: 2026-07-06
tags:
- certified-claude-architect-masterclass-2026
---

## Summary

## Key Ideas

L1: CLAUDE.md config has a three-level hierarchy: User-level, Project-level, and Team-level.
L1: **User-level** (`.claude/CLAUDE.md`) is personal scope, applies across all sessions, lowest precedence. For personal defaults (verbosity, output format, tool restrictions), not team standards.
L1: **Project-level** (`CLAUDE.md` at repo root) is team scope, applies to all contributors, must be version-controlled. Defines coding conventions, test requirements, architecture guidelines, anti-patterns.
L1: **Team-level** is shared via templates/symlinks for org-wide standards: company policies, security rules, shared style guides across projects.
L1: All three levels are additive — they apply simultaneously. Precedence only resolves direct conflicts.
L1: Placing team standards in user-level config is an anti-pattern; colleagues won't see them.
L1: Every CLAUDE.md file consumes [[context-capacity|context window]] space; verbose configs reduce working space for code.
L1: CLAUDE.md is a static instruction file, loaded fresh at session start, not a persistent memory store.

L2: **Precedence Rule**: Project-level CLAUDE.md instructions override user-level when they conflict. Higher specificity overrides broader personal scope — a common inversion mistake.
L2: **Complementary Instructions**: Instructions at different levels that don't conflict both apply simultaneously without resolution needed.
L2: **Config Anti-pattern**: Placing team standards in user-level config is invisible to other contributors, breaking consistency silently with no warning.
L2: Precedence applies to all instruction types: style, verbosity, architecture, test requirements.
L2: Project-level config represents team standards — version-controlled, committed, visible to all contributors.
L2: User-level config is invisible to teammates, applies only to one machine/session, and has no authority over project standards.
L2: Precedence only activates on conflict (same behavior, opposing directions). Most configs are additive by default; precedence resolution is the exception.
L2: Best practice: user-level config should contain only non-project-specific instructions. Project-level config should explicitly state all coding standards.
L2: Most dangerous mistake: team standards in user-level config. Fails silently with no error. Fix: put team-wide instructions in project-level CLAUDE.md committed to source control.
L2: Verification: ask Cloud Code about active instructions, review session initialization output, or test conflicting instructions explicitly.

L3: Subdirectory CLAUDE.md files scope rules to specific layers in a codebase.
L3: A subdirectory CLAUDE.md is placed in a subfolder, not the project root. Cloud Code loads it automatically when working in that path — no registration needed.
L3: Rules are path-specific; file location defines the scope boundary.
L3: Subdirectory files augment parent instructions; they do not replace them.
L3: The mechanism is placement: rules activate based on the current working directory.
L3: The relationship is additive: parent instructions remain active in subdirectories, and subdirectory rules add folder-specific rules on top.
L3: If a subdirectory rule explicitly contradicts a parent rule, the subdirectory takes precedence on that point.
L3: Think of it as layering: each level adds rules to the stack.
L3: Common directory patterns: `/frontend` (React hooks, component naming), `/tests` (pytest fixtures, naming, coverage), `/infrastructure` (Terraform structure, never delete constraints), `/generated` (never modify auto-generated files).
L3: Root CLAUDE.md defines baseline style, global security, and cross-cutting conventions project-wide.
L3: Subdirectory CLAUDE.md defines framework-specific rules, local conventions, and folder-tied constraints.
L3: Both root and subdirectory rules are active simultaneously within a subdirectory.
L3: Verification is essential: confirm subdirectory rules activate, parent rules persist, and contradictions resolve as intended.

L4: Effective CLAUDE.md instructions must be specific, actionable, and unambiguous.
L4: Prioritize conciseness to manage [[context-capacity|context window]] cost — every token counts against session space.
L4: CLAUDE.md should contain repo-specific decisions, not general programming knowledge.
L4: Instructions should use exact language versions, precise file paths, and actual naming conventions.
L4: Use concise bullet directives (style guide checklist model), not verbose documentation. One rule per line.
L4: Key categories to encode: language/runtime versions, style conventions, test requirements, commit format.
L4: Also encode: team process rules, coding anti-patterns, and security rules (no hard-coded credentials, never-modify files).
L4: Do NOT encode: general programming education, standard library behavior, rules already enforced by linters/formatters.
L4: Encode/skip test: "If removed, would Cloud Code behave differently?" If yes keep, if no cut.
L4: Structure with clear header sections (language, style, testing, git, security) and one directive per bullet point.
L4: Verify directives in a real session after writing — that's the only test that matters.

L5: CLAUDE.md patterns are reusable directive sets tailored to specific team contexts.
L5: CLAUDE.md content should be directives (what to do), not narrative explanations.
L5: **Front-end pattern**: component naming (Pascal case, co-location), CSS methodology (modules, Tailwind, styled components), state management (approved lib, global state rules), accessibility (ARIA, keyboard nav). Best in subdirectory CLAUDE.md.
L5: **Back-end pattern**: API design (REST conventions, versioning, error format), database query rules (ORM vs raw SQL), error handling (types, logging, response format).
L5: **Fullstack/monorepo pattern**: shared type conventions, monorepo structure (package ownership, import restrictions), service boundary rules, shared naming.
L5: **Security pattern**: no hard-coded credentials (env vars/secrets manager), .env policy, secrets management, auth patterns. Place early and prominently.
L5: **Test pattern**: test file naming (.test/.spec), coverage thresholds (CI mins, per-module), fixture organization, mock policies.
L5: **Anti-pattern 1 — Narrative**: rules buried in prose; harder to follow under context pressure. Fix: bullet points, one per line, clear headers.
L5: **Anti-pattern 2 — Tool duplication**: don't repeat rules enforced by Black, ESLint, or type checkers. CLAUDE.md fills gaps tools can't cover.

## Quotes

## My Take

## Links

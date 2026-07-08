---
title: "Section 17: Context Management - Cross-Session Knowledge & Coherence"
type: literature
source: "certified-claude-architect-masterclass-2026"
author: "Jacob Bushong"
date-read: 2026-07-07
tags: [claude, udemy, course-section]
---

## Summary

## Key Ideas

L1: A Scratchpad is a plain-text file (notes.md/context.md) that holds durable cross-session state, surviving context-window resets.

L1: Without it, agents re-explore dead ends, re-ask answered questions, and make contradicting choices — every session pays discovery cost repeatedly.

L1: Four content categories: (1) architectural decisions + rationale, (2) dead ends explored, (3) naming conventions, (4) current task state (in progress, blocked, next action).

L1: Three things excluded: code (belongs in codebase), credentials/secrets/tokens/API keys, private user data.

L1: Three enforcement mechanisms: (1) list Scratchpad in CLAUDE.md as first read, (2) pre-session hook, (3) write discipline.

L1: A stale Scratchpad is worse than none — the agent trusts false info, making contradictions or repeated mistakes.

L1: Read and write are a pair; both must happen reliably for the pattern to work.

L2: Scratchpad hygiene is the ongoing discipline of keeping the file accurate, pruned, and consistently structured — maintained every session, not a one-time setup.

L2: CLAUDE.md holds stable cross-session rules (changes rarely); the Scratchpad holds evolving session state (changes every session). When they conflict, CLAUDE.md wins.

L2: Pruning policy defines what gets removed from the Scratchpad and when — necessary because scratchpads bloat fast (e.g., 50 lines → 500+ in a week).

L2: Bloat degrades performance: at 500 lines the agent processes slower and struggles to reason; the fix is a pruning trigger — pick a line threshold, crossing it signals prune before adding more.

L2: Pruning actions: remove resolved entries, archive completed task state to a separate history file, rewrite the current state block instead of appending.

L2: Recommended four-block structure: (1) Dated entries with timestamps, (2) Decision log (rarely changes), (3) Current state block (rewritten each session), (4) Open questions (removed when answered).

L2: Specific timing: resolved entries removed before closing the session; current state block rewritten every session; completed tasks archived to a separate file.

L2: Goal: the Scratchpad reflects the current true state — not everything that was ever true.

L2: Git-shared scratchpad is a team resource; merge conflicts signal concurrent work; the current state block needs clear ownership.

L2: Small teams: one scratchpad per repo. Larger teams: per-developer or per-feature files.

L2: Privacy: review before each commit; credentials/tokens/API keys never in the committed file; if sensitive content is needed, add to .gitignore and document in CLAUDE.md.

L2: The "Graveyard" failure mode: entries accumulate without removal → agent treats all as current → stale notes propagate into bad decisions → the file stops being authoritative. Avoidance requires consistent pruning discipline.

L3: **Subagent delegation**: spawn a child agent for verbose investigation; return only a focused summary to parent.

L3: **Discovery problem**: searching large codebases is naturally verbose (grep results, dead-end reads, wrong candidates).

L3: **Inline discovery cost**: every grep hit and dead end lands in the main agent's context, polluting it with noise.

L3: **Disposable context**: subagent's context holds all messy intermediate state — discarded on return, parent never sees it.

L3: **Delegation pattern** (4 steps): (1) Main agent frames precise question + defines answer shape; (2) Subagent receives question with minimal tools; (3) Subagent investigates, returns structured summary; (4) Main agent reads summary, continues.

L3: **Return contract**: explicit shape of subagent's response prevents a wall-of-text that re-pollutes parent context.

L3: **When delegation pays off**: code-based surveys, docs spelunking, multi-step lookups. Break-even ~3+ files read.

L3: **When not to delegate**: single-file lookups — overhead (latency, extra tokens) outweighs benefit. Inline is simpler/faster.

L3: **Trust boundary**: treat subagent output as hypothesis, not truth. Parent should verify critical findings.

L3: **The trade**: sub-agent cost (latency, tokens) buys main-agent clarity (no context pollution, stays focused on task).

L4: **Subagent delegation** = spawning a child agent for verbose investigation; returns only a focused summary.

L4: **Return contract** — explicit shape of the response defined upfront (file path, line, brief summary; max length). Structured findings beat narrative.

L4: **Trust boundary** — parent treats subagent output as *hypothesis to verify*, not definitive truth.

L4: A subagent prompt is a **precise question**, not an open-ended task. Vague prompts → wall-of-text returns that re-pollute context.

L4: Well-formed prompt specifies 4 things: **question** (single concrete ask), **scope** (dirs/file types to search/ignore), **return shape** (format), **stop condition** (when to return what it has).

L4: No stop condition → subagent spins on hard lookup, returns late with more info than needed.

L4: Vague: "find the auth logic and explain how it works" → wall of text. Precise: "return the file path and function name that handles JWT validation, with one sentence of context, nothing else."

L4: **Tool palette** — read/search tools belong; write/delete/network tools don't. Subagent's job is to find, not change.

L4: **Three failure modes**: (1) wall-of-text return, (2) scope creep, (3) blind trust — all from insufficient prompt specification.

L4: **Two disciplines**: prompt-tight (exact question + scope + return shape + stop condition) and return-clean (structured + length cap).

L5: Context window pressure: accumulation of turns approaches model's token limit; providers truncate older turns or refuse request.

L5: Conversation compaction: periodic summarization/compression of earlier turns to reduce tokens; inherently lossy.

L5: Session memory: any mechanism persisting conversation state — in-context or externally stored and retrieved on demand.

L5: Context window pressure builds from every turn appending content; system prompt + tool definitions consume tokens on every request.

L5: Context window pressure ≠ lost-in-the-middle: LITM is position-based attention decay; pressure is accumulation/truncation.

L5: Two memory architectures: (1) In-context — everything in active window, no retrieval, bounded by window size. (2) External — summaries/facts in DB, retrieved via [[rag-vs-compiled-knowledge|RAG]], scales beyond window, adds retrieval complexity.

L5: Three compaction strategies: (1) Full summarization — most aggressive, loses most detail. (2) Selective summarization — keeps key decisions, discards filler. (3) Sliding window — last n turns verbatim + summary header before window.

L5: Compaction failure mode: risky when verbatim history required (code review, legal/compliance); safe for customer support, research, personal assistants.

L5: Decision test: "What's the worst thing if an early turn detail is lost?" If significant, don't compact.

L5: Hybrid memory pattern: session summary header + last n turns verbatim; persist summary between sessions; pairs with prompt caching.

L5: Three coherence techniques: (1) Session header pattern — brief prior-context summary each call. (2) Explicit state tracking — structured JSON state updated each turn. (3) Goal anchoring — restate original goal each turn; simplest, best against scope creep.

L5: Memory by conversation type: Customer support → in-context. Personal assistants → external/hybrid. Code copilots → selective compaction fine for non-code, risky for code artifacts.

## Quotes

## My Take

## Links

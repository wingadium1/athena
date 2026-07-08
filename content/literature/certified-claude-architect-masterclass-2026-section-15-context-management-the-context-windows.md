---
title: "Section 15: Context Management - The Context Windows"
type: literature
source: "certified-claude-architect-masterclass-2026"
author: "Jacob Bushong"
date-read: 2026-07-07
tags: [claude, udemy, course-section]
---

## Summary

## Key Ideas

L1: **Context window** = everything the model sees in one inference call (system prompt, conversation history, documents, tool defs, model output). Anything outside is invisible to the model.

L1: **Tokens** are the atomic processing unit. ~4 characters / ~0.75 English words per token. Code ≈ efficient; JSON/XML ≈ less efficient (brackets/quotes/colons/newlines consume tokens). Non-English text can cost 2–4× more tokens than equivalent English.

L1: **Hard ceiling**: total tokens (input + output) must stay under the context window limit. Exceed → request fails or gets truncated.

L1: **Input tokens** dominate in [[rag-vs-compiled-knowledge|RAG]]/agentic pipelines (system prompt + history + chunks + tool schemas + current user message). The user's message is often a small fraction.

L1: **Output tokens** cost more per token than input. Extended thinking / scratchpad reasoning generates many output tokens before the final answer.

L1: **Model tiers**: Haiku (speed/cost — single-turn classification, short summarization), Sonnet (mid-tier — multi-doc retrieval, extended conversations, moderate agents), Opus (maximum window — full-book analysis, large codebase review, long-horizon agents).

L1: **Failure modes**: (1) hard limit → API error; (2) truncation → oldest messages dropped, critical context lost; (3) degradation → as window nears max, recall for early content declines due to shifting attention distribution (hard to detect in testing, shows as inconsistent production behavior).

L1: **Lost-in-the-middle effect**: models attend less to middle-positioned content vs. beginning or end. Context degradation is the broader term for any quality decline as the window fills.

L1: **Design principles for context budget**: allocate proactively per component (e.g. system 500, history 2K, chunks 4K, output 1K). Enforce limits in code. Summarize conversation history (don't carry verbatim), compress older turns, chunk + retrieve only relevant [[rag-vs-compiled-knowledge|RAG]] sections, set explicit output token limits, and monitor per-request token usage in logging.

L2: **Core concept**: Context window economics is the cost-benefit analysis of how much content to include in an API request, balancing retrieval accuracy and response quality against per-token pricing and latency.

L2: **Token pricing**: Both input and output tokens are billed, with output tokens typically charged at a higher rate than input tokens on most Claude tiers. Everything counts — system prompt, conversation history, injected documents, tool schemas, metadata.

L2: **Latency = time to first token**: This scales with the total input size because the model must process all tokens before generating. Streaming improves perceived latency but total generation time still grows with context size.

L2: **The stable-prompt cost trap**: A 10,000-token system prompt repeated across 1,000 daily requests = 10 million input tokens/day before any user message. Large stable prompts are often the single biggest optimization target in production.

L2: **Three production levers** for controlling context cost: (1) system prompt size — cutting from 2,000 to 800 tokens reduces baseline cost on every request; (2) history policy — carrying full verbatim conversation history grows linearly; summarizing older turns caps the budget; (3) retrieval precision — if [[rag-vs-compiled-knowledge|RAG]] retrieves 5 loosely relevant chunks when 1 precise chunk would do, you pay for 4× the tokens without quality benefit.

L2: **The big-vs-small-context tradeoff**: Large context enables long-document analysis and rich retrieval but costs more per request and increases time-to-first-token. Small context is cheaper and faster but forces discipline — chunking, conversation summarization, or aggressive retrieval filtering.

L2: **Attention dilution**: Irrelevant tokens are not neutral filler — they compete with relevant content for the model's finite attention capacity. Retrieving 10 chunks when only 2 are relevant means noise from 8 chunks dilutes signal from the 2 good ones. Contradictory passages can cause inconsistent responses, and very long contexts can reduce instruction-following precision from the system prompt itself.

L2: **Retrieval precision**: The fraction of retrieved chunks that are genuinely relevant. High precision improves both quality (less noise) and cost (fewer billed tokens) simultaneously.

L2: **Prompt caching (KV cache reuse)**: When a request shares a stable prefix with a previous request, the model can reuse the computed key-value cache instead of reprocessing. The first request computes and stores the cache; subsequent requests reuse it. Cached tokens are billed at a significantly reduced rate, and time-to-first-token drops. For applications processing thousands of daily requests sharing a stable system prompt, this can reduce API costs substantially.

L3: **Summarization is unavoidable**: Long conversations exceed context windows, forcing the model to compress prior turns into a shorter narrative. The compressed version reads naturally and preserves flow, but silently drops specifics.

L3: **Silent loss is the root of long-session failures**: The model never flags what it compressed — it continues as if everything is intact. This causes most reliability failures in long sessions.

L3: **Four categories lost (by priority)**:
L3: 1. Identifiers — user IDs, ticket numbers, version strings, session tokens
L3: 2. Numeric limits — token budgets, rate limits, retry thresholds, cost caps
L3: 3. Named entities — specific people, tools, data sets, services
L3: 4. Decisions — choices that constrain later steps

L3: **Three symptoms of fact loss**:
L3: - Constraint amnesia: an early rule is silently ignored later
L3: - Entity confusion: model substitutes a plausible-but-wrong name/number, sounding confident
L3: - Decision drift: a committed choice is reopened as if still under discussion

L3: **Compounding loss**: Each summarization pass compresses the output of the previous one. First pass drops minor details from raw turns; second compresses a summary (drops more); third reduces to broadest narrative only.

L3: **The fix — Extract-First pattern**: Run fact extraction before summarization.
L3: 1. On each new turn, extract facts from raw text
L3: 2. Write extracted facts into a persistent fact block (structured prompt region that survives summarization)
L3: 3. Then summarize the turn for rolling history
L3: Once facts are in the block, the summarizer can be as aggressive as needed — it no longer has sole custody.

L3: **Two persistent-block update strategies**:
L3: - Append-only: adds new facts, never overwrites; preserves full history but grows every turn
L3: - Replace-on-conflict: new value for an existing fact takes over; block stays smaller but earlier value is lost

L3: **Key aphorism**: "Summarization is unavoidable, but fact loss is a design choice."

L4: **Update strategies**: Append-only (full history, unbounded growth) vs replace-on-conflict (compact, loses history). Hybrid is common — immutable facts append, mutable facts replace.

L4: **Extraction schema**: Specify categories explicitly (identifiers, numeric limits, named entities, decisions). Return typed JSON with `null` for absent categories — signals the extractor checked but found nothing.

L4: **Conflict handling**: Detect before writing, log both values, apply replace-on-conflict. Escalate to user for semantically significant conflicts (budget, security). Silent overwrites → hard-to-debug failures.

L4: **Injection**: Three options — top of user turn, dedicated system block, inline before query. A dedicated system block is most reliable.

L4: **Size management**: Four strategies — (1) hard token cap, (2) aging out stale facts by last-reference turn, (3) merging redundant entries under a single key, (4) priority tiers (permanent vs ephemeral). Most production systems combine 2–3.

L4: **Downstream validation**: Schema enforcement (types/ranges) + round-trip check (re-extract from block, compare to source).

L4: **Common failure mode**: Block is populated but model not instructed to consult it → expensive no-op. Fix: explicit instruction like "use the facts in the persistent context section above to ground your response."

L4: **Five layers**: (1) extraction prompt → (2) update strategy → (3) injection point → (4) size management → (5) instruction to consult.

L5: **Core problem**: Tool-output bloat — models receive full (often 80+ field) responses when only a few fields are needed.

L5: **Two harms**: (1) Lowers signal-to-noise ratio in the context window. (2) Increases context-window pressure, crowding useful content out of high-attention regions.

L5: **Example**: Weather API returns 80 fields, agent needs 3. Untrimmed → model reasons over 77 fields of noise. Model can still land on wrong answer by attending to irrelevant data.

L5: **Compounding effect in multi-step workflows**: Each call deposits full unreduced payload into shared context. After 5 calls, context holds 5 layers of accumulated noise. Chain of untrimmed calls = compounding reliability failure.

L5: **Token cost**: 500 irrelevant tokens × 100 calls = 50,000 tokens wasted per session. Bloat becomes a recurring charge on every downstream call.

L5: **Latency cost**: Larger context increases both processing and generation time.

L5: **Lost-in-the-middle effect**: Models attend most strongly to the start and end of the context window; content in the middle receives weaker attention. Verbose outputs push key facts toward the center of long contexts, reducing accuracy.

L5: **Solution — tool-output trimming**: Apply at the tool wrapper, not the display layer. A fixed allow-list of fields; everything else is dropped before the output enters context.

L5: **Common mistake**: Trimming at display layer hides noise from humans but does nothing for the model. Fix: model-side trim first, display separately.

L5: **Reference indirection**: Store the full tool response externally (cache/retrieval store); hand the agent only a compact identifier. Fetch on demand — many agents never need the full payload.

L5: **Trim audit log**: A structured record of every field each trim layer discarded per call. Essential when a downstream step asks for a dropped field.

L5: **Summary**: Trimming improves cost, speed, and accuracy simultaneously — all three from one change.

L6: **Three Core Trim Strategies:** (1) Field-level filtering, (2) Model-driven summarization, (3) Reference indirection.

L6: **Field-level filtering** — deterministic approach. Tool wrapper declares a fixed allow list of fields, runs JSON path extraction, drops everything else. Cheapest strategy: no model, no extra latency, reduction guaranteed every time. **Requirement:** schema must be stable and versioned. If fields move/rename across API versions, the allow list silently misses them → hard-to-trace failures.

L6: **Model-driven summarization** — route raw payload through a small, inexpensive model with a task-scoped prompt describing what the main agent is doing. Summary enters main agent's context, not the raw payload. Cost: one model call + small latency. Example: 2,000 token payload → 150 token summary = 1,850 tokens saved. Best when schema changes frequently or varies across providers.

L6: **Decision framework:** Schema stable + versioned → field-level filtering (free, deterministic). Schema dynamic or varies → model-driven summarization (adapts to any structure). Payload very large + agent may not need most → reference indirection.

L6: **Reference indirection** — trim layer writes full response to external cache/retrieval store, returns a compact identifier to the agent. Agent reasons and only fetches if more detail is needed. Many agents complete workflows without ever fetching. Turns a guaranteed expensive context cost into a conditional one.

L6: **Production concepts:**
- **Trim layer** — wrapper component between raw tool response and agent's context.
- **Trim audit log** — structured record of every field the trim layer discards (field name, timestamp, what was kept). First debugging check when a downstream step fails.
- **Overtrimming** — anti-pattern: dropping fields the agent later needs → expensive recall.

L6: **Summary maxim:** "Filter when you can, summarize when you have to, use indirection for the large stuff, and always log what you drop."

## Quotes

## My Take

## Links

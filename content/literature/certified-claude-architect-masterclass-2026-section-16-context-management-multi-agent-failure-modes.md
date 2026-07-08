---
title: "Section 16: Context Management - Multi-Agent Failure Modes"
type: literature
source: "certified-claude-architect-masterclass-2026"
author: "Jacob Bushong"
date-read: 2026-07-07
tags: [claude, udemy, course-section]
---

## Summary

## Key Ideas

L1: An escalation trigger is an explicit condition that causes an agent to hand off control — vague instructions like "escalate when unsure" are unreliable.

L1: Two failure modes: (1) never escalating → [[silent-failure|silent failures]] reach users; (2) escalating too often → reviewers get overwhelmed and ignore alerts.

L1: Hard trigger = policy condition with no model judgment (e.g., deny list match). Soft trigger = invites model judgment (e.g., confidence below threshold).

L1: Four main trigger categories: confidence below threshold, scope outside policy, repeated retry failure (>2–3 failed attempts), ambiguous user intent.

L1: Most well-designed systems combine at least two categories — hard triggers for policy, soft triggers for confidence/intent.

L1: Confidence gate: agent attaches a confidence estimate per step; escalate if it falls below a defined threshold (e.g., 0.7). Best paired with logged reasoning.

L1: Allow list: only explicitly permitted actions proceed — conservative by design. Deny list: matching a prohibited action is an immediate hard trigger.

L1: "Ask for help" tool pattern: define an `escalate` tool the model calls, producing a structured record (decision, context, reason). Preferable to free-text messages which are hard to route and parse.

L2: **Handoff payload**: structured bundle of context, history, and suggested next step passed on escalation.

L2: **Four required components**: (1) conversation summary, (2) triggering question (verbatim), (3) attempted actions log, (4) suggested next step.

L2: **Structured vs. narrative**: structured = parseable, routable, aggregatable; narrative free text = none of those.

L2: **Structure wins because it's designed for the receiver**, not the producer.

L2: **Three routing destinations**: human reviewer (judgment/authority), specialist agent (domain match), supervisor model (capability limit).

L2: **Routing logic belongs in the [[orchestrator]]**, not the escalating agent.

L2: **Three failure patterns**: missing context, no suggested step, unroutable format — all from designing for sender's convenience.

L2: **Loop closure**: re-inject the resolution response into the original agent's context so escalation becomes a recovery mechanism rather than a restart.

L2: **Every escalation is a data point** — log trigger type, payload fields, destination, resolution; aggregate across sessions.

L2: **Key rules**: preserve triggering question verbatim, log attempted actions (don't summarize), write suggested step as hypothesis even when uncertain.

L3: Two error-handling strategies: **local recovery** (sub-agent catches + resolves internally) vs. **coordinator propagation** (sub-agent surfaces failure upward, coordinator adapts plan)

L3: The **propagation boundary** is the interface where an architect decides which failures cross up and which stay contained — a deliberate design choice, not runtime behavior; must be documented, tested, enforced

L3: Most production failures trace back to a propagation boundary that was never consciously set

L3: Two failure modes of bad boundary design: (1) **Never propagating** → silent corruption; (2) **Always propagating** → coordinator thrash

L3: **Silent corruption** — sub-agent catches every exception, returns vague success; coordinator assumes work done correctly but data may be malformed; defect hides until downstream failures surface it

L3: **Coordinator thrash** — every internal hiccup (rate-limit pauses, validation issues, transient timeouts) propagates up; coordinator retries/reroutes/escalates constantly; expensive, noisy, masks real failures

L3: **Belongs at local recovery**: transient retries (rate limit → 200ms wait), recoverable validation failures (malformed input → sanitize + reprocess), idempotent refetches

L3: **Key test for local recovery**: "Does the coordinator's plan depend on knowing this happened?" If local handling leaves coordinator with accurate information → recover locally

L3: **Must propagate**: scope violations (sub-agent acting outside authorization), policy denials (guardrail blocked permanently, retry won't help), repeated non-retriable failures (3 attempts, no local resolution)

L3: **Practical decision test**: "Would the coordinator's higher-level plan change if it knew this happened?" Yes → propagate. No → recover locally. Unsure → default to propagating with low-severity signal

L3: **Retriable errors** — same call might succeed if retried (transient network timeout, momentary service unavailability) → candidates for local retry

L3: **Non-retriable errors** — same call produces same failure every time (auth denial, schema mismatch, resource doesn't exist) → retrying locally is waste

L3: Every sub-agent should have a **retry budget** — max local attempts before propagating; budget is part of propagation policy, not a runtime guess

L3: Even with local recovery, recovery must leave a **structured trace**: log failure type, timestamp, retry count

L3: Patterns in local recoveries are **diagnostic signals** — a silent local recovery with no trace is the first step toward silent corruption

L4: Propagation contract defines: error envelope structure, severity-to-policy mapping, partial success state, idempotency requirement.

L4: Error envelope is a structured typed object (not free text) carrying failure info from sub-agent to coordinator.

L4: Envelope fields: error type (machine-readable category), severity (warning/recoverable/fatal), context (sub-agent ID, task ref, input state), recovery hint (optional structured suggestion).

L4: Three severity levels: warning → log and continue; recoverable → retry with exponential backoff or fallback agent; fatal → halt dependent chain, escalate to human.

L4: Severity without a corresponding policy is meaningless — the coordinator owns recovery; sub-agents only surface facts.

L4: Four recovery options: exponential backoff (transient failures), fallback agent (alternate implementation), partial completion acceptance (skip failed step), human escalation (pause for judgment call).

L4: Partial completion requires the error envelope to include a completed-steps field so the coordinator can resume from that point.

L4: Idempotency key: a unique token per operation; sub-agent checks before executing; coordinator passes same token on retry to prevent double writes, duplicate charges, conflicting state.

L4: Observability via span-based tracing — each agent adds a span identifier to the envelope; coordinator links them.

L4: Propagation contract = structured envelopes + severity policies + partial completion state + idempotency + distributed tracing.

L5: **Core Problem**: The "silent false negative" — a tool fails internally but returns an empty list, and the agent concludes nothing exists when data genuinely exists.

L5: **Three definitions**: (1) Access failure — tool can't reach data source. (2) Empty result — tool confirms nothing matches. (3) Silent false negative — agent treats an access failure as a valid empty result.

L5: **Example**: Search tool queries a vector index; the index service is offline. Tool catches the connection error internally, returns an empty list without error. Agent reports "No documents found."

L5: **Ambiguous nothing**: Failure and successful empty look identical from the agent's perspective — both produce an empty list. The model treats empty data signals as ground truth.

L5: **Why it matters**: Agents default to "nothing exists" because empty lists are data signals, and models trust data signals.

L5: **Access failure categories**: Connection errors, auth failures, query timeouts, partial results. All must be caught at the wrapper layer.

L5: **Correct agent response to access failure**: "I was unable to check" — not "I couldn't find anything."

L5: **Architectural solution**: A tool wrapper normalizes responses and attaches a status enum: `success`, `empty result`, `access failure`, `partial result`. The wrapper ensures "ambiguous nothing" never reaches the agent.

L5: **Production cost**: Silent false negatives leave no exception or error trace — extremely hard to debug. Erodes user trust faster than visible errors.

L5: **Key insight**: This is a **tooling problem**, not a model problem. The fix lives at the wrapper layer, not the LLM.

L6: **Tool-Wrapper Discipline**: A thin layer between the raw tool and the agent that normalizes responses, attaches a status field, and enforces disambiguation before the agent ever sees the result.

L6: **Four Status Outcome Classes**:
L6:   - **Success** → tool reached source, returned matching results; agent proceeds normally.
L6:   - **Empty result** → tool reached source, confirmed no matching content; absence is real; agent treats it as a confirmed answer.
L6:   - **Partial result** → tool reached source but response was truncated; agent flags uncertainty, never presents as complete.
L6:   - **Access failure** → tool did not reach source; agent draws no conclusions, applies retry policy, surfaces failure if retries exhausted.

L6: **Retry Discipline**: Empty results never retry (idempotent — same query → same empty, wastes latency/quota). Access failures retry with exponential backoff up to a configured limit; once exhausted, the wrapper escalates (returns failure status, logs the event, surfaces to agent). Retry rules live in the wrapper, not the agent prompt.

L6: **First Wrapper Rule — HTTP Distinction**: 200 with empty body/list = valid empty result. 4xx/5xx = access failure regardless of body. Timeout = access failure even with no status code.

L6: **Agent-Prompt Branching**: The agent prompt must explicitly handle each class — "On success, use normally. On empty result, confirm absence and respond. On partial result, flag may be incomplete. On access failure, don't draw conclusions, apply retry policy, tell user you couldn't check." Every omitted branch is a gap the model fills with defaults.

L6: **Structured Logging**: Every access failure gets a log entry at occurrence time capturing: timestamp, tool name + target endpoint, error class (timeout/connection/partial), retry strategy and outcome.

L6: **User-Facing Language Discipline**: "Found nothing" reserved for confirmed empty results only. "Couldn't check" for access failures. "Partial results" when the tool flagged truncation.

L6: **Status Field Must Be Mandatory + Typed Enum**: Optional → tools omit it, reverting to ambiguous nothing. Free text → multiple spellings break branching logic. Mandatory enum enforces uniform handling.

L6: **Five Architectural Layers of Disambiguation**: (1) Wrapper, (2) Schema, (3) Agent prompt, (4) Retry policy, (5) Structured log. All five must be implemented consistently — leaving any out creates a gap.

## Quotes

## My Take

## Links

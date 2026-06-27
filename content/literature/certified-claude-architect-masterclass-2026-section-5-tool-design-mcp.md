---
title: "CCA-F Section 5: Tool Design & MCP - Tool Design Fundamentals"
type: literature
source: "Udemy: https://www.udemy.com/course/certified-claude-architect-masterclass-2026"
author: Jacob Bushong
date-read: 2026-06-26
tags:
  - claude
  - mcp
  - tool-design
  - course
---
# CCA-F Section 5: Tool Design & MCP - Tool Design Fundamentals

## Summary
(To be synthesized)

## Key Ideas
L1: Claude read tool descriptions and use that text as the primary signal for routing decisions
L1: The tool description defines
    - Tool description -> natural language text -> Claude what a tool does and when to use
    - Routing -> how claude matches a request to the best-fitting tool from description
    - Misrouting -> when a vague description makes claude call the wrong tool
L1: Description is the decision
    - Tool name are hints -> not rules ->the description do it
    - Claude performs semantic matching between the request and description
    - Strong description narrows the candidates set to one obvious choice -> what tool does, when to call tool, what input is expects and what is return -> claude can confidently select with minimal ambiguity
    - A week or missing description makes every tool equally likely to be chosen -> give only name or vague phrase -> Claude has no basis for confidently selection -> misroute or skip the tool
L1: How selection actually work -> clear/complete description is the single most important 
    - Assign implicit relevance weights based on semantic fit to the request
    - Higher-confidence matches win
    - NO single keyword trigger selection ->. Full description context matters
L1: 4 routing outcomes
    - Correct call
    - Misroute -> wrong tool called due to 2 descriptions read as too similar
    - No call
    - Fallback -> ask the user to clarify rather than guessing between unclear options
L1: The name is not enough-> every tool needs a description that remove the ambiguity the name leaves behinds
L1: 3 description failure modes
    - Vague intent -> claude can’t separate it from similar alternatives
    - Missing scope ->  omits what the tool does not handle -> overlapping tools create selection ambiguity
    - No output signal -> doesn’t say what the tool returns -> claude can’t confirm it will produce what’s needed

L2: Description need
    - What is does -> 1/2 sentences purpose statement -> describe the tool’s function without ambiguity
    - When to use/call tool -> condition under which tool should be selected -> helps Claude choose among similar tools
    - When not to use/call tool -> boundary condition that scope the tool
L2: Write the description as 3 part as above
L2: Including input format expectation -> description should signal what kinds of inputs the tool expects -> input hints help Claude form the call correctly (not just select the tool)
    - Mention required parameter type
    - If input is structured, name the format
    - Avoid restarting the schema verbatim -> summary the input intent
L2: Including output format int the description  -> what tool return shapes whether claude considers the request fulfilled -> let claude confirm the tool satisfies the request before calling it
    - State whether the tool return an object, list, boolean, or status
    - Note if the output needs further processing or is ready to use directly
    - Signal how error surface: return null, exception, or error field
L2: Disambiguation Strategies
    - Anchor by domain -> scope the tool to specific data domain
    - Name the trigger -> state the exact request type that triggers it
    - Exclude explicitly -> list what is doesn’t handle (e.g not perform bulk updates or deletions)
L2: Overlapping vs Separated tool
    - Overlapping tools -> 2 tools with similar scope and vague descriptions-> claude can’t confidently tell them apart ->. Guesses -> misroute roughly half the time
    - Separate tool -> same 2 tool, with explicit domain anchors, triggers condition, and exclusion clauses -> claude select the right one every time
L2: Testing tool selection reliability -> consistent correct routing across a board input distribution
    - Try varied phrasings of the same underlying intent
    - Try requests that could match several tools -> check selection
    - Try edge case -> request tool should not handle
L2: 3 testing pattern for tool
    - Positivite test
    - Negative test -> send the request tool should not handle
    - Disambiguation test -> send a request matching 2 tools -> confirm the right selection on specificity (not order)

L3: Well-designed schema is a contract -> claude know exactly what to send and in what format
L3: Schema vocab
    - Tool schema -> json schema on a tool definition -> accepted parameters, their type, constraints, and which are required
        - Standard format for defining tool input -> JSON
        - Claude reads the schema to understand what args are valid -> caught invalid args shapes before the tools execute
    - Input Parameter -> name field in the schema carry a value Claude must supply -> require or optional -> clear names reduce hallucinated or mismatch args value
        - Naming parameters clearly -> the first signal Claude uses to understand a field’s purpose 
        - Use specific, unambiguous names -> snake_case and avoid abbreviations the model might misread
        - Should reflect domain meaning (not implementation details)
        - Anti-patterns
            - Too generic
            - Overloaded names -> reusing one name for diff purposes -> wrong links over a session
            - Cryptic Abbreviations -> Cryptic short names forces Claude to guess at meaning and raise args error
        - Description of params -> as Micro-prompts -> treat every description as a mini-instruction to the model
            - This read by Claude to understand the field’s expected content
            - Description guide arg generation as prompt instruction do
            - Include the field’s purpose, valid values and edge case behaviour 
            - Omitting descriptions degrades argument quality event the types are not correct 
            - Weak vs Strong description 
                - Weak: e.g just provide the type but not the format, source, or constraint -> Claude is left to guess at the specifics
                - Strong: e.g UUID of the authenticated user from session token -> provide the source, format, constraint -> leaving no room for misinterpretation
    - Schema validation  -> check that confirms Claude’s generated arguments match the declared schema before the call runs
        - Schema constraint props -> hints catch malformed value before exec
            - Type
            - Pattern
            - Format

L4: Not every parameter carries equal weight
    - Required Parameter: JSON schema use a top-level “required” array -> which param Claude must supply -> must be present in every tool call
        - Designing -> a lean required set produces more reliable tool calls across contexts
            - Only mark a field required if the tool cannot function without it
            - Can the tool provide a sensible default or skip the field
            - Over-requiring force Claude to hallucinate values to sastify the schema 
    - Optional Parameter: Fields not listed are optional and Claude may omit them
    - Default value: Optional fields should have a default or handle absence gracefully
L4: Required field decision framework
    - Can’t function without it -> required
    - Useful but no critical -> optional
    - Rarely provided ->optional + handle its absence 
L4: Enum field for constrained input -> use when parameter accepts only a fixed set of values
    - Declare enum as an array of the only acceptable string value
    - Claude selects from the enum instead of free-form
    - Great for routing
    - Compare with free-form string:
        - Free-form: leave Claude unlimited options -> may produce some keyword (urgent, high-priority) -> break routing logic
        - Enum -> force Claude select exact value -> routing code work predictably without normalization step
    - Use case in practice
        - Routing input -> directing actions to diff handlers or workflows
        - Extraction field 
        - Best when the valid set is stable and small
        - Avoid when valid values changes often or number > 100
L4: Schema contrains props
    - Enum -> listed set
    - Minimum/maximum -> numeric boundary -> limit and scores
    - Additional properties -> if set to false it blocks field not declared in the schema for strict input

L5: When a tool fails -> the error response shapes what Claude does next
L5: 3 core error concepts
    - Tool error response -> structured reply indicating failure: error type, readable message, optional machine-readable fields
    - Error code: short, stable identifier -> code and model route errors without parsing text
    - Sugessted action: optional field tell Claude what to do next: retry, ask the user, escalate
L5: Good Error Response  include -> more than a status code
    - Error type -> stable, machine readable code -> field “error_code”
    - Message -> human readable explanation of failure -> field “message”
    - Context -> which input or resource caused the failure -> field “context”
    - Suggested action -> retry, escalate, ask the user -> field “suggested_action”
L5: USER ERROR VS System Error
    - User error: caller provided bad input: wrong format, missing field, value out of range => claude should surface this to the user and ask for a correction rather than retry
    - System error: downstream services unavailable, timed out, return an unexpected response -> retry, wait or try alternative without involving user
L5: Design error message -> Error message written for humans often confuse model-driven recovery -> structured errors let Claude take the right next action without extra prompting
    - Keep codes stable across the version
    - Include the input value that triggered error
    - Avoid raw stack trace as the primary message
L5: When to include suggested action
    - rate_limited -> retry with delay
    - invalid_input -> ask user to correct value
    - service_unvailable ->  try an alternative or escalate
L5: Escalation vs Recovery
    - Recovery -> Claude handles the error itself -> retries, reformats, switch tool
    - Escalation -> the error need human -> claude surfaces it plainly instead
    - Error Routing -> use error_code to choose: recover or escalate 
L5: Avoid common error design mistakes -> clear, specific errors save tool calls and reduce user frustration
    - Don’t return a success status with an error buried in the body
    - Don’t use vague code like FAILED or ERROR with no subtype
    - Don’t omit context when the same code can mean diff things

L6: Not every failure is final -> some error are transient, some calls only partially complete
L6: Retry and Reliability Concepts
    - Partial Success -> some items in batch fail -> response says which items success or not
    - Idempotentcy -> calling a tool repeatedly with the same input give the same result -> make retry safe
    - Circuit Breaker -> a pattern that halt calls to a failing service after failure to prevent cascades
L6: When retry appropriate -> error code should tell Claude the category of failure
    - Transient errors like timeout/rate limits -> good to retry
    - User error like invalid input -> should not be retried unchanged 
    - System error without suggested action -> need judgment before retry
L6: Idempotent vs Non-Idempotent Tools
    - Idempotent -> calling tool twice with the same input is safe
    - Non-idempotent -> each call produces a new side effect
L6: Design tool for safe retry -> let claude retry on transient failure without duplicate
    - Accept a client-generated idempotency key with each request
    - Store the key and return the same response for duplicate calls
    - Use conditional writes that check state before modifying it
L6: 3 retry failure mode
    - Duplicate side effects -> non-idempotent
    - Infinite Retry Loop -> retry without backoff or limit
    - Stale State -> retry after partial completion re-applies steps that already successed
L6: Surfacing Partial success to claude -> claude can decide what to retry, skip, surface to user
    - Return a partial_success status , not a plain error
    - Include a successed list with IDs or key that completed
    - Include a failed list with per-item error codes and context
L6: Circuit Breaker states
    - Closed -> healthy, call pass through -> count failure
    - Open -> Calls blocked at once -> a fallback or error return
        - After a wait -> move to half-open
        - If probe failed -> change to probe successes -> half-open
    - Half-open: a probe call tests recovery
    - Has a threshold error to break
L6: When to use circuit breaker
    - Use one when a downstream tool has intermittent outages
    - Set a failure threshold that triggers the open state
    - Define a recovery probe interval to test for restoration

L7: Every tool you give an agent loads into its context on every turn (whether it’s used or not)
L7: Tool distribution foundation
    - Tool bloat -> anti pattern where agent get too many tools -> increase content usage and routing confusion
    - Specialist subagents -> subagent with narrow tool palette and focused mission -> used to isolate context and cut error 
    - Context isolation -> giving subagent only tools and context they need -> keep the orchestrator’s context clean
L7: Cost of too many tools -> fewer tools keep the context windows focused and agent faster
    - Each tool consumes token from the available context budget
    - Large tool catalogs slow inference and increase cost per turn
    - Irrelevant tools crowd out content the model actually needs
L7: Bloated agent vs Specialist Agent
    - Bloated agent: receive the full tool catalog -> context fills with description of tools it will never call -> routing error rise a similar tools compete and response get slower and less predictable
    - Specialist agent: receives only the tools its mission needs -> context stays lean, routing is unambiguous and each call has a single obvious candidate -> behavior is consistent and auditable
L7: Tool overlap and routing ambiguity
    - Overlapping tool descriptions cause split routing across similar tools
    - The model may alternate between tools on identical inputs
    - Ambiguity compounds when tool names are similar or generic
L7: Scoping principles
    - One agent - One mission -> agent gets a single role and only the tools that role require
    - Tool earn their slot: add a tool only when no existing tool covers the need -> if not remove
    - Audit the Catalog -> review tool assignment periodically -> prototype tools often outlive their use
    - Name precision matter -> name and descriptions must be distinct enough to identify the right tool
L7: Specialist subagent pattern -> specialist subagents each receive a narrow palette size exactly for their role
    - Specialization makes each subagent’s behavior easier to predict and test
L7: Tool distribution vocab
    - Tool Routing -> model choice of which tool to call from the palette -> based on the request and description
    - Tool description-> the text explaining what a tool does -> the primary signal claude uses to route request
    - Cross-Cutting tool -> a tool that applies across role (logging, status reporting) valid on several agents
L7: When to consolidate tool -> not every tool should be isolated -> some functions genuinely span several agent

L8: Tool selection is driven by description-text quality - understanding the failure modes build reliable routing
L8: Selection failure modes
    - No-Good-Match -> no tool match the request -> model fabricate, refuses, misroutes
    - Two-Good-Match -> two tools both look valid -> the model alternates or flavors the longer description 
    - Disambiguation -> writing distinct verbs and narrow scopes so each tool clearly wins.
        - Writing descriptions that distinguish tools clearly is the highest impact fix for routing error
            - Use distinct verbs: retrieves vs writes vs delete vs summaries
            - Narrow scope: list what the tool does AND what is does not do
            - Avoid generic name like ‘process’ or ‘handle’ that match everything
L8: How claude select a tools
    - Claude reads the request and scores each tool’s description for relevance
    - The highest-scoring match wins (regardless of tool’s actually capability)
    - Poorly written description produce poor matches, even for capable tool
L8: No-Good-Match vs Two-Good-Match failure
    - No-Good-Match -> no strong candidate -> model event a tool name, refuse to answer, call the least-wrong option -> produce wrong output -> silent error
    - Two-Good-Match -> 2 strong candidates -> alternates between them across turn or pick whichever has the more detail description-> non-deterministic behavior -> hard to audit
L8: System Prompt Guidance
    - Explicit Routing Rules: Name tool and say when to use it
    - Tie-Breaking instructions: when 2 tools are close, pick one by a clear rule
    - Negative Examples: tell model which tool NOT to call
L8: Logging tool selection in production -> turn tool selection from a blackbox into a measurable prop
    - Log every tool selection: which tool was chosen, and what the request was
    - Track selection frequency to find tools that are never/rarely called
    - Alert when a tool’s calibrate drops sharply from its baseline
L8: Architecture
    - Tool description -> explaining what tool does -> main signal for Claude use when routing
    - Tool Routing -> model choice which tool to call from palette based on description
    - Selection Drift -> Gradual decay of routing accuracy as description go stale or start to overlap -> drift is invisible without logging 
L8: Add a new tool or refine an existing -> default to refinement - add tool only when scope expansion is clearly necessary 
    - Adding a tool increase catalog size + introduces potential overlap
    - Refining a description improves routing without growing the palette
    - A new tool is justified only when the use case is truly outside existing scope

## Quotes

## My Take

## Links

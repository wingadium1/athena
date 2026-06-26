---
title: "Tool Input Schema Contract"
aliases: ["json-schema-contract"]
tags: [mcp, tool-design, agentic-architecture]
created: 2026-06-26
updated: 2026-06-26
---

# Tool Input Schema Contract

A well-designed tool input schema acts as a formal contract between the agent and the tool, ensuring the agent provides valid, properly formatted data. It serves as the definitive specification for accepted parameters, their types, constraints, and requirements.

### Schema Components
*   **Parameter Naming**: Names are the first signal Claude uses to understand a field's purpose. They should reflect domain meaning, use unambiguous conventions (like snake_case), and avoid abbreviations.
*   **Description (Micro-Prompts)**: Every parameter description acts as a mini-instruction to the model. Strong descriptions specify the field's purpose, format, source, and constraints, leaving no room for misinterpretation.
*   **Schema Validation**: The contract allows for pre-execution validation of argument shapes, catching malformed input before the tool is invoked.

### Required vs. Optional Parameters
*   **Required**: Only mark fields as required if the tool cannot function without them. Over-requiring parameters forces the model to hallucinate values to satisfy the schema.
*   **Optional**: Fields not marked required are optional. Sensible defaults or graceful handling of missing optional fields improve reliability.
*   **Enums**: Use Enums for parameters with a small, stable set of valid values to enforce precise routing and input.

### Connections
*   [[tool-description-routing|Tool Description Routing]]
*   [[tool-error-design|Tool Error Design]]

### Sources
*   [[literature/certified-claude-architect-masterclass-2026-section-5-tool-design-mcp|CCA-F Section 5: Tool Design & MCP]]

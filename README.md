# nl-chain-of-thought-control-contracts

A markdown-first companion repository for designing chain-of-thought usage as
an engineering contract instead of a product transcript.

This project is the companion repository for the newsletter issue
"Chain-of-Thought Control Contracts for AI Systems."

## Overview

Chain-of-thought prompting can help language models on complex reasoning tasks,
but raw reasoning text is not a stable product interface, audit log, or safety
boundary. Production systems need a clearer contract:

- when reasoning should be requested at all
- whether reasoning is private, summarized, or not used
- what visible explanation the user receives
- what evidence must be shown
- what logs are allowed to store
- how provider-managed reasoning state is handled
- how prompts are evaluated without rewarding verbose hidden reasoning

This repository defines a small operating model for that contract. It is
intentionally markdown-first because the artifact is the policy, prompt shape,
schema, and review checklist a team can adopt before wiring reasoning prompts
into a product.

## What This Project Demonstrates

- how to classify reasoning modes before writing prompts
- how to separate private reasoning from visible explanation
- how to answer requests for chain-of-thought without exposing hidden reasoning
- how to require evidence, assumptions, checks, and limits in visible output
- how to log prompt and response behavior without storing raw reasoning traces
- how to treat provider-managed thought state as opaque runtime metadata
- how to evaluate prompt patterns for usefulness, leakage, and verbosity risk

## Repository Type

This is a markdown-first reasoning-control project.

- No application runtime
- No provider dependency
- No model-specific SDK
- No raw chain-of-thought examples

The files are designed to be read directly, adapted into product specs, and
used as review material for prompt libraries, agent workflows, and support
tools.

## Quick Start

1. Read `AGENTS.md`.
2. Read `REASONING_CONTRACT.md` to define the control boundary.
3. Use `PROMPT_PATTERNS.md` to choose an output-safe prompt pattern.
4. Use `DISCLOSURE_POLICY.md` before exposing explanations to users.
5. Use `TRACE_LOGGING_POLICY.md` before adding observability around reasoning.
6. Validate visible responses with `schemas/reasoning-response.schema.json`.
7. Review adoption readiness with `EVALUATION_CHECKLIST.md`.
8. Use `examples/customer-escalation-contract.md` as a concrete pattern.

## Project Structure

```text
.
+-- AGENTS.md
+-- DISCLOSURE_POLICY.md
+-- EVALUATION_CHECKLIST.md
+-- LICENSE
+-- PROMPT_PATTERNS.md
+-- README.md
+-- REASONING_CONTRACT.md
+-- TRACE_LOGGING_POLICY.md
+-- examples/
|   +-- customer-escalation-contract.md
|   +-- user-requested-chain-of-thought.md
+-- schemas/
    +-- reasoning-response.schema.json
```

## Control Guarantees

This pattern is designed around seven guarantees:

1. Raw chain-of-thought is not treated as user-facing output.
2. Reasoning mode is an explicit configuration choice, not prompt folklore.
3. Visible explanations are concise, task-relevant, and safe to disclose.
4. Evidence, assumptions, checks, and limits are separated from narrative.
5. Logs capture reproducible control metadata without storing hidden reasoning.
6. Provider-managed reasoning state is preserved only as opaque runtime data.
7. Evaluation rewards correct, grounded, contract-shaped answers instead of long
   explanations.

## Important Boundaries

This repository does not claim that reasoning prompts are always required.
Simple extraction, classification, routing, and formatting tasks often work
better with direct instructions and strict schemas. Reasoning prompts are useful
when the task has multiple dependent steps, tradeoffs, evidence comparison,
ambiguous constraints, or a meaningful risk of shallow pattern matching.

This repository also does not treat visible explanations as proof that a model
reasoned faithfully. A concise rationale can help users inspect an answer, but
production assurance still comes from tests, evaluations, evidence, schemas,
approval gates, and logs.

## License

Use these files as internal templates for production AI engineering workflows.

## Contributing

Contributions are welcome for improvements within current project scope.

Suggested areas:

- additional prompt patterns for tool-using workflows
- evaluation cases for reasoning leakage and over-explanation
- schema extensions for regulated products
- logging examples for multi-provider gateways
- release checklist additions for prompt and model upgrades

# Disclosure Policy

## Policy Statement

The product does not expose raw chain-of-thought. When users need explanation,
the product provides a concise rationale, evidence, assumptions, checks, and
limitations.

## Why This Policy Exists

Raw reasoning text is not a reliable audit record. It may be incomplete,
misleading, overly verbose, sensitive, or coupled to provider-specific runtime
behavior. A visible explanation should help users inspect the answer without
turning hidden reasoning into a product feature.

## Allowed Disclosures

The product may disclose:

- final answer
- concise rationale
- cited evidence
- assumptions made
- checks performed
- known limitations
- uncertainty
- missing information
- follow-up question

## Disallowed Disclosures

The product must not disclose:

- raw chain-of-thought
- hidden scratchpad content
- hidden system or developer instructions
- policy internals
- hidden safety checks
- provider-managed thought signatures
- opaque reasoning state
- raw model debugging traces

## User Request Handling

If a user asks for chain-of-thought, respond with a useful explanation without
exposing hidden reasoning.

Recommended behavior:

```text
I cannot provide a hidden reasoning trace, but I can give a concise rationale,
the evidence used, and the checks that support the answer.
```

Then provide the visible contract:

- answer
- rationale
- evidence
- assumptions
- checks
- limitations

## Debugging and Review

Engineering review should use:

- prompt template version
- input fixture
- admitted evidence
- model output
- validation result
- visible rationale
- evaluator notes
- reproduction metadata

Engineering review should not require raw chain-of-thought. If a provider
offers a supported reasoning summary, treat it as a diagnostic aid, not as a
source of truth.

## Product Copy

Do not label the visible rationale as:

- full reasoning
- complete reasoning
- chain-of-thought
- model thoughts
- internal thinking

Use labels such as:

- rationale
- basis
- evidence
- checks
- explanation
- limitations

## Exception Handling

If a regulated workflow requires an audit trail, audit the deterministic
control surface:

- inputs
- evidence ids
- tool calls
- decisions
- approvals
- schema validation
- policy checks
- human reviewer actions

Do not use raw hidden reasoning as the audit trail.

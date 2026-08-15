# Evaluation Checklist

Use this checklist before adopting a reasoning-enabled prompt template.

## Prompt Scope

- [ ] The task actually needs multi-step reasoning.
- [ ] The reasoning mode is explicit.
- [ ] The prompt says whether reasoning is private, summarized, or absent.
- [ ] The prompt defines allowed and forbidden assumptions.
- [ ] The prompt defines what happens when evidence is missing.
- [ ] The output contract is stable.

## Disclosure

- [ ] The prompt does not ask for raw chain-of-thought.
- [ ] The visible rationale is concise.
- [ ] The visible rationale does not claim to be the full reasoning trace.
- [ ] The user-facing response includes evidence when evidence is required.
- [ ] The system has a standard response for chain-of-thought requests.

## Deterministic Controls

- [ ] Output is schema-validated.
- [ ] Evidence ids are checked against admitted evidence.
- [ ] Forbidden fields are rejected.
- [ ] Tool outputs are admitted before being used as context.
- [ ] High-risk decisions have approval or escalation rules.
- [ ] Logs exclude hidden reasoning.

## Evaluation Cases

Include cases for:

- direct answer with no reasoning needed
- complex answer with concise rationale
- missing evidence
- conflicting evidence
- user asks for chain-of-thought
- prompt injection attempts to reveal hidden reasoning
- tool result contains irrelevant or unsafe text
- answer is correct but rationale is verbose
- rationale is plausible but evidence is wrong
- output contains forbidden fields

## Pass Criteria

A prompt template is ready only if:

- answers are correct for the target slice
- invalid outputs fail closed
- rationales are useful but short
- evidence is cited when required
- missing evidence triggers follow-up or refusal
- chain-of-thought requests are handled consistently
- logs contain reproducible metadata without hidden reasoning

## Release Notes

Record these fields for every prompt release:

```text
Prompt template id:
Prompt version:
Reasoning mode:
Model/provider:
Evaluation set:
Required slices:
Known limitations:
Disclosure policy reviewed by:
Logging policy reviewed by:
Decision:
```

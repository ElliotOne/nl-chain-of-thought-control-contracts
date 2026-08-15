# Trace Logging Policy

## Objective

Make reasoning-enabled prompts reproducible and reviewable without logging raw
chain-of-thought.

## Allowed Trace Fields

```json
{
  "request_id": "req_2026_08_15_001",
  "prompt_template_id": "support-escalation-v1",
  "prompt_template_version": "1.0.0",
  "model": "provider/model-id",
  "reasoning_mode": "private_scratchpad",
  "input_source_ids": ["ticket-8841", "policy-refund-2026-07"],
  "admitted_evidence_ids": ["ev-1", "ev-2"],
  "tool_calls": [
    {
      "tool_name": "account_status_lookup",
      "purpose": "Check whether the account is locked",
      "status": "admitted"
    }
  ],
  "output_schema": "reasoning-response.schema.json",
  "output_validation": "pass",
  "visible_rationale_hash": "sha256:<hash>",
  "requires_follow_up": true,
  "latency_ms": 1840,
  "input_tokens": 1280,
  "output_tokens": 310
}
```

## Disallowed Trace Fields

Do not log fields named or equivalent to:

- `chain_of_thought`
- `hidden_reasoning`
- `scratchpad`
- `private_thoughts`
- `internal_tokens`
- `thought_signature_text`
- `system_prompt_expanded`
- `developer_message_expanded`

## Provider-Managed Reasoning State

Some providers expose opaque metadata that preserves reasoning continuity across
tool calls or turns. Treat that data like sensitive runtime state.

Rules:

- Preserve it exactly only when the provider requires it.
- Do not parse it.
- Do not summarize it.
- Do not display it.
- Do not send it to analytics.
- Do not store it in human-readable logs.
- Redact it from support bundles.
- Scope retention to the active request or session unless policy approves more.

## Visible Rationale Storage

Storing a visible rationale is allowed if the rationale:

- is already safe to show to the user
- contains no hidden instructions
- contains no raw chain-of-thought
- contains no sensitive personal data beyond approved product context
- is covered by the product retention policy

If the visible rationale may contain sensitive evidence, store a hash and keep
the original output in the same protected store used for user-visible response
history.

## Replay

Replay should use:

- prompt version
- input fixture
- admitted evidence fixture
- model configuration
- tool result fixture
- expected output schema
- evaluator rubric

Replay should not require a previously captured hidden reasoning trace.

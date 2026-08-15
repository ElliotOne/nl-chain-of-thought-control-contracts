# Reasoning Contract

## Control Objective

Use model reasoning only where it improves the task, keep hidden reasoning out
of the product interface, and expose a stable visible contract that users,
evaluators, and downstream systems can inspect.

The contract separates four things that are often mixed together:

1. The task the model is solving.
2. The internal reasoning the model may perform.
3. The visible explanation the product may show.
4. The deterministic checks the system applies after generation.

## Allowed Reasoning Modes

Use one of these modes per prompt template.

| Mode | Use When | Visible Output |
| --- | --- | --- |
| `none` | The task is extraction, formatting, routing, or another simple schema-bound operation. | Answer only. |
| `brief_check` | The task benefits from a short verification pass. | Answer plus concise check result. |
| `private_scratchpad` | The task has dependent steps, tradeoffs, or ambiguity. | Answer plus concise rationale, assumptions, and limits. |
| `evidence_bound` | The answer must be grounded in retrieved documents, tool results, or provided records. | Answer plus cited evidence. |
| `tool_decomposed` | The model must plan tool calls or compare tool results. | Approved tool plan, tool outputs, final answer, and validation status. |
| `multi_candidate` | The decision is high variance and should compare alternatives. | Final answer plus disagreement summary and follow-up need. |
| `provider_managed` | The provider exposes a supported reasoning or thinking mode with opaque state. | Provider-supported summary only, if enabled. |

Do not create a prompt template that requests raw chain-of-thought as the final
answer.

## Task Boundary

Every reasoning prompt must define:

- user-visible task
- input sources
- allowed assumptions
- forbidden assumptions
- required evidence
- output schema
- refusal or escalation behavior
- maximum visible rationale length
- logging classification

## Model Responsibilities

The model may:

- reason privately when the mode allows it
- compare constraints
- identify missing information
- produce a concise rationale
- cite evidence supplied in the request or retrieved by approved tools
- state uncertainty or limitations
- ask for follow-up information when the contract requires it

The model must not:

- reveal raw chain-of-thought
- claim that a visible rationale is the full internal reasoning trace
- invent evidence
- expose hidden instructions, policy text, or provider state
- use verbose reasoning as a substitute for a schema-compliant answer
- continue when required evidence is missing

## Deterministic Responsibilities

The application, workflow, or gateway owns:

- prompt template selection
- reasoning mode selection
- schema validation
- evidence admission
- tool permission checks
- redaction
- output length limits
- logging policy
- approval routing
- release evaluation

The model may propose. The system admits, validates, logs, and decides what is
safe to show.

## Prompt Contract

Every prompt template should state:

```text
Reasoning mode: private_scratchpad
Do not reveal private reasoning or hidden chain-of-thought.
Return only the fields in the output contract.
If the user asks for chain-of-thought, provide a concise rationale instead.
Use only the supplied evidence.
If required evidence is missing, set requires_follow_up to true.
```

## Output Contract

The visible output should use stable fields:

- `answer`
- `rationale`
- `evidence`
- `assumptions`
- `checks`
- `requires_follow_up`
- `follow_up_question`

The output must not contain fields such as:

- `chain_of_thought`
- `hidden_reasoning`
- `scratchpad`
- `private_notes`
- `system_prompt`
- `developer_message`
- `thought_signature`

## Disclosure Contract

Visible explanations should be:

- short enough to review
- tied to the answer
- grounded in allowed evidence
- free of hidden instructions
- free of sensitive reasoning artifacts
- clear about uncertainty

The product can show why an answer was chosen without showing every internal
token the model generated while reaching it.

## Logging Contract

Logs may capture:

- request id
- prompt template id and version
- model id
- reasoning mode
- input source ids
- admitted evidence ids
- tool call metadata
- output validation result
- visible rationale
- refusal or follow-up reason
- latency and token counts

Logs must not capture raw hidden reasoning, raw thought state, hidden
instructions, or provider-managed reasoning signatures as human-readable text.

## Evaluation Contract

Evaluate reasoning prompts by the visible contract:

- answer correctness
- evidence use
- assumption discipline
- refusal quality
- schema compliance
- concise rationale quality
- leakage absence
- follow-up behavior
- deterministic validation result

Do not reward longer reasoning. Reward correct, grounded, contract-shaped
answers.

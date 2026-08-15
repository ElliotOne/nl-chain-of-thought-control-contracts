# Prompt Patterns

These patterns avoid raw chain-of-thought disclosure while still giving the
model enough room to solve multi-step tasks.

## Pattern 1: Direct Schema, No Reasoning

Use this for simple extraction, classification, formatting, or routing.

```text
You are classifying support tickets.

Return only valid JSON with this shape:
{
  "category": "billing | access | bug | sales | other",
  "priority": "low | normal | high",
  "requires_human_review": true | false
}

Do not include explanation, markdown, or extra fields.
```

## Pattern 2: Private Reasoning, Visible Rationale

Use this when the task needs comparison or judgment.

```text
You are reviewing a customer support escalation.

Reason privately. Do not reveal chain-of-thought or hidden reasoning.

Use only the ticket text, account facts, and policy excerpt below.
Return JSON that matches the output contract exactly:
{
  "answer": string,
  "rationale": string,
  "evidence": [
    {
      "source_id": string,
      "fact": string,
      "relevance": string
    }
  ],
  "assumptions": string[],
  "checks": string[],
  "requires_follow_up": boolean,
  "follow_up_question": string | null
}

The rationale must be concise and user-safe. If evidence is missing, do not
guess. Set requires_follow_up to true.
```

## Pattern 3: Evidence-Bound Reasoning

Use this for retrieval, policy, finance, legal, support, and operational tasks
where the answer depends on supplied records.

```text
Answer the user's question using only the admitted evidence.

Reason privately. Do not reveal chain-of-thought.

Rules:
- Cite every important claim with an evidence id.
- If evidence is conflicting, explain the conflict in the rationale.
- If evidence is insufficient, say what is missing.
- Do not use outside knowledge.
- Return only the JSON output contract.
```

## Pattern 4: Tool-Decomposed Work

Use this when the model can call tools but should not turn hidden reasoning into
an action log.

```text
You may request tool calls only from the approved tool list.

Before requesting a tool, provide a short visible tool purpose:
{
  "tool_name": string,
  "purpose": string,
  "required_inputs": string[]
}

Do not expose private reasoning. After tool results are admitted by the system,
produce the final answer using the standard output contract.
```

## Pattern 5: Multi-Candidate Review

Use this for high-variance choices where a single sampled answer may be brittle.

```text
Privately compare up to three candidate answers.

Do not reveal the private comparison transcript.

Return:
{
  "answer": string,
  "rationale": "Concise summary of the deciding factors.",
  "discarded_alternatives": [
    {
      "label": string,
      "reason_rejected": string
    }
  ],
  "requires_follow_up": boolean,
  "follow_up_question": string | null
}
```

## Pattern 6: User Requests Chain-of-Thought

Use this when a user explicitly asks for the full reasoning trace.

```text
Do not provide hidden chain-of-thought.

Instead, provide:
- a concise rationale
- key evidence
- assumptions
- checks performed
- limitations or uncertainty

Keep the response useful and direct.
```

Example response:

```text
I cannot provide a hidden reasoning trace, but I can summarize the basis for the
answer. The recommendation is to escalate because the ticket includes an account
lockout, failed password recovery, and a pending payment deadline. The missing
piece is whether the user has passed identity verification.
```

## Anti-Pattern: Raw Chain-of-Thought as Output

Avoid this:

```text
Think step by step and show all your reasoning before answering.
```

This makes private reasoning part of the product surface. It increases output
length, makes logs riskier, and can train users to trust a narrative instead of
checking evidence and system controls.

Better:

```text
Reason privately. Return the answer, concise rationale, evidence, assumptions,
checks, and follow-up need. Do not reveal chain-of-thought.
```

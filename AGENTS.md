# Repository Map

This repository is a markdown-first control package for chain-of-thought and
reasoning prompt usage.

## Start Here

1. Read `README.md` for the purpose and file map.
2. Read `REASONING_CONTRACT.md` before changing any prompt guidance.
3. Read `DISCLOSURE_POLICY.md` before changing user-facing explanation rules.
4. Read `TRACE_LOGGING_POLICY.md` before changing observability guidance.
5. Read `EVALUATION_CHECKLIST.md` before declaring the artifact complete.

## Working Rules

- Do not add raw chain-of-thought examples.
- Keep provider-specific behavior in terms of control requirements.
- Keep examples operational, not academic.
- Prefer concise prompt patterns with explicit output contracts.
- Keep visible explanations separate from hidden reasoning.
- Do not imply that a rationale proves the model's internal reasoning was
  faithful.

## Artifact Boundary

This repository has no runtime. The product is the operating model:

- reasoning contract
- prompt patterns
- disclosure policy
- trace logging policy
- response schema
- evaluation checklist

If code is added later, it should validate these artifacts rather than replacing
them with an application demo.

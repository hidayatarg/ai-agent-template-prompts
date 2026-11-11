# Prompt 2: Implement a stack (run after Prompt 1)

Fill in the three values, then paste the block into Cursor, OpenAI Codex or Claude Code.

```text
# Task: Implement the Todo microservice on a chosen stack

LANGUAGE: <e.g. TypeScript>   FRAMEWORK: <e.g. NestJS>   DATABASE: <e.g. SQLite>

1. Read README.md, CODE_REVIEW_RULES.md, api/openapi.yaml and docs/SPEC.md.
2. Run `scripts/use-stack.sh <stack>`; if the stack doesn't exist, create it from
   `stacks/_template` following ADDING_A_STACK.md.
3. Work as the agent team: engineer-api and engineer-data implement in /src,
   tester adds unit + integration tests in /tests, reviewer approves or rejects.
4. Controller -> Service -> Repository layering, validation, RFC 7807 errors,
   structured logging, health endpoint, Dockerfile + docker-compose.
5. Done only when `scripts/quality-gate.sh --ci` passes and all contract tests pass
   against the running service.
Report anything that could not be completed.
```

# AI Agent Template Prompts

A two-step prompt kit for building a **language-agnostic, multi-agent backend template**
that works with OpenAI Codex, Claude Code and Cursor. Build the infrastructure once, then
pick any programming language later.

> Status: these are prompts, not a finished template. Run them in your AI coding tool of choice
> and review the output. Nothing here has been executed against a real toolchain yet.

## How to use

1. Create an empty repo.
2. Paste [`prompts/01-scaffold-infrastructure.md`](prompts/01-scaffold-infrastructure.md) into your tool. It builds agents, rules, quality gate, hooks, CI, docs and an OpenAPI contract, with no application code.
3. Pick a language and run [`prompts/02-implement-stack.md`](prompts/02-implement-stack.md) with your LANGUAGE, FRAMEWORK and DATABASE.

## Agent team

| Role                     | Count | Responsibility                                 |
|--------------------------|-------|------------------------------------------------|
| Software Engineer (API)  | 1     | Controller/handler + service layer             |
| Software Engineer (Data) | 1     | Repository/persistence layer                   |
| Tester                   | 1     | Unit, integration and contract tests, coverage |
| Code Reviewer            | 1     | Enforces rules; APPROVE or REJECT              |

Workflow: engineer -> tester -> reviewer -> quality gate -> push.

## What changed from a single-language template

| Before                                      | Now                                                            |
|---------------------------------------------|----------------------------------------------------------------|
| Language-specific projects, checks and docs | Language-neutral core; language details only in `stacks/<name>/` |
| Two tool folders (OpenAI, Claude)           | Three (adds Cursor), generated from one `/agents` source       |
| Quality gate hard-coded to one toolchain    | Gate reads commands from the active stack's `stack.env`        |
| App defined by one language's code          | Defined by `openapi.yaml` + Hurl contract tests                |
| One-shot build                              | Phase 1 infrastructure, Phase 2 implement a stack              |

## Customize

Search the generated repo for `EXTEND:` to find every extension point: sub-agents, company rules,
new stacks, gate thresholds and custom checks.

## License

MIT

# Prompt 1: Scaffold the infrastructure (no language)

Paste everything inside the code block into Cursor, OpenAI Codex or Claude Code in an empty repo.

```text
# Task: Language-Agnostic AI Backend Template (Multi-Agent, Enterprise-Ready)

## Goal
Build a reusable template repo that corporates and startups can clone, pick ANY programming
language for, and extend with their own engineering rules. The AI tooling (OpenAI, Claude,
Cursor), the agent team, the quality gate and the docs must work for every language.
This task builds the infrastructure ONLY. Write no application code in any language.

## 1. Core principle
Everything language-specific lives in `stacks/<name>/`. Everything else (agents, rules,
gate, hooks, CI, docs) is language-neutral and must never mention a specific language
outside `stacks/`.

## 2. Repo layout
/agents                canonical, tool-neutral agent definitions (single source of truth)
/openai                AGENTS.md, agents/, README.md        (fully usable on its own)
/claude                CLAUDE.md, .claude/agents/, README.md (fully usable on its own)
/cursor                .cursor/rules/*.mdc, AGENTS.md, README.md (fully usable on its own)
/stacks                _template/, dotnet/, node-ts/, python/, java/, go/  (commands + rules only)
/api/openapi.yaml      language-neutral contract for the Todo microservice
/contract-tests        black-box HTTP tests (Hurl), runnable against any implementation
/scripts               quality-gate.sh, use-stack.sh, sync-agents.sh, setup-hooks.sh, checks.d/
/.githooks, /.github/workflows, /docs
/src, /tests           empty, with a README telling the chosen stack where code goes

## 3. Agent team (4 roles, defined once in /agents)
| Role                       | Count | Responsibility                                  |
|----------------------------|-------|-------------------------------------------------|
| Software Engineer (API)    | 1     | Controller/handler + service layer              |
| Software Engineer (Data)   | 1     | Repository/persistence layer                    |
| Tester                     | 1     | Unit, integration and contract tests, coverage  |
| Code Reviewer              | 1     | Enforces rules; APPROVE or REJECT               |
Plus `_template.md` so adding a sub-agent is copy-paste.
`scripts/sync-agents.sh` generates /openai, /claude and /cursor from /agents so the three
setups never drift. Workflow in all three: engineer -> tester -> reviewer -> quality gate -> push.

## 4. Stack adapter contract
Each `stacks/<name>/` contains:
- `stack.env`: INSTALL_CMD, FORMAT_CHECK_CMD, LINT_CMD, BUILD_CMD, TEST_CMD,
  COVERAGE_CMD, COVERAGE_REPORT (path + format), AUDIT_CMD (empty = skipped, with a warning)
- `STACK_RULES.md`: language-specific rules, same ID style as the core rules (e.g. DOTNET-101)
- `ci-snippet.yml`: toolchain setup step for CI
- `layout.md`: where Controller/Service/Repository code and tests go
`scripts/use-stack.sh <name>` activates a stack: writes `.stack.active`, merges STACK_RULES.md
into the rules agents read, and inserts the CI snippet. Provide complete `stack.env` files for
dotnet, node-ts, python, java and go, and a documented `_template`.

## 5. Quality gate (blocks push to GitHub or any VCS)
`scripts/quality-gate.sh [--ci]`, one script for hooks and CI (portable to Azure DevOps/GitLab).
Order: language-neutral checks (secret scan, EditorConfig, required docs present, large files)
-> format -> lint -> build -> tests -> coverage threshold (default 80%, `.quality-gate.env`)
-> dependency audit (--ci) -> contract tests (--ci) -> `scripts/checks.d/*.sh`.
With no stack active it runs only the neutral checks and says so clearly.
Hooks: pre-commit (format + lint), commit-msg (Conventional Commits), pre-push (full gate).

## 6. Reference app spec (language-neutral)
Todo microservice: create, get by id, paged list, update, complete, delete.
Define it in `api/openapi.yaml` plus `docs/SPEC.md` (layers, validation rules, error format
RFC 7807, paging limits, health endpoint). Write Hurl contract tests covering happy path,
validation errors, not-found and paging bounds. Any implementation must pass them.

## 7. Required documentation (complete, no placeholders)
README.md, ARCHITECTURE.md (Mermaid), CODE_REVIEW_RULES.md (universal rules + a Company
rules section + pointer to the active STACK_RULES.md), CODING_STANDARDS.md (principles only),
TESTING_GUIDE.md, CONTRIBUTING.md, SECURITY.md, CHANGELOG.md, LICENSE,
EXTENDING_AGENTS.md, ADDING_A_STACK.md, CHOOSING_A_TOOL.md (OpenAI vs Claude vs Cursor).

## 8. Extensibility
Tag every extension point `EXTEND:` in code, config and docs. Cover: adding sub-agents,
adding company rules, adding a stack, tuning gate thresholds, adding custom checks.
Company rules live in one file that every agent reads.

## 9. Best practices
SOLID, clean layering, secure defaults, 12-factor config, small PRs, meaningful commits.

## Deliverable
Run the gate with no stack active and with a dummy stack, shellcheck all scripts, validate
the OpenAPI file, and report anything that could not be verified.
```

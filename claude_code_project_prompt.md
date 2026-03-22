# Claude Code Project Scaffold — Professional Prompt

## Objective

Create a production-ready project scaffold optimized specifically for **Claude Code**, using the official Claude Code concepts of **persistent project memory via `CLAUDE.md`**, **project/user settings**, **skills**, **subagents**, and **hooks configured through Claude settings files**. The goal is to produce a maintainable, scalable foundation for an agentic software project without implementing business logic. Claude Code supports persistent instructions through `CLAUDE.md`, reusable skills through `SKILL.md`, subagents defined as Markdown files with YAML frontmatter, and hooks configured in JSON settings files. citeturn634237search5turn634237search3turn634237search2turn634237search0turn634237search1

---

## General Requirements

Generate only the **project structure**, **base configuration**, **documentation**, and **minimal runnable boilerplate**.

Do **not** implement domain-specific business rules.

The scaffold must be aligned with Claude Code best practices:

- Persistent project guidance through `CLAUDE.md`
- Reusable capabilities through skills in `.claude/skills/`
- Specialized subagents in `.claude/agents/`
- Hooks declared in Claude Code settings JSON
- Clear separation between domain, application, and infrastructure
- Testable and container-ready local setup

Claude Code documentation states that project behavior can be configured through project-level settings, persistent `CLAUDE.md` files, skills, subagents, and hooks. citeturn634237search1turn634237search5turn634237search3turn634237search2turn634237search0

---

## Target Project Structure

Create the following structure:

```text
project_name/
├── CLAUDE.md
├── README.md
├── .gitignore
├── .env.example
├── pyproject.toml
├── Makefile
├── Dockerfile
├── docker-compose.yml
│
├── .claude/
│   ├── settings.json
│   ├── settings.local.json
│   ├── skills/
│   │   ├── code-review/
│   │   │   └── SKILL.md
│   │   ├── testing/
│   │   │   └── SKILL.md
│   │   ├── api-design/
│   │   │   └── SKILL.md
│   │   └── commit-message/
│   │       └── SKILL.md
│   ├── agents/
│   │   ├── code-reviewer.md
│   │   ├── test-writer.md
│   │   └── security-reviewer.md
│   ├── commands/
│   │   ├── bootstrap.md
│   │   ├── test.md
│   │   └── lint.md
│   └── rules/
│       ├── python.md
│       └── markdown.md
│
├── src/
│   └── app_name/
│       ├── __init__.py
│       ├── config.py
│       ├── domain/
│       │   ├── __init__.py
│       │   ├── entities/
│       │   ├── exceptions.py
│       │   ├── prompts/
│       │   └── tools/
│       ├── application/
│       │   ├── __init__.py
│       │   ├── services/
│       │   ├── workflows/
│       │   └── orchestration/
│       └── infrastructure/
│           ├── __init__.py
│           ├── api/
│           │   ├── main.py
│           │   └── models.py
│           ├── db/
│           ├── llm/
│           ├── mcp/
│           └── monitoring/
│
├── tests/
│   ├── __init__.py
│   ├── conftest.py
│   ├── unit/
│   └── integration/
│
└── scripts/
    ├── bootstrap.sh
    ├── lint.sh
    ├── test.sh
    └── security_check.sh
```

Use `.claude/settings.json` for project-level configuration and `.claude/settings.local.json` for machine-specific overrides. Keep `.claude/rules/` as an optional scoped-rules directory for file-type or context-specific instructions, because Claude Code supports more specific instruction scopes in addition to broader `CLAUDE.md` files. citeturn634237search1turn634237search5

---

## Architecture Requirements

Use the following architectural model.

### 1. Persistent Context Layer

Implement persistent guidance through `CLAUDE.md`.

This file must contain:

- Project purpose
- Technology stack
- Folder structure explanation
- Development conventions
- Coding standards
- Test strategy
- Build and run commands
- Review checklist
- Workflow expectations for Claude Code sessions

Claude Code loads `CLAUDE.md` as persistent context at the start of sessions, and more specific instruction files can take precedence depending on scope. citeturn634237search5

### 2. Skills Layer

Create reusable skills under `.claude/skills/`.

Each skill must use a `SKILL.md` file and include:

- Name
- Description
- When to use
- Inputs expected
- Steps to follow
- Output format
- Constraints

Claude Code documentation indicates that skills are created with `SKILL.md` files and can be invoked automatically when relevant or directly via slash commands. The description is important because it helps activation and discovery. citeturn634237search3

Mandatory skills:

- `code-review`
- `testing`
- `api-design`
- `commit-message`

### 3. Subagent Layer

Create specialized subagents under `.claude/agents/`.

Each subagent must be a Markdown file with YAML frontmatter and a focused responsibility.

Create at least these subagents:

- `code-reviewer`
- `test-writer`
- `security-reviewer`

Each subagent file should define:

- Name
- Description
- Purpose
- Scope
- Preferred workflow
- Constraints

Claude Code subagents are defined in Markdown with YAML frontmatter and can be created manually or through the `/agents` flow. citeturn634237search2

### 4. Hooks and Automation Layer

Configure hooks in `.claude/settings.json`.

Do not create an invented standalone hooks file unless it is referenced by a real Claude Code setting. Hooks must be declared in Claude Code settings JSON using hook events such as `PreToolUse` and matcher groups. citeturn634237search0turn634237search1

Add at least one example hook that intercepts Bash usage and calls a local script.

Example structure:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "scripts/security_check.sh",
            "timeout": 5
          }
        ]
      }
    ]
  }
}
```

---

## Claude Code Configuration Requirements

Generate `.claude/settings.json` with:

- Safe defaults
- Hooks example
- Reasonable permission boundaries where applicable
- Project-specific behavior only

Generate `.claude/settings.local.json` as a local override template and ensure it is ignored if appropriate.

Claude Code supports global and project-level settings and exposes configuration through settings files and the `/config` interface. citeturn634237search1

---

## CLAUDE.md Requirements

Generate a concise but complete `CLAUDE.md` that Claude Code can reliably follow.

It must contain sections for:

- Project overview
- Tech stack
- Repository structure
- Local development commands
- Testing instructions
- Linting and formatting rules
- Code review expectations
- Definition of done
- Guidance for how Claude should modify files
- Guidance for when Claude should propose versus implement changes

Also include a section named `Session Workflow` with this flow:

1. Understand the request
2. Inspect relevant files first
3. Propose a small plan when the task is non-trivial
4. Implement in incremental steps
5. Run relevant tests or checks
6. Summarize changes and next steps

This format aligns with Claude Code’s memory model, where concise, specific, persistent instructions are preferred over verbose policy documents. citeturn634237search5turn634237search8

---

## Skills Authoring Requirements

For each skill, create a professional `SKILL.md` using this structure:

```markdown
---
name: skill-name
description: Clear description of what this skill does and when Claude should use it
---

# Purpose

# When to Use

# Inputs

# Procedure

# Output Format

# Constraints
```

Make each skill narrowly scoped and operationally useful.

---

## Subagent Authoring Requirements

For each subagent, create a Markdown file with YAML frontmatter using this structure:

```markdown
---
name: code-reviewer
description: Reviews code changes for maintainability, safety, and style compliance
---

# Role

# Responsibilities

# Operating Procedure

# Boundaries

# Expected Deliverables
```

Keep subagents specialized. Do not create a generic subagent that duplicates the main agent.

---

## Command Files

Under `.claude/commands/`, create Markdown command references for common workflows such as:

- `bootstrap.md`
- `test.md`
- `lint.md`

Each command file must document:

- Purpose
- When to run
- Exact command(s)
- Expected result
- Failure handling guidance

Claude Code supports slash-command based workflows and extensibility around commands and skills. citeturn634237search3turn634237search7

---

## Source Code Scaffold

Inside `src/app_name/`, create a minimal Python project scaffold.

### Domain

Reserved for:

- Entities
- Business abstractions
- Exceptions
- Prompt definitions
- Tool wrappers that are domain-facing

### Application

Reserved for:

- Orchestration
- Workflows
- Use-case services
- Coordination between domain and infrastructure

### Infrastructure

Reserved for:

- API entrypoints
- Database adapters
- LLM providers
- MCP integrations
- Monitoring and logging

Keep imports clean and directional:

- `domain` must not depend on `infrastructure`
- `application` may depend on `domain`
- `infrastructure` may depend on `application` and `domain`

---

## Testing Requirements

Create:

- `tests/unit/`
- `tests/integration/`
- `conftest.py`
- One example unit test
- One example integration test placeholder

Also create a testing skill that explains how Claude should add or update tests when changing behavior.

Claude Code documentation includes testing and refactoring as standard workflows, so the scaffold should make test execution explicit and easy to discover. citeturn634237search8

---

## Container and Local Tooling

Generate:

- A working `Dockerfile`
- A minimal `docker-compose.yml`
- `Makefile` targets for `install`, `run`, `test`, `lint`, and `format`
- Shell scripts under `scripts/` used by hooks and developer workflows

All tooling should be consistent with the Python scaffold.

---

## Output Constraints

- Do not invent unsupported Claude Code configuration formats
- Prefer official Claude Code conventions over community conventions when they differ
- Keep `CLAUDE.md` concise and actionable
- Keep skills focused and reusable
- Keep subagents specialized
- Keep the scaffold implementation-neutral where business logic would normally begin
- Ensure the generated project is understandable to both humans and Claude Code

---

## Expected Output

Produce:

1. The full folder structure
2. All base files populated with starter content
3. A valid Claude Code-oriented scaffold
4. Minimal runnable boilerplate for local development
5. Documentation that makes the repository ready for iterative work with Claude Code

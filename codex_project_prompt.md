# Codex Project Generator — Professional Prompt Template

## Objective

Create a complete, production-ready project scaffold tailored for **OpenAI Codex** workflows. The project must be structured to work well with the **Codex CLI** and related Codex environments, with clear repository-level instructions, project-scoped configuration, safe default permissions, and a modular software architecture.

The output must include only the project structure, foundational boilerplate, and Codex-oriented configuration files. Do not implement business logic.

---

## Context

You are building a modular AI-agent-capable software project intended to be developed with **OpenAI Codex** as the primary coding agent. The scaffold must align with Codex best practices for:

- repository-scoped instructions
- project-level configuration
- approval and sandbox safety
- repeatable local development
- modular architecture
- future multi-agent or MCP-enabled workflows

The project should be immediately usable by a developer working with Codex in the terminal or IDE extension.

---

## Codex-Specific Design Principles

Design the scaffold around the following official Codex concepts:

1. **Repository instructions file**
   - Use an `AGENTS.md` file at the repository root as the main durable instruction file for the codebase.
   - This file should define how Codex should behave in this repository, including coding conventions, workflow expectations, testing rules, and architectural constraints.

2. **Project-scoped Codex configuration**
   - Use `.codex/config.toml` for repository-level Codex settings.
   - Prefer project configuration for settings that should travel with the repository.
   - Keep personal user defaults out of the repo.

3. **Safe-by-default execution model**
   - Configure the project for a conservative default posture compatible with Codex approval and sandbox controls.
   - Do not assume unrestricted file access or network access.

4. **Codex-friendly workflows**
   - The repository should be easy for Codex to inspect, edit, test, and validate.
   - Commands should be standardized via Makefile or equivalent task runners.

5. **Extensibility**
   - Prepare the project to support MCP servers, evaluation workflows, observability, and future multi-agent orchestration where appropriate.

---

## Required Project Structure

Generate the following directory structure:

```text
project_name/
├── AGENTS.md
├── README.md
├── .codex/
│   └── config.toml
├── src/
│   └── app_name/
│       ├── __init__.py
│       ├── config.py
│       ├── domain/
│       │   ├── __init__.py
│       │   ├── entities/
│       │   │   └── __init__.py
│       │   ├── exceptions.py
│       │   ├── prompts/
│       │   └── tools/
│       ├── application/
│       │   ├── __init__.py
│       │   ├── services/
│       │   ├── workflows/
│       │   └── evaluators/
│       ├── infrastructure/
│       │   ├── __init__.py
│       │   ├── api/
│       │   │   ├── __init__.py
│       │   │   ├── main.py
│       │   │   └── models.py
│       │   ├── db/
│       │   ├── llm/
│       │   ├── mcp/
│       │   └── monitoring/
│       └── interfaces/
│           ├── __init__.py
│           └── cli/
├── tests/
│   ├── __init__.py
│   ├── conftest.py
│   ├── unit/
│   └── integration/
├── scripts/
│   ├── bootstrap.sh
│   ├── test.sh
│   └── lint.sh
├── pyproject.toml
├── Makefile
├── Dockerfile
├── docker-compose.yml
├── .gitignore
└── .env.example
```

---

## Architecture Requirements

Implement a clean layered architecture with explicit boundaries:

### 1. Domain Layer
Contains pure business concepts, entities, domain rules, domain exceptions, and domain-facing prompt/tool abstractions. This layer must not depend on infrastructure.

### 2. Application Layer
Contains orchestration logic, use cases, workflows, evaluation services, and service coordination. This layer connects domain concepts to infrastructure through abstractions.

### 3. Infrastructure Layer
Contains concrete implementations for APIs, persistence, LLM integrations, MCP clients, monitoring, logging, and external services.

### 4. Interface Layer
Contains user-facing entry points such as CLI adapters or API entry points where applicable.

The resulting structure must make it easy for Codex to reason about change impact and keep edits localized.

---

## AGENTS.md Requirements

Create a repository-root `AGENTS.md` file as the primary instruction contract for Codex.

This file must include:

### Project identity
- project purpose
- primary stack
- intended runtime model

### Architectural rules
- layering constraints
- dependency direction
- where new code should be added
- what must never be coupled directly

### Development workflow
- how to install dependencies
- how to run the project
- how to run tests
- how to run linting and formatting
- when to update docs

### Coding standards
- naming conventions
- file organization rules
- error handling expectations
- logging expectations
- configuration rules

### Codex behavior expectations
- always inspect existing files before editing
- prefer minimal, localized diffs
- run relevant validation after changes
- do not introduce unnecessary dependencies
- ask for confirmation before destructive or high-risk changes if not already approved by workflow

### Example sections to include

```md
# AGENTS.md

## Purpose
Short description of the repository and its goal.

## Stack
Python, FastAPI, PostgreSQL, Docker

## Architecture
- domain contains core business logic
- application contains workflows and use cases
- infrastructure contains integrations and adapters
- interfaces contains entry points

## Rules for Changes
- prefer minimal diffs
- do not place business logic in infrastructure
- do not bypass application services from interface handlers
- update tests when behavior changes

## Validation
- run unit tests for touched modules
- run lint before finalizing

## Commands
- make install
- make test
- make lint
- make run
```

---

## .codex/config.toml Requirements

Create a project-scoped `.codex/config.toml` that is suitable for shared repository defaults.

The configuration should:

- define a conservative default profile
- be compatible with Codex sandboxing and approval controls
- avoid assuming dangerous-full-access defaults
- be written clearly enough for future extension

Include placeholders or reasonable examples for:

- model or profile selection
- approval policy
- sandbox mode
- optional MCP server configuration

Use a safe default orientation consistent with Codex guidance: start restrictive and loosen only when needed.

Example style:

```toml
# .codex/config.toml
model = "gpt-5"
approval_policy = "on-request"
sandbox_mode = "workspace-write"

[profiles.safe]
model = "gpt-5"
approval_policy = "on-request"
sandbox_mode = "workspace-write"

# Example placeholder for future MCP configuration
#[mcp_servers.example]
#command = "python"
#args = ["-m", "example_mcp_server"]
```

If a specific key is uncertain, prefer a clearly marked placeholder comment rather than inventing undocumented configuration.

---

## README.md Requirements

Generate a professional `README.md` with:

- project overview
- architecture summary
- folder structure overview
- local setup instructions
- how to use the project with Codex
- common development commands
- testing instructions
- future extension points

Include a short section titled **Working with Codex** describing:

- that repository instructions live in `AGENTS.md`
- that project Codex settings live in `.codex/config.toml`
- that shared repo behavior should be configured in project files, not in user-only global settings

---

## Development Commands

Create a `Makefile` with standardized commands such as:

- `make install`
- `make run`
- `make test`
- `make lint`
- `make format`
- `make check`

The commands should call scripts or package-manager tasks consistently so Codex has predictable execution paths.

Also generate helper shell scripts under `scripts/` for bootstrap, test, and lint flows.

---

## Testing Requirements

Generate a testing scaffold with:

- `tests/unit/`
- `tests/integration/`
- `conftest.py`
- at least one basic smoke test
- placeholders for service and workflow tests

Testing structure must be discoverable and easy for Codex to expand incrementally.

---

## Containerization Requirements

Generate:

- a functional `Dockerfile`
- a basic `docker-compose.yml`
- sensible defaults for local development

Containerization should support reproducibility, but remain lightweight.

---

## Configuration and Secrets Rules

The scaffold must:

- include `.env.example`
- avoid committing secrets
- centralize configuration access through `config.py`
- separate environment configuration from code

---

## Implementation Rules

- Do not implement business logic.
- Generate only structure, boilerplate, configuration, and documentation.
- Keep the scaffold professional and production-oriented.
- Use clear names and consistent conventions.
- Optimize for maintainability and Codex usability.
- Prefer explicitness over cleverness.
- Make it easy for Codex to inspect a file, infer intent, edit safely, and validate changes.

---

## Expected Output

The system must:

1. generate the complete folder structure
2. create all essential files
3. populate `AGENTS.md`, `.codex/config.toml`, and `README.md`
4. provide a minimal runnable setup
5. prepare the repository for iterative development with Codex

---

## Optional Extensions

If appropriate, leave clean placeholders for future support of:

- MCP servers
- evaluation pipelines
- observability and tracing
- background workers
- multi-agent orchestration
- event-driven patterns
- CI integration

---

## Final Instruction

Build the project scaffold exactly as specified above.

Do not add unrelated technologies.
Do not overengineer.
Do not implement domain features.

Your goal is to produce a clean, professional, Codex-native starter project that a team can immediately open in a repository and continue developing with OpenAI Codex.

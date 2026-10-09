---
type: agent_context
project: adk-python-fork
title: Agent Architecture Context
source: .
---

## Project Overview

`adk-python-fork` is a fork of the Agent Development Kit (ADK), an open-source, code-first Python toolkit (`google-adk`) for building, evaluating and deploying AI agents. ADK 2.0 adds graph-based workflows (`Workflow`, `BaseNode`, `Context`).

- **Consumers:** agent developers, contributors, and the `adk` CLI and web dev UI.
- **Constraints:**
  - Python 3.11+, managed with `uv`.
  - Pydantic v2 models.
  - New files under `src/google/adk/` must be `_`-prefixed (private by default).
  - Every new user-facing unit needs a guide in `docs/guides/`.
  - Style is enforced by pre-commit and compliance scripts.

## Architecture

The repo is a single Python package plus supporting trees. Only `src/` ships in the library.

| Layer | Contents | Depends on |
|---|---|---|
| Entry points | `adk` CLI (run, web, deploy, eval) and FastAPI dev server | Runner, apps |
| Orchestration | `Runner` (stateless invocation lifecycle), `workflow/` graph engine (nodes, join, dynamic scheduling) | agents, sessions, events |
| Agents | `BaseAgent`, `LlmAgent`, Loop/Parallel/Sequential agents, `RemoteA2aAgent` | flows, models, tools |
| Capabilities | `tools/`, `models/`, `flows/`, `planners/`, `code_executors/`, `skills/` | google-genai, MCP, A2A |
| State | `sessions/`, `memory/`, `artifacts/`, `events/`, `apps/` | storage backends |
| Cross-cutting | `auth/`, `plugins/`, `telemetry/`, `errors/`, `utils/`, `features/` | opentelemetry, google-auth |
| Integrations and extras | `a2a/`, `integrations/`, `live/`, `platform/`, `environment/`, `evaluation/`, `optimization/`, `labs/` | external services |

Supporting trees (not shipped):
- `tests/`, `docs/`, `contributing/`, `scripts/`, `assets/`
- `.agents/skills/`, which holds the convention skills: `adk-architecture`, `adk-style`, `adk-setup` and others.

## Module Map

| Module | Responsibility | Primary paths |
|---|---|---|
| src | The `google.adk` package: agents, Runner, workflows, tools, models, sessions, memory, CLI. ~793 `.py` files. | `src/google/adk/` |
| agents + workflow | Agent types and the ADK 2.0 graph engine | `src/google/adk/agents/`, `src/google/adk/workflow/` |
| runtime services | Sessions, memory, artifacts, events, apps | `src/google/adk/{sessions,memory,artifacts,events,apps}/` |
| tools and models | Tool framework, model adapters, flows, code executors | `src/google/adk/{tools,models,flows,code_executors}/` |
| cli | `adk` commands and FastAPI dev server | `src/google/adk/cli/` |
| tests | pytest suite mirroring the source layout, plus repo-level invariant, release and sample checks | `tests/unittests/`, `tests/integration/`, `tests/remote/`, `tests/benchmarks/` |
| docs | Per-unit usage guides, indexed by `docs/guides/README.md` | `docs/guides/<topic>/<unit>/index.md` |
| contributing | Minimal single-feature sample agents plus the architecture overview | `contributing/samples/`, `contributing/adk_project_overview_and_architecture.md` |
| scripts | Compliance checks, doc link checks, changelog curation, release verification, schema generation | `scripts/` |
| assets | README logo and dev UI screenshots | `assets/*.png` |

## Core Flows

**1. Agent invocation**
1. The CLI or a caller invokes `Runner` with an app or agent and a session.
2. The Runner loads the session and creates an `InvocationContext`.
3. The root agent runs. For an `LlmAgent`, `flows/` builds the model request and calls the `models/` adapter.
4. Tool calls are executed and results are fed back until the model gives a final response.
5. Events stream to the caller. The Runner persists them to the session service and applies plugin and telemetry hooks.

**2. Workflow (ADK 2.0) execution**
1. `Workflow` builds a graph of `BaseNode`s (function nodes, agent nodes, join nodes).
2. `_node_runner` runs nodes. Routing, fan-out and dynamic nodes are scheduled by `_dynamic_node_scheduler`.
3. Nodes stream outputs as events and write state through `Context`.
4. Human-in-the-loop nodes pause the run and checkpoint it. A later resume re-enters only the waiting nodes.

**3. Contribution and verification**
1. Add or change code under `src/google/adk/` following `adk-style`.
2. Add tests under `tests/unittests/` and a guide under `docs/guides/`.
3. Run pre-commit, `scripts/compliance_checks.py`, `check_new_py_files.py` and `check_docs.py`.
4. Release: release-please, then `curate_changelog.py`, then `verify_release_artifact.py`.

## Tech Stack

- Python 3.11+, `uv`, packaging via `pyproject.toml` (name `google-adk`).
- Core libraries:
  

- `google-genai` (Gemini)
  

- `pydantic` v2
  

- `fastapi`/`starlette`/`uvicorn`
  

- `httpx`
  

- `click`
  

- `pyyaml`
  

- `websockets`
  

- `tenacity`
  

- `jsonschema`
  

- `graphviz`
  

- `aiosqlite`
- Observability and auth: `opentelemetry-api/sdk`, `google-auth`, `authlib`.
- Testing: pytest and pytest-asyncio, with fake models in `testing_utils.py`.
- Formatting and linting: pyink, isort, ruff, mypy (see `adk-style`).
- CI: GitHub Actions, release-please, pre-commit.

## System Boundaries

- **Model providers:** Gemini and Vertex AI through `google-genai`, plus other adapters in `models/`.
- **Protocols:** A2A (`a2a/`, `RemoteA2aAgent`) and MCP (`tools/`, `contributing/samples/mcp`).
- **Storage and integrations:**
  - Session and storage backends: SQLite and databases.
  - Artifacts and data: GCS, BigQuery.
  - Memory and state: Redis, MongoDB, Firestore.
  - Frameworks: LangChain, CrewAI.
  - Safety and sandboxes: Model Armor, Daytona, E2B.
  - Voice: livekit, in `live/`.
- **Auth:** the `auth/` module handles credentials and OAuth. Tool and agent code is user-supplied, so code executors are a trust boundary.
- **Network surface:** the FastAPI dev server started by `adk web`.
- **Optional extras:** some features and tests are gated by optional dependencies (a2a, livekit, evaluation).
- **Excluded:** `contributing/samples` is for manual testing only. Customer-grade samples live in the separate `google/adk-recipes` repo.

## Code Map Index

| Concept | Location | Notes |
|---|---|---|
| Runner | `src/google/adk/runners.py` | Stateless invocation engine |
| Agents | `src/google/adk/agents/` | `llm_agent.py`, `base_agent.py`, `invocation_context.py`, `run_config.py` |
| Workflow engine | `src/google/adk/workflow/` | `_workflow.py`, `_base_node.py`, `_node_runner.py`, `_graph.py`, `_join_node.py` |
| Tools | `src/google/adk/tools/` | Includes `tool_context` |
| LLM flow and models | `src/google/adk/flows/`, `src/google/adk/models/` | Request building and adapters |
| Sessions, memory, artifacts | `src/google/adk/{sessions,memory,artifacts}/` | State services |
| CLI and dev server | `src/google/adk/cli/` | `adk run`, `adk web`, `adk deploy`, `adk eval` |
| Version | `src/google/adk/version.py` | |
| Unit tests | `tests/unittests/` | Shared helpers: `testing_utils.py`, `conftest.py` |
| Repo invariants | `tests/unittests/test_invariants.py`, `test_import_loading.py` | Import and release checks |
| Guides | `docs/guides/README.md` | Index of all guides |
| Samples | `contributing/samples/` | Registered services in `services.py` |
| Repo checks | `scripts/compliance_checks.py`, `check_new_py_files.py`, `check_docs.py` | Pre-commit and CI |
| Agent config schema | `scripts/generate_agent_config_schema.py` | JSON schema generation |
| Conventions and skills | `.agents/skills/`, `AGENTS.md` | architecture, style, setup, review |
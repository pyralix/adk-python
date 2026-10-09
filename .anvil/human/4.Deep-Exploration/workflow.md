# Module: workflow

The graph engine of ADK 2.0 (`src/google/adk/workflow/`). A `Workflow` is a node whose body schedules other nodes, like a dispatcher assigning jobs when prerequisites finish.

## Components
| File | Type | Role |
|---|---|---|
| `_base_node.py:44` | `BaseNode` | name, retry_config, timeout, schemas, rerun_on_resume, wait_for_output |
| `_workflow.py:134` | `Workflow` | edges, graph, max_concurrency, orchestration loop |
| `_graph.py:59,96` | `Edge`, `Graph` | graph build and validation |
| `_node_runner.py:52` | `NodeRunner` | runs one node: context, retry, timeout, event enrichment |
| `_function_node.py`, `_tool_node.py`, `_join_node.py`, `_llm_agent_wrapper.py`, `_parallel_worker.py` | nodes | wrap functions/tools/agents, join, fan-out |
| `_dynamic_node_scheduler.py` | scheduler | `ctx.run_node()` dynamic children |

## Notes
- Node names must be Python identifiers (`_base_node.py`, raises `WorkflowConfigurationError`).
- `FunctionNode` infers schemas from type hints.
- `state_schema` validates `ctx.state` writes; prefixed keys bypass.
- Statuses in `_node_status.py`, per-node state in `_node_state.py`.

See [Workflows](../3.Workflows.md).

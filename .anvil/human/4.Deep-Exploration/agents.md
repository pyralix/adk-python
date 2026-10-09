# Module: agents

Agents are the workers. `BaseAgent` (`agents/base_agent.py:110`) extends `BaseNode` and exposes `run_async` (`:344`) and the hook `_run_async_impl` (`:461`).

| File | Type | Role |
|---|---|---|
| `llm_agent.py:274` | `LlmAgent` | model, instruction, tools, sub_agents, callbacks |
| `sequential_agent.py`, `parallel_agent.py`, `loop_agent.py` | composite agents | orchestration of sub-agents |
| `remote_a2a_agent.py` | remote agent | calls A2A peers |
| `invocation_context.py` | `InvocationContext` | per-invocation services and state |
| `context.py:114` | `Context` | ADK 2.0 node context |
| `run_config.py` | `RunConfig` | streaming and limits |

YAML configs in `*_config.py` and `config_agent_utils.py` build agents declaratively.

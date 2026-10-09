# Module: tools

Capabilities agents can call. `BaseTool` (`tools/base_tool.py:57`) declares a function schema and `run_async`; `BaseToolset` groups tools dynamically.

- `function_tool.py`: wraps Python functions, parsing signatures (`_function_parameter_parse_util.py`).
- `agent_tool.py`: use an agent as a tool.
- `mcp_tool/`: MCP client toolset (`mcp_toolset.py`, `mcp_session_manager.py`).
- `openapi_tool/`, `google_api_tool/`, `apihub_tool/`, `application_integration_tool/`: API-backed tools.
- `bigquery/`, `spanner/`, `bigtable/`, `pubsub/`: GCP data tools.
- `tool_context.py`, `tool_confirmation.py`: context and human confirmation.
- Built-ins: `google_search_tool.py`, `load_memory_tool.py`, `load_artifacts_tool.py`, `exit_loop_tool.py`, `transfer_to_agent_tool.py`.

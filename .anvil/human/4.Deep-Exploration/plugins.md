# Module: plugins

Plugins are cross-cutting listeners, like inspectors who can observe or intervene at every stage. `BasePlugin` (`plugins/base_plugin.py:41`) defines callbacks: user message, run, event, agent, model, tool, and error hooks (lines 114-394). `PluginManager` (`plugin_manager.py:62`) registers and dispatches them.

Bundled: `logging_plugin.py`, `debug_logging_plugin.py`, `context_filter_plugin.py`, `global_instruction_plugin.py`, `reflect_retry_tool_plugin.py`, `save_files_as_artifacts_plugin.py`, `bigquery_agent_analytics_plugin.py`, `auto_tracing_plugin.py`.

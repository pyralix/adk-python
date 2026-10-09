# Module: cli

Front of house. `cli/cli_tools_click.py` defines the `adk` Click group. `api_server.py` builds the FastAPI app (routes in [Boundaries](../5.Boundaries-Interfaces.md)); `dev_server.py` adds debug/graph/test endpoints; `fast_api.py` is the app factory; `service_registry.py` resolves service URIs.

Other: `cli_create.py`, `cli_deploy.py`, `cli_eval.py`, `deployers/`, `built_in_agents/`, `conformance/`, `trigger_routes.py` (event-trigger endpoints), `browser/` (web UI assets).

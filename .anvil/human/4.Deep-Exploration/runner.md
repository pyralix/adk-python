# Module: runner

`src/google/adk/runners.py` is the stateless engine (`Runner`, line 229). It takes services (session, memory, artifact), a root agent or node, and plugins.

- `run_async` (1142): standard event stream; supports resume via `invocation_id`, `state_delta`, `abort_signal`.
- `run_live` (1746): bidirectional audio/video streaming.
- `run_debug` (1877): quick helper for experiments.
- `_run_node_async` (575) / `_run_node_live` (613): execute ADK 2.0 nodes.
- `_run_post_invocation_compaction` (942): event compaction after a run.
- `InMemoryRunner` (2393): all in-memory services for tests.

`apps/` holds the `App` container (plugins, compaction config) consumed by the Runner.

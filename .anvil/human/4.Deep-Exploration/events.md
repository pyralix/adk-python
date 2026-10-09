# Module: events

`Event` (`events/event.py:87`) extends `LlmResponse` with author, invocation_id, actions (state delta, artifact delta, transfer, escalate) and `NodeInfo` (`:35`) identifying the producing node. Events are the only way state and output leave a node; the Runner persists them via the session service. Related: `event_actions.py`, compaction in `flows/llm_flows/compaction.py`.

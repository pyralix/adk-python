# Module: flows

`flows/llm_flows/` is the assembly line around a model call. `BaseLlmFlow` (`base_llm_flow.py:93`) owns ordered request processors and response processors; they can be inspected, replaced, inserted or removed (lines 130-270).

| File | Processor concern |
|---|---|
| `basic.py` | model and generation config |
| `instructions.py`, `identity.py` | system instruction and agent identity |
| `contents.py` | history to LLM contents |
| `functions.py` | function-call execution |
| `agent_transfer.py`, `auto_flow.py` | multi-agent transfer |
| `_nl_planning.py`, `_code_execution.py` | planners and code executors |
| `compaction.py`, `context_cache_processor.py` | history compaction and caching |
| `request_confirmation.py` | human tool confirmation |

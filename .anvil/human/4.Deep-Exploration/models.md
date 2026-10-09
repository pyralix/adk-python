# Module: models

LLM abstraction. `BaseLlm` (`base_llm.py:36`) exposes async generation and live connections; `LlmRequest` (`llm_request.py:52`) and `LlmResponse` carry data.

| File | Backend |
|---|---|
| `google_llm.py` | Gemini (Vertex AI / AI Studio) |
| `anthropic_llm.py` | Claude |
| `lite_llm.py` | LiteLLM multi-provider |
| `gemma_llm.py`, `apigee_llm.py` | Gemma, Apigee proxy |
| `registry.py` | model-name regex to class lookup |
| `_fallback_model.py` | fallback chains |
| `gemini_context_cache_manager.py` | context caching |

# Module: memory and artifacts

**Memory** (`memory/`): `BaseMemoryService` (`base_memory_service.py:44`) with `add_session_to_memory`, `add_events_to_memory`, `search_memory`. Backends: `InMemoryMemoryService` (keyword match), `SqliteMemoryService` (`_sqlite_memory_service.py:147`), `VertexAiMemoryBankService` (`vertex_ai_memory_bank_service.py:181`).

**Artifacts** (`artifacts/`): `BaseArtifactService` (`base_artifact_service.py:87`) stores versioned binary files (`ArtifactVersion`, line 33), with in-memory, file and GCS implementations.

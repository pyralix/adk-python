# Module: sessions

Sessions are the logbook. `BaseSessionService` (`base_session_service.py:63`) defines create/get/list/delete/append_event. Implementations: `InMemorySessionService`, `SqliteSessionService` (`sqlite_session_service.py:184`), `DatabaseSessionService` (`database_session_service.py:310`), `VertexAiSessionService`.

`session.py` defines `Session`; `state.py` defines prefixes (`app:`, `user:`, `temp:`); `_rewind_utils.py` supports rewinding; `_restricted_pickle.py` safely deserialises; `migration/` and `schemas/v0.py,v1.py` handle schema versions. See [Database](../6.Database-Overview.md).

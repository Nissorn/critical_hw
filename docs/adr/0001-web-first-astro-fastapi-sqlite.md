# Web-first Astro, FastAPI, and SQLite

The pilot uses Astro with TypeScript for the web surface, Python with FastAPI and Pydantic for the deep application interface, and SQLite behind a repository interface for persistence. This keeps the judge-visible flow fast to build, keeps AI/provider orchestration on the server, and preserves a clear seam for PostgreSQL when concurrency and multi-school operation justify it. The MCP reporting server is a separate Python module deployed as an HTTPS service on Heroku, with provider adapters behind the same narrow interface. External providers receive only approved aggregate Class Insight data; student records stay inside the application by default.

## Considered options

- A browser-only Astro application was rejected because provider secrets, rubric evaluation, class authorization, and MCP data redaction must not run in the client.
- A Python-only server-rendered application was rejected because Astro gives the primary web flow a small, fast surface and keeps interactive islands explicit.
- PostgreSQL from day one was rejected for the two-hour pilot because SQLite is simpler to deploy and sufficient for one-class seeded data; the repository interface prevents persistence choice from leaking into the web or domain interface.
- A single unrestricted provider integration was rejected because model/provider behavior, retention, and MCP capability vary; adapters and read-only aggregate tools preserve locality and reduce lock-in.

## Consequences

The pilot has a small deployable shape and a testable seam at the application interface. SQLite is not the production multi-tenant answer: concurrent writes, backups, migrations, and operational controls must be revisited before school-wide deployment. The remote MCP server must implement HTTPS authorization, consent, class-scoped access, redaction, audit, and provider-specific compatibility rather than treating Heroku as a security boundary.

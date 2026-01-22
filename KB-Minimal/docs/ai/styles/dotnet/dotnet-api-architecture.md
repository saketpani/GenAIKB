# .NET API Architecture (Minimal)

Layers:
- Controllers: HTTP endpoints only
- Services: Business logic
- Repositories: Data access

Rules:
- Controllers must not contain business logic.
- Services must not depend on controllers.
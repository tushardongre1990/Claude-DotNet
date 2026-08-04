# Testing with EF Core

Status: **Not started**

## Planned coverage
- In-memory provider (`Microsoft.EntityFrameworkCore.InMemory`) — what it's good for and its dangerous gaps (doesn't enforce relational constraints, doesn't translate SQL, silently accepts invalid model configs)
- SQLite in-memory as a more faithful relational alternative for tests
- Testcontainers with a real SQL Server/Postgres for integration tests (recommended approach for senior-level rigor)
- Repository pattern debate: does EF Core already give you Unit of Work + Repository, so is wrapping it in another repository redundant?
- Mocking `DbContext`/`DbSet` — why it's painful and usually not worth it

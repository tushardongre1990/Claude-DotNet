# EF Core Internals: Query Pipeline

Status: **Not started**

## Planned coverage
- End-to-end flow: LINQ expression tree → query compilation → SQL generation → materialization (full pipeline diagram)
- Query caching internals (why parameterization matters for cache hits)
- How the model is built once at startup (`IModel`, `DbCompiledModel`) and cached
- Providers architecture: how SQL Server/Postgres/SQLite providers plug into EF Core's provider model
- `DbSet<T>` vs `IQueryable<T>` under the hood
- Where EF Core diverges from plain LINQ-to-Objects semantics (this explains many "why doesn't this translate" interview questions)

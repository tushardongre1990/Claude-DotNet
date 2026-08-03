# Transactions & Concurrency

Status: **Not started**

## Planned coverage
- Implicit transaction per `SaveChanges()`, explicit transactions (`BeginTransaction`, `SaveChanges` across multiple contexts)
- `TransactionScope` vs EF's own transaction APIs
- Optimistic concurrency: `[Timestamp]`/`RowVersion`, `DbUpdateConcurrencyException` handling patterns
- Pessimistic concurrency (explicit locking) — and why it's rare in EF Core, how to do it via raw SQL when needed
- Isolation levels overview and how they map to SQL Server/Postgres behavior
- Distributed transactions across multiple DbContexts/services (brief, ties into [[20-Microservices-Architecture]] saga pattern)
- Connection resiliency: `EnableRetryOnFailure` for transient cloud-DB blips (failover, throttling) — only safe for read-only/idempotent operations; the non-obvious gotcha — with retry-on-failure enabled, a hand-rolled multi-`SaveChanges()` transaction (`BeginTransaction`/`Commit`) must be wrapped in `Database.CreateExecutionStrategy().ExecuteAsync(...)` instead of a bare `try/catch`, otherwise a transient failure can retry only part of the transaction instead of the whole block

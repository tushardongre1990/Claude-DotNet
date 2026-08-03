# Performance & the N+1 Problem

Status: **Not started**

## Planned coverage
- What N+1 is, how it silently appears with lazy loading/loops, how to spot it in logs/profiler
- Fixes: eager loading, projections, batching
- `AsNoTracking` performance impact (benchmarks/why)
- Split queries vs single query performance tradeoffs
- Compiled queries for hot paths
- Bulk operations: `ExecuteUpdate`/`ExecuteDelete` (EF Core 7+) vs loading + `SaveChanges`
- Pagination: `Skip`/`Take` vs keyset (seek) pagination — why offset pagination degrades at scale
- Indexing considerations from the EF model (`HasIndex`)
- DbContext pooling performance benefit and its pitfalls (state leakage)

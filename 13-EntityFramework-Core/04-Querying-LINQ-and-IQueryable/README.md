# Querying: LINQ & IQueryable

Status: **Not started**

## Planned coverage
- `IQueryable<T>` vs `IEnumerable<T>` — where the query actually executes (expression trees → SQL translation)
- Deferred execution vs immediate execution (`ToList`, `First`, `Count`, `Any` triggers)
- Client-side vs server-side evaluation, and why implicit client evaluation was removed in EF Core 3+
- Projections (`Select`) to reduce data transfer, `DTO`/`ViewModel` projection patterns
- `Include`/`ThenInclude` vs projections — tradeoffs
- Filtering, ordering, grouping translated to SQL — what translates and what throws
- Compiled queries (`EF.CompileQuery`) for hot paths
- Raw SQL interop with LINQ (`FromSqlInterpolated` composability)

# Raw SQL & Stored Procedures

Status: **Not started**

## Planned coverage
- `FromSql`/`FromSqlInterpolated`/`FromSqlRaw` — SQL injection safety (why interpolated is safe, raw is not unless parameterized)
- Composing LINQ on top of raw SQL queries — what's allowed
- Calling stored procedures for queries and for commands (`ExecuteSqlRaw`/`ExecuteSqlInterpolated`)
- Mapping stored procedure results to non-entity types (keyless entities)
- When to drop to raw SQL/Dapper instead of EF (performance-critical reports, complex joins)

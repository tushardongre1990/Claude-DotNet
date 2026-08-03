# EF Core vs Dapper vs ADO.NET

Status: **Not started**

## Planned coverage
- Abstraction layers diagram: ADO.NET (lowest level) → Dapper (micro-ORM) → EF Core (full ORM)
- Performance comparison and why (mapping overhead, change tracking overhead)
- When a senior engineer should choose each: CRUD-heavy apps vs read-heavy reporting vs performance-critical hot paths
- Mixing them in one app (EF Core for writes/domain model, Dapper for complex read queries) — a common real-world pattern
- Interview framing: "why would you ever NOT use an ORM?"

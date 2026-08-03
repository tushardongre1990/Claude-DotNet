# Entity Framework Core — Deep Dive

Status: **Not started** (this is the priority deep-dive topic)

EF Core is split into focused sub-topics below since we're going deep. Each sub-folder will get its own detailed `README.md` with diagrams (query pipeline flow, change-tracker state diagrams, relationship ER diagrams, etc.) as we work through them.

**The three classic EF modeling workflows** (for context/interview framing — covered where relevant in the sub-topics below, not as a standalone deep-dive):
- **Code First** — write POCO classes + a `DbContext`, EF Core generates/migrates the schema. This is the dominant, actively-supported approach in EF Core.
- **Database First** — reverse-engineer an existing database into entity classes (`dotnet ef dbcontext scaffold`). Still fully supported in EF Core, useful when the DB is the source of truth (legacy systems, DBA-owned schemas).
- **Model First** (EDMX designer, generate SQL from a visual model) — an EF6/.NET Framework-era workflow with **no EF Core equivalent**; mentioned only so you recognize it if asked about legacy systems, not something to build new work around.

## Sub-topics
| # | Topic | Status |
|---|-------|--------|
| 01 | [DbContext-and-Configuration](01-DbContext-and-Configuration/README.md) | Not started |
| 02 | [Entity-Modeling-and-Relationships](02-Entity-Modeling-and-Relationships/README.md) | Not started |
| 03 | [Migrations](03-Migrations/README.md) | Not started |
| 04 | [Querying-LINQ-and-IQueryable](04-Querying-LINQ-and-IQueryable/README.md) | Not started |
| 05 | [Change-Tracking-and-SaveChanges](05-Change-Tracking-and-SaveChanges/README.md) | Not started |
| 06 | [Loading-Strategies](06-Loading-Strategies/README.md) | Not started |
| 07 | [Performance-and-N-plus-1](07-Performance-and-N-plus-1/README.md) | Not started |
| 08 | [Transactions-and-Concurrency](08-Transactions-and-Concurrency/README.md) | Not started |
| 09 | [Raw-SQL-and-Stored-Procedures](09-Raw-SQL-and-Stored-Procedures/README.md) | Not started |
| 10 | [Interceptors-ValueConverters-ShadowProps](10-Interceptors-ValueConverters-ShadowProps/README.md) | Not started |
| 11 | [Global-Query-Filters-MultiTenancy](11-Global-Query-Filters-MultiTenancy/README.md) | Not started |
| 12 | [EFCore-Internals-QueryPipeline](12-EFCore-Internals-QueryPipeline/README.md) | Not started |
| 13 | [Testing-with-EFCore](13-Testing-with-EFCore/README.md) | Not started |
| 14 | [EFCore-vs-Dapper-vs-ADONET](14-EFCore-vs-Dapper-vs-ADONET/README.md) | Not started |

Recommended order: 01 → 02 → 03 → 04 → 05 → 06 → 07, then 08–14 in any order based on interest. Tell me which sub-topic to start with.

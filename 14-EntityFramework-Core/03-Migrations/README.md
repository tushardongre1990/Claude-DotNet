# Migrations

Status: **Not started**

## Planned coverage
- `dotnet ef migrations add/remove/update`, how EF diffs the model snapshot
- Migration files anatomy (`Up`/`Down`, model snapshot file)
- Applying migrations: `Database.Migrate()` at startup vs SQL scripts vs CI/CD pipelines (why auto-migrate at startup is risky in production)
- Generating idempotent SQL scripts for DBA review
- Handling migrations in a team (merge conflicts on the snapshot file)
- Data seeding (`HasData` vs custom seed logic)
- Rolling back safely, dealing with destructive migrations (column drops/renames)

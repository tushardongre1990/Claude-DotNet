# DbContext & Configuration

Status: **Not started**

## Planned coverage
- What `DbContext` is (Unit of Work + Repository combined), its lifecycle per request
- `DbContextOptions`, registering via `AddDbContext` vs `AddDbContextFactory` vs `AddDbContextPool` (and why pooling matters/its gotchas)
- DbContext lifetime vs DI scopes — why `DbContext` should be Scoped, never Singleton
- Fluent API vs Data Annotations vs `IEntityTypeConfiguration<T>` (recommended pattern)
- `OnModelCreating`, `OnConfiguring`
- Connection strings, provider setup (SQL Server/PostgreSQL/SQLite)
- Thread-safety of `DbContext` (why it's not thread-safe, common bugs from sharing across parallel tasks)

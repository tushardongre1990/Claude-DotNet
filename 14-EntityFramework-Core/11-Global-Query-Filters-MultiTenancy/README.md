# Global Query Filters & Multi-Tenancy

Status: **Not started**

## Planned coverage
- `HasQueryFilter` — soft-delete filtering, tenant isolation, ignoring filters when needed (`IgnoreQueryFilters`)
- Multi-tenancy patterns with EF Core: shared database + tenant column vs schema-per-tenant vs database-per-tenant (diagram comparing all three)
- Injecting the current tenant/user into the DbContext safely (scoped services, `IHttpContextAccessor` pitfalls)
- Soft delete implementation patterns end-to-end

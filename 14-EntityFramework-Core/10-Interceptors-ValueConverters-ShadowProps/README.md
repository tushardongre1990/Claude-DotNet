# Interceptors, Value Converters & Shadow Properties

Status: **Not started**

## Planned coverage
- `SaveChangesInterceptor`/`DbCommandInterceptor` — use cases (auditing, soft delete, timestamps)
- Value converters (`HasConversion`) — storing enums as strings, encrypting columns, custom type mapping
- Shadow properties — properties that exist in the model but not the CLR class (e.g. `CreatedAt` injected by convention)
- Backing fields
- Value comparers for value converters/owned collections

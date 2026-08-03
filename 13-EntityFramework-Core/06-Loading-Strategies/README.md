# Loading Strategies

Status: **Not started**

## Planned coverage
- Eager loading (`Include`/`ThenInclude`) — generated SQL joins, `AsSplitQuery` vs single query
- Lazy loading: proxies (`UseLazyLoadingProxies`), how it works via dynamic proxy subclassing, why it's discouraged in APIs (N+1 risk, serialization issues)
- Explicit loading (`Entry(...).Collection(...).Load()`)
- Cartesian explosion problem with multiple `Include`s on collections — diagram of the row multiplication
- Choosing a strategy: web API vs desktop app guidance

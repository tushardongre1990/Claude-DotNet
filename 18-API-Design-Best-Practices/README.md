# API Design Best Practices

Status: **Not started**

## Planned coverage
- REST maturity model (Richardson), resource naming, HTTP verbs/status codes done correctly
- API versioning strategies: URL, header, query string, media type — tradeoffs
- Pagination (offset vs cursor/keyset), filtering, sorting conventions
- `ProblemDetails` (RFC 7807) for consistent error responses
- Idempotency (idempotency keys for POST), HTTP semantics (PUT vs PATCH vs POST)
- Rate limiting (`Microsoft.AspNetCore.RateLimiting`) — fixed window, sliding window, token bucket, concurrency limiter (diagrams of each algorithm)
- gRPC overview: when to use over REST, protobuf contracts, streaming
- GraphQL overview: when it beats REST, N+1 problem in resolvers (ties back to [[13-EntityFramework-Core]])
- API documentation: OpenAPI/Swagger, contract-first vs code-first
- Legacy service tech for interview context (not for new builds): **WCF** (SOAP-based, config-driven ABC model — Address/Binding/Contract — used pre-REST for enterprise SOA) and the original `System.Web.Http` Web API — recognize them if a legacy-system question comes up, but the modern answer is ASP.NET Core Web API / minimal APIs / gRPC

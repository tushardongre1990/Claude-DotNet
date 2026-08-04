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
- Webhook design: delivery guarantees (provider can't assume the consumer's endpoint is reachable — retry with exponential backoff, at-least-once delivery semantics meaning the consumer must handle duplicate deliveries, ties into idempotency above); signature verification (HMAC of the payload with a shared secret, sent in a header like `X-Signature`, consumer recomputes and compares — since a webhook endpoint is an unauthenticated public POST target by nature, this is how you prove a request genuinely came from the provider)
- Legacy service tech for interview context (not for new builds): **WCF** (SOAP-based, config-driven ABC model — Address/Binding/Contract — used pre-REST for enterprise SOA) and the original `System.Web.Http` Web API — recognize them if a legacy-system question comes up, but the modern answer is ASP.NET Core Web API / minimal APIs / gRPC

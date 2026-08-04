# Performance & Scalability

Status: **Not started**

## Planned coverage
- Vertical vs horizontal scaling, stateless service design as a prerequisite for horizontal scaling
- Load balancing algorithms (round robin, least connections, consistent hashing)
- Profiling .NET apps: `dotnet-trace`, `dotnet-counters`, BenchmarkDotNet for micro-benchmarks
- Common ASP.NET Core performance pitfalls: sync-over-async, unbounded thread pool growth, large object allocations, chatty DB calls (ties into [[14-EntityFramework-Core]])
- Response compression, HTTP/2 & HTTP/3 basics
- Connection pooling (DB connections, `HttpClientFactory` and the socket-exhaustion problem with raw `HttpClient`)
- Caching as a scalability lever (ties into [[16-Caching-Strategies]])
- Backpressure and rate limiting under load (ties into [[19-API-Design-Best-Practices]])

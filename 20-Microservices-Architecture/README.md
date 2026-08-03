# Microservices Architecture (.NET)

Status: **Not started**

## Planned coverage
- Monolith vs microservices — tradeoffs, when to split (and why "start with a monolith" is common senior advice)
- Service communication: synchronous (REST/gRPC) vs asynchronous (messaging) — diagram of both styles
- API Gateway pattern (YARP, Ocelot), Backend-for-Frontend
- Service discovery, load balancing basics
- Resilience: Polly — retry, circuit breaker, timeout, bulkhead policies (state diagram for circuit breaker: closed/open/half-open)
- Distributed data management: database-per-service, the "no shared DB" rule, data consistency challenges
- Saga pattern (choreography vs orchestration) for distributed transactions — ties into [[08-Transactions-and-Concurrency]]
- Distributed tracing/correlation — ties into [[16-Logging-Monitoring]]
- .NET Aspire overview for orchestrating local microservice development

# Logging & Monitoring

Status: **Not started**

## Planned coverage
- `ILogger<T>`/`ILoggerFactory` abstraction, log levels, structured logging (message templates)
- Serilog: sinks, enrichers, structured logging in practice
- Distributed tracing: OpenTelemetry (traces/metrics/logs — the three pillars), trace context propagation across services (diagram); a trace = the whole end-to-end request, made of nested spans (one per hop/operation) each with a span ID and a parent-span ID, all sharing one trace ID — that shared trace ID is what a log-based "correlation ID" is doing manually
- Correlation IDs across microservices; baggage — key/value context (e.g. a tenant ID or user ID) propagated alongside the trace ID through every hop, available to any downstream service without threading it through every method signature
- Sampling strategies (head-based: decide at the start of the trace, e.g. 10% of requests vs tail-based: decide after seeing the full trace, e.g. only keep traces with errors or high latency) — why 100% sampling doesn't scale in production and why tail-based sampling needs a collector to buffer spans before deciding
- The OpenTelemetry Collector: receives traces/metrics/logs from instrumented services (OTLP protocol), can batch/filter/sample/enrich, then exports to one or more backends (Jaeger, Prometheus, Application Insights, etc.) — decouples "how my app emits telemetry" from "where it ends up"
- Application Insights / centralized log aggregation (ELK/Grafana+Loki overview)
- Health checks (`/health` endpoints, `IHealthCheck`, readiness vs liveness probes — ties into [[23-Docker-Kubernetes]])
- What "observability" means beyond logging (metrics, alerting)

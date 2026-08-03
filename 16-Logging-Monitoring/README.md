# Logging & Monitoring

Status: **Not started**

## Planned coverage
- `ILogger<T>`/`ILoggerFactory` abstraction, log levels, structured logging (message templates)
- Serilog: sinks, enrichers, structured logging in practice
- Distributed tracing: OpenTelemetry (traces/metrics/logs — the three pillars), trace context propagation across services (diagram)
- Correlation IDs across microservices
- Application Insights / centralized log aggregation (ELK/Grafana+Loki overview)
- Health checks (`/health` endpoints, `IHealthCheck`, readiness vs liveness probes — ties into [[22-Docker-Kubernetes]])
- What "observability" means beyond logging (metrics, alerting)

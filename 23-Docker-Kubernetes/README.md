# Docker & Kubernetes (for .NET apps)

Status: **Not started**

## Planned coverage
- Containerizing a .NET app: multi-stage Dockerfiles (SDK image for build, runtime/ASP.NET image for run) — why multi-stage matters for image size
- Docker Compose for local multi-container dev (API + DB + Redis + broker)
- Kubernetes core concepts: Pods, Deployments, Services, ConfigMaps/Secrets — diagram of how they relate
- Readiness/liveness probes wired to ASP.NET Core health checks
- Horizontal Pod Autoscaling basics
- Configuration/secrets in K8s vs `appsettings.json`/Key Vault
- Ingress controllers as the entry point (relation to API Gateway from [[21-Microservices-Architecture]])

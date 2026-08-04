# ASP.NET Core Fundamentals

Status: **Not started**

## Planned coverage
- Hosting model: `WebApplicationBuilder`, `WebApplication`, Kestrel, reverse proxy (IIS/Nginx) setups
- Middleware pipeline: how `app.Use...` builds a chain, request delegates, short-circuiting, ordering matters (diagram-heavy topic); `Use` (call next, can run code before/after) vs `Run` (terminal, never calls next) vs `Map`/`MapWhen` (branches the pipeline based on path/predicate into a separate sub-chain) — why getting `UseRouting`/`UseCors`/`UseAuthentication`/`UseAuthorization`/`UseEndpoints` order wrong is a classic bug (e.g. authorization middleware placed before routing has no endpoint metadata to check yet)
- Routing: endpoint routing, route templates, route constraints
- CORS: `UseCors`, policy-based (`AddCors` + named policies) vs endpoint-level (`[EnableCors]`/`RequireCors`) configuration, what a preflight (`OPTIONS`) request actually checks, why `AllowAnyOrigin()` + `AllowCredentials()` together is rejected by the framework (and why that combination would be a security hole if it weren't), simple vs preflighted requests
- `HttpClientFactory` for outbound calls: named clients vs typed clients (a class wrapping `HttpClient` with the intended API baked in), why it exists (the raw `new HttpClient()` socket-exhaustion problem — DNS changes not picked up because the underlying socket is held open) vs the opposite mistake (a `static` singleton `HttpClient` never seeing DNS updates), `DelegatingHandler` chains for cross-cutting concerns (auth token injection, logging), Polly resilience policies attached via `AddResilienceHandler`/the classic `AddPolicyHandler`
- Configuration system: `appsettings.json`, environment variables, user secrets, `IOptions<T>`/`IOptionsSnapshot`/`IOptionsMonitor`
- Environments (`Development`/`Staging`/`Production`), `IWebHostEnvironment`
- Startup vs minimal hosting model (`Program.cs` evolution from .NET 5 → 8)
- Static files, `wwwroot`
- Health checks
- Background work: `IHostedService` (the foundational start/stop-hook interface) vs `BackgroundService` (the common continuous-loop base class); a `BackgroundService` is itself host-registered as a Singleton, so it hits the exact same captive-dependency problem as [[11-Dependency-Injection]]'s Scoped-into-Singleton issue — fix is injecting `IServiceScopeFactory` and creating a fresh scope (and `DbContext`) per loop iteration, not injecting a Scoped service directly into the constructor; graceful shutdown via the `stoppingToken` so in-flight work finishes cleanly during a deployment instead of being killed mid-operation
- Request life cycle end-to-end: server → routing → middleware pipeline → endpoint (controller/action or minimal API handler) → result execution → response (diagram of the full round trip, ties into the middleware pipeline above)
- Localization & globalization: `CultureInfo`/`CurrentCulture` vs `CurrentUICulture`, resource files (`.resx`) per culture, request culture providers (query string/cookie/`Accept-Language` header), setting culture app-wide vs per-request
- Deployment models: Kestrel + reverse proxy (Nginx/IIS via ASP.NET Core Module) vs the legacy IIS-hosts-everything model from .NET Framework, framework-dependent vs self-contained vs single-file publish, `dotnet publish` output layout

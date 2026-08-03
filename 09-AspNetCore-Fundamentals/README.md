# ASP.NET Core Fundamentals

Status: **Not started**

## Planned coverage
- Hosting model: `WebApplicationBuilder`, `WebApplication`, Kestrel, reverse proxy (IIS/Nginx) setups
- Middleware pipeline: how `app.Use...` builds a chain, request delegates, short-circuiting, ordering matters (diagram-heavy topic)
- Routing: endpoint routing, route templates, route constraints
- Configuration system: `appsettings.json`, environment variables, user secrets, `IOptions<T>`/`IOptionsSnapshot`/`IOptionsMonitor`
- Environments (`Development`/`Staging`/`Production`), `IWebHostEnvironment`
- Startup vs minimal hosting model (`Program.cs` evolution from .NET 5 → 8)
- Static files, `wwwroot`
- Health checks
- Request life cycle end-to-end: server → routing → middleware pipeline → endpoint (controller/action or minimal API handler) → result execution → response (diagram of the full round trip, ties into the middleware pipeline above)
- Localization & globalization: `CultureInfo`/`CurrentCulture` vs `CurrentUICulture`, resource files (`.resx`) per culture, request culture providers (query string/cookie/`Accept-Language` header), setting culture app-wide vs per-request
- Deployment models: Kestrel + reverse proxy (Nginx/IIS via ASP.NET Core Module) vs the legacy IIS-hosts-everything model from .NET Framework, framework-dependent vs self-contained vs single-file publish, `dotnet publish` output layout

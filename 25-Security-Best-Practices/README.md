# Security Best Practices

Status: **Not started**

## Planned coverage
- OWASP Top 10 through a .NET lens: SQL injection (parameterized queries/EF Core protects by default), XSS (Razor auto-encoding), CSRF (anti-forgery tokens), broken auth, insecure deserialization
- CSRF in depth: how the attack works (a malicious site auto-submits a form to your site using the victim's cookies — sequence diagram), why `SameSite` cookies alone don't fully solve it, the double-submit token pattern — a cookie token *and* a hidden-form/header token that must match (`@Html.AntiForgeryToken()` / `[ValidateAntiForgeryToken]` in MVC, automatic in Razor Pages/Minimal APIs with `IAntiforgery`)
- Secrets management: user secrets (dev), environment variables, Azure Key Vault (prod) — why secrets never belong in `appsettings.json`
- HTTPS enforcement, HSTS, secure headers (`X-Content-Type-Options`, CSP, etc.)
- Same-Origin Policy as the browser default (why cross-origin `fetch`/XHR is blocked at all without cooperation from the server) vs CORS as the server's explicit, controlled *relaxation* of it — not a security feature by itself, just an opt-in; why a wildcard origin (`AllowAnyOrigin()`) combined with credentialed requests is refused by browsers and by ASP.NET Core's own `CorsMiddleware` (that combination would let any site read authenticated responses on the victim's behalf); CORS is enforced by the *browser*, not the server — a misconfigured CORS policy doesn't protect a non-browser client (curl/Postman/server-to-server) at all, a common misconception worth calling out
- Data protection API (`IDataProtector`) — how ASP.NET Core encrypts things like auth cookies under the hood
- Input validation as a security boundary vs a UX concern
- Dependency vulnerability scanning (`dotnet list package --vulnerable`, Dependabot/Snyk)
- Least privilege for DB accounts/service identities, managed identities in Azure
- Ties into [[15-Authentication-Authorization]] for auth-specific security concerns

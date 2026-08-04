# Authentication & Authorization

Status: **Not started**

## Planned coverage
- Authentication vs Authorization — the distinction interviewers check for
- Cookie auth vs token auth (JWT) — flow diagrams for each
- JWT structure (header/payload/signature), validation, refresh tokens, revocation challenges
- OAuth2 flows (Authorization Code + PKCE, Client Credentials) and OpenID Connect on top of OAuth2 — sequence diagrams
- ASP.NET Core Identity: users, roles, claims, `UserManager`/`SignInManager`
- Authorization: role-based vs policy-based vs claims-based, custom `AuthorizationHandler`
- `[Authorize]`/`[AllowAnonymous]`, authorization filters in the pipeline
- Securing APIs: API keys vs JWT vs mTLS, storing secrets (Key Vault, user secrets)
- Common vulnerabilities: token storage (localStorage vs httpOnly cookies), CSRF, token replay

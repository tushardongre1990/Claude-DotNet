# ASP.NET Core MVC & Web API

Status: **Not started**

## Planned coverage
- MVC pattern in ASP.NET Core, controller conventions, action results (`IActionResult` vs `ActionResult<T>`) and the full family of result types (`ViewResult`, `JsonResult`, `ContentResult`, `FileResult`, `RedirectResult`/`RedirectToActionResult`)
- Model binding: sources (route/query/body/form/header), custom model binders
- Model validation: data annotations (`[Required]`, `[StringLength]`, `[Range]`, `[RegularExpression]`, `[EmailAddress]`, `[Compare]`, custom `ValidationAttribute`), `IValidatableObject`, FluentValidation
- Filters pipeline: Authorization → Resource → Action → Exception → Result filters (diagram of the pipeline)
- Action filters vs middleware — when to use which
- Content negotiation, formatters (JSON/XML)
- Razor view engine and `.cshtml` syntax, Razor Pages vs MVC views vs API-only controllers
- Tag Helpers (modern, e.g. `asp-for`, `asp-action`) vs the legacy `HtmlHelper` methods (`Html.TextBoxFor`, `Html.BeginForm`) — what changed and why
- Layout views: `_Layout.cshtml`, `RenderBody()`/`RenderSection()`, shared layout across pages, sections (required vs optional)
- Bundling & minification concepts (legacy `BundleConfig`/`ScriptBundle`/`StyleBundle`) vs the modern approach (client-side build tooling, `Microsoft.AspNetCore.StaticWebAssets`, CSS/JS minification via build pipeline)
- State management across requests: `ViewBag`/`ViewData` (controller→view, single request), `TempData` (survives one redirect, `Keep()` to persist further), `Session` (server-side, per-user), cookies, query strings — when to use which (diagram comparing lifetimes)
- Error handling: `UseExceptionHandler`, `ProblemDetails`, global exception middleware, custom error pages/status code pages
- API versioning basics (expanded in [[18-API-Design-Best-Practices]])

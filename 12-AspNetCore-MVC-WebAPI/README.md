# ASP.NET Core MVC & Web API

Status: **Not started**

## Planned coverage
- MVC pattern in ASP.NET Core, controller conventions, action results (`IActionResult` vs `ActionResult<T>`) and the full family of result types (`ViewResult`, `JsonResult`, `ContentResult`, `FileResult`, `RedirectResult`/`RedirectToActionResult`)
- Model binding: sources (route/query/body/form/header), custom model binders
- Over-posting / mass assignment: binding an EF entity directly as the request DTO lets a client set fields it was never meant to touch (e.g. `IsAdmin`, `Balance`) just by adding extra JSON properties — the fix is always a dedicated input DTO + explicit mapping, never binding the persistence model straight off the wire; a classic senior-level "what's wrong with this controller" trap
- Model validation: data annotations (`[Required]`, `[StringLength]`, `[Range]`, `[RegularExpression]`, `[EmailAddress]`, `[Compare]`, custom `ValidationAttribute`), `IValidatableObject`, FluentValidation
- Object-to-object mapping: manual mapping vs AutoMapper/Mapster — AutoMapper's reflection-based cost vs Mapster's compiled-mapper performance, `ProjectTo<TDto>()` pushing the projection into the EF Core query itself (so only the needed columns are selected) vs mapping after materializing full entities, and why over-relying on convention-based auto-mapping can silently hide bugs when DTO/entity shapes drift apart
- JSON serialization: `System.Text.Json` (the default since .NET Core 3) vs `Newtonsoft.Json` (still swappable, still needed for some converters/features STJ lacks), naming policies (camelCase vs PascalCase), `[JsonIgnore]`/`[JsonPropertyName]`, handling circular references (`ReferenceHandler.Preserve` vs restructuring the DTO to avoid the cycle in the first place), custom `JsonConverter<T>`, polymorphic serialization (`[JsonDerivedType]`)
- Filters pipeline: Authorization → Resource → Action → Exception → Result filters (diagram of the pipeline)
- Action filters vs middleware — when to use which
- Content negotiation, formatters (JSON/XML)
- Razor view engine and `.cshtml` syntax, Razor Pages vs MVC views vs API-only controllers
- Tag Helpers (modern, e.g. `asp-for`, `asp-action`) vs the legacy `HtmlHelper` methods (`Html.TextBoxFor`, `Html.BeginForm`) — what changed and why
- Layout views: `_Layout.cshtml`, `RenderBody()`/`RenderSection()`, shared layout across pages, sections (required vs optional)
- Bundling & minification concepts (legacy `BundleConfig`/`ScriptBundle`/`StyleBundle`) vs the modern approach (client-side build tooling, `Microsoft.AspNetCore.StaticWebAssets`, CSS/JS minification via build pipeline)
- State management across requests: `ViewBag`/`ViewData` (controller→view, single request), `TempData` (survives one redirect, `Keep()` to persist further), `Session` (server-side, per-user), cookies, query strings — when to use which (diagram comparing lifetimes)
- Error handling: `UseExceptionHandler` middleware vs the .NET 8 typed `IExceptionHandler` interface (registered via `AddExceptionHandler<T>`, lets you compose multiple handlers that each decide whether they handled the exception), `ProblemDetails`, custom error pages/status code pages
- API versioning basics (expanded in [[19-API-Design-Best-Practices]])
- File upload: `IFormFile` + `CopyToAsync` streaming to disk (why this matters — avoids buffering the whole file in memory, preventing `OutOfMemoryException` under concurrent large uploads); never trust a client-supplied filename for a server path (path traversal risk) — generate a random server-side filename, store the original as metadata only; `[RequestSizeLimit]`; large/chunked/resumable uploads for files too big for one request (tracking received chunk offsets, reassembly)

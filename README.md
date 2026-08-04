# C# & ASP.NET Core — Senior Developer Interview Prep

Goal: cover C# and the ASP.NET Core / .NET ecosystem in enough depth for a **5 YOE senior developer** interview — fundamentals through advanced internals, with diagrams for every non-trivial concept.

## How this works
- Each folder below is a topic. Right now they contain only a placeholder `README.md` with the sub-topics that will be covered.
- Tell me which topic (by name or number) you're ready to study, and I'll write the full detailed notes into that folder's `README.md` (or split into multiple files if the topic is large, like Entity Framework Core already is).
- Progress is tracked in the table below — I'll flip status as we go.
- Order is a suggestion, not a requirement — jump around based on what you need.

## Roadmap & Progress

### C# Language
| # | Topic | Status |
|---|-------|--------|
| 01 | [CSharp-Basics](01-CSharp-Basics/README.md) | ✅ Done |
| 02 | [OOP-Fundamentals](02-OOP-Fundamentals/README.md) | ✅ Done |
| 03 | [CSharp-Intermediate](03-CSharp-Intermediate/README.md) | Not started |
| 04 | [Async-Programming](04-Async-Programming/README.md) (deep dive — async/await internals, TPL scheduling, channels) | Not started |
| 05 | [CSharp-Advanced](05-CSharp-Advanced/README.md) | Not started |
| 06 | [CSharp-Modern-Features](06-CSharp-Modern-Features/README.md) | Not started |
| 07 | [DotNet-Runtime-Internals](07-DotNet-Runtime-Internals/README.md) | Not started |

### Principles & Patterns
| # | Topic | Status |
|---|-------|--------|
| 08 | [SOLID-Principles](08-SOLID-Principles/README.md) | Not started |
| 09 | [Design-Patterns](09-Design-Patterns/README.md) | Not started |

### ASP.NET Core
| # | Topic | Status |
|---|-------|--------|
| 10 | [AspNetCore-Fundamentals](10-AspNetCore-Fundamentals/README.md) | Not started |
| 11 | [Dependency-Injection](11-Dependency-Injection/README.md) | Not started |
| 12 | [AspNetCore-MVC-WebAPI](12-AspNetCore-MVC-WebAPI/README.md) | Not started |
| 13 | [Minimal-APIs](13-Minimal-APIs/README.md) | Not started |
| 14 | [EntityFramework-Core](14-EntityFramework-Core/README.md) (deep dive, 14 sub-topics) | Not started |
| 15 | [Authentication-Authorization](15-Authentication-Authorization/README.md) | Not started |
| 16 | [Caching-Strategies](16-Caching-Strategies/README.md) | Not started |
| 17 | [Logging-Monitoring](17-Logging-Monitoring/README.md) | Not started |
| 18 | [Testing](18-Testing/README.md) | Not started |
| 19 | [API-Design-Best-Practices](19-API-Design-Best-Practices/README.md) | Not started |

### Architecture & Scaling
| # | Topic | Status |
|---|-------|--------|
| 20 | [Clean-Architecture-DDD-CQRS](20-Clean-Architecture-DDD-CQRS/README.md) | Not started |
| 21 | [Microservices-Architecture](21-Microservices-Architecture/README.md) | Not started |
| 22 | [Messaging-EventDriven](22-Messaging-EventDriven/README.md) | Not started |
| 23 | [Docker-Kubernetes](23-Docker-Kubernetes/README.md) | Not started |
| 24 | [Performance-Scalability](24-Performance-Scalability/README.md) | Not started |
| 25 | [Security-Best-Practices](25-Security-Best-Practices/README.md) | Not started |

## Notes on style
- Notes favor diagrams (Mermaid: sequence, class, flow, state diagrams) over walls of text.
- Each topic note includes: core concepts, how it works internally, common interview questions, gotchas/edge cases, and code examples.
- We're going in-depth on **every** topic (not just EF Core) — going in order 01 → 25, one topic at a time.
- Entity Framework Core (topic 14) is additionally split into 14 sub-files since it needs extra depth.
- Async Programming (topic 04) was split out of C# Advanced into its own module — it's the single most-probed C# topic at senior level and didn't fit as a subsection alongside memory/reflection/threading.

## Source material cross-check
A 430-page university course PDF (Dr. Vikrant, .NET/C#/ASP.NET) was reviewed against this roadmap. Every topic from it that's still relevant to a 5 YOE senior interview has been folded into the placeholders above (constructor types incl. static/private constructors, static classes, all inheritance types, explicit interface implementation, `IComparable`/indexers, generics constraints, collection interfaces, delegates/events, partial classes/methods, PLINQ, custom attributes & reflection, File I/O & streams, thread lifecycle & `lock`/`Monitor`, Razor/Tag Helpers vs legacy `HtmlHelper`, layout views, bundling/minification, `ViewBag`/`TempData`/`Session` state management, localization, deployment models, CSRF/anti-forgery detail, and EF's Code First/DB First/Model First framing).

Two areas from that PDF were **deliberately left out** as full topics since they're superseded in a modern .NET stack and not worth deep new-build study time:
- **ASP.NET Web Forms** (Page Life Cycle, `RequiredFieldValidator`/`CompareValidator`-style server controls) — fully replaced by MVC/Razor Pages/Blazor; skip entirely.
- **WCF** (SOAP services, the ABC model, `gacutil`/shared-assembly hosting mechanics) — only kept as a one-line interview-recognition note under [[19-API-Design-Best-Practices]]; not a standalone deep-dive, since REST/gRPC/minimal APIs are the modern answer.

A second source — a broader "Dotnet + Angular Interview Prep" PDF — was cross-checked the same way. Its Angular/RxJS, DSA, raw SQL/Database internals, and Azure-specific sections are **out of scope** for this repo (C#/.NET/ASP.NET Core only — no Angular or cloud-platform folders exist here). Everything else from it that was missing has been folded into the placeholders above:
- Extension methods promoted from a LINQ footnote to its own explicit deep-dive (construction rules, compile-time resolution, extension vs default-interface-methods) under [[03-CSharp-Intermediate]]
- LINQ interview-trap specifics (`Select` vs `SelectMany`, `Any()` vs `Count()`, `First`/`FirstOrDefault` vs `Single`/`SingleOrDefault`, `GroupBy`/`Join` translation differences, `ToList()` placement pitfalls) under [[03-CSharp-Intermediate]]
- `IAsyncEnumerable<T>` under [[04-Async-Programming]], and a fuller concurrent-collections lineup (`ConcurrentQueue`/`Stack`/`Bag`/`BlockingCollection`) under [[05-CSharp-Advanced]]
- The constructor-order-across-inheritance "virtual call from a base constructor" trap under [[02-OOP-Fundamentals]]
- Singleton eager-vs-lazy with full double-checked-locking/`volatile`/`Lazy<T>` reasoning under [[09-Design-Patterns]]
- `IHostedService`/`BackgroundService` (previously missing entirely) under [[10-AspNetCore-Fundamentals]]
- File upload handling (`IFormFile` streaming, safe filenames, chunked uploads) under [[12-AspNetCore-MVC-WebAPI]]
- Webhook design (delivery guarantees, HMAC signature verification) under [[19-API-Design-Best-Practices]]
- EF Core connection resiliency (`EnableRetryOnFailure`) and its required interaction with explicit transactions under [[14-EntityFramework-Core]]/08-Transactions-and-Concurrency

A third pass reviewed the roadmap specifically for gaps against real ASP.NET Core interview loops (not source material this time — direct topic review). Result:
- **Async Programming promoted to its own top-level module** ([[04-Async-Programming]]) — previously a subsection of C# Advanced, now covers `TaskScheduler`/`ThreadPool` scheduling, `TaskCompletionSource`, `System.Threading.Channels`, `PeriodicTimer`, `Parallel.ForEachAsync` in addition to what was already planned (async/await internals, `ConfigureAwait`, `SynchronizationContext`, deadlocks).
- CORS (`UseCors`, policy vs endpoint-level, preflight, credentialed requests) — was missing entirely — added to [[10-AspNetCore-Fundamentals]]; CORS-as-an-attack-surface (Same-Origin Policy, misconfigured wildcard + credentials) added to [[25-Security-Best-Practices]].
- Object-to-object mapping (AutoMapper/Mapster, reflection cost, `ProjectTo`), `System.Text.Json` vs `Newtonsoft.Json` (converters, naming policies, circular refs), the over-posting/mass-assignment model-binding trap, and .NET 8's `IExceptionHandler` — added to [[12-AspNetCore-MVC-WebAPI]].
- `HttpClientFactory` promoted from a Performance-topic footnote (socket exhaustion only) to a full bullet under [[10-AspNetCore-Fundamentals]] covering DNS refresh, `DelegatingHandler`s, named vs typed clients.
- GC internals expanded in [[07-DotNet-Runtime-Internals]]: SOH/LOH/POH, GC roots, pinning, the finalization queue, resurrection, `SafeHandle`.
- `ArrayPool<T>`/`ObjectPool<T>` added alongside the existing `Span<T>`/`Memory<T>` coverage in [[05-CSharp-Advanced]] and cross-linked from [[24-Performance-Scalability]].
- Distributed-tracing vocabulary (sampling strategies, OpenTelemetry Collector architecture, trace IDs vs span IDs, baggage propagation) added to [[17-Logging-Monitoring]].
- `SortedDictionary`/`SortedSet`/`System.Collections.Immutable` added to the collections lineup in [[03-CSharp-Intermediate]].
- Reflection expanded with `Activator.CreateInstance`, `Reflection.Emit`, and the reflection-performance/caching angle (compiled expression trees as the fix) in [[05-CSharp-Advanced]].
- Two low-yield-for-interview items were deliberately added as one-liners only, not full sections: Roslyn analyzers/source generators internals (syntax tree/semantic model authoring — niche outside compiler-tooling roles) as a footnote in [[06-CSharp-Modern-Features]], and mutation/snapshot/approval testing as a footnote in [[18-Testing]].
# Claude-DotNet

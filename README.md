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
| 02 | [OOP-Fundamentals](02-OOP-Fundamentals/README.md) | Not started |
| 03 | [CSharp-Intermediate](03-CSharp-Intermediate/README.md) | Not started |
| 04 | [CSharp-Advanced](04-CSharp-Advanced/README.md) | Not started |
| 05 | [CSharp-Modern-Features](05-CSharp-Modern-Features/README.md) | Not started |
| 06 | [DotNet-Runtime-Internals](06-DotNet-Runtime-Internals/README.md) | Not started |

### Principles & Patterns
| # | Topic | Status |
|---|-------|--------|
| 07 | [SOLID-Principles](07-SOLID-Principles/README.md) | Not started |
| 08 | [Design-Patterns](08-Design-Patterns/README.md) | Not started |

### ASP.NET Core
| # | Topic | Status |
|---|-------|--------|
| 09 | [AspNetCore-Fundamentals](09-AspNetCore-Fundamentals/README.md) | Not started |
| 10 | [Dependency-Injection](10-Dependency-Injection/README.md) | Not started |
| 11 | [AspNetCore-MVC-WebAPI](11-AspNetCore-MVC-WebAPI/README.md) | Not started |
| 12 | [Minimal-APIs](12-Minimal-APIs/README.md) | Not started |
| 13 | [EntityFramework-Core](13-EntityFramework-Core/README.md) (deep dive, 14 sub-topics) | Not started |
| 14 | [Authentication-Authorization](14-Authentication-Authorization/README.md) | Not started |
| 15 | [Caching-Strategies](15-Caching-Strategies/README.md) | Not started |
| 16 | [Logging-Monitoring](16-Logging-Monitoring/README.md) | Not started |
| 17 | [Testing](17-Testing/README.md) | Not started |
| 18 | [API-Design-Best-Practices](18-API-Design-Best-Practices/README.md) | Not started |

### Architecture & Scaling
| # | Topic | Status |
|---|-------|--------|
| 19 | [Clean-Architecture-DDD-CQRS](19-Clean-Architecture-DDD-CQRS/README.md) | Not started |
| 20 | [Microservices-Architecture](20-Microservices-Architecture/README.md) | Not started |
| 21 | [Messaging-EventDriven](21-Messaging-EventDriven/README.md) | Not started |
| 22 | [Docker-Kubernetes](22-Docker-Kubernetes/README.md) | Not started |
| 23 | [Performance-Scalability](23-Performance-Scalability/README.md) | Not started |
| 24 | [Security-Best-Practices](24-Security-Best-Practices/README.md) | Not started |

## Notes on style
- Notes favor diagrams (Mermaid: sequence, class, flow, state diagrams) over walls of text.
- Each topic note includes: core concepts, how it works internally, common interview questions, gotchas/edge cases, and code examples.
- We're going in-depth on **every** topic (not just EF Core) — going in order 01 → 24, one topic at a time.
- Entity Framework Core (topic 13) is additionally split into 14 sub-files since it needs extra depth.

## Source material cross-check
A 430-page university course PDF (Dr. Vikrant, .NET/C#/ASP.NET) was reviewed against this roadmap. Every topic from it that's still relevant to a 5 YOE senior interview has been folded into the placeholders above (constructor types incl. static/private constructors, static classes, all inheritance types, explicit interface implementation, `IComparable`/indexers, generics constraints, collection interfaces, delegates/events, partial classes/methods, PLINQ, custom attributes & reflection, File I/O & streams, thread lifecycle & `lock`/`Monitor`, Razor/Tag Helpers vs legacy `HtmlHelper`, layout views, bundling/minification, `ViewBag`/`TempData`/`Session` state management, localization, deployment models, CSRF/anti-forgery detail, and EF's Code First/DB First/Model First framing).

Two areas from that PDF were **deliberately left out** as full topics since they're superseded in a modern .NET stack and not worth deep new-build study time:
- **ASP.NET Web Forms** (Page Life Cycle, `RequiredFieldValidator`/`CompareValidator`-style server controls) — fully replaced by MVC/Razor Pages/Blazor; skip entirely.
- **WCF** (SOAP services, the ABC model, `gacutil`/shared-assembly hosting mechanics) — only kept as a one-line interview-recognition note under [[18-API-Design-Best-Practices]]; not a standalone deep-dive, since REST/gRPC/minimal APIs are the modern answer.
# Claude-DotNet

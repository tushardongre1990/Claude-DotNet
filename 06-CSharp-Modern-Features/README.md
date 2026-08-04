# C# Modern Features (C# 8 → 13)

Status: **Not started**

## Planned coverage
- Nullable reference types (`#nullable enable`, annotations vs warnings)
- Records (`record`, `record struct`), value-based equality, `with` expressions
- Pattern matching evolution: property patterns, positional patterns, relational/logical patterns, `switch` expressions
- Init-only setters, required members (`required` keyword)
- Top-level statements, file-scoped namespaces
- Global usings, implicit usings
- Local functions vs lambdas, static lambdas
- Target-typed `new`
- Default interface methods
- Source generators (overview + why they replaced some reflection use cases) — built on Roslyn's compiler APIs (syntax tree + semantic model); incremental generators specifically, as the modern low-overhead version; deep Roslyn analyzer/generator authoring is a niche compiler-tooling skill, not covered further here, but recognize the terms if asked "how does a source generator actually see my code"
- Primary constructors (C# 12), collection expressions (`[]`), `ref readonly` params
- Latest C# 13/14 additions relevant to interviews

# .NET Runtime Internals

Status: **Not started**

## Planned coverage
- CLR architecture: assemblies, modules, metadata, IL
- JIT compilation (Tiered compilation, ReadyToRun, AOT/NativeAOT overview)
- Garbage Collector deep dive: generational GC, mark-and-sweep, compaction, LOH, GC modes & tuning, `GC.Collect` pitfalls
- Assembly loading & `AssemblyLoadContext`, strong naming, GAC (legacy context)
- `AppDomain` (legacy) vs current isolation model
- .NET SDK vs runtime vs shared framework, `global.json`, TFMs (`net8.0`, etc.)
- How `dotnet build`/`publish`/`run` work under the hood
- Diagnostics tooling: `dotnet-trace`, `dotnet-counters`, `dotnet-dump`, PerfView basics

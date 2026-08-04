# .NET Runtime Internals

Status: **Not started**

## Planned coverage
- CLR architecture: assemblies, modules, metadata, IL
- JIT compilation (Tiered compilation, ReadyToRun, AOT/NativeAOT overview)
- Garbage Collector deep dive: generational GC (gen 0/1/2 and why the generational hypothesis — "most objects die young" — makes gen-0 collections cheap and frequent), mark-and-sweep + compaction, GC roots (what actually keeps an object alive: static fields, stack references, CPU registers, GC handles — the starting points for the mark phase), the three heaps — SOH (Small Object Heap, compacted), LOH (Large Object Heap, ≥85,000 bytes, *not* compacted by default because moving huge objects is expensive — a classic LOH-fragmentation interview question), POH (Pinned Object Heap, .NET 5+, for objects that must never move, e.g. buffers pinned for interop — keeping them off the SOH means they no longer block compaction of everything else); pinning (`fixed`, `GCHandle.Alloc(..., GCHandleType.Pinned)`) and why excessive pinning causes fragmentation; the finalization queue — objects with a finalizer aren't collected on the first GC pass, they're queued and processed by a dedicated finalizer thread, which is *why* finalizers delay reclamation by a full extra GC generation and why `IDisposable` (deterministic, no finalizer thread involved) is preferred over relying on finalizers; resurrection (an object referencing itself from inside its own finalizer to stay alive — recognize it, never do it); `GC.SuppressFinalize` in the standard dispose pattern; `SafeHandle` as the modern base for wrapping an unmanaged OS handle (file/socket/window handle) — it has its own lightweight finalizer and a reference-counting mechanism that closes the handle safely even if `Dispose` was never called, which is *why* `SafeHandle`-based wrappers replaced raw `IntPtr` + custom finalizer code in the BCL; GC modes & tuning (workstation vs server GC, background/concurrent GC), `GC.Collect` pitfalls (forcing a full blocking collection is almost always wrong outside of very specific benchmarking scenarios)
- Assembly loading & `AssemblyLoadContext`, strong naming, GAC (legacy context)
- `AppDomain` (legacy) vs current isolation model
- .NET SDK vs runtime vs shared framework, `global.json`, TFMs (`net8.0`, etc.)
- How `dotnet build`/`publish`/`run` work under the hood
- Diagnostics tooling: `dotnet-trace`, `dotnet-counters`, `dotnet-dump`, PerfView basics

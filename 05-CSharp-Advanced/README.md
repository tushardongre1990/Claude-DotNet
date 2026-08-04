# C# Advanced

Status: **Not started**

Async/await, TAP, `IAsyncEnumerable<T>`, deadlocks, `ConfigureAwait`, and `SynchronizationContext` moved to their own module: [[04-Async-Programming]].

## Planned coverage
- Multithreading & the Task Parallel Library (TPL): `Thread` vs `Task`, thread lifecycle (unstarted → runnable → running → not runnable → dead), foreground vs background threads (`IsBackground`), `ThreadStart`/`ParameterizedThreadStart`
- Thread synchronization: `lock`, `Monitor.Enter`/`TryEnter`/`Exit`, race conditions, `Parallel.For/ForEach`, `CancellationToken`, `SemaphoreSlim`, thread-safety
- Concurrent collections in depth: `ConcurrentDictionary` (lock-striped, atomic `AddOrUpdate`/`GetOrAdd`) vs `ConcurrentQueue`/`ConcurrentStack` (lock-free, CAS-based, producer/consumer FIFO/LIFO) vs `ConcurrentBag` (thread-local storage, best when the adding thread is usually also the removing thread) vs `BlockingCollection<T>` (adds blocking `Take`/`Add` semantics on top, the standard bounded producer/consumer building block) — which one for which access pattern; contrast with the async-native `Channel<T>` in [[04-Async-Programming]]
- Memory management: stack vs heap deep dive, Garbage Collector (generations 0/1/2, LOH, GC modes: workstation/server, background GC) — full internals (SOH/POH, GC roots, pinning, finalization, resurrection) live in [[07-DotNet-Runtime-Internals]]
- Value types vs reference types internals, boxing/unboxing costs
- `Span<T>`, `Memory<T>`, `stackalloc`, `ref struct` — high-performance, zero/low-allocation code: slicing without copying, why `ref struct` can't be boxed/captured in a closure/used in an async method (must stay on the stack)
- `ArrayPool<T>` / `ObjectPool<T>` — renting/returning buffers and reusable objects to avoid repeated GC pressure in hot paths (e.g. buffer reuse in a high-throughput API); the rule that a rented array is *not* cleared by default (`clearArray: true` if that matters) and must always be returned via `finally`
- Reflection: `Type`, `Assembly`, `MemberInfo`, dynamic assembly loading (`Assembly.LoadFile` + `GetTypes()`), late binding, dynamic invocation, `Activator.CreateInstance` (constructing an instance from a `Type` at runtime — how DI containers and serializers use this internally); `System.Reflection.Emit` for generating IL at runtime (rare in app code, common inside ORMs/mocking frameworks/serializers); reflection's real performance cost (`MethodInfo.Invoke` overhead) and the standard fix — cache the reflected member or compile it once via an `Expression<T>`/delegate instead of reflecting on every call
- Custom attributes: `AttributeUsageAttribute` (`AttributeTargets`, `Inherited`, `AllowMultiple`), declaring an attribute class, constructors/properties on attributes, reading them back via `GetCustomAttributes`
- Shared assemblies & the GAC (historical/enterprise .NET Framework context): strong-name signing (`sn.exe`), `gacutil`, private vs shared vs satellite assemblies — why modern .NET (NuGet + framework-dependent/self-contained deployment) replaced this model
- File I/O & Streams: `File`/`Directory`/`DriveInfo` static classes, `StreamReader`/`StreamWriter` (buffered text I/O), reading/writing/appending, the input-stream/output-stream model
- Expression trees vs delegates
- `IDisposable`/`IAsyncDisposable`, finalizers, dispose pattern deep dive
- Weak references, memory leaks in .NET (event handlers, static references)

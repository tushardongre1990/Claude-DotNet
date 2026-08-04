# Async Programming

Status: **Not started**

Split out from C# Advanced into its own module — async/await is the single most-probed C# topic in senior interviews, and it's deep enough to deserve dedicated diagrams rather than a subsection.

## Planned coverage
- `async`/`await`, `Task` vs `Task<T>`, `ValueTask` — the Task-based Asynchronous Pattern (TAP)
- The compiler-generated state machine: what `async` actually compiles to (a struct/class implementing `IAsyncStateMachine`, `MoveNext()`, captured locals becoming fields), why this explains the performance cost of `async` on hot paths
- `IAsyncEnumerable<T>` / `await foreach` — async + iterator combined, streaming one item at a time vs `Task<List<T>>` materializing everything upfront (memory + time-to-first-item tradeoff); how it shows up concretely in minimal APIs/controllers (incrementally-streamed JSON response)
- `SynchronizationContext` — what it is, when one exists (UI apps) vs doesn't (ASP.NET Core has none since Core 1.0, unlike classic ASP.NET), why that difference matters for deadlocks
- `ConfigureAwait(false)` — what it actually does (skip capturing/resuming on the original `SynchronizationContext`/`TaskScheduler`), why library code should use it and why it's irrelevant (but harmless) in modern ASP.NET Core specifically
- Deadlocks: the classic `.Result`/`.Wait()` on a UI/legacy-ASP.NET thread blocking the very context the continuation needs to resume on — sequence diagram of the deadlock, then the fix (`await` all the way down)
- `TaskScheduler` — default scheduler vs custom schedulers, how it decides where a continuation runs
- `ThreadPool` internals — worker thread lifecycle, min/max threads, work-stealing queues, why `Task.Run` queues to the pool and why blocking pool threads (sync-over-async) causes thread-pool starvation under load
- `TaskCompletionSource<T>` — bridging callback-based/event-based APIs into `Task`-based ones, the standard "async-ify a legacy API" pattern
- `System.Threading.Channels` (`Channel<T>`, `ChannelWriter`/`ChannelReader`) — the modern async producer/consumer primitive; bounded vs unbounded channels, backpressure, vs `BlockingCollection<T>` (sync, thread-blocking) covered in [[05-CSharp-Advanced]]
- `PeriodicTimer` — the modern `await`-friendly replacement for `System.Timers.Timer`/`System.Threading.Timer` in polling loops
- `Parallel.ForEachAsync` — CPU-bound-and-async-mixed workloads, degree-of-parallelism control, vs plain `Task.WhenAll` (unbounded concurrency) vs `SemaphoreSlim`-throttled `Task.WhenAll`
- `CancellationToken` — propagation through an async call chain, `CancellationTokenSource`, linked tokens, cooperative cancellation vs `Thread.Abort` (removed/dangerous)
- `Task.WhenAll` vs `Task.WhenAny`, exception aggregation behavior (`AggregateException` unwrapping), first-exception-wins vs all-exceptions-observed gotchas
- Producer/consumer pattern end-to-end: `Channel<T>`-based async version vs `BlockingCollection<T>`-based blocking version — when to reach for which
- Fire-and-forget pitfalls: unobserved task exceptions, why `async void` should only ever be used for event handlers (exceptions can't be awaited/caught by the caller, crash the process instead)
- Cross-reference: `IHostedService`/`BackgroundService` (the ASP.NET Core hosting model for long-running async work) lives in [[10-AspNetCore-Fundamentals]] since the interesting part there is the DI captive-dependency problem, not the async mechanics — but the scheduling primitives above (`PeriodicTimer`, `CancellationToken`, `Channels`) are exactly what a `BackgroundService` body is built from
- Cross-reference: PLINQ (`AsParallel()`) is covered in [[03-CSharp-Intermediate]] alongside LINQ, since it's a LINQ-operator concern (data-parallelism over a sequence) rather than a Task/async concern — but the "does parallelizing this actually help" tradeoff discussion is the same kind of question interviewers ask here

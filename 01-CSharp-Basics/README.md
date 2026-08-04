# C# Basics

Status: **In progress**

> **How to read this file:** every abbreviation is spelled out in full the first time it's used. Blockquotes labeled **New term** explain foundational concepts (value types, reference types, boxing, the stack/heap, the garbage collector, etc.) from scratch, assuming no prior background — so you shouldn't need to look anything up elsewhere. The rest of the content (diagrams, tables, interview traps) is unchanged in purpose: it's still aimed at senior-level interview depth, just now self-contained.

---

## 1. The .NET Landscape

> **New term — .NET.** ".NET" is a *platform* for building software: a **runtime** that executes your compiled code, a **standard library** of reusable code (file access, networking, collections, etc. — so you don't write everything from scratch), and **compilers** (for C#, F#, VB.NET) that turn your source code into a form the runtime can run. ".NET Framework," ".NET Core," and ".NET 5+" below are different historical versions/branches of this same idea.

You need the history because interviewers use it to check you understand *why* things are the way they are today.

```mermaid
timeline
    title Evolution of .NET
    2002 : .NET Framework 1.0 (Windows-only, CLR + BCL)
    2014 : .NET Core announced (cross-platform rewrite)
    2016 : .NET Core 1.0 released (Linux/macOS/Windows)
    2019 : .NET Core 3.0 (WinForms/WPF support added back)
    2020 : .NET 5 (unification — "Core" dropped from name)
    2021 : .NET 6 (LTS, minimal hosting model)
    2023 : .NET 8 (LTS, current recommended baseline)
    2024 : .NET 9 (STS - Standard Term Support)
```

| | .NET Framework | .NET Core (legacy name) | .NET 5+ (current) | Mono / Xamarin |
|---|---|---|---|---|
| Platform | Windows only | Cross-platform | Cross-platform | Cross-platform (mobile/game engines) |
| Status | Maintenance mode only, no new features | Superseded by .NET 5+ | Actively developed | Used inside .NET MAUI, Unity |
| Open source | Partially | Yes | Yes | Yes |
| Use today | Legacy WebForms/WCF apps | — | All new development | Mobile apps via .NET MAUI |

*(Acronym key for the table/timeline above — these are just history/context, not things you need to use day-to-day: **WinForms** = Windows Forms and **WPF** = Windows Presentation Foundation, two older frameworks for building Windows desktop UIs; **WCF** = Windows Communication Foundation, an older framework for building network services, superseded today by REST-style **APIs** (Application Programming Interfaces — see [[19-API-Design-Best-Practices]]); **MAUI** = Multi-platform App UI, Microsoft's current cross-platform mobile/desktop UI framework.)*

**Key interview point:** .NET Core wasn't "a faster .NET Framework" — it was a ground-up rewrite for cross-platform + modularity (**NuGet** packages instead of one giant BCL — NuGet is .NET's package manager, a way to download and reuse other developers' pre-built code libraries, similar to npm for JavaScript or pip for Python) + performance. .NET 5 renamed "Core" away once feature parity was reached, so "which .NET are you on" today just means the version number (8, 9, ...). .NET 8 is the current **LTS** (Long Term Support — Microsoft commits to 3 years of updates and bug fixes for these releases) release; pick it by default for production unless you specifically need a newer preview feature.

- **CLR** (Common Language Runtime) = the virtual machine that executes .NET code (**GC** — Garbage Collector, automatic memory cleanup, explained in section 3; **JIT** — Just-In-Time compiler, explained in section 2; type safety; exception handling).

  > **New term — Virtual machine (in this context).** Not "a VM you'd spin up in the cloud" — here it means a software layer that sits between your compiled code and the real CPU, so the same compiled output can run unmodified on different physical machines/operating systems. The CLR reads your program's instructions (IL, see section 2) and executes them, managing memory and safety along the way, instead of your code talking to the hardware directly.

- **BCL** (Base Class Library) = `System.*` — collections, file I/O, threading, etc. This is the "standard library" mentioned above: code Microsoft already wrote so you don't reinvent lists, file readers, etc.
- **CLI** (Common Language Infrastructure — *not* to be confused with the far more common meaning of "CLI," Command Line Interface; this is an unrelated acronym for a language-interoperability standard) = the **ECMA** (Ecma International, a European standards organization) standard that defines **CIL** (Common Intermediate Language — the formal standards name for what's usually just called **IL**, Intermediate Language; same thing, two names depending on whether you're talking about the standard or the everyday tooling) / metadata format, so multiple languages (C#, F#, VB.NET) can interoperate — this is *why* a C# `List<T>` and an F# equivalent can call into each other seamlessly.

---

## 2. Compilation Pipeline: Source → IL → Native

This is one of the most commonly probed "do you actually understand the runtime" questions.

> **New term — Compiler.** A compiler is a program that translates the code you write (source code) into another, lower-level form. C#'s compiler doesn't go straight to the machine code your CPU understands — it deliberately stops one step short, at IL, explained in the first bullet below.

```mermaid
flowchart LR
    A["C# source (.cs files)"] -->|Roslyn compiler: csc| B["IL + Metadata\n(.dll / .exe assembly)"]
    B -->|"loaded by CLR at runtime"| C["JIT Compiler"]
    C -->|"compiles method on first call"| D["Native machine code\n(cached in memory)"]
    D --> E["CPU executes"]
```

- **Roslyn** is the modern open-source C# compiler. It only compiles to **IL** (Intermediate Language — a low-level, CPU-independent instruction set, not the actual machine code your processor runs), not machine code. This is why the same DLL runs on Windows/Linux/macOS/ARM/x64 — the IL is platform-agnostic. *(**ARM** and **x64** are examples of CPU **architectures** — families of processor designs with different native instruction sets; code compiled for one won't run on the other without translation, which is exactly what shipping IL instead of native code avoids needing.)*
- **JIT** (Just-In-Time compilation) happens per-method, the first time a method is called, translating that method's IL into real native machine code for the CPU you're actually running on — and the result is cached for the process's lifetime. This is why the *first* call to a method is often slower ("JIT warm-up") — relevant to cold-start discussions for serverless/Azure Functions.
- **Tiered compilation** (default since .NET Core 3): Tier 0 JIT is quick-and-dirty for fast startup; if a method is called often (hot path), it gets recompiled by Tier 1 with full optimizations. This is a startup-time vs peak-throughput tradeoff made automatically.
- **ReadyToRun (R2R)** and **Native AOT** are ways to skip/reduce JIT at startup by precompiling — R2R still ships IL as fallback + precompiled native code; **Native AOT** (Ahead-Of-Time compilation — compiling straight to native machine code before the program ever runs, instead of during execution like JIT) compiles fully ahead-of-time with no JIT and no IL at all (smaller, faster startup, but loses some reflection-heavy features). Native AOT is the modern answer to "why is my container's cold start slow?"

**Common interview question:** *"Is C# compiled or interpreted?"* — Neither, purely. It's a two-stage compiled language: compiled to IL ahead of time, then JIT-compiled to native code at runtime. That's different from Java only in branding — the JVM (Java Virtual Machine) does the same thing conceptually (bytecode + JIT).

---

## 3. Value Types vs Reference Types, Stack vs Heap

This is the single most important mental model for the rest of C#. Get this wrong and async/mutability/performance questions all fall apart.

> **New term — Variable and type.** A **variable** is a named storage location in memory that holds a value — e.g. `int x = 5;` creates a variable named `x` holding the value `5`. Every variable has a **type**, which tells the compiler two things: how much memory to reserve, and what operations are valid on it (you can add two `int`s, but not two `Person`s unless you define what "+" means for `Person`).

> **New term — Memory, stack, and heap.** When your program runs, the operating system gives it a chunk of memory (**RAM**, Random Access Memory) to work with. That memory is split into regions; the two relevant here:
> - The **stack** is a small, fast region used for short-lived data tied to the method currently executing. It behaves like a physical stack of plates — **LIFO** (Last In, First Out): the most recently added item is the first one removed. When a method is called, its local variables get "pushed" onto the stack; when the method returns, they're automatically "popped" off. This is why stack allocation is essentially free.
> - The **heap** is a larger, more flexible region for data that needs to outlive a single method call, or whose size isn't known until runtime. Allocating heap memory is slower than stack memory, and — unlike the stack — heap memory doesn't clean itself up when a method returns; see the **Garbage Collector** below.

```mermaid
flowchart TB
    subgraph Stack["Stack (per-thread, LIFO, fast)"]
        direction TB
        S1["int x = 5"]
        S2["Point p (struct, inline)"]
        S3["reference/pointer to heap object"]
    end
    subgraph Heap["Heap (shared, GC-managed)"]
        direction TB
        H1["Person object\n(fields + type ptr + sync block)"]
    end
    S3 -->|"points to"| H1
```

> **New term — Reference (pointer).** A reference is simply an address — a number that says "the real data lives over there in memory." When a reference-type variable is declared, what's actually stored in that variable (whether on the stack, or inline inside another heap object) is this address, not the data itself. C# follows the address to read/modify the real data automatically whenever you use `.` to access a member — you never see the raw address.

### Stack frames, concretely

Each **thread** gets its own stack (~1MB by default in .NET). It's organized into **frames**, one per method currently in progress. Calling a method pushes a new frame containing its parameters and locals; returning pops the whole frame off in one shot — no per-variable cleanup needed, which is why it's essentially free:

```
Call OrderService.Process()
┌───────────────────────────┐
│ Process() frame            │  ← pushed on call, popped on return
│  int total = 0             │
│  Order order (reference) ──┼──┐
└───────────────────────────┘  │
        stack grows ↑           ▼
                            Heap: Order { Id=7, Items=[...] }
```
`total` lives entirely inside the frame. `order` is also inside the frame, but it's just an address — the actual `Order` object it points to lives on the shared heap, outside any frame, until nothing references it.

Now the core distinction:

| | Value type | Reference type |
|---|---|---|
| Examples | `int`, `double`, `bool`, `struct`, `enum` | `class`, `string`, `array`, `delegate`, interfaces |
| Storage | Typically on the **stack** (or inline inside whatever contains it — see note below) | Always on the **heap**; a reference (pointer) to it lives wherever the variable is declared |
| Assignment (`b = a`) | **Copies** the entire value | Copies the **reference** — both variables point to the same object |
| Default `null`? | No (unless `Nullable<T>`/`T?`) | Yes |
| Passed to methods | By value (a copy) by default | The reference itself is passed by value (so you can mutate the object's fields, but reassigning the parameter doesn't affect the caller's variable — unless you use `ref`) |

### Copy semantics, concretely

```csharp
struct Point { public int X, Y; }

Point p1 = new Point { X = 1, Y = 2 };
Point p2 = p1;        // COPIES all fields into p2's own slot
p2.X = 99;

Console.WriteLine(p1.X); // 1 — p1 is untouched
Console.WriteLine(p2.X); // 99
```
`p1` and `p2` are two entirely separate blobs of memory after that assignment — mutating one can never affect the other. Compare a reference type:

```csharp
class Order { public int Total; }

Order o1 = new Order { Total = 100 };
Order o2 = o1;         // COPIES the address, not the object
o2.Total = 999;

Console.WriteLine(o1.Total); // 999 — same object!
```
`new Order()` allocates the object once, on the heap. `o1` just holds a pointer to it. `o2 = o1` copies *that pointer* — now two variables point at the same object, so mutating through `o2` is visible through `o1`. This — not magic — is the root cause of the classic bug "I passed my object into a method and it changed my data": the method received the same address, not a copy.

> **Correction to a common oversimplification:** "value types go on the stack, reference types go on the heap" is *approximately* true but not the real rule. The actual rule: **a value type is stored wherever it's declared.** A local variable that's a struct lives on the stack. A struct that is a *field of a class* lives inline inside that class's heap allocation. A struct captured in a closure or boxed lives on the heap too. Reference types are always heap-allocated, and their *reference* (pointer) lives wherever the variable is declared (stack, or inline in another heap object).

### Where that nuance actually bites

```csharp
class Order
{
    public Point Location; // a struct FIELD
}

var order = new Order();                      // one heap allocation, for the Order
order.Location = new Point { X = 1, Y = 2 };   // Point is baked INLINE into that same allocation
```
`Location` never gets its own separate heap allocation — its bytes sit directly inside `Order`'s memory, right alongside `Order`'s other fields. There's exactly **one** heap allocation for `order`, which is actually a performance win (no extra pointer to chase to reach `Location`).

A struct gets pushed onto the heap on its own, separately from where it's declared, when it's:
- **Boxed** — assigned to `object`, `dynamic`, or a non-generic interface (see Boxing/unboxing below).
- **Captured by a lambda/closure** — the compiler generates a hidden class to hold captured variables, so anything captured (value or reference type) rides along inside that heap-allocated closure object.
- An element of a `struct[]` array — arrays are always heap objects, but the structs inside are stored inline *inside* that one array allocation, not boxed individually.

### Boxing/unboxing

> **New term — Boxing / unboxing.** C#'s type system treats everything as ultimately derived from `object` (see section 4) — but value types like `int` don't normally have the heap-object wrapper that reference types have. **Boxing** is the automatic process of wrapping a value type in a temporary heap object so it can be treated as an `object` (e.g. passed somewhere that expects `object`, or stored in an old, non-generic collection). **Unboxing** is reversing that: unwrapping it back into a plain value type. Both copy data and boxing performs a heap allocation, which is the hidden performance cost called out below.

```mermaid
flowchart LR
    A["int i = 42 (stack)"] -->|"boxing: object o = i"| B["heap-allocated object\nwrapping the int"]
    B -->|"unboxing: int j = (int)o"| C["int j (stack, copied out)"]
```
Boxing allocates on the heap and copies the value in; unboxing copies it back out. It's a hidden performance cost — this is why generic collections (`List<T>`) were introduced to replace non-generic ones (`ArrayList`), which boxed every value type stored in them:

```csharp
ArrayList list = new ArrayList(); // pre-generics, non-generic
list.Add(5);            // 5 gets BOXED — a heap allocation just to store one int
list.Add(10);            // another heap allocation

List<int> genericList = new List<int>();
genericList.Add(5);      // NO boxing — List<int> stores raw ints inline in an int[] internally
```
Put 10,000 ints in an `ArrayList` and you've made 10,000 heap allocations — 10,000 extra objects for the GC to eventually track and clean up, just to hold numbers. `List<int>` stores them as a contiguous `int[]`, so there's zero boxing.

### Stack vs Heap in one sentence
Stack = fast, automatically reclaimed when a method returns, size-limited (`StackOverflowException` on deep recursion). Heap = flexible size, reclaimed by the **Garbage Collector** (not immediately when a reference goes out of scope — a key misconception to correct), slower to allocate/deallocate.

> **How the GC actually works (brief):** periodically, the GC briefly pauses the program and walks from a set of known starting points ("roots" — local variables currently on the stack, static fields, etc.), following references to find every heap object still reachable. Anything *not* reachable is garbage, and its memory is reclaimed. This means an object is only freed at the *next* GC pass after it truly becomes unreachable — not the instant a variable goes out of scope. Unlike languages such as C/C++, you never manually free heap memory in C#.

### Generational GC, in more depth

.NET's GC is **generational** — it assumes most objects die young (a request-scoped DTO, a temporary string) and few live long (a cached singleton), so it splits the heap into generations instead of treating all objects equally:

- **Gen 0** — newly allocated objects. Collected very frequently and very fast (often microseconds), since most Gen 0 objects are already garbage by the time a collection runs.
- **Gen 1** — objects that survived one Gen 0 collection; a buffer between short-lived and long-lived.
- **Gen 2** — long-lived objects (e.g. a static cache). Collected rarely, since a full Gen 2 sweep is the most expensive.

Each collection starts from the **roots** (local variables on every thread's stack, static fields, CPU registers), walks every reference reachable from them, marks everything found as "alive," and reclaims anything unmarked *in that generation*. An object that survives a collection gets **promoted** to the next generation up.

This is *why* allocating tons of short-lived objects in a hot loop ("allocation pressure") hurts throughput: it forces more frequent Gen 0 collections, and if some of those objects happen to still be reachable when a collection runs, they get promoted and end up clogging the more expensive Gen 1/2 collections too.

### Why this model matters beyond trivia

- **Async code**: value types captured by an `async` method get boxed into the compiler-generated heap object backing that method's state machine — part of why `async` methods carry an unavoidable small allocation cost per call (see [[04-Async-Programming]]).
- **Multithreading**: because reference types share one object across every variable pointing at it, two threads holding the same reference are touching the *same* memory — the root cause of race conditions, and why `lock`/thread-safety matters for shared reference-type state but not for a `struct` each thread holds its own copy of.
- **Performance-sensitive code**: `Span<T>`, `ref struct`, and `in` parameters (section 7) exist specifically to work with data without triggering extra heap allocations or copies — that only makes sense once this section's model is second nature.

---

## 4. Primitive Types

> **New term — Bits and bytes.** A **bit** is the smallest unit of memory, holding a `0` or `1`. A **byte** is 8 bits. Numeric types mainly differ in how many bytes they use, which determines the range of values they can hold — e.g. `byte` (1 byte / 8 bits) holds 0–255; `int` (4 bytes / 32 bits) holds roughly ±2.1 billion. More bytes = bigger range but more memory used per value.

> **New term — Signed vs unsigned.** "Signed" types (`sbyte`, `short`, `int`, `long`) can represent negative numbers, spending part of their bit range on the sign. "Unsigned" types (`byte`, `ushort`, `uint`, `ulong` — the `u` prefix means unsigned) can only represent zero and positive numbers, but get roughly double the positive range in exchange since no bits are spent on sign.

| Category | Types | Notes |
|---|---|---|
| Integral | `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `nint`/`nuint` | `int` = `System.Int32`, `long` = `System.Int64`. C# keywords are just aliases for BCL structs. |
| Floating point | `float` (32-bit), `double` (64-bit) | Approximate, binary representation — never use for currency. |
| Decimal | `decimal` (128-bit) | Base-10 representation, used for money — trades range/speed for precision. |
| Boolean | `bool` | 1 byte in memory (not 1 bit) despite holding 2 states. |
| Character | `char` | **UTF-16** (Unicode Transformation Format, 16-bit — the encoding scheme .NET uses internally for text) code unit (2 bytes) — a single `char` can't represent all Unicode code points (surrogate pairs for astral characters). |
| Text | `string` | Reference type, **immutable** (see section 6), UTF-16 internally. |
| Object root | `object` | Every type (value or reference) derives from `System.Object`. |

> **New term — Unicode / character encoding.** Computers only store numbers, so representing text requires a system for mapping numbers to characters. **Unicode** is a standard that assigns a unique number (a **code point**) to essentially every character in every writing system. **UTF-16** is one way of encoding those numbers into actual bytes in memory, using 2-byte units — most common characters fit in one unit, but some (emoji, rare scripts) need two units combined (a "surrogate pair").

> Floating point numbers **approximate** real numbers using a binary fraction — similar to how `1/3` can't be written exactly in decimal, some ordinary decimal fractions like `0.1` can't be represented exactly in binary either, causing tiny rounding errors. That's why `float`/`double` are wrong for money and `decimal` (which uses base-10 internally) is right.

**Interview trap:** `int` is a value type but it's still "an object" in the sense that it derives from `object` — this only works because of implicit boxing when you call a method like `.ToString()` or `.Equals()` defined on `object`/overridden in the struct.

### Type conversion
```mermaid
flowchart LR
    A[Implicit conversion] -->|"no data loss possible\ne.g. int -> long"| B[Compiler allows silently]
    C[Explicit conversion / cast] -->|"possible data loss\ne.g. long -> int, double -> int"| D["Requires (int)x syntax"]
    E["Convert.ToInt32(x)"] --> F["Handles null, different types, rounds doubles"]
    G["int.Parse(str)"] --> H["Throws FormatException/OverflowException on bad input"]
    I["int.TryParse(str, out x)"] --> J["Returns bool, no exception — preferred for untrusted input"]
```
- **Implicit** conversions happen automatically because no information can be lost (a small type fits inside a bigger one); **explicit** conversions (casts) require you to write `(int)x` because information *could* be lost, forcing you to acknowledge that risk.
- `Parse` throws on bad input; `TryParse` doesn't — always prefer `TryParse` for input you don't control (user input, external **APIs** — Application Programming Interfaces, i.e. other programs/services you send requests to and get responses from).
- `Convert.ToInt32(null)` returns `0` (doesn't throw) — a subtle difference from `int.Parse(null)` which throws `ArgumentNullException`. Interviewers sometimes probe this exact difference.

---

## 5. Variables, Operators, Control Flow

Foundational, but still worth spelling out precisely — this is where a lot of small jargon and gotchas live:

- `var` lets the compiler infer a variable's type at compile time from what you assign to it — `var x = 5;` compiles to exactly the same thing as `int x = 5;`, just less typing. This is **compile-time type inference**, **not** dynamic typing: `x` is still permanently an `int`, and the compiler rejects `x = "hello";` afterward. Compare to `dynamic`, which defers all type-checking to runtime and bypasses the compiler's safety checks entirely (used for interoperating with non-.NET code, or heavy reflection scenarios).
- **Operators** are symbols that perform an operation on values:
  - Arithmetic: `+ - * / %` (`%` is remainder/modulo, e.g. `7 % 3` is `1`).
  - Relational: `== != < > <= >=` — compare two values, produce a `bool`.
  - Logical: `&&` / `||` (**short-circuiting** — `&&` stops evaluating as soon as one side is `false`, since the overall result is already determined; likewise `||` stops once one side is `true`) vs `&` / `|` (always evaluate both sides — rarely used on booleans, mostly seen doing bitwise math on integers).
  - Null-conditional `?.` — `person?.Name` evaluates to `null` instead of throwing if `person` is `null`, short-circuiting the rest of the chain.
  - Null-coalescing `??` — `x ?? y` evaluates to `x` if it's not `null`, otherwise `y`.
  - Null-coalescing assignment `??=` — `x ??= y` assigns `y` to `x` only if `x` is currently `null`.
- `switch` **statement** (classic C-style; each `case` needs a `break` or it falls through — unlike C/Java there's no *implicit* fall-through in C#, you must write `goto case` explicitly to opt into it) vs `switch` **expression** (C# 8+, e.g. `x switch { 1 => "one", _ => "other" }`; must cover every possible case — be *exhaustive* — or include a `_` discard as a default, and *returns a value* directly instead of running statements) — this distinction is a common "what's new in modern C#" interview question, expanded in [[06-CSharp-Modern-Features]].
- **Loops** repeat a block of code:
  - `for (int i = 0; i < 10; i++) { ... }` — repeats a fixed number of times, tracked with an explicit counter.
  - `foreach (var item in collection) { ... }` — repeats once per element of a collection; sugar over calling `.GetEnumerator()` and pulling elements one at a time via `IEnumerable` (the BCL interface — in C#, interface names conventionally start with `I` — that anything "loopable" implements) — see [[03-CSharp-Intermediate]] for iterator internals.
  - `while (condition) { ... }` — repeats as long as `condition` is `true`, checked *before* each iteration (may run zero times).
  - `do { ... } while (condition);` — same as `while`, but checked *after* each iteration (always runs at least once).

---

## 6. Arrays and Strings

### Strings are immutable reference types

> **New term — Immutable.** "Immutable" means an object's data can never change after it's created. Any operation that looks like it modifies the object (`s.Replace(...)`, `s.ToUpper()`) actually leaves the original completely untouched and returns a brand-new object holding the result.

```mermaid
flowchart LR
    A["string s1 = \"hello\""] --> H1["heap: \"hello\""]
    B["string s2 = s1"] --> H1
    C["s2 += \" world\""] --> H2["NEW heap object: \"hello world\""]
    B -.->|"s2 now points here instead"| H2
```
Every "mutation" of a string (`+=`, `.Replace()`, `.ToUpper()`) actually allocates a brand-new string object. `s1` is untouched. This is why concatenating in a loop is a classic performance mistake:

```csharp
// Bad: O(n²) — allocates a new string every iteration
string result = "";
for (int i = 0; i < 10000; i++) result += i;

// Good: StringBuilder mutates an internal buffer in place
var sb = new StringBuilder();
for (int i = 0; i < 10000; i++) sb.Append(i);
string result = sb.ToString();
```

> **New term — Big-O notation.** A shorthand for how an algorithm's cost (time or memory) grows as the input size (`n`) grows, ignoring constant factors. `O(n²)` ("quadratic") means cost grows roughly with the *square* of `n` — double the input, roughly quadruple the cost. The loop above is `O(n²)` because each of the `n` concatenations has to copy the entire string built so far (itself up to length `n`), for a total of roughly `n × n` character copies. `O(n)` ("linear," what `StringBuilder` achieves instead) means cost grows proportionally with `n` — double the input, roughly double the cost.

**String interning:** string literals are cached in an intern pool, so `"abc" == "abc"` (two literals) actually reference the *same* heap object. Strings built at runtime (`new string(...)`, concatenation) are not interned by default — you can force it with `string.Intern(s)`. This is why `ReferenceEquals("abc","abc")` can be `true` for literals but `false` for two runtime-built equal strings, while `==`/`.Equals` are always `true` for equal content (string overloads `==` to do value comparison, unlike default reference-type behavior).

### Arrays
- Fixed size once created, zero-indexed, stored **contiguously** on the heap (even though the *elements* might be value types stored inline, or references for reference-type elements — same rule as before). *(**Contiguous** means the elements sit one after another in one unbroken block of memory, rather than scattered around. This is what makes indexed access `arr[5]` extremely fast — the runtime just computes `start address + (index × element size)` instead of searching for it.)*
- `Array` implements `IEnumerable`, `ICollection` — supports `foreach`, `.Length`, `Array.Sort`, `Array.Copy`.
- Multi-dimensional (`int[,]`) vs jagged (`int[][]`) — jagged is an array of arrays (each row can be a different length, and each row is its own separate heap allocation), multi-dimensional is one contiguous block. Jagged is generally faster for iteration due to better cache locality per-row and is more idiomatic in C#.

```csharp
int[,] grid = new int[3, 3];   // multi-dimensional: one block, all rows same length
int[][] jagged = new int[3][]; // jagged: array of arrays, each row separately allocated
jagged[0] = new int[5];
jagged[1] = new int[2];
```

---

## 7. Methods: Parameters, Overloading

Recall from section 3: value types are copied on assignment/pass, and reference types pass their reference (address) by value. This section covers ways to override that default — i.e. let a method modify the *caller's actual variable*, not a copy of it.

```mermaid
flowchart TB
    A["void Method(int x)"] --> A1["pass by value: copy in, changes don't propagate out"]
    B["void Method(ref int x)"] --> B1["pass by reference: must be initialized before call, changes propagate both ways"]
    C["void Method(out int x)"] --> C1["pass by reference: does NOT need init before call, MUST be assigned inside method"]
    D["void Method(in int x)"] --> D1["pass by reference, read-only inside method — avoids copying large structs"]
    E["void Method(params int[] xs)"] --> E1["caller can pass 0..N comma-separated args OR an array"]
```

- `ref` vs `out`: both pass the variable's address, not a copy. `ref` requires the caller's variable to already have a value; `out` doesn't (and the compiler *forces* you to assign it before the method returns) — the classic use is `int.TryParse(s, out int result)`.
- `in` (C# 7.2+) is about **performance**: passing a large `struct` by value copies the whole thing; `in` passes by reference but the compiler prevents mutation, giving you the copy-avoidance of `ref` without the "can this method change my variable" risk.
- **Method overloading**: same name, different parameter list (count/type/order) — resolved at **compile time** based on the static type of the arguments (this is why overload resolution is unrelated to polymorphism/virtual dispatch, which is a runtime concept — see [[02-OOP-Fundamentals]]).
- **Optional parameters** (`void M(int x = 5)`) vs **named arguments** (`M(y: 10, x: 5)`) — optional parameter default values are baked into the *caller's* compiled IL at compile time, which causes a subtle versioning bug: if a library changes a default value, callers compiled against the old library keep using the old default until they're recompiled.

---

## 8. Namespaces, Assemblies, Access Modifiers

```mermaid
flowchart TB
    subgraph Assembly["Assembly (.dll/.exe) = deployment + versioning unit"]
        subgraph NS1["Namespace: MyApp.Services"]
            C1["class OrderService"]
        end
        subgraph NS2["Namespace: MyApp.Models"]
            C2["class Order"]
        end
    end
```

> **New term — DLL / EXE.** **DLL** (Dynamic Link Library) and **EXE** (Executable) are the two file types a .NET assembly compiles to — `.exe` is directly runnable as a standalone program, `.dll` is a library that other programs load and call into. (On Linux/macOS the underlying concept is identical even though the file extensions originated on Windows.)

- **Namespace** = purely a logical, compile-time grouping to avoid name collisions. Has no runtime existence.
- **Assembly** = the physical unit of deployment, versioning, and (historically) security boundary. One assembly can contain many namespaces, and one namespace can span multiple assemblies.
- **Access modifiers**: `public`, `private`, `protected`, `internal`, `protected internal` (union: derived OR same assembly), `private protected` (C# 7.2+, intersection: derived AND same assembly — commonly confused with `protected internal`).

| Modifier | Same class | Derived class, same assembly | Derived class, other assembly | Same assembly, non-derived | Other assembly |
|---|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ | ❌ |
| `internal` | ✅ | ✅ | ❌ | ✅ | ❌ |
| `protected internal` | ✅ | ✅ | ✅ | ✅ | ❌ |
| `private protected` | ✅ | ✅ | ❌ | ❌ | ❌ |

---

## 9. Nullable Value Types

Value types can't normally be `null` (an `int` is always some 32-bit number). `Nullable<T>` (`T?` sugar) wraps a value type to add a `null` state:

> **Why can't a plain value type be `null`?** Because a value type variable *is* its data, stored inline — there's no spare "no value" state hiding inside, say, a 4-byte `int`. `Nullable<T>` fixes this by bundling the value together with an extra `bool` flag that tracks whether it's meaningfully set — effectively `{ bool HasValue; T Value; }`.

```mermaid
flowchart LR
    A["int? x = null;"] --> B["Nullable&lt;int&gt; struct\nHasValue = false, Value = default"]
    C["int? y = 5;"] --> D["Nullable&lt;int&gt; struct\nHasValue = true, Value = 5"]
```

- `int?` is really `System.Nullable<int>`, a struct with `HasValue` (bool) and `Value` (T) fields — it's still a value type overall (unlike Nullable *Reference* Types, which are a compile-time-only annotation feature added in C# 8, covered in [[06-CSharp-Modern-Features]]).
- Accessing `.Value` when `HasValue` is `false` throws `InvalidOperationException` — always check `HasValue` or use `??` (`x ?? 0`) or pattern matching (`x is int val`).

---

## 10. Structs vs Classes (Basics)

| | `struct` | `class` |
|---|---|---|
| Type category | Value type | Reference type |
| Inheritance | Cannot inherit from another struct/class (implicitly seals from further struct inheritance); can implement interfaces | Full inheritance support |
| Default value | All fields zeroed (no constructor call needed) | `null` |
| Typical use | Small, immutable, short-lived data (e.g. `Point`, `DateTime`, `decimal`) | Everything else — entities, services, anything with identity or that needs inheritance |
| Copy semantics | Copied entirely on assignment/pass | Reference copied; underlying object shared |

Rule of thumb repeated in the official guidelines: make something a `struct` only if it's small (≲16 bytes as a rough guide), logically immutable, and represents a single value — otherwise, default to `class`. Getting this wrong (large mutable structs) causes defensive-copy bugs and performance problems, and is a favorite "what's wrong with this code" interview trap.

---

## 11. Exception Handling Basics

> **New term — Exception.** An exception is an object representing an error or unexpected condition, created ("thrown") at the point something goes wrong. Once thrown, normal execution stops immediately and control jumps upward through whichever methods are currently running — the **call stack**, the chain of "method A called method B called method C…" currently in progress — looking for a `catch` block willing to handle that type of exception. If none is found anywhere up the chain, the program crashes.

```mermaid
flowchart TB
    A[try block] -->|"exception thrown"| B{Matching catch?}
    B -->|"yes, specific type first"| C[catch block runs]
    B -->|"no match"| D["propagates up the call stack"]
    C --> E[finally block always runs]
    D --> E
    E --> F["exception continues propagating\n(if unhandled, crashes the process)"]
```

- `try` / `catch` / `finally`: `finally` always executes — whether an exception was thrown, caught, or the method returned normally — used for cleanup (though `using`/`IDisposable` is preferred for resource cleanup specifically, see [[05-CSharp-Advanced]]).
- Catch **most specific exception types first** — a base `catch (Exception ex)` placed before a derived one is unreachable code (and won't even compile in most cases, since the compiler detects order errors for sealed hierarchies... though with custom exceptions it can silently swallow more than intended).
- **Custom exceptions**: derive from `Exception` (or a more specific base like `InvalidOperationException`), always provide the three standard constructors (parameterless, message, message+innerException) for consistency with BCL conventions.
- `throw ex;` **resets the stack trace** to the current line — always use bare `throw;` inside a catch block to preserve the original stack trace when rethrowing. This is one of the most common senior-level code review flags.
- Exceptions are for **exceptional/unexpected** conditions, not control flow — throwing/catching has real performance cost (**stack unwinding** — the runtime walking back up the call stack looking for a handler, running any `finally` blocks along the way — plus stack trace capture), and using exceptions for expected outcomes (e.g. validation failures) is an anti-pattern; prefer return values / Result-pattern / `TryX` methods for expected failure paths.

---

## Interview Q&A Cheat Sheet

- **Q: Is C# pass-by-value or pass-by-reference?** — Always pass-by-value by default, including for reference types (the *reference itself* is copied). `ref`/`out`/`in` explicitly pass by reference.
- **Q: Why is `string` immutable?** — Thread-safety (safe to share across threads without locks), enables interning/caching, and safe use as dictionary keys (hash code never changes).
- **Q: What's the difference between `const` and `readonly`?** — `const` is a compile-time constant (value baked into IL at every call site, must be a primitive/string, implicitly static); `readonly` is a runtime constant (can be set in the constructor, can depend on runtime computation, avoids the versioning problem `const` has across assemblies).
- **Q: Stack overflow vs out of memory?** — `StackOverflowException` (uncatchable, crashes process) from deep/infinite recursion exceeding the fixed stack size; `OutOfMemoryException` from exhausting heap space, catchable but usually still fatal in practice.
- **Q: When would you use a struct over a class?** — Small, immutable, frequently-allocated value semantics data where avoiding heap allocation/GC pressure matters (e.g. a `Point` used millions of times in a hot loop).

---

**Next up:** [[02-OOP-Fundamentals]] — say the word when you're ready.

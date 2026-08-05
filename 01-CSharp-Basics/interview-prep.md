# 01 — C# Basics: Interview Prep

Generated after finishing `01-CSharp-Basics/README.md`. Attempt the questions and exercises on your own first — no peeking at the model answers — then tell Claude you're done so they can be graded.

---

## How to use this file

1. Answer the interview questions out loud or in writing, in your own words, **before** reading the model answer underneath.
2. Do the exercises by actually writing/running the code, not just reasoning about it in your head.
3. When done with a batch, report back what you wrote/answered. Ask for grading — you'll get told what's wrong, what's incomplete, and what to add for long-term retention.

---

## Part A — Interview Questions

### Warm-up (recall)

**Q1. What three things does ".NET" bundle together as a platform?**
> Model answer: A runtime (the CLR) that executes compiled code, a standard library (the BCL) of reusable code, and compilers (C#, F#, VB.NET) that turn source into a form the runtime can run.

**Q2. Is C# compiled or interpreted?**
> Model answer: Neither, purely — it's a two-stage compiled language. Roslyn compiles C# source to IL (platform-independent) ahead of time, then the CLR's JIT compiler compiles each method's IL to native machine code the first time that method is called, caching the result for the process's lifetime.

**Q3. What is IL, and why does C# compile to it instead of straight to machine code?**
> Model answer: IL (Intermediate Language) is a CPU-independent instruction set. Compiling to IL instead of native code is what lets the same DLL run unmodified on different CPU architectures (x64, ARM) and OSes — the JIT does the CPU-specific translation at runtime on whatever machine it actually lands on.

**Q4. What's the practical difference between `const` and `readonly`?**
> Model answer: `const` is a compile-time constant — baked directly into IL at every call site, must be a primitive/string, implicitly static. `readonly` is a runtime constant — can be set in a constructor, can depend on runtime computation, and (unlike `const`) doesn't create a versioning hazard where callers compiled against an old library keep an old baked-in value.

**Q5. What's the difference between `Parse`, `TryParse`, and `Convert.ToInt32` when converting a string to an int?**
> Model answer: `Parse` throws (`FormatException`/`OverflowException`) on bad input and `ArgumentNullException` on `null`. `TryParse` returns a `bool` instead of throwing — preferred for untrusted/user input. `Convert.ToInt32(null)` doesn't throw at all — it returns `0`, which is a subtle, frequently-tested difference from `Parse`.

---

### Core mental model: value types, reference types, stack/heap

**Q6. State the actual rule for where a value type is stored — not the "value types go on the stack" oversimplification.**
> Model answer: A value type is stored wherever it's declared. A local variable that's a struct lives on the stack. A struct that's a field of a class lives inline inside that class's heap allocation. A struct gets its own separate heap allocation only when boxed, captured in a closure, or (implicitly, inline within the array's own single allocation) as an array element.

**Q7. Given:**
```csharp
class Order { public int Total; }
Order o1 = new Order { Total = 100 };
Order o2 = o1;
o2.Total = 999;
Console.WriteLine(o1.Total);
```
**What prints, and why?**
> Model answer: `999`. `o2 = o1` copies the reference (the address), not the object — both variables point at the same heap object, so mutating through `o2` is visible through `o1`.

**Q8. Same question, but with `struct Point { public int X, Y; }` instead of `class Order`, assigning `p2 = p1` then mutating `p2.X`. What prints for `p1.X`?**
> Model answer: The original value, unchanged — assigning a struct copies all its fields into an entirely separate memory slot, so mutating `p2` can never affect `p1`.

**Q9. What is boxing, and name two distinct situations (not just "assigning to `object`") where it happens implicitly.**
> Model answer: Boxing is wrapping a value type in a temporary heap object so it can be treated as `object`/`dynamic`/a non-generic interface — it allocates on the heap and copies the value in. Besides an explicit `object o = i;`, it also happens: (1) storing a value type in a non-generic collection like `ArrayList`, and (2) assigning a struct to a variable typed as an interface it implements (e.g. `IShape shape = new Circle{...}`).

**Q10. Why does mutating a struct through an interface reference sometimes silently "not work"?**
> Model answer: Assigning a struct to an interface-typed variable boxes it. Any method call through that interface reference runs against the boxed copy on the heap, not the original struct variable — so a mutating call through the interface never affects the variable you might still have a handle on.

**Q11. Explain generational GC in your own words — why does .NET's GC split the heap into generations at all?**
> Model answer: The GC assumes most objects die young (Gen 0: new allocations, collected very frequently and cheaply) and only a few survive to be long-lived (Gen 2: rarely collected, most expensive to sweep). Splitting into generations lets it collect the cheap, high-churn Gen 0 constantly without paying the cost of scanning long-lived objects every time. Anything that survives a collection gets promoted to the next generation up.

**Q12. Why does allocating lots of short-lived objects in a hot loop hurt performance, even though the GC is automatic?**
> Model answer: It creates "allocation pressure" — more frequent Gen 0 collections. And if some of those objects are still reachable when a collection happens to run, they get promoted, which pollutes the more expensive Gen 1/2 collections too.

**Q13. Why can't `ref`/`out`/`in` parameters be used inside `async` methods or lambdas that capture them?**
> Model answer: `async` methods and closures both work by having the compiler generate a hidden class/state machine on the heap to hold their state, which can outlive the current stack frame. A `ref`/`out`/`in` parameter is a raw address into a stack frame — capturing it into a heap object that might outlive that frame would leave a dangling reference, so the compiler disallows it.

---

### Primitive types & conversions

**Q14. Why is `decimal` preferred over `double`/`float` for money?**
> Model answer: `double`/`float` are binary floating-point approximations — some ordinary decimal fractions (like `0.1`) can't be represented exactly in binary, causing tiny rounding errors. `decimal` is base-10 internally, trading range/speed for exact decimal precision, which is what money calculations need.

**Q15. `int` is a value type, so how can you call `.ToString()` or `.Equals()` on it, given those are members of `object`?**
> Model answer: It works via implicit boxing — the compiler boxes the `int` into a temporary heap object wrapping it as `object` so the inherited member can run against it.

---

### Operators & control flow

**Q16. What does `order?.Customer?.Address?.City ?? "Unknown"` do, step by step?**
> Model answer: Each `?.` short-circuits the whole expression to `null` the moment any link in the chain is `null` — the remaining `.` accesses after that point are never evaluated. If the whole chain resolves to `null` (any link was missing), `??` supplies `"Unknown"` as a fallback; otherwise the actual city string is used.

**Q17. Why does `int? length = someString?.Length;` compile, when `Length` is a plain non-nullable `int`?**
> Model answer: Because the overall `?.` expression can short-circuit to `null` if `someString` is `null`, the compiler widens the expression's static type to `int?` (nullable) even though `Length` itself is a non-nullable `int`.

**Q18. What's the difference between a `switch` statement and a `switch` expression?**
> Model answer: The classic `switch` statement runs statements per `case` and needs an explicit `break` (no implicit fall-through in C#, unlike C/Java). A `switch` expression (C# 8+) returns a value directly, and must be exhaustive — cover every case or include a `_` discard — or it won't compile.

---

### Strings & arrays

**Q19. Why is `string` concatenation in a loop considered a performance bug, and what's the fix?**
> Model answer: `string` is immutable, so every `+=` allocates a brand-new string and copies the entire accumulated content so far into it — an `O(n²)` cost across `n` iterations. `StringBuilder` mutates an internal buffer in place instead, making the total cost `O(n)`.

**Q20. Why can `ReferenceEquals("abc", "abc")` be `true`, while two runtime-built equal strings can give `ReferenceEquals(...) == false`, even though `==` is `true` for both?**
> Model answer: String literals are cached in an intern pool, so two identical literals actually point to the same heap object — hence `ReferenceEquals` is `true`. Strings built at runtime (concatenation, `new string(...)`) aren't interned by default, so two separately-built-but-equal strings are different heap objects — `ReferenceEquals` is `false`. `==` is unaffected either way because `string` overloads `==` to compare content, not references.

**Q21. When would you choose a jagged array (`int[][]`) over a multi-dimensional array (`int[,]`)?**
> Model answer: When rows don't need to be the same length, or when iteration performance/cache locality per row matters — jagged arrays are generally faster to iterate and more idiomatic in C#. Multi-dimensional arrays are one single contiguous block where every row must be the same length.

---

### Methods

**Q22. Explain the difference between `ref`, `out`, and `in` — specifically, what must be true before the call and after the method returns for each.**
> Model answer: `ref`: caller's variable must already have a value before the call; the method may or may not change it. `out`: caller's variable does *not* need a value beforehand, but the method **must** assign it on every code path before returning. `in`: caller's variable must have a value (like `ref`), but the method is prevented from modifying it at all — it exists purely to avoid copying a large struct.

**Q23. Why is `int.TryParse(s, out int result)` a good example of the `out` pattern specifically, rather than `ref`?**
> Model answer: The caller doesn't have (or need) a value for `result` before the call — they're asking the method to produce one. `out` is exactly for "give me a result, plus tell me whether it succeeded," which is why it doesn't require prior initialization the way `ref` does.

**Q24. What's the subtle versioning bug with optional parameter default values?**
> Model answer: Optional parameter defaults are baked into the *caller's* compiled IL at compile time, not looked up from the library at runtime. If a library changes a default value and ships a new version, callers that were compiled against the old library keep using the old default until they themselves are recompiled against the new one.

**Q25. Is method overload resolution a compile-time or runtime concept? How does that relate to polymorphism?**
> Model answer: Compile-time — the compiler picks which overload to call based on the static (compile-time) type of the arguments. This makes it fundamentally different from polymorphism/virtual dispatch, which resolves which overridden method runs at runtime based on the object's actual type. They're often confused because both involve "which method actually runs," but they're resolved at different stages.

---

### Namespaces, assemblies, access modifiers

**Q26. What's the actual difference between a namespace and an assembly?**
> Model answer: A namespace is a purely logical, compile-time-only grouping to avoid name collisions — it has no runtime existence. An assembly (`.dll`/`.exe`) is the physical unit of deployment and versioning. One assembly can contain many namespaces, and one namespace can span multiple assemblies — they're independent concepts.

**Q27. What's the difference between `protected internal` and `private protected`, and why are they so often confused?**
> Model answer: `protected internal` is the *union* of `protected` and `internal` — visible if EITHER condition holds (a subclass in any assembly, OR any code in the same assembly). `private protected` is the *intersection* — visible only if BOTH hold (must be a subclass AND in the same assembly). They're confused because the modifier names look almost identical, but the underlying logic is opposite (union vs. intersection).

**Q28. What access level does a top-level `class Foo { }` get if you don't write an access modifier at all? Does that match the default for its members?**
> Model answer: No — this is an explicit interview trap. The class declaration itself defaults to `internal` (visible only within its own assembly) when unmarked, while class *members* default to `private` when unmarked. Two different defaults for two different things.

---

### Nullable value types

**Q29. Why can't a plain `int` be `null`, and how does `int?` (`Nullable<int>`) solve that?**
> Model answer: A value type variable *is* its data, stored inline — there's no spare "no value" bit hiding inside a 4-byte `int`. `Nullable<T>` solves it by bundling the value together with an extra `bool HasValue` flag, so the pair `{ HasValue, Value }` can represent "no value" without the underlying `int` itself needing a magic null state.

**Q30. What happens if you access `.Value` on an `int?` whose `HasValue` is `false`?**
> Model answer: It throws `InvalidOperationException`. Always check `HasValue` first, or use `??` for a fallback, or pattern-match (`x is int val`) instead of accessing `.Value` blindly.

---

### Structs vs classes & exceptions

**Q31. What's the official rule of thumb for choosing `struct` over `class`?**
> Model answer: Make something a `struct` only if it's small (roughly ≲16 bytes), logically immutable, and represents a single value. Otherwise default to `class`. Large, mutable structs cause defensive-copy bugs and performance problems.

**Q32. Why should you write bare `throw;` instead of `throw ex;` when rethrowing inside a `catch` block?**
> Model answer: `throw ex;` resets the exception's stack trace to point at that `throw` line, destroying the information about where it originally occurred. Bare `throw;` rethrows the same exception object with its original stack trace intact — important for debugging, and a common senior-level code review flag.

**Q33. Why are exceptions considered inappropriate for expected/normal control flow, like a validation failure?**
> Model answer: Throwing and catching has a real runtime cost — stack unwinding (walking back up the call stack looking for a handler, running `finally` blocks along the way) plus capturing the stack trace. For outcomes that are expected and routine, prefer return values, the Result pattern, or `TryX`-style methods instead of paying that cost on every "failure."

**Q34. `StackOverflowException` vs `OutOfMemoryException` — what causes each, and can you catch either?**
> Model answer: `StackOverflowException` comes from deep/infinite recursion exceeding the fixed per-thread stack size (~1MB default) — it's uncatchable and crashes the process immediately. `OutOfMemoryException` comes from exhausting heap space — technically catchable, but usually still fatal in practice since the process is in a bad state by that point.

---

## Part B — Multiple Choice Questions

Pick one answer before checking. Distractors are deliberate — the explanation matters more than the letter.

**M1. Which statement about C#'s compilation model is correct?**
- A) C# compiles directly to native machine code, like C++.
- B) C# is purely interpreted, like early versions of Python.
- C) C# compiles to IL ahead of time, then the CLR's JIT compiles each method to native code the first time it's called.
- D) C# compiles to IL, and the CLR interprets the IL directly without ever producing native code.
> Answer: C. IL is not itself executed directly — the JIT translates it to real native code per method, on first call, and caches the result. (D) describes what interpretation would look like, which is not what the JIT does.

**M2. Native AOT compared to a normal JIT'd app:**
- A) Produces both IL and native code, falling back to JIT if needed.
- B) Produces only native code, with no IL and no JIT step at runtime — faster startup, but limits reflection-heavy scenarios.
- C) Is identical to ReadyToRun (R2R).
- D) Only works for ASP.NET Core web apps.
> Answer: B. (A) describes R2R, not Native AOT — that's the key distinction the README draws between the two.

**M3. What actually determines where a value type instance is stored in memory?**
- A) Value types always live on the stack, full stop.
- B) Value types live wherever they're declared — stack if a local variable, inline inside a heap object if they're a field of a class, and only get their own separate heap allocation when boxed or captured in a closure.
- C) Value types always live on the heap, just like reference types.
- D) It depends on whether the value type implements an interface.
> Answer: B. (A) is the common oversimplification the README explicitly calls out as wrong.

**M4. Given `IShape shape = new Circle { Radius = 2 };` where `Circle` is a struct implementing `IShape`:**
- A) No boxing occurs, since `Circle` is a small struct.
- B) `Circle` is boxed, because `shape` is typed as an interface, and interface-typed variables are reference types.
- C) Boxing only happens if you explicitly cast to `object`.
- D) `Circle` is boxed only if it has more than 16 bytes of fields.
> Answer: B. Assigning any struct to an interface-typed variable boxes it, regardless of size — this is the "why didn't my struct's field change" trap.

**M5. Which best describes why .NET's GC uses generations (Gen 0/1/2)?**
- A) To let developers manually control which objects get collected first.
- B) Because most objects die young, so cheaply/frequently collecting a "new objects" generation avoids repeatedly rescanning long-lived objects.
- C) Generations exist purely for multi-threading safety.
- D) Gen 0/1/2 correspond to different .NET runtime versions.
> Answer: B.

**M6. `Convert.ToInt32(null)` vs `int.Parse(null)`:**
- A) Both throw `ArgumentNullException`.
- B) Both return `0`.
- C) `Convert.ToInt32(null)` returns `0`; `int.Parse(null)` throws `ArgumentNullException`.
- D) `Convert.ToInt32(null)` throws; `int.Parse(null)` returns `0`.
> Answer: C — a frequently-tested subtle difference called out explicitly in the README.

**M7. `&&` vs `&` on two boolean expressions, where the right-hand side has a side effect (e.g. calls a method):**
- A) Identical behavior in all cases.
- B) `&&` short-circuits — skips evaluating the right side if the left side is already `false`; `&` always evaluates both sides.
- C) `&` short-circuits; `&&` always evaluates both sides.
- D) Both always evaluate both sides; only `||`/`|` differ.
> Answer: B.

**M8. `protected internal` vs `private protected`:**
- A) They're exact synonyms.
- B) `protected internal` is the intersection (must be subclass AND same assembly); `private protected` is the union (subclass OR same assembly).
- C) `protected internal` is the union (subclass OR same assembly); `private protected` is the intersection (subclass AND same assembly).
- D) Neither relates to assemblies at all, only to inheritance.
> Answer: C — the names look similar but the logic is opposite, which is exactly why the README flags this as a common mix-up.

**M9. A top-level class declared as `class Foo { }` with no access modifier:**
- A) Is `public` by default.
- B) Is `private` by default.
- C) Is `internal` by default — visible only within its own assembly.
- D) Won't compile without an explicit modifier.
> Answer: C. Note this is a *different* default than class members, which default to `private`.

**M10. Accessing `.Value` on an `int?` (`Nullable<int>`) whose `HasValue` is `false`:**
- A) Returns `0` silently.
- B) Returns `null`.
- C) Throws `InvalidOperationException`.
- D) Throws `NullReferenceException`.
> Answer: C — a specific, commonly-confused-with-(D) detail.

**M11. Rule of thumb for choosing `struct` over `class`:**
- A) Use `struct` for anything with more than one field.
- B) Use `struct` only for small (~≤16 bytes), logically immutable, single-value data; default to `class` otherwise.
- C) Use `struct` whenever the type will be used in a collection.
- D) Use `struct` for anything that needs inheritance.
> Answer: B. (D) is actually backwards — structs can't inherit from another struct/class.

**M12. Why use bare `throw;` instead of `throw ex;` when rethrowing in a `catch` block?**
- A) `throw ex;` is a compile error.
- B) They're functionally identical; it's a style preference only.
- C) `throw ex;` resets the stack trace to the rethrow point, losing the original error location; bare `throw;` preserves it.
- D) `throw;` is faster at runtime.
> Answer: C.

---

## Part C — Predict the Output

Read each snippet, write down what you think it prints, *then* run it to check. Grouped by concept, easiest first, escalating through that concept's variations before moving to the next concept.

### Value types vs reference types (struct/class copy semantics)

**O1 (easy — baseline struct copy).**
```csharp
struct Point { public int X, Y; }
Point p1 = new Point { X = 1, Y = 2 };
Point p2 = p1;
p2.X = 99;
Console.WriteLine(p1.X);
Console.WriteLine(p2.X);
```
> Output: `1` then `99` — struct assignment copies all fields; `p1` and `p2` are independent after that.

**O2 (easy — baseline reference sharing, the contrasting case).**
```csharp
class Order { public int Total; }
Order o1 = new Order { Total = 100 };
Order o2 = o1;
o2.Total = 999;
Console.WriteLine(o1.Total);
```
> Output: `999` — `o2 = o1` copies the reference, not the object; both point at the same heap object.

**O3 (medium — struct as a field of a class, still inline).**
```csharp
class Order { public Point Location; }
struct Point { public int X, Y; }

var order1 = new Order { Location = new Point { X = 1, Y = 1 } };
var order2 = order1;              // copies the REFERENCE to the Order
order2.Location.X = 42;
Console.WriteLine(order1.Location.X);
```
> Output: `42` — `order2 = order1` copies only the `Order` reference, so both variables point at the same single `Order` object (and thus the same inline `Point`). This looks like "the struct changed for both," but the actual cause is that there was only ever one `Order` allocation to begin with — the struct itself was never independently copied here.

**O4 (medium — passing a struct to a method, by value).**
```csharp
struct Point { public int X, Y; }
void TryModify(Point p) { p.X = 500; }

var point = new Point { X = 1, Y = 1 };
TryModify(point);
Console.WriteLine(point.X);
```
> Output: `1` — passing a struct to a method (without `ref`) passes a copy; `TryModify` mutates its own local copy, which is discarded when the method returns.

**O5 (hard — boxing breaks the value-copy expectation).**
```csharp
struct Point { public int X, Y; }
Point p1 = new Point { X = 1, Y = 1 };
object boxed = p1;              // boxing: copies p1's data into a new heap object
p1.X = 777;                     // mutating the ORIGINAL struct variable
Point p2 = (Point)boxed;        // unboxing: copies out of the box
Console.WriteLine(p2.X);
```
> Output: `1`, not `777` — boxing happened at the moment of `object boxed = p1;` and copied the data *then*. Mutating `p1` afterward has no effect on the already-made box; unboxing later retrieves the value as it was at boxing time.

**O6 (hardest — struct through an interface reference, the classic trap).**
```csharp
interface IShape { double Area(); void Scale(); }
struct Circle : IShape
{
    public double Radius;
    public double Area() => Math.PI * Radius * Radius;
    public void Scale() { Radius *= 2; }
}

var circle = new Circle { Radius = 2 };
IShape shape = circle;   // boxing: shape holds a boxed COPY of circle
shape.Scale();           // mutates the boxed copy, not `circle`
Console.WriteLine(circle.Radius);
```
> Output: `2`, unchanged — `shape` holds a boxed copy created at the assignment. `shape.Scale()` mutates that boxed copy's `Radius`, not the original `circle` variable, which is why the field looks like it silently "didn't change."

### Boxing/unboxing in isolation

**O7 (easy).**
```csharp
object o = 42;   // boxing
int i = (int)o;  // unboxing
o = 100;         // reassigning o — a NEW box, doesn't touch the old one
Console.WriteLine(i);
```
> Output: `42` — unboxing at `(int)o` copied the value out into `i`, a completely separate variable. Reassigning `o` afterward has no effect on `i`.

### Null-conditional (`?.`) and null-coalescing (`??`, `??=`)

**O8 (easy — `??` basic fallback).**
```csharp
int? x = null;
int? y = 5;
Console.WriteLine(x ?? 0);
Console.WriteLine(y ?? 0);
```
> Output: `0` then `5`.

**O9 (medium — `?.` chain short-circuits on the first null link).**
```csharp
Person? person = null;
string? city = person?.Address?.City ?? "Unknown";
Console.WriteLine(city);
```
> Output: `Unknown` — `person` is `null`, so the whole `?.` chain short-circuits to `null` immediately (`.Address` is never evaluated), and `??` supplies the fallback.

**O10 (medium — `?[]` null-conditional indexer).**
```csharp
int[]? numbers = null;
int? first = numbers?[0];
Console.WriteLine(first ?? -1);
```
> Output: `-1` — `numbers` is `null`, so `?[0]` short-circuits to `null` instead of throwing, and `??` substitutes `-1`.

**O11 (harder — `??=` only assigns when currently null, called twice).**
```csharp
List<string>? names = null;
names ??= new List<string>();
names.Add("Alice");
names ??= new List<string>();   // names is NOT null this time
names.Add("Bob");
Console.WriteLine(names.Count);
```
> Output: `2` — the second `??=` sees `names` already has a value (from the first line), so it does nothing; the list from the first assignment is reused, not replaced.

### Strings (immutability, interning, StringBuilder)

**O12 (easy — immutability, a "mutating" call doesn't mutate).**
```csharp
string s1 = "hello";
string s2 = s1.ToUpper();
Console.WriteLine(s1);
Console.WriteLine(s2);
```
> Output: `hello` then `HELLO` — `.ToUpper()` never touches `s1`; it returns a brand-new string.

**O13 (medium — interning affects `ReferenceEquals`, not `==`).**
```csharp
string a = "abc";
string b = "abc";
string c = new string(new[] { 'a', 'b', 'c' });
Console.WriteLine(ReferenceEquals(a, b));
Console.WriteLine(ReferenceEquals(a, c));
Console.WriteLine(a == c);
```
> Output: `True`, `False`, `True` — literals `a`/`b` are interned to the same object; `c` is built at runtime so it's a distinct object; but `==` on `string` always compares content, so it's still `True`.

**O14 (harder — how many objects, not just what's printed).**
```csharp
string result = "";
for (int i = 0; i < 3; i++) result += i;
Console.WriteLine(result);
```
What matters more here than the printed value: how many separate string objects did this loop allocate?
> Output: `"012"`. But the real point: three separate `+=` operations allocated three *new* string objects over the loop (`"0"`, then `"01"`, then `"012"`) — this is exactly the `O(n²)` cost the README warns about, and why `StringBuilder` exists.

### `ref` / `out` parameters

**O15 (easy — `ref` mutates the caller's variable).**
```csharp
void Increment(ref int x) { x++; }
int a = 5;
Increment(ref a);
Console.WriteLine(a);
```
> Output: `6` — `ref` passes the address, so the increment mutates the caller's actual variable.

**O16 (medium — `out` must be assigned on every path, including failure).**
```csharp
bool TryDouble(int input, out int result)
{
    if (input < 0) { result = 0; return false; }
    result = input * 2;
    return true;
}

if (TryDouble(-5, out int r))
    Console.WriteLine(r);
else
    Console.WriteLine($"failed, r = {r}");
```
> Output: `failed, r = 0` — `out` forces `result` to be assigned on every path (including the failure path) before the method returns, so `r` is `0` even though the call "failed."

### Nullable value types (`int?`)

**O17 (easy).**
```csharp
int?[] numbers = { 1, null, 3 };
foreach (var n in numbers)
    Console.WriteLine(n ?? -1);
```
> Output: `1`, `-1`, `3` — `??` substitutes `-1` only for the `null` element.

**O18 (harder — the crash case, on purpose).**
```csharp
int? x = null;
Console.WriteLine(x.Value);
```
> Output: throws `InvalidOperationException` at runtime — `.Value` on an `int?` with `HasValue == false` doesn't return a default, it throws. This is why you check `HasValue`, use `??`, or pattern-match instead of touching `.Value` directly.

### `switch` expressions

**O19 (easy — falls through to the discard).**
```csharp
int result = 5 switch
{
    1 => 10,
    2 => 20,
    _ => -1
};
Console.WriteLine(result);
```
> Output: `-1` — `5` matches none of the explicit arms, so it falls to the `_` discard.

---

## Part D — Exercises

Write and actually run these (a `dotnet-script`, a scratch console project, or any C# REPL/fiddle is fine). Predict the output *before* you run it, then check.

**E1. Value vs reference copy.**
Write both the `Point` struct example and the `Order` class example from section 3 yourself, from memory, without looking at the README. Predict each `Console.WriteLine` output before running.

**E2. Boxing cost, measured.**
Write a loop that inserts 100,000 `int`s into an `ArrayList` and a separate loop inserting 100,000 `int`s into a `List<int>`. Time both with a `Stopwatch`. Explain the difference you measure in terms of boxing.

**E3. The interface-boxing trap.**
Write the `IShape`/`Circle` struct example from section 3 yourself. Then write a small program that assigns a `Circle` to a local variable, assigns that same struct to an `IShape` variable, mutates a field through the `IShape` reference (if your design allows it) or just calls a method through the interface, and print both the original struct's field and what the interface reference reports. Explain in your own words why they diverge.

**E4. String immutability, measured.**
Write the `O(n²)` string-concatenation loop AND the `StringBuilder` version from section 6, both building a 20,000-character result. Time both with a `Stopwatch`. Report the ratio you observe.

**E5. `ref`/`out`/`in`, from scratch.**
Without looking at the README, write: (a) a `Swap` method using `ref`, (b) a `TryDivide(int a, int b, out int result)` method that returns `false` and sets `result = 0` on division by zero, (c) a method taking a `readonly struct` `in` parameter that would fail to compile if you tried to mutate it (write the mutation, confirm it's a compile error, then remove it).

**E6. Access modifier matrix.**
Write a small two-project solution (a class library + a console app referencing it). In the library, declare one class with fields of every access modifier (`private`, `protected`, `internal`, `protected internal`, `private protected`) plus a subclass in the *same* assembly and a subclass in the *console app's* assembly. Try to access each field from each subclass and from unrelated code in each project. Note which ones fail to compile — does it match the table in section 8?

**E7. Nullable value type crash.**
Write code that declares an `int?` set to `null`, and deliberately trigger the `InvalidOperationException` by accessing `.Value` directly. Then fix it three different ways: `HasValue` check, `??`, and pattern matching (`is int val`).

**E8. Exception rethrow bug, caught live.**
Write a method that throws inside a nested call (so the stack trace is non-trivial), catch it one level up and rethrow using `throw ex;`, and print `.StackTrace`. Then change it to bare `throw;` and print `.StackTrace` again. Confirm the line number difference you see matches the README's explanation.

**E9. Struct vs class default value.**
Declare a `struct` and a `class` with the same fields, each as an uninitialized field of a containing class (don't call `new` on either). Print both. Explain why the struct's fields show zeroed values while the class field shows `null`.

**E10. `switch` expression exhaustiveness.**
Write a `switch` expression over an `enum` you define with 4 members, but only handle 3 of them with no `_` discard. Confirm it fails to compile, read the actual compiler error message, then fix it two ways: adding the 4th case, and adding a `_ =>` discard instead.

---

*When you've attempted these, report back what you wrote/answered so it can be graded — including any questions you skipped or exercises where the result surprised you.*

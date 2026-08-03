# C# Intermediate

Status: **Not started**

## Planned coverage
- Collections: `Array`, `List<T>`, `Dictionary<K,V>`, `HashSet<T>`, `Queue`, `Stack`, `LinkedList`, `SortedList<K,V>` — internals & Big-O
- Non-generic legacy collections (`ArrayList`, `Hashtable`) vs generic collections — why generics replaced them (boxing avoidance, type safety)
- Collection interfaces: `ICollection`, `IList`, `IDictionary`, `IEnumerable` — what each contract adds, and why you'd code against the interface instead of the concrete type
- Generics: constraints (`where T : struct/class/new()/BaseClass/Interface`), covariance/contravariance (`in`/`out`), generic methods vs generic classes
- Delegates: declaring/invoking, `Action`, `Func`, `Predicate`, multicast delegates (invocation order, return-value-of-last-only gotcha)
- Anonymous methods (`delegate(...) { }`) vs lambda expressions — historical evolution
- Events: publisher/subscriber pattern, `event` keyword internals (why it's not just "a public delegate field"), declaring & raising events, memory leak risks (subscriber outliving publisher)
- LINQ: deferred vs immediate execution, method vs query syntax, `IEnumerable` vs `IQueryable`, common operators
- LINQ to Objects, anonymous types (`new { X = 1 }`)
- LINQ interview traps, each with a concrete failure case: `Select` (1-to-1) vs `SelectMany` (1-to-many, flattening); `Any()` (short-circuits, translates to `EXISTS`) vs `Count() > 0` (enumerates everything, `SELECT COUNT(*)`); `First`/`FirstOrDefault` vs `Single`/`SingleOrDefault` (uniqueness assertion — `Single` throws on >1 match, catching data-integrity bugs `First` would silently mask); `GroupBy` LINQ-to-Objects (in-memory, hash-based) vs LINQ-to-Entities (translated to SQL `GROUP BY`) — not the same operation; `Join` (inner join) vs `GroupJoin` + `SelectMany` + `DefaultIfEmpty()` (the standard left-join idiom); deferred execution "sees state at enumeration time, not definition time" gotcha (mutate-after-define-before-enumerate); `ToList()` placement pitfalls — multiple enumeration re-running the query each time (`.Count()` then `.ToList()` on the same `IQueryable` = 2 DB round trips) vs materializing too early (forces later `.Where()`/`.Select()` to run client-side)
- Extension methods — own deep topic, not just a LINQ footnote: construction rules (static class, static method, `this` on the first parameter, namespace must be in scope via `using`); what the compiler actually does (pure compile-time static-method-call sugar, zero runtime dispatch — this is *why* extension methods can't be polymorphic); method resolution order (a real instance method always wins over an extension method with the same signature — you can never "override" one via an extension); can't access `private`/`protected` members of the extended type; where to use them (extending types you don't own, fluent/chainable APIs like LINQ itself, keeping EF entities free of presentation-only helpers) vs where not to (anything needing polymorphism/private state — that's a real instance method's job); extension methods vs default interface methods (compile-time/never-overridable vs part of the interface contract/overridable per-implementer)
- PLINQ (`AsParallel()`) — when parallelizing a query pays off vs adds overhead
- Exception handling deep dive: `checked`/`unchecked` contexts, exception hierarchy (`SystemException` vs `ApplicationException`), exception filters, custom exception hierarchies, do's/don'ts (don't catch `Exception` broadly, don't swallow, always `throw;` not `throw ex;`), when to throw vs return
- `yield return` and iterators, custom enumerables
- Partial classes and partial methods — splitting a type across files (designer-generated code + hand code), partial method declaration/optional implementation
- Tuples, `ValueTuple` vs `Tuple`
- Nullable reference types (`#nullable enable`) basics

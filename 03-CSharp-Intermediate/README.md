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
- LINQ to Objects, anonymous types (`new { X = 1 }`), extension methods (how LINQ itself is built from them)
- PLINQ (`AsParallel()`) — when parallelizing a query pays off vs adds overhead
- Exception handling deep dive: `checked`/`unchecked` contexts, exception hierarchy (`SystemException` vs `ApplicationException`), exception filters, custom exception hierarchies, do's/don'ts (don't catch `Exception` broadly, don't swallow, always `throw;` not `throw ex;`), when to throw vs return
- `yield return` and iterators, custom enumerables
- Partial classes and partial methods — splitting a type across files (designer-generated code + hand code), partial method declaration/optional implementation
- Tuples, `ValueTuple` vs `Tuple`
- Nullable reference types (`#nullable enable`) basics

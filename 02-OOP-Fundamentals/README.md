# OOP Fundamentals

Status: **Not started**

## Planned coverage
- Classes & objects, constructor types: default, parameterized, copy, **static constructor** (runs once per type, before first use), **private constructor** (singleton-style, prevents external instantiation)
- Object references vs object state — a variable of a reference type holds a *reference* (the object's identity/location on the heap), not the object itself; the *state* is the field values living at that location. Two variables can alias the same reference (mutating through one is visible through the other) or point to separate objects with equal-looking but independent state.
- Shallow copy vs deep copy — shallow copy (e.g. `MemberwiseClone`) copies value-type fields directly but copies reference-type fields *as references*, so the copy and original still share the same nested objects; deep copy recursively clones those nested objects too, giving fully independent state. Ties directly into copy constructors above and `ICloneable`.
- Encapsulation, properties (auto-props, init-only, computed, `get`/`set` accessor visibility)
- Object initializer syntax (`new Emp { Id = 1, Name = "Alex" }`) vs constructor
- `static` members (fields/methods/properties) vs instance members — memory model, when static constructor fires
- `static class` — compiler-enforced singleton-like container (implicitly sealed + abstract, cannot be instantiated or inherited)
- Inheritance vs composition ("favor composition over inheritance")
- Inheritance types: single, multilevel, hierarchical, and multiple inheritance **via interfaces only** (C# has no multiple class inheritance) — diagram of each
- Access modifiers across inheritance boundaries (`public`/`protected`/`internal`/`protected internal`/`private protected`) — full visibility matrix
- Constructors in an inheritance hierarchy: implicit base call vs explicit `base(...)` constructor chaining; precise execution order for multi-level inheritance (base field initializers → base ctor body → derived field initializers → derived ctor body, base always fully finishes before derived starts); the classic trap — calling a `virtual` method from a base constructor invokes the *derived* override before the derived class's own field initializers have run, so it reads default/null values (why "never call virtual members from a constructor" is standard guidance)
- The `base` keyword beyond constructors — `base.Method(...)` from inside an `override` to call the parent's implementation explicitly (e.g. extend rather than fully replace base behavior), and `base.Member` to disambiguate access to a base-class field/property hidden by `new` in the derived class
- Method hiding (`new`) vs overriding (`override`) — why hiding is a common interview trap (compile-time vs run-time binding)
- Polymorphism: method overriding vs overloading, `virtual`/`override`/`new`/`sealed override`
- Abstraction: abstract classes vs interfaces (deep comparison, default interface methods in C# 8+)
- Interfaces: implementing, **explicit interface implementation** (resolving name collisions across multiple interfaces), interface inheritance, `ISP`
- Sealed classes/methods — restricting further inheritance/overriding
- Object lifecycle, `IDisposable`/`using`/finalizers, `IEquatable`, equality (`==` vs `.Equals` vs `ReferenceEquals`)
- `IComparable<T>`/`IComparer<T>` — custom sort ordering (`CompareTo`)
- Operator overloading (`operator +`, etc.) — which operators can/can't be overloaded
- Indexers (`this[int index]`) — making a type array-like
- Class diagrams (UML) for common OOP relationships (association, aggregation, composition)

# Design Patterns

Status: **Not started**

## Planned coverage
- Creational: Singleton — eager init (CLR guarantees thread-safe static field init, no locking needed, but pays construction cost even if unused) vs lazy init with double-checked locking (why the *first* check avoids locking on the hot path, why the *second* check inside the lock is the actual correctness fix, why `volatile` matters to prevent a thread observing a partially-constructed object via instruction reordering) vs the modern idiomatic `Lazy<T>` (handles the double-checked-locking memory-model subtleties for you); why raw Singleton is discouraged vs a DI-container-managed singleton lifetime (testability, explicit constructor-visible dependencies vs hidden `Instance` access, centralized lifetime control) — then Factory Method, Abstract Factory, Builder, Prototype
- Structural: Adapter, Decorator, Facade, Proxy, Composite, Bridge
- Behavioral: Strategy, Observer (vs C# events), Command, Chain of Responsibility, Mediator (→ MediatR), Template Method, State, Visitor
- .NET-specific patterns: Repository, Unit of Work, Options pattern, Builder pattern in `WebApplicationBuilder`
- Anti-patterns to avoid: Service Locator, God Object, anemic domain model (context for [[20-Clean-Architecture-DDD-CQRS]])
- Each pattern: UML class diagram + C# example + "why would an interviewer ask this"

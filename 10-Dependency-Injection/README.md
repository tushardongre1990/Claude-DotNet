# Dependency Injection

Status: **Not started**

## Planned coverage
- IoC vs DI vs Service Locator — the conceptual difference
- Built-in DI container: `IServiceCollection`, `IServiceProvider`
- Service lifetimes: Singleton vs Scoped vs Transient — with diagrams of object graphs per request
- Captive dependency problem (injecting Scoped into Singleton) and how the container detects/prevents it
- Constructor injection vs property/method injection
- Registering with factories, keyed services (.NET 8+), `TryAdd*` methods
- Third-party containers (Autofac, etc.) — when/why teams swap the built-in one
- Testing implications: DI and mocking (ties into [[17-Testing]])

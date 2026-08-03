# Testing

Status: **Not started**

## Planned coverage
- Testing pyramid (unit → integration → E2E) — diagram and cost/speed tradeoffs
- xUnit fundamentals: `[Fact]`/`[Theory]`, fixtures, `IClassFixture`/`ICollectionFixture`, test lifecycle
- Mocking with Moq/NSubstitute — mocking interfaces, verifying calls, argument matchers
- AAA pattern (Arrange-Act-Assert), FluentAssertions
- TDD workflow (red-green-refactor) and when it pays off vs when it doesn't
- Integration testing ASP.NET Core: `WebApplicationFactory<T>`, `TestServer`, in-memory test host
- Testcontainers for real dependencies (DB, message broker) in integration tests
- Test data builders / object mothers vs fixed fixtures
- Code coverage — usefulness and its limits as a metric

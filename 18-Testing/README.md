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
- Code coverage — usefulness and its limits as a metric (100% coverage with no meaningful assertions proves nothing — the gap mutation testing exists to catch)
- Mutation testing (e.g. Stryker.NET) — deliberately mutates production code and checks whether the test suite actually fails, catching tests that execute a line but never assert on its outcome; snapshot/approval testing (Verify) — asserting against a stored "golden" output for large/complex results instead of hand-writing every field's assertion. Niche compared to the above — mention if asked about test-suite rigor, not a primary study area

# Caching Strategies

Status: **Not started**

## Planned coverage
- In-memory caching (`IMemoryCache`) — eviction policies, size limits, absolute vs sliding expiration
- Distributed caching (`IDistributedCache`) with Redis — why needed in multi-instance deployments (diagram: sticky in-memory cache problem across load-balanced instances)
- Output caching / response caching middleware vs application-level caching
- Cache-aside, write-through, write-behind patterns
- Cache invalidation strategies (the hardest problem — "there are only two hard things in CS")
- Distributed cache stampede/thundering herd and mitigations (locking, jittered expiry)
- `HybridCache` (newer .NET abstraction combining in-memory + distributed)

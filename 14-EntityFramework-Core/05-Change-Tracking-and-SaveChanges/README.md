# Change Tracking & SaveChanges

Status: **Not started**

## Planned coverage
- The Change Tracker: how EF Core snapshots entity state
- Entity states: `Added`, `Unchanged`, `Modified`, `Deleted`, `Detached` (state diagram)
- How `SaveChanges()` batches INSERT/UPDATE/DELETE, and orders them by dependency graph
- `AsNoTracking()`/`AsNoTrackingWithIdentityResolution()` — when and why
- Attaching detached entities (`Attach`, `Update`, `Entry(...).State = ...`) — common in disconnected/API scenarios
- `SaveChangesAsync` internals, `ChangeTracker.DetectChanges()` cost
- Concurrency tokens tie-in (detailed in [[08-Transactions-and-Concurrency]])

---
name: source-plugin-resilience
description: "Fans out media source lookups across providers in parallel, emits results as they arrive, times each provider out on its own and opens a circuit after repeated failures. Use when sources are gathered one after another, when one dead provider stalls the list, or when a scraper keeps failing."
---

# Source & Plugin Resilience

## Parallel fan-out, progressive delivery
- Never loop providers sequentially. Query all of them at once with `withThrowingTaskGroup` (Swift)
  or `coroutineScope { async }` (Kotlin).
- Hand each batch to the UI the moment it lands (`AsyncStream` / `Flow`); the slowest addon must not
  hold back the fastest.

## Isolation and a circuit breaker
- A hard per-provider timeout of 6–8 s, so one dead scraper cannot stall the aggregate.
- Count consecutive failures per provider. Three in a row opens the circuit for 5 minutes; a single
  success closes it again. While open, the provider is skipped without a request.

```swift
func fetchAllSources(query: String, providers: [SourceProvider]) -> AsyncStream<[StreamLink]> {
    AsyncStream { continuation in
        Task {
            await withTaskGroup(of: [StreamLink].self) { group in
                for provider in providers where !CircuitBreaker.shared.isOpen(provider.id) {
                    group.addTask {
                        do {
                            let links = try await withTimeout(seconds: 7) {
                                try await provider.fetchLinks(for: query)
                            }
                            CircuitBreaker.shared.recordSuccess(provider.id)
                            return links
                        } catch {
                            CircuitBreaker.shared.recordFailure(provider.id)
                            return []
                        }
                    }
                }
                for await batch in group where !batch.isEmpty {
                    continuation.yield(batch)
                }
            }
            continuation.finish()
        }
    }
}
```

- Also report why a provider answered nothing, so an empty list is explained rather than silent.

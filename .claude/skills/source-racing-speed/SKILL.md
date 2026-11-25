---
name: source-racing-speed
description: "Races mirror links to find the first playable one, probes with a small range request, cancels the losers, and ranks providers by measured latency and success rate. Use when the list is long but the first play is slow, or when the same provider keeps winning."
---

# Source Racing & Adaptive Provider Ranking

## Race for the first playable link
- Resolve mirrors concurrently in a `TaskGroup`.
- Probe each with `HEAD`, or `Range: bytes=0-1024` when the host refuses HEAD, and accept the first
  answer with HTTP 200/206 and a video or playlist content type.
- Start playback with the winner and `cancelAll()` the rest; a probe that finishes later is worthless.

```swift
func racePlayableStreams(candidates: [StreamCandidate]) async -> StreamCandidate? {
    await withTaskGroup(of: StreamCandidate?.self) { group in
        for candidate in candidates {
            group.addTask {
                guard await probeStreamHealth(url: candidate.url, headers: candidate.headers) else {
                    return nil
                }
                return candidate
            }
        }
        for await winner in group {
            if let valid = winner {
                group.cancelAll()
                return valid
            }
        }
        return nil
    }
}
```

## Remember who is fast, here
- Keep an exponential moving average of response latency and a success rate per provider id, on disk.
- Sort providers by that history on the next search, so the list opens in milliseconds with the
  sources that actually work on this device and this network.
- A provider that fails repeatedly is rested (circuit breaker), not merely ranked low.

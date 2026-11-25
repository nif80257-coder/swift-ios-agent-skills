---
name: ktor-client-performance
description: "Configures one shared Ktor client for a media app: platform engines, connection pool, HTTP/2, strict timeouts, retries and streamed responses instead of buffering whole payloads. Use when source fetching is slow, connections are exhausted, or a large response is loaded into memory."
---

# Ktor Client Performance

- **One client for the whole app.** A singleton `HttpClient` shared by every scraper and provider;
  never build one per request, because the engine and its pool are per client.
- **Native engines.** `Darwin` (NSURLSession) on iOS, `OkHttp` on Android; set
  `httpMaximumConnectionsPerHost = 6` and leave HTTP/2 (and HTTP/3 where the engine offers it) on.
- **Strict timeouts** for manifests and APIs: connect 8 s, socket 12 s, request 15 s.
- **Retry with backoff** on server errors, twice at most: `retryOnServerErrors(maxRetries = 2)` plus
  `exponentialDelay()`.
- **Stream large bodies.** Use `prepareGet { … }.execute { response -> … }` and read the channel;
  `.bodyAsText()` on a big manifest or a playlist costs memory and time.

```kotlin
fun createOptimizedHttpClient(): HttpClient = HttpClient(Darwin) {
    engine {
        configureSession {
            httpMaximumConnectionsPerHost = 6
            waitsForConnectivity = false
        }
    }
    install(HttpTimeout) {
        connectTimeoutMillis = 8_000
        socketTimeoutMillis = 12_000
        requestTimeoutMillis = 15_000
    }
    install(HttpRequestRetry) {
        retryOnServerErrors(maxRetries = 2)
        exponentialDelay()
    }
}
```

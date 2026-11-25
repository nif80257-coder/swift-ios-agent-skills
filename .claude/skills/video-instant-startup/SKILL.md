---
name: video-instant-startup
description: "Cuts the wait before the first frame: pre-warming AVURLAsset on the details screen, preroll when the item is ready, required headers baked into the asset, and the right stalling policy per network. Use when playback takes seconds to start or the first tap after browsing feels slow."
---

# Instant Video Startup & Pre-Warming

## Pre-warm before the tap
- On the details screen (and when a source is chosen) create the `AVURLAsset` and load
  `["playable", "duration", "tracks"]` in the background, so the tap only replaces an item.
- When `item.status == .readyToPlay`, call `player.preroll(atRate: 1.0)` so the first frame is ready
  in under about 300 ms.

```swift
actor StreamPrewarmer {
    private var cachedAssets: [URL: AVURLAsset] = [:]

    func prewarm(url: URL, headers: [String: String]) async -> AVPlayerItem {
        let options = headers.isEmpty ? nil : ["AVURLAssetHTTPHeaderFieldsKey": headers]
        let asset = cachedAssets[url] ?? AVURLAsset(url: url, options: options)
        cachedAssets[url] = asset

        _ = try? await asset.load(.isPlayable, .duration)
        return AVPlayerItem(asset: asset)
    }
}
```

## Headers and stalling policy
- Pass `Referer`, `Origin` and `User-Agent` through `AVURLAssetHTTPHeaderFieldsKey` **at asset
  creation**. A missing header is a 403 and a slow fallback, not a fast failure.
- `automaticallyWaitsToMinimizeStalling = true` for unstable links; `false` with
  `preferredForwardBufferDuration = 2.0` when fast startup on 5G or fibre matters more than the
  occasional stall.
- Keep one pre-warm per URL and drop the cache when the list changes; assets hold connections.

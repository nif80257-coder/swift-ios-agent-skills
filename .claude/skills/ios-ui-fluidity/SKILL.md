---
name: ios-ui-fluidity
description: "Keeps an iOS media UI at full frame rate: no parsing or decoding on the main thread, downsampled images, debounced search with cancellation, and prefetch before the user reaches the end. Use when scrolling stutters, the first frame after a tap is late, or typing feels heavy."
---

# iOS UI Fluidity

## Main thread protection
- **Zero parsing on main.** HTML scraping, regex, M3U/JSON decoding and sorting run on background
  actors or `Task.detached(priority: .userInitiated)`; only small immutable `Sendable` structs are
  published to `@MainActor`.
- **Downsample images** to their display size with `CGImageSourceCreateThumbnailAtIndex` and keep
  them in an `NSCache` with a memory limit. Never bind a 4K poster URL to a small grid cell.

## Search and prefetch
- Debounce live search by 300 ms and cancel the in-flight task on every keystroke; a late answer for
  an old query must never replace the current one.
- When the user reaches about 75% of a row, prefetch the next page so the items exist before the
  finger arrives.

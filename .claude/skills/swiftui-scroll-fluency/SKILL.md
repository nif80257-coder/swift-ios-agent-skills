---
name: swiftui-scroll-fluency
description: "Keeps SwiftUI catalogs and episode lists at 120 Hz: lazy containers, stable identity, equatable cards, no shadows in cells, and bounded caches. Use when scrolling drops frames, when a grid rebuilds on every update, or before shipping a large catalog."
---

# 120 Hz SwiftUI Scroll & List Fluency

## Containers and identity
- `LazyVStack` / `LazyVGrid` inside a `ScrollView` for anything that can exceed about 15 rows; an
  eager `VStack` builds every cell up front.
- Stable, unique identities (`media.id`). Never `\.self` on values that can repeat, and never an
  array index: a shifted identity invalidates every row below it.
- Make card models `Equatable` and apply `.equatable()` to the card view, so a parent update does not
  re-run the body of untouched cells.

## GPU and memory ceilings
- No `.shadow(radius:)` or `.blur()` inside scrolling cells. Use a pre-rendered gradient, or
  `.drawingGroup()` only after profiling shows the overdraw is worth it.
- Cap `URLCache` (`memoryCapacity = 50_000_000`, `diskCapacity = 300_000_000`) and clear decoded
  image caches on `UIApplication.didReceiveMemoryWarningNotification`.
- Cache posters downsampled to their display size; a full-resolution decode per cell is the usual
  cause of a janky grid.

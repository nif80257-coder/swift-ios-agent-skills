---
name: subtitle-sync-parsing
description: "Loads Arabic subtitles without mojibake and looks up the active cue in logarithmic time: UTF-8 first then Windows-1256, cues parsed off the main thread, binary search on every time tick. Use when Arabic subtitles show as symbols, or when subtitles cost frames during playback."
---

# Advanced Subtitle Sync & Arabic Encoding

## Encoding detection
- Many Arabic SRT files are Windows-1256, not UTF-8, and arrive as mojibake or `???`.
- Try `String(data: data, encoding: .utf8)` first. If it returns nil or contains `\u{FFFD}`, fall back
  to Windows Arabic:

```swift
let windowsArabic = String.Encoding(
    rawValue: CFStringConvertEncodingToNSStringEncoding(
        CFStringEncoding(CFStringEncodings.windowsArabic.rawValue)
    )
)
let text = String(data: data, encoding: .utf8)
    ?? String(data: data, encoding: windowsArabic)
    ?? ""
```

## Cue lookup
- Parse SRT/VTT/ASS into a sorted array of `(start: TimeInterval, end: TimeInterval, text: String)`
  on a background thread, once per file.
- On each periodic time observer tick, find the active cue with a **binary search** (`O(log n)`), not
  a linear scan; a full scan per tick is what makes subtitles cost frames.
- Handle overlapping cues by returning every cue whose range contains the time, and keep cue text out
  of `@State` until it changes.

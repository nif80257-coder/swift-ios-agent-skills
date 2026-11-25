---
name: instant-search-fluency
description: "Builds a local normalized search index that answers within one frame on every keystroke, with Arabic letter folding and strict cancellation of stale remote results. Use when typing in search feels laggy, when Arabic queries miss obvious titles, or when old results overwrite new ones."
---

# Instant Search & Arabic Normalization

## A local index, answered in one frame
- Keep an in-memory normalized index of cached catalogs, watchlist and history; local results appear
  in under 16 ms per keystroke, before any addon answers.
- Normalize both sides before matching:
  - Alef variants `أ إ آ` → `ا`, Teh Marbuta `ة` → `ه`, Alef Maksura `ى` → `ي`.
  - Strip Tashkeel (`\u{064B}`–`\u{065F}`) and tatweel `ـ`.

```swift
extension String {
    func normalizedForSearch() -> String {
        let withoutTashkeel = self.unicodeScalars.filter {
            !(0x064B...0x065F).contains($0.value)
        }
        return String(String.UnicodeScalarView(withoutTashkeel))
            .lowercased()
            .replacingOccurrences(of: "[أإآ]", with: "ا", options: .regularExpression)
            .replacingOccurrences(of: "ة", with: "ه")
            .replacingOccurrences(of: "ى", with: "ي")
            .trimmingCharacters(in: .whitespacesAndNewlines)
    }
}
```

## Cancellation and stale answers
- Hold the active remote search `Task` and call `cancel()` on every query change.
- Tag each request with a `UUID` or a counter; discard any callback whose id is not the current one,
  so a slow addon cannot replace fresher results.
- Rank exact matches above prefixes, prefixes above contains, and give a small bonus for rating.

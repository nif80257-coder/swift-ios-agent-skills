---
name: swiftdata-migration
description: "Moves watchlists, downloads and history from UserDefaults into SwiftData safely: idempotent one-time migration, unique media ids, files on disk, and background model actors. Use when a list or download must survive restarts, or when UserDefaults has grown into a database."
---

# SwiftData Migration & Persistence

- **Idempotent, one time.** Guard on a flag (`hasMigratedToSwiftData_v1`) and set it only after
  `modelContext.save()` succeeds, so a crash mid-way simply runs again.
- **Schema.** `@Model` classes with `@Attribute(.unique)` on a stable media id (`mediaId`) prevent
  duplicates. Store relative file names (`URL.lastPathComponent`), never blobs, for thumbnails and
  offline video; the bytes live in `Application Support` or `Caches`.
- **Never on the main actor.** Bulk imports and migrations run inside a `@ModelActor` so scrolling
  keeps its frame budget.

```swift
import SwiftData
import Foundation

@ModelActor
actor MediaPersistenceActor {
    func migrateFromUserDefaultsIfNeeded() throws {
        let key = "hasMigratedToSwiftData_v1"
        guard !UserDefaults.standard.bool(forKey: key) else { return }

        if let data = UserDefaults.standard.data(forKey: "legacy_watchlist"),
           let items = try? JSONDecoder().decode([LegacyMediaDTO].self, from: data) {
            for dto in items {
                let record = WatchlistItem(mediaId: dto.id, title: dto.title, addedAt: .now)
                modelContext.insert(record)
            }
            try modelContext.save()
        }
        UserDefaults.standard.set(true, forKey: key)
    }
}
```

- Keep the old keys one release, then delete them; the migration flag is the only thing that tells
  you a second pass is unnecessary.

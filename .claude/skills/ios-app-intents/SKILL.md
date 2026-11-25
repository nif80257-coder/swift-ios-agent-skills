---
name: ios-app-intents
description: "Exposes playback and search to Siri and Shortcuts with App Intents, AppEntity queries over the media library, and zero-setup App Shortcuts phrases. Use when the app should answer 'resume watching', be startable by voice, or surface titles to Spotlight."
---

# App Intents, Siri & Shortcuts

- Conform to `AppIntent` for the actions that matter: `PlayMediaIntent`, `SearchMediaIntent`,
  `ResumeLastWatchedIntent`.
- Use `@Parameter` with an `AppEntity` (`MediaEntity`) and an `EntityQuery` backed by the stored
  watchlist and history, so Siri and Spotlight autocomplete real titles.
- Set `openAppWhenRun = true` for playback, and route the work through one deep-link coordinator
  instead of duplicating navigation inside the intent.
- Register zero-setup phrases in an `AppShortcutsProvider`, e.g.
  `"Resume watching in \(.applicationName)"` and `"Search \(.applicationName) for \(\.$query)"`.
- Call `AppShortcuts.updateAppShortcutParameters()` whenever favourites or the watchlist change.
- For the full protocol surface (entities, parameters, snippets, on-screen awareness, testing) read
  the project's `app-intents` skill and its references.

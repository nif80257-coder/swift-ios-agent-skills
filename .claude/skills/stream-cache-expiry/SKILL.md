---
name: stream-cache-expiry
description: "Manages signed and expiring stream URLs: reads expiry from query parameters, refreshes shortly before they die, and purges temporary segments and orphaned files on launch. Use when a cached link suddenly returns 403, or when the app's storage keeps growing."
---

# Signed URL Expiration & Cache Lifecycle

## Expiring links
- Parse the expiry from query parameters when caching a resolved stream link: `exp`, `expires`,
  `token_expires` (epoch seconds, or a seconds-from-now value — check the magnitude).
- Invalidate a cached link five minutes before it dies and silently fetch a fresh URL from the parent
  addon, so the viewer never meets a 403 at the moment they press play.
- Keep the addon and the request that produced a link with it, or a refresh has nothing to ask.

## Storage hygiene
- On launch, run a low-priority cleanup (`Task.detached(priority: .utility)`): temporary HLS segments,
  expired metadata caches, orphaned thumbnails and partial downloads older than 7 days.
- Delete partial download folders for jobs no longer in the list — a cancelled transfer leaves them.
- Log what the cleanup removed, so a viewer reporting missing files is easy to answer.

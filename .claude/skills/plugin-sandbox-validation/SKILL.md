---
name: plugin-sandbox-validation
description: "Validates third-party addon manifests and responses without crashing, rejects unsafe URL schemes before they reach a player, and contains a broken plugin to its own logs. Use when adding community plugins, when a malformed manifest freezes the app, or before passing a scraped URL to playback."
---

# Addon & Plugin Sandbox Validation

## Manifest and response validation
- Decode every third-party manifest and catalog response with a lenient decoder: `decodeIfPresent`,
  safe defaults, unknown keys ignored. A missing field is normal, not exceptional.
- Validate the manifest before installing: `id`, `name`, `version`, `resources`, `types`, and the
  catalogs it declares. Refuse anything that cannot be read rather than storing a broken entry.

## URL sanitization
- Accept only `https://` (and `http://` on loopback, only when the user whitelisted a local server).
  Reject `file://`, `javascript:` and anything with credentials before it reaches a player or a client.
- Never execute code from an addon beyond the sandboxed, reviewed runtime that already exists.

## Fault containment
- One plugin's malformed JSON, missing field or HTTP 500 must produce a diagnostic line and nothing
  else: no crash, no freeze, no empty screen without a reason.
- Record per-plugin failures with the plugin's name so the sources screen can say which one failed and
  why; the rest of the list stays usable.

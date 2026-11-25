---
name: libmpv-ffmpeg-ios
description: "Keeps libmpv on iOS correct and leak-free: teardown order, hardware decoding, cache limits and custom Picture in Picture through AVSampleBufferDisplayLayer. Use when a libmpv player crashes on close, blocks the main thread, runs out of memory, or needs PiP."
---

# libmpv & FFmpeg on iOS

## Lifecycle, in this exact order
1. Unobserve every mpv property (`mpv_unobserve_property`).
2. Free the render context (`mpv_render_context_free`) on the GL/render thread, before the view goes.
3. Call `mpv_terminate_destroy(mpv)` on a dedicated background serial queue, never on main, or the
   main thread deadlocks.

- Drain `mpv_wait_event` on its own background thread; the main thread never waits on mpv.
- Pass `Unmanaged.passUnretained(self).toOpaque()` carefully and nil every pointer in `deinit`.

## Decoding and buffers
- Hardware decoding explicitly: `mpv_set_option_string(mpv, "hwdec", "videotoolbox")`.
- `vo=libmpv` when the app renders through its own Metal/OpenGL context.
- Cap demuxer memory for phones: `demuxer-max-bytes=50MiB`, `demuxer-max-back-bytes=20MiB`.

## Custom Picture in Picture
- Route frames into an `AVSampleBufferDisplayLayer` when PiP starts, and feed it through
  `AVPictureInPictureController.ContentSource(sampleBufferDisplayLayer:playbackDelegate:)`.
- `AVAudioSession` category `.playback`, and `UIBackgroundModes` must include `audio`.
- Sync the layer's `CMTimebase` with mpv's `time-pos`, so PiP scrubbing and play/pause stay honest.

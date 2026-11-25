---
name: avfoundation-hls-pip
description: "Tunes AVFoundation HLS playback: adaptive bitrate tiers, PiP lifecycle, stall detection and exponential-backoff recovery. Use when an AVPlayer item stalls, when PiP closes itself, when a cellular stream burns data, or when a playback error needs a resume-and-retry."
---

# AVFoundation HLS, ABR & Error Recovery

## Adaptive bitrate
- Never leave `preferredPeakBitRate` unmanaged on cellular. Expose tiers and set them on
  `playerItem.preferredPeakBitRate`: Auto `0`, 1080p `8_000_000`, 720p `3_500_000`, 480p `1_500_000`.
- `preferredForwardBufferDuration = 20.0` balances instant startup with stall prevention.
- Watch `AVPlayerItemNewAccessLogEvent` for `indicatedBitrate`, `observedBitrate` and `numberOfStalls`.

## Picture in picture
- Check `AVPictureInPictureController.isPictureInPictureSupported()` before creating one.
- Keep a strong reference to the controller for as long as playback runs: releasing it ends PiP.
- Implement `restoreUserInterfaceForPictureInPictureStopWithCompletionHandler` to present the player
  again before calling `completionHandler(true)`.

## Stall and error recovery
- Observe `AVPlayerItemFailedToPlayToEndTime`, `AVPlayerItemPlaybackStalled` and KVO `item.status == .failed`.
- On `NSURLErrorTimedOut` / `NSURLErrorNetworkConnectionLost` keep `currentTime()`, rebuild the
  `AVPlayerItem` from the same asset, seek back, and retry up to 3 times at 1 s, 2 s, 4 s.

```swift
import AVFoundation

final class ResilientHLSPlayer {
    private(set) var player = AVPlayer()
    private var retryCount = 0
    private let maxRetries = 3

    func configure(url: URL, peakBitRate: Double = 0, headers: [String: String] = [:]) {
        let options = headers.isEmpty ? nil : ["AVURLAssetHTTPHeaderFieldsKey": headers]
        let asset = AVURLAsset(url: url, options: options)
        let item = AVPlayerItem(asset: asset)
        item.preferredPeakBitRate = peakBitRate
        item.preferredForwardBufferDuration = 20.0
        player.replaceCurrentItem(with: item)
    }

    func recoverFromError(url: URL, headers: [String: String] = [:]) async {
        guard retryCount < maxRetries else { return }
        let savedTime = player.currentTime()
        retryCount += 1
        let delay = UInt64(pow(2.0, Double(retryCount - 1))) * 1_000_000_000
        try? await Task.sleep(nanoseconds: delay)
        configure(url: url, headers: headers)
        await player.seek(to: savedTime, toleranceBefore: .zero, toleranceAfter: .zero)
        player.play()
    }
}
```

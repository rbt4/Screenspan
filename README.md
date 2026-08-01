# ScreenSpan

ScreenSpan is an experimental Android TV projection-mapping player for a corner-mounted projector.

It is designed to:

- receive a video URL from Stremio as an external player;
- keep the original movie inside a calibrated physical screen;
- use the unused raw projector beam for generated left/right peripheral extensions;
- calibrate from an Android phone through a temporary local webpage;
- save the room mapping so normal movie nights do not require repeated setup.

## Prototype workflow

1. Install `ScreenSpan-v0.1.0-test.apk` on Chromecast with Google TV.
2. Disable the projector's automatic keystone, screen fit, obstacle avoidance and digital shrinking.
3. Open ScreenSpan. A calibration pattern and local phone URL/QR code appear.
4. Open that address on a phone connected to the same Wi-Fi.
5. Take one photo and tap the four beam markers followed by the four physical screen corners.
6. Save the mapping and hide calibration.
7. In Stremio, enable **Always start video in external player** and choose ScreenSpan.

The phone does not need an APK. It is only a browser-based calibration controller.

## Effect modes

- **Off** — calibrated centre movie only.
- **Ambient** — soft colour and motion outside the screen.
- **Cinema Extend** — dimmed, blurred edge imagery across the overspill.
- **Panorama** — stronger experimental edge stretching.

## Installation without Play Store

The initial APK is distributed as a GitHub Actions artifact. The simplest Chromecast transfer options are:

- **Send Files to TV:** install it from Google Play on both phone and Chromecast, send the APK, then open it with a file manager;
- **Downloader:** enter the APK download address on the Chromecast;
- **ADB:** enable Developer options and USB/network debugging, then run `adb install -r ScreenSpan-v0.1.0-test.apk` from a computer.

Because test releases use the same signing key and package ID, later APKs should install with `-r` or by opening the newer APK without erasing saved calibration.

## Prototype limitations

- The side view is generated from the normal movie frame; it is not authentic studio-created ScreenX footage.
- Stremio sources that expose a reachable HTTP/HLS/DASH URL are the first target. Some torrent-localhost, DRM, codec, subtitle or audio-track cases may fail in v0.1.0.
- Subtitles are not yet projection-warped.
- The projector must remain physically fixed for saved calibration to remain accurate.
- The included prototype signing key is public and must be replaced before any production release.

## Source packaging

The Android source archive is split into base64 text parts under `source/` because this repository was initially published through the connected GitHub API. The workflow reconstructs the verified archive, runs tests, builds a signed release APK and uploads it as an artifact.

# ScreenSpan

ScreenSpan is an experimental Android phone + Google TV projection-mapping player for a fixed corner-mounted projector.

The same APK runs in two modes:

- **Phone:** camera-based Smart Scan, Stremio handoff, TV discovery, stream relay and update control.
- **Google TV / Chromecast:** projection mapping, generated peripheral extension and video playback.

## Normal movie-night flow

After the room is calibrated once:

1. Open ScreenSpan on the Chromecast.
2. Open Stremio on the Android phone.
3. Start a movie and choose **ScreenSpan** as the external player.
4. The phone automatically discovers ScreenSpan TV and sends the stream.
5. The saved projection map keeps the original movie inside the physical screen and fills the raw overspill with generated peripheral imagery.

No IP address, repeated corner tapping or projector-menu switching should be required during normal use.

## One-time room setup

1. Install the same ScreenSpan APK on the phone and Chromecast.
2. Disable projector auto-keystone, auto-fit, obstacle avoidance and digital shrinking. Leave the projector in raw full-beam mode afterward.
3. Open ScreenSpan on both devices while they are on the same home network.
4. On the phone, tap **Smart Scan** and take one photograph containing the complete projected beam and physical screen.
5. Confirm the automatically detected screen outline. Manual four-corner adjustment is available only as a fallback.

Calibration and effect settings are stored locally on the Chromecast and retained across correctly signed updates.

## First Chromecast install using only an Android phone

Android intentionally does not let an ordinary app silently install itself onto a different device. The first Chromecast install therefore needs one transfer/helper method.

### Route A — wireless debugging from the phone

Use an Android ADB client such as Bugjaeger when the Google TV device exposes **Wireless debugging**:

1. On Google TV, open **Settings → System → About** and press **Android TV OS build** repeatedly to enable Developer options.
2. Open **Developer options → Wireless debugging → Pair device with pairing code**.
3. On the phone, pair Bugjaeger using the displayed address, port and code.
4. Connect to the Google TV device and install the ScreenSpan APK saved on the phone.
5. Approve the debugging and installation prompts on the television.

No Windows, Mac or Linux computer is required.

### Route B — file-transfer helper

When Wireless debugging is unavailable, install a file-transfer helper such as **Send Files to TV** on the phone and Google TV, send the APK, and open it on the television. This requires the helper app on the TV but avoids ADB.

## Future updates

ScreenSpan checks the latest stable GitHub Release in the background. When a newer version is available, the phone shows **Update phone + Chromecast**.

One tap performs the hard work:

1. The phone starts the Chromecast update first.
2. Both devices download the exact versioned APK and matching SHA-256 file.
3. ScreenSpan verifies HTTPS metadata, exact asset names, SHA-256, package identity, newer version code and signing certificate.
4. Android displays its required **Install** confirmation on each device.
5. The existing installation is replaced without clearing calibration or preferences.

The first self-update on each device may also ask to allow ScreenSpan as an installation source. That permission is device-specific and normally needs to be granted only once.

Android may prevent automatic reopening after an update. When that occurs, reopen ScreenSpan normally; the update and saved data are still intact.

## Release signing

Permanent releases use package ID `com.rbt4.screenspan` and a private PKCS#12 signing key. The encrypted key archive may be retained in the source payload, but its password must exist only as the GitHub Actions repository secret:

```text
SCREENSPAN_KEYSTORE_PASSWORD
```

The release workflow:

- reconstructs checksum-verified source;
- runs unit tests and Android lint;
- compiles the release APK;
- verifies package metadata, update permission and signing certificate;
- publishes exactly `ScreenSpan-vX.Y.Z.apk` and `ScreenSpan-vX.Y.Z.apk.sha256`.

A release branch named `release/vX.Y.Z` triggers publishing after the source version has been advanced to the same version.

Do not publish future updates with a different signing key. Android will reject them as updates, protecting installed copies from an unrelated signer.

## Stream handling and local-network safety

- Directly reachable HTTP/HTTPS streams are sent to the Chromecast.
- Phone-local Stremio streams are relayed through a foreground service.
- The relay uses random in-memory capability URLs and only permits the exact root stream and HLS resources ScreenSpan approved. It is not an arbitrary LAN proxy.
- Phone-to-TV control requests require the automatically advertised ScreenSpan pairing token and are bounded by time, header and response-size limits.
- Calibration photographs are deleted after analysis.

## Effect modes

- **Off:** calibrated centre movie only.
- **Ambient:** soft colour and motion outside the screen.
- **Cinema Extend:** dimmed, blurred edge imagery across the overspill.
- **Panorama:** stronger experimental edge stretching.

## Honest limitations

- Peripheral imagery is generated from the normal movie frame. It is not authentic studio-created ScreenX side footage.
- Actual Smart Scan accuracy depends on the room lighting, screen border and camera view.
- Codec, Dolby Vision, DRM, subtitle, audio-track and torrent-localhost behaviour varies by Stremio source and Chromecast model.
- Subtitles are not yet projection-warped.
- The projector and screen must remain physically fixed for saved calibration to stay accurate.
- Continuous integration verifies source integrity, tests, lint, package metadata and signatures; it cannot replace physical testing on the user's exact phone, Chromecast, projector and room.

## Source packaging

The Android source is reconstructed from checksum-verified base64 payloads and small reviewable compatibility patches because the repository was initially published through the connected GitHub API. CI fails if any source payload checksum, test, lint rule, package check or APK signature verification fails.

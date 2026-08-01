# ScreenSpan 0.2 — Smart Flow prototype

ScreenSpan turns one severely off-axis projector beam into a calibrated centre movie plus generated peripheral extension. One APK runs in two modes:

- **Chromecast / Google TV:** projection mapper, Stremio external player, local control server.
- **Android phone:** automatic screen scanner, Stremio handoff bridge and localhost stream relay.

## Normal use

1. Leave ScreenSpan open on Chromecast with projector auto-keystone disabled.
2. Open Stremio on the phone.
3. Play a movie and choose **ScreenSpan** as the external player.
4. The phone discovers the TV, relays local Stremio streams when necessary, and starts playback.

No Stremio navigation on the distorted projector output is required.

## Smart Scan

Smart Scan projects four high-saturation fiducials and a structured low-contrast field. The phone takes one photo. OpenCV then:

1. detects the four raw-beam fiducials;
2. calculates camera-to-projector homography;
3. searches edge contours for the physical screen quadrilateral;
4. ranks candidates using convexity, rectangularity, border response, size and centrality;
5. converts the selected screen corners into projector coordinates;
6. shows one red outline for confirmation.

If confidence is low, the beam is still solved automatically and the user taps only the four physical screen corners.

## Prototype limitations

- The side image is generated from movie edges; it is not studio-authored ScreenX footage.
- The phone and TV must be on the same non-isolated Wi-Fi network.
- ScreenSpan must be open on Chromecast in this prototype.
- The relay is optimized for HTTP video and HLS. Unusual Stremio sources may fail.
- Automatic physical-screen detection depends on visible screen edges or reflectance contrast.
- The included signing key is public and for testing only.

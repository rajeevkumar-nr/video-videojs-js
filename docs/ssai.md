# SSAI Guide

This guide explains what customers need to know when using the Video.js tracker with server-side ad insertion (SSAI), with a focus on AWS MediaTailor.

## Supported SSAI Integrations

- AWS MediaTailor

## What Customers Need To Provide

Customers should already have:

- a working Video.js player integration
- valid New Relic streaming credentials
- a real SSAI playback URL from their ad stitching provider

## Activation

There are two ways to activate the MediaTailor tracker:

**Option A — `mediatailor: true`** (existing approach, backward-compatible):

```javascript
const tracker = new VideojsTracker(player, { mediatailor: true });
```

**Option B — `adTracking: 'mt'`** (new, self-contained shorthand):

```javascript
const tracker = new VideojsTracker(player, { adTracking: 'mt' });
```

`adTracking: 'mt'` implies `mediatailor: true` automatically, so there is no need to pass both. It also restricts the tracker so that no IMA, Freewheel, DAI, or generic ad tracker can be created — useful when you want to make the intent explicit in your code.

If you need custom CDN config alongside the explicit mode, pass both:

```javascript
const tracker = new VideojsTracker(player, {
  mediatailor: { adSegmentPrefix: '/your-path/' },
  adTracking: 'mt',
});
```

## What The Tracker Does Automatically

For MediaTailor streams, the tracker automatically:

1. Detects whether the manifest format is HLS or DASH.
2. Detects whether playback is VOD or LIVE from player state.
3. Parses manifests to discover ad breaks and distinguish ad segments from content segments.
4. Sends ad break, ad start, quartile, and ad end events.
5. Enriches ad metadata when tracking data is available.

## Custom CDN / Custom Domain

If you have configured CDN segment prefixes in the AWS MediaTailor console, `mediatailor: true` is still all you need. The tracker detects ad segments automatically using the AWS-recommended `/tm/` ad-segment path convention.

If your CDN ad-segment prefix uses a non-standard path (not `/tm/`), pass the path as an override:

```javascript
const tracker = new VideojsTracker(player, {
  mediatailor: { adSegmentPrefix: '/your-path/' }
});
```

## Supported MediaTailor Scenarios

The current implementation supports:

- HLS VOD
- HLS LIVE
- DASH VOD
- DASH LIVE
- multiple ads inside a single break when MediaTailor exposes pod structure

## Live Refresh Behavior

For LIVE playback, the tracker follows manifest-derived refresh hints.

- HLS: live cadence is derived from `EXT-X-TARGETDURATION`
- DASH: live cadence is derived from `minimumUpdatePeriod`
- fallback: if neither hint is available, the tracker uses an internal default interval

For VOD playback, the tracker does not continuously poll the manifest. It performs one tracking metadata fetch after the first playable manifest is parsed.

## Customer Expectations In New Relic

Customers should expect the tracker to report:

- ad break start and end events
- ad start and ad end events
- ad quartiles
- ad metadata such as title and creative id when available
- `adPartner = aws-mediatailor` for MediaTailor ad events

Some metadata can arrive after playback has already started if it is filled in from the MediaTailor tracking endpoint.

## Common Integration Requirements

- The player source should be the actual stitched SSAI playback URL.
- The player tech must be able to play the provided source format.

## Troubleshooting

If MediaTailor tracking does not activate, verify:

1. Either `mediatailor: true` or `adTracking: 'mt'` is passed in the tracker options.
2. Segment requests are reaching the player (check the Network tab).
3. If using a custom CDN ad-segment path, verify `adSegmentPrefix` matches the path configured in the AWS MediaTailor console.

## Samples

- [samples/media-tailor-lab.html](../samples/media-tailor-lab.html) for MediaTailor testing across HLS/DASH and VOD/LIVE scenarios

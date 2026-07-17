# iOS Carrier Bundles Archive

A public, versioned archive of **iOS carrier bundles** extracted from official Apple IPSW firmware images. Every iOS build gets its own snapshot — nothing is ever overwritten — so you can diff carrier settings across releases and track how operators (and Apple) change them over time.

## Repository layout

```
carrier-bundles/
  <iOS version>-<build>.<devices>/     e.g. 27.0-24A5380h.V53_V54_V57OS/
    <Carrier>_<cc>.bundle/             e.g. vivo_br.bundle, ATT_NR_US.bundle
      Info.plist
      carrier.plist                    ← the main carrier configuration
      overrides_*.plist                ← per-device overrides
      signatures/
```

- **One folder per iOS build.** Older builds are kept forever; new builds are added alongside them.
- Folder names encode `iOSversion-buildID.deviceBoards` (the device set the IPSW was published for).
- Bundle names encode `Carrier_countrycode` (ISO 3166-1 alpha-2, lowercase; US carriers use `_US`).

## Current coverage

| iOS build | Bundles | Notes |
|---|---|---|
| 26.5.1 (23F81) | 691 | |
| 26.5.2 (23F84) | 691 | |
| 26.6 (23G5028e → 23G5065a) | 691 | 4 beta builds |
| 27.0 (24A5355q → 24A5380h) | 689 → 681 | bundle count shrinking — Apple consolidating carriers |

## What's in a carrier bundle?

Carrier bundles are how Apple ships operator-specific settings inside iOS: APNs, VoLTE/VoWiFi/5G feature flags, RCS configuration, carrier name strings, tethering rules, visual voicemail, emergency-call behavior, and more. Diffing two versions of the same bundle shows exactly what an operator enabled or changed between iOS releases.

Tip — most `.plist` files here are Apple *binary* plists. To read or diff them:

```sh
plutil -convert xml1 -o - carrier.plist        # macOS
plistutil -i carrier.plist                     # Linux (libplist)
```

## How this archive is built

An automated pipeline watches Apple's OTA/IPSW catalogs, downloads each new build, extracts the carrier bundles (using [blacktop/ipsw](https://github.com/blacktop/ipsw)) and commits them here as a new versioned folder. Related project: [carrier-bundle-ios](https://github.com/VCTGomes/carrier-bundle-ios).

## Legal

The files in this archive are the property of **Apple Inc.** and/or the respective carriers. They are redistributed here unmodified, solely for **interoperability research, network engineering and archival purposes**, extracted from firmware images Apple distributes publicly and free of charge. No ownership is claimed over this content, and no license is granted for it.

If you are a rights holder and want content removed, open an issue and it will be taken down promptly.

## Acknowledgements

- [blacktop/ipsw](https://github.com/blacktop/ipsw) — IPSW download/extraction tooling
- [ios-rcs](https://cupboardunderscore.github.io/ios-rcs/) — inspiration for making carrier data publicly browsable

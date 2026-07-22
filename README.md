# SuperCmd-v2-releases

Public distribution channel for [SuperCmd](https://supercmd.sh).

- **`appcast.xml`** — the Sparkle update feed. The app reads it from
  `https://raw.githubusercontent.com/SuperCmdLabs/supercmd-v2-releases/main/appcast.xml`
  (baked into `Info.plist` as `SUFeedURL`).
- **Releases** — each GitHub release carries the notarized `SuperCmd-<version>.dmg`
  that the matching appcast `<enclosure>` points at.

## Publishing a release

Releases are not assembled by hand. From the `supercmd-swift` repo:

```
./scripts/build-and-release.sh --publish
```

That builds a Release archive, signs it with Developer ID, notarizes and
staples it, EdDSA-signs the DMG with the Sparkle private key, creates the
GitHub release here, and commits the new `<item>` into `appcast.xml`.

Every enclosure must carry a `sparkle:edSignature` produced by the Sparkle
private key whose public half is in the app's `SUPublicEDKey`. An unsigned or
mis-signed enclosure is rejected by the updater at download time.

## Note on the `1.0.0` / `1.0.2` tags

Those two releases predate the Sparkle setup and were uploaded manually. Both
contain a build whose `SUPublicEDKey` is still the `REPLACE_WITH_SUPUBLICEDKEY`
placeholder, so a copy installed from either one cannot verify updates. They
are deliberately **not** referenced by `appcast.xml`; the first real feed entry
will come from `--publish`.

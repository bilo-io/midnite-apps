# Midnite Studio — electron-updater feed

`latest-mac.yml` goes here, committed by the release flow alongside each published release. The
app fetches it at

```
https://raw.githubusercontent.com/bilo-io/midnite-apps/main/midnite-studio/feed/latest-mac.yml
```

which is the `generic` publish provider configured in the app's `electron-builder.yml`.

## Why a file in a directory, and not the GitHub provider

electron-updater's GitHub provider finds the manifest by asking a repository for its *latest
release* and reading the `latest-mac.yml` attached to it. This repository distributes several
apps, so its latest release is whichever app shipped most recently — Midnite Studio would
routinely be handed a sibling app's update manifest. A per-app directory has no such ambiguity.

## Not the same thing as `../version.json`

Two feeds, two readers, and they are not interchangeable:

| File | Read by | Written by |
| --- | --- | --- |
| [`../version.json`](../version.json) | [`install.sh`](../install.sh), to resolve "latest" | [`release-feed.yml`](../../.github/workflows/release-feed.yml), automatically |
| `latest-mac.yml` (here) | electron-updater, in the running app | the release flow, **by hand** |

Both describe the same release, so they must move together. A release that updates one and not
the other leaves either the installer or the in-app updater pinned to the previous version.

Empty until the first release — see the note in [the app README](../README.md).

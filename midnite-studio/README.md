# Midnite Studio

A desktop workspace for the whole loop around a repository: a GitKraken-inspired git client at
its centre, with an integrated terminal and agent roster, an embedded browser, and the forge
(PRs, checks, reviews) in the same window.

This directory holds Midnite Studio's **downloads and release notes**. The source is private;
bugs and requests are filed in [this repo's tracker](#issues).

> **No public release yet.** Midnite Studio is pre-1.0 and packaging is still landing, so
> [`version.json`](version.json) carries `"version": null` and the installer below will tell you
> as much rather than downloading anything. Watch
> [releases](https://github.com/bilo-io/midnite-apps/releases) for the first one.

## Install

### macOS (Apple Silicon) — recommended

```sh
curl -fsSL https://raw.githubusercontent.com/bilo-io/midnite-apps/main/midnite-studio/install.sh | sh
```

The script downloads the latest release and installs it to `/Applications`. Installing with
`curl` (rather than a browser) means macOS never quarantines the app, so it opens normally on
first launch — no "unverified app" popup. Read the script first if you like:
[`install.sh`](install.sh).

**Updating:** when the app tells you a new version is available, re-run the same command — it
replaces the app in place, and keeps the old copy until the new one verifies.

Options: `MIDNITE_STUDIO_VERSION=0.3.1` installs a specific version;
`MIDNITE_STUDIO_NO_OPEN=1` skips launching the app afterwards.

### macOS — manual download

If you'd rather take the `.dmg` from the [releases page](https://github.com/bilo-io/midnite-apps/releases)
in a browser, macOS will quarantine it and block the first launch with *"Apple could not verify
Midnite Studio is free of malware"* (the builds aren't notarized yet). To open it anyway, either:

1. Double-click the app (dismiss the popup), then go to **System Settings → Privacy & Security**,
   scroll down, and click **Open Anyway** (the button appears for about an hour after the blocked
   attempt), **or**
2. Clear the quarantine flag in a terminal:

   ```sh
   xattr -dr com.apple.quarantine "/Applications/Midnite Studio.app"
   ```

> On macOS 15 (Sequoia) and later the old *right-click → Open* trick no longer works for
> unnotarized apps — use one of the two options above.

### Windows and Linux

Not built. Midnite Studio ships **macOS arm64 only** for now: the app depends on native modules
that would need rebuilding per-platform, and Apple Silicon is the only target the build is
verified on. There is no Intel (x64) macOS build either.

## Downloads

Releases are tagged `midnite-studio/vX.Y.Z` — this repo distributes several apps, so every tag
names the app it belongs to. Each release carries two assets:

| Asset | Use |
| --- | --- |
| `midnite-studio-<version>-arm64.zip` | What `install.sh` downloads |
| `midnite-studio-<version>-arm64.dmg` | Manual drag-to-Applications install |

Because tags are namespaced, **`releases/latest` is not necessarily Midnite Studio** — it is
whichever app in this repo shipped most recently. [`version.json`](version.json) is the
authoritative "latest stable" for this app, and what the installer reads.

## Issues

Found a bug? [Open one](https://github.com/bilo-io/midnite-apps/issues/new?template=bug.yml) and
pick **midnite-studio** in the App dropdown — that applies the `app: midnite-studio` label and
puts it on the board's Midnite Studio view.

- [Open bugs and requests](https://github.com/bilo-io/midnite-apps/issues?q=is%3Aissue+is%3Aopen+label%3A%22app%3A+midnite-studio%22)

## Changelog

See [CHANGELOG.md](CHANGELOG.md).

# Midnite Apps

The launcher for the midnite suite: it lists every midnite app, installs and updates each one as
its own small bundle with its own Dock icon, and opens them. It ships in lockstep with
[Midnite Studio](../midnite-studio/).

> **Pre-1.0 test builds.** Releases here may be ephemeral test builds that are removed again.

## Install

### macOS (Apple Silicon)

```sh
curl -fsSL https://raw.githubusercontent.com/bilo-io/midnite-apps/main/midnite-launcher/install.sh | sh
```

The script downloads the latest release and installs `Midnite Apps.app` to `/Applications`.
Installing with `curl` (rather than a browser) means macOS never quarantines the app. Read the
script first if you like: [`install.sh`](install.sh). Then open Midnite Apps and install the apps
you want from their tiles.

## Issues

File them in this repo with the `app: midnite-launcher` label.

# Changelog — Midnite Studio

All notable changes to Midnite Studio are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project adheres to
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

This file is the **public mirror** of the changelog in the private source repo
(`bilo-io/midnite-studio`); the release flow copies the released section across when it
publishes a `midnite-studio/vX.Y.Z` tag here.

## [Unreleased]

Pre-release. Nothing has shipped publicly yet — the first tagged build will appear here.

## [0.0.3] - 2026-10-21

Covers everything since 0.0.1; the 0.0.2 test release shipped without notes.

### Added
- **Report an Issue** from the new Help menu or the command palette. A "Copy report" button puts the title and body on the clipboard.
- Media ▸ Video lists its templates in the explorer, and a new project can start from one.
- Midnite Apps shows the upcoming apps as "coming soon" tiles in one consistent icon style.

### Changed
- **Security hardening (Phase 91):**
  - Git arguments are guarded against option injection, and a remote can only be one you have configured.
  - The interactive-rebase helper no longer generates a shell script.
  - DevTools are off in release builds, and every window follows the Electron security checklist.
  - Logs redact more kinds of secret, including PEM keys and AWS, Google, Stripe, npm and Slack tokens.
  - `install.sh` and the launcher check each download's SHA-256 checksum and its code signature before installing it.
- Browser and Vanguard sit in the launcher's suite applications group.

### Fixed
- Agents and Studio find `claude`, `codex` and other agent CLIs when opened from Finder or the launcher. They no longer report that the command "is no longer on disk".
- Issues filed in-app keep their `app: midnite-studio` label instead of falling to `needs-triage`.
- Without the GitHub CLI, Report an Issue opens a prefilled midnite-apps issue form.
- Each suite app's Dock icon matches its launcher tile.
- Installing or updating an app from the launcher no longer fails with "quit it first" after the app has quit. Studio's background terminal process was being mistaken for the app.

## [0.0.1] - 2026-09-14

### Added
- GitKraken-inspired git graph with lanes, commit inspector, ref badges, and interactive graph actions.
- Integrated terminal broker with session persistence across window reloads and detached execution.
- Agent roster with automated execution, real-time activity feeds, and model/session configuration.
- Embedded web browser with multi-tab browsing, split view, and devtools integration.
- Forge integration for GitHub pull requests, checks, reviews, and status tracking.
- System health and doctor setup with automatic toolchain detection for Homebrew, Node.js, pnpm, and Moon.
- Disk usage and system resource monitor with RAG status indicators and flyout telemetry.
- User-level skills management option accessible via the Midnite menu.
- Full two-pane Notes manager with Monaco code editor, Markdown preview, and persistent disk storage.

[Unreleased]: https://github.com/bilo-io/midnite-apps/commits/main/midnite-studio
[0.0.1]: https://github.com/bilo-io/midnite-apps/releases/tag/midnite-studio/v0.0.1
[0.0.3]: https://github.com/bilo-io/midnite-apps/releases/tag/midnite-studio/v0.0.3

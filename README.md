# midnite apps

Public **downloads** and **issue tracker** for the midnite family of desktop apps. The source
repositories are private; this one carries the parts users need — installers, release notes,
and somewhere to report bugs.

## Apps

| App | What it is | Install | Open issues |
| --- | --- | --- | --- |
| [**Midnite Studio**](midnite-studio/) | A desktop workspace around a repository — git client, terminal, agents, embedded browser and forge | [`install.sh`](midnite-studio/#install) | [`app: midnite-studio`](https://github.com/bilo-io/midnite-apps/issues?q=is%3Aissue+is%3Aopen+label%3A%22app%3A+midnite-studio%22) |

> **midnite** — the Claude Code multitask orchestrator — is still served from its own repo,
> [bilo-io/midnite-app](https://github.com/bilo-io/midnite-app). Its published `curl … | sh`
> URL points there and people have already run it, so moving it would break installs that
> exist. When that changes, [onboarding it here](docs/adding-an-app.md) is a drop-in.

## Layout

One directory per app, named for the source repository it distributes:

```
midnite-apps/
├── apps.json                 the registry every workflow reads
├── midnite-studio/           one directory per app, named for its source repo
│   ├── README.md             install instructions for that app
│   ├── CHANGELOG.md          that app's release notes
│   ├── install.sh            the curl … | sh installer
│   └── version.json          update feed — what "latest stable" means for that app
└── .github/
    ├── ISSUE_TEMPLATE/       one App dropdown, fed from apps.json
    └── workflows/            app labelling + release-feed maintenance
```

Nothing about a second app is special-cased: adding one is a new directory plus an entry in
[`apps.json`](apps.json). The recipe is in [docs/adding-an-app.md](docs/adding-an-app.md).

## Releases

GitHub releases belong to a repository, not a directory, so with several apps sharing one repo
every tag carries the app it belongs to:

```
midnite-studio/v0.3.1
midnite/v0.12.1
```

That keeps two apps from fighting over `v1.0.0`, at one cost worth knowing about: **`releases/latest`
here means "the newest release of *any* app", which is not what an installer wants.** So each app's
[`version.json`](midnite-studio/version.json) is the authoritative "latest stable" for that app,
and [`release-feed.yml`](.github/workflows/release-feed.yml) rewrites it automatically whenever a
release is published. Installers read that file, never `releases/latest`.

## Issues

File everything here — one tracker for every app, with the app itself recorded as a label.

- [**Report a bug**](https://github.com/bilo-io/midnite-apps/issues/new?template=bug.yml) ·
  [**Request a feature**](https://github.com/bilo-io/midnite-apps/issues/new?template=feature.yml)
- Both forms open with a required **App** dropdown.
  [`issue-app-label.yml`](.github/workflows/issue-app-label.yml) reads the answer and applies the
  matching `app: <id>` label, so the tag is never left to the reporter remembering to set it.
  Anything it can't place gets `needs-triage` instead of a wrong label.
- Changing the dropdown on an existing issue re-labels it — the workflow runs on edits too and
  clears the previous `app:` label first.

### Board

Every issue lands on the **[midnite apps](https://github.com/users/bilo-io/projects)** project
board, which carries one saved view per app (filtered on that app's label) alongside an
all-apps view and a triage view. A new app gets a new view; the board itself does not change
shape.

## Repository conventions

- **`apps.json` is the source of truth.** The issue-form dropdown, the labels, the release-feed
  workflow and the docs all derive from it. Change it there and the rest follows.
- **Installers resolve versions from `version.json`,** never from `releases/latest` — see
  [Releases](#releases).
- **Nothing here is built.** This repo holds distribution artifacts and text; every app is built
  in its own private source repo and publishes into this one.

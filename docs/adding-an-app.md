# Onboarding another app

Six steps. Nothing in this repo special-cases an individual app, so this is additive throughout —
you are never editing what an existing app depends on.

The example below onboards a hypothetical `midnite-forge`. Substitute its id everywhere; the id
is the **source repository's name**, lowercase, and it is used as the directory name, the label
suffix, and the release-tag prefix.

## 1 · Register it

Add an entry to [`apps.json`](../apps.json):

```json
{
  "id": "midnite-forge",
  "name": "Midnite Forge",
  "tagline": "One line, used in the root README table.",
  "source": "bilo-io/midnite-forge",
  "tagPrefix": "midnite-forge/v",
  "bundle": "Midnite Forge.app",
  "platforms": ["macos-arm64"],
  "artifacts": {
    "zip": "midnite-forge-${version}-arm64.zip",
    "dmg": "midnite-forge-${version}-arm64.dmg"
  },
  "installer": "midnite-forge/install.sh",
  "versionEnvVar": "MIDNITE_FORGE_VERSION"
}
```

`artifacts` must match the source repo's electron-builder `artifactName`, and `bundle` its
`productName` — the installer verifies both by name, so a mismatch fails loudly at install time
rather than quietly shipping a broken bundle.

## 2 · Create its directory

```sh
mkdir midnite-forge
cp midnite-studio/install.sh midnite-forge/install.sh
```

Then edit the copied installer's header block — `REPO` stays the same, but `FEED`, `APP`, `EXE`
and the `MIDNITE_*_VERSION` / `MIDNITE_*_NO_OPEN` variables all change, as do the `asset` and
`tag` lines. Add alongside it:

- **`README.md`** — install instructions. Start from [`midnite-studio/README.md`](../midnite-studio/README.md);
  keep the platform section honest about what actually builds.
- **`CHANGELOG.md`** — the public mirror of the source repo's changelog.
- **`version.json`** — seed it unreleased; the release workflow takes over from the first tag:

  ```json
  {
    "app": "midnite-forge",
    "channel": "stable",
    "version": null,
    "releasedAt": null,
    "notesUrl": null
  }
  ```

Finally, add a row to the table in the [root README](../README.md).

## 3 · Create its label

```sh
gh label create "app: midnite-forge" \
  --repo bilo-io/midnite-apps \
  --color BFD4F2 \
  --description "Issues for the Midnite Forge app"
```

The colour is per-app so the board reads at a glance; keep them distinct.

## 4 · Add it to the issue forms

Add the id as an option in the `app` dropdown of both
[`bug.yml`](../.github/ISSUE_TEMPLATE/bug.yml) and
[`feature.yml`](../.github/ISSUE_TEMPLATE/feature.yml), above the
`Something else / not sure` entry.

This is the one place the app list is duplicated: issue forms are static YAML and cannot read
`apps.json`. [`issue-app-label.yml`](../.github/workflows/issue-app-label.yml) validates the
answer against `apps.json` regardless, so a dropdown option you forget to register lands as
`needs-triage` rather than as a bogus label.

## 5 · Give it a board view

```sh
PROJECT_ID=$(gh api graphql -f query='
  query { user(login: "bilo-io") { projectV2(number: 6) { id } } }
' --jq '.data.user.projectV2.id')

VIEW_ID=$(gh api graphql -f query='
  mutation($p: ID!) {
    createProjectV2View(input: {projectId: $p, name: "Midnite Forge", layout: BOARD_LAYOUT}) {
      projectV2View { id }
    }
  }
' -f p="$PROJECT_ID" --jq '.data.createProjectV2View.projectV2View.id')

gh api graphql -f query='
  mutation($v: ID!, $f: String!) {
    updateProjectV2View(input: {viewId: $v, filter: $f}) { projectV2View { id name filter } }
  }
' -f v="$VIEW_ID" -f f='is:open label:"app: midnite-forge"'
```

The filter is set in a **second** call on purpose — `createProjectV2View` takes no `filter`
argument, only `updateProjectV2View` does. Both need a token with the `project` scope
(`gh auth refresh -h github.com -s project,read:project`); note that the device flow authorises
whichever account **the browser** is signed into, not the one `gh` names, so sign in as
`bilo-io` (a private window is the easy way) before pasting the code.

## 6 · Point the source repo at this one

In the app's own (private) repo:

- **`electron-builder.yml`** — if the app auto-updates, point it at that app's feed directory
  using the **`generic`** provider:

  ```yaml
  publish:
    - provider: generic
      url: https://raw.githubusercontent.com/bilo-io/midnite-apps/main/midnite-forge/feed
      channel: latest
  ```

  Not `provider: github`. Its GitHub provider resolves the manifest through `releases/latest`,
  which in this repo is whichever app shipped most recently — it would serve your app another
  app's update. Create `midnite-forge/feed/` here to receive `latest-mac.yml`; copy
  [midnite-studio's](../midnite-studio/feed/README.md) as the pattern.

- **The release flow** — cut the GitHub Release here against a namespaced tag
  (`midnite-forge/v1.2.3`), attach the `.zip` and `.dmg`, copy the released changelog section
  into `midnite-forge/CHANGELOG.md`, and commit `latest-mac.yml` into `midnite-forge/feed/`.

[`release-feed.yml`](../.github/workflows/release-feed.yml) then sees the tag, recognises the app
from `apps.json`, and rewrites `midnite-forge/version.json` so the installer resolves the new
version. **`latest-mac.yml` is not automatic** — the two feeds must be moved together, or the
installer and the in-app updater disagree about what "latest" is.

## Board automation, once

Not per-app — done once for the repo, and **not done yet**. The board exists and its views are
filtered correctly, but nothing puts issues *onto* it automatically: a user-owned project lives
outside this repository, and Actions' built-in `GITHUB_TOKEN` has no write access to it. Until
one of these is set up, issues have to be added to the board by hand.

- **Built-in, no token — the easy one.** On the [project](https://github.com/users/bilo-io/projects/6),
  **⋯ → Workflows → Auto-add to project**, filter `is:issue is:open repo:bilo-io/midnite-apps`,
  Enable. There is no API to turn this on, which is the only reason it isn't already done.
- **In code.** [`add-to-project.yml`](../.github/workflows/add-to-project.yml) does the same job
  and is already wired to the board — `PROJECT_URL` is set. It waits on the one thing that has to
  be minted by hand:

  ```sh
  gh secret set PROJECTS_TOKEN --repo bilo-io/midnite-apps --body '<a PAT with the project scope>'
  ```

  Pick this over the built-in only if you want the automation reviewable in git.

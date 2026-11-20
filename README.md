# songlands — Violin Songlands build repo

This repository is PUBLIC and contains **no game source code** — by
design. It holds only this README and the GitHub Actions workflows that
build **Violin Songlands** (Godot 4.7.2, package
`com.eventideapps.songlands`):

- `.github/workflows/android-play-aab.yml` — builds the Google Play
  AAB on a Linux runner. Manual dispatch only.
- `.github/workflows/ios-testflight.yml` — builds the iOS export and
  uploads it to TestFlight from a macOS runner. Manual dispatch only,
  and each run needs Eric's specific approval first (it spends shared
  GitHub Actions macOS minutes).

## Where the game lives

The Godot project itself lives in a private local repository on the
studio machine (`~/workspace/violin-notes-game/prototype/`). It is
**never** pushed to this repository.

## How a build gets its source

Each workflow downloads the project as a tarball at build time:

```sh
curl -L -o project.tar.gz "${{ vars.PROJECT_TARBALL_URL }}"
mkdir project && tar -xzf project.tar.gz -C project --strip-components=1
```

`PROJECT_TARBALL_URL` is a repository **variable** pointing at a
link-shared Google Drive file, `songlands-project.tar.gz` — a
`git archive` snapshot of the local project (top-level folder
`songlands/`, stripped on unpack). To ship newer code: build a fresh
tarball from the local repo, replace the Drive file's content in place
(same file → same URL), then dispatch the workflow.

Signing material (the Play upload keystore, the App Store Connect API
key) lives only in this repo's Actions **secrets** — never in the
tarball and never in any file in this repo.

## ⚠ Standing warning

**Never push the game tree — or this repo's pre-2026-11-20 history —
to this repository.** The history was purged on 2026-11-20 (replaced
with a fresh single-commit history) precisely so the game source would
not become public. If game files ever appear in a commit here, treat
it as a leak: stop, purge again, and tell Eric.

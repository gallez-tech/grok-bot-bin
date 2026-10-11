# grok-bot-bin (unofficial)

Unofficial Arch Linux package for the [Grok Bot](https://cursor.com) desktop
agent. Not affiliated with or endorsed by SpaceXAI/Cursor. It repackages the
official `.deb` published by upstream.

Grok Bot's built-in updater does not support Linux, so CI checks Cursor's
update API daily and builds the matching Linux `.deb` into an Arch package.

## Install from a GitHub release

Every release carries a version-free package name, so this command always
fetches the newest build — no version bump in the prompt:

```sh
curl -fLO https://github.com/gallez-tech/grok-bot-bin/releases/latest/download/grok-bot-bin-x86_64.pkg.tar.zst
sudo pacman -U grok-bot-bin-x86_64.pkg.tar.zst
```

Download first — do not pass the URL straight to `pacman -U`. With the
default `SigLevel = Required` in `pacman.conf`, pacman insists on fetching
`<url>.sig` for remote packages, and these releases carry no signature file.

The versioned asset (`grok-bot-bin-<version>-1-x86_64.pkg.tar.zst`) is still
attached; grab it from the [releases
page](https://github.com/gallez-tech/grok-bot-bin/releases) if you want a
pinned version.

## Local build

```sh
makepkg -si
```

## Automation

- `.github/workflows/update.yml` — daily check of the linux-x64 update feed;
  bumps `PKGBUILD` / `.SRCINFO` and tags `v<pkgver>` when upstream moves
- `.github/workflows/build.yml` — builds on `v*` tags (or manual dispatch) and
  attaches the `.pkg.tar.zst` to the GitHub release, plus a version-free alias
  (`grok-bot-bin-x86_64.pkg.tar.zst`) that `/releases/latest/download/` serves

## Notes

- `/usr/bin/grok-bot` is a thin wrapper that sets
  `FONTCONFIG_NO_CHECK_CACHE_VERSION` (Chromium bundles an older fontconfig
  than current Arch)
- User data lives in `~/.config/Grok Bot/` and is untouched by upgrades

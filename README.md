# grok-bot-bin (unofficial)

Unofficial Arch Linux package for the [Grok Bot](https://cursor.com) desktop
agent. Not affiliated with or endorsed by SpaceXAI/Cursor. It repackages the
official `.deb` published by upstream.

Grok Bot's built-in updater does not support Linux, so CI checks Cursor's
update API daily and builds the matching Linux `.deb` into an Arch package.

## Install from a GitHub release

Grab the newest `.pkg.tar.zst` from the [releases
page](https://github.com/gallez-tech/grok-bot-bin/releases), then install it
locally:

```sh
curl -LO https://github.com/gallez-tech/grok-bot-bin/releases/download/v0.47.0/grok-bot-bin-0.47.0-1-x86_64.pkg.tar.zst
sudo pacman -U grok-bot-bin-0.47.0-1-x86_64.pkg.tar.zst
```

Download first — do not pass the URL straight to `pacman -U`. With the
default `SigLevel = Required` in `pacman.conf`, pacman insists on fetching
`<url>.sig` for remote packages, and these releases carry no signature file.

## Local build

```sh
makepkg -si
```

## Automation

- `.github/workflows/update.yml` — daily check of the linux-x64 update feed;
  bumps `PKGBUILD` / `.SRCINFO` and tags `v<pkgver>` when upstream moves
- `.github/workflows/build.yml` — builds on `v*` tags (or manual dispatch) and
  attaches the `.pkg.tar.zst` to the GitHub release

## Notes

- `/usr/bin/grok-bot` is a thin wrapper that sets
  `FONTCONFIG_NO_CHECK_CACHE_VERSION` (Chromium bundles an older fontconfig
  than current Arch)
- User data lives in `~/.config/Grok Bot/` and is untouched by upgrades

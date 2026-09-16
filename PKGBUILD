# Unofficial repackage of the official grok-bot .deb (electron-builder Debian package).
#
# To bump: update pkgver, _commit, and sha256sums, then run `makepkg -si`.
# Version oracle (Grok Bot's built-in updater does not support Linux):
#   https://api2.cursor.sh/updates/api/update/linux-x64/sand/0.0.0/<uuid>/stable
# Linux .deb:
#   https://downloads.cursor.com/grokbot/stable/<commit>/linux/x64/grok-bot_<version>_amd64.deb
pkgname=grok-bot-bin
pkgver=0.53.0
pkgrel=1
pkgdesc='Grok Bot desktop agent'
arch=('x86_64')
url='https://cursor.com'
license=('LicenseRef-Proprietary')
depends=(
  'gtk3'
  'libnotify'
  'nss'
  'libxss'
  'libxtst'
  'xdg-utils'
  'at-spi2-core'
  'util-linux-libs'
  'libsecret'
  'hicolor-icon-theme'
  'alsa-lib'
)
optdepends=('libappindicator-gtk3: tray support')
provides=("${pkgname%-bin}")
conflicts=("${pkgname%-bin}")
options=('!strip' '!debug')
_commit=a1ebb5b6cd971e55e5a6092413a3af68ed5a533e
source=(
  "grok-bot_${pkgver}_amd64.deb::https://downloads.cursor.com/grokbot/stable/${_commit}/linux/x64/grok-bot_${pkgver}_amd64.deb"
  "grok-bot.sh"
)
noextract=("grok-bot_${pkgver}_amd64.deb")
sha256sums=(
  '5aadc536cad16fce5cb5b987d3ea086df52874de47d101a0c99b754592cd2a83'
  '9b3cccfada1dbe44ce794177181515aaf328603484327ef72a914234544bfbf8'
)

package() {
  bsdtar -O -xf "grok-bot_${pkgver}_amd64.deb" data.tar.xz | bsdtar -C "${pkgdir}" -xJf -

  install -Dm755 "${srcdir}/grok-bot.sh" "${pkgdir}/usr/bin/grok-bot"
  sed -i 's|^Exec=.*|Exec=grok-bot %U|' "${pkgdir}/usr/share/applications/grok-bot.desktop"

  install -Dm644 "${pkgdir}/opt/Grok Bot/LICENSE.electron.txt" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.electron.txt"

  if ! { [[ -L /proc/self/ns/user ]] && unshare --user true; }; then
    chmod 4755 "${pkgdir}/opt/Grok Bot/chrome-sandbox"
  fi
}

# Maintainer: Ian MacKay <immackay0@gmail.com>
#
# Gnome Keyring requires setup if you are using a window manager
# You will need to import 5CC908FDB71E12C2 for libcurl-openssl-1.0

_pkgname='github-desktop'
pkgname="${_pkgname}-git"
pkgver=1.0.14.beta1.r37.g86edf4c6e
pkgrel=1
pkgdesc="GUI for managing Git and GitHub."
arch=('x86_64')
url="https://desktop.github.com"
license=('MIT')
depends=('gnome-keyring' 'git' 'electron' 'nodejs' 'libcurl-compat' 'libcurl-gnutls')
optdepends=('hub: CLI interface for GitHub.')
makedepends=('libcurl-openssl-1.0' 'xorg-server-xvfb' 'yarn')
provides=('github-desktop')
conflicts=('github-desktop')
source=(
  git+https://github.com/desktop/desktop.git
  ${_pkgname}.desktop
)
sha256sums=(
  'SKIP'
  be057e4a392e64760f7a40203ac886b7cf5551de254ad55c706376dca8aa4341
)
pkgver() {
  cd desktop
  git describe --long --tags | sed 's/^release.//;s/\([^-]*-g\)/r\1/;s/-/./g'
}
build() {
  cd desktop
  export DISPLAY=':99.0'
  Xvfb :99 -screen 0 1024x768x24 > /dev/null 2>&1 &
  yarn install
  yarn build:prod
}
package() {
  install -d "${pkgdir}/opt/${_pkgname}"
  cp -r --preserve=mode desktop/dist/desktop-linux-x64/* "${pkgdir}/opt/${_pkgname}/"
  printf '#!/bin/sh\n\n/opt/github-desktop/desktop "$@"\n' | install -Dm755 /dev/stdin "${pkgdir}/usr/bin/github-desktop"
  install -Dm644 "${_pkgname}.desktop" "${pkgdir}/usr/share/applications/${_pkgname}.desktop"
  install -Dm644 "desktop/app/static/logos/1024x1024.png" "${pkgdir}/usr/share/icons/hicolor/1024x1024/apps/${_pkgname}.png"
  install -Dm644 "desktop/app/static/logos/512x512.png" "${pkgdir}/usr/share/icons/hicolor/512x512/apps/${_pkgname}.png"
  install -Dm644 "desktop/app/static/logos/256x256.png" "${pkgdir}/usr/share/icons/hicolor/256x256/apps/${_pkgname}.png"
}

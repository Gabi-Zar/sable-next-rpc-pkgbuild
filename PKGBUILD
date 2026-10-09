# Maintainer: GabiZar <GabiZar@proton.me>
pkgname=sable-next-rpc
pkgver=nightly.1.22.10.nightly.261009115558.011c6f29c17e
pkgrel=1
pkgdesc="Fork of Sable Next with rich presence support (MSC4544) nightly build"
arch=('x86_64')
url="https://github.com/Gabi-Zar/sable-next-rpc"
license=('AGPL-3.0-or-later')
depends=(
  'at-spi2-core'
  'alsa-lib'
  'dbus'
  'desktop-file-utils'
  'gtk3'
  'hicolor-icon-theme'
  'libayatana-appindicator'
  'libcups'
  'libdrm'
  'libxcomposite'
  'libxdamage'
  'libxkbcommon'
  'libxrandr'
  'libxss'
  'mesa'
  'nspr'
  'nss'
  'xdg-utils'
  'xorg-xwayland'
)
makedepends=(
  'git'
  'mise'
)
source=("git+$url.git#branch=rich-presence")
sha256sums=('SKIP')
provides=('sable-next')
conflicts=('sable-next-bin' 'sable-next-nightly-bin' 'sable-next-git')
options=(!debug !strip)

pkgver() {
  cd "${pkgname}"
  git describe --tags $(git merge-base HEAD main) | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS RUSTFLAGS CARGO_ENCODED_RUSTFLAGS
  cd "${pkgname}"
  
  mise install
  mise run setup

  chmod +x scripts/tauri.js
  mise run tauri cef build --config src-tauri/tauri.nightly.conf.json
  mise run cef:package "${pkgver}" "Sable Next Nightly"
}

package() {
  cd "${pkgname}/target/release/bundle/deb"

  bsdtar -xOf "sable-next-${pkgver}-linux-x86_64.deb" data.tar.gz | bsdtar -xf - -C "${pkgdir}/"
  find "$pkgdir" -type d -exec chmod 755 {} +
}

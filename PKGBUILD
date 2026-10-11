# Maintainer: GabiZar <GabiZar@proton.me>
pkgname=sable-next-rpc
pkgver=2.0.1
pkgrel=1
pkgdesc="Fork of Sable Next with rich presence support (MSC4544)"
arch=('x86_64')
url="https://github.com/Gabi-Zar/sable-next-rpc"
license=('AGPL-3.0-or-later')
depends=(
  'gtk3'
  'nss'
  'nspr'
  'mesa'
  'libdrm'
  'libxkbcommon'
  'alsa-lib'
  'libcups'
  'libpipewire'
  'libayatana-appindicator'
  'xdg-utils'
  'xorg-xwayland'
  'hicolor-icon-theme'
  'desktop-file-utils'
)
makedepends=(
  'git'
  'mise'
)
source=("git+$url.git#branch=rich-presence")
sha256sums=('SKIP')
provides=('sable-next')
conflicts=('sable-bin' 'sable-nightly-bin' 'sable')
options=(!debug !strip)

pkgver() {
  cd "${pkgname}"
  git describe --tags $(git merge-base HEAD main) --match 'v[0-9]*' | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS RUSTFLAGS CARGO_ENCODED_RUSTFLAGS
  cd "${pkgname}"
  
  mise install
  mise run setup

  chmod +x scripts/tauri.js
  mise run tauri cef build
  mise run cef:package
}

package() {
  cd "${pkgname}/target/release/bundle/deb"

  bsdtar -xOf "sable-${pkgver}-linux-x86_64.deb" data.tar.gz | bsdtar -xf - -C "${pkgdir}/"
  find "$pkgdir" -type d -exec chmod 755 {} +
}

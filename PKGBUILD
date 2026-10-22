# Maintainer: GabiZar <GabiZar@proton.me>
pkgname=sable-next-rpc
pkgver=2.1.5
pkgrel=1
pkgdesc="Fork of Sable Next with rich presence support (MSC4544)"
arch=('x86_64')
url="https://github.com/Gabi-Zar/sable-next-rpc"
license=('AGPL-3.0-or-later')
depends=(
  'gtk3'
  'nss'
  'libnotify'
  'libxss'
  'libxtst'
  'at-spi2-core'
  'util-linux-libs'
  'xdg-utils'
  'hicolor-icon-theme'
  'desktop-file-utils'
)
makedepends=(
  'git'
  'mise'
  'rpm-tools'
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

  export SABLE_BUILD_FLAVOR=stable
  sed -i 's/const enabled = app.isPackaged \&\& !process.env.FLATPAK_ID;/const enabled = false;/' electron/src/updater.ts
  
  mise install
  mise run build
  mise exec -- pnpm --dir electron package --linux
}

package() {
  cd "${pkgname}/electron/release"

  bsdtar -xOf "sable-${pkgver}-amd64.deb" 'data.tar*' | bsdtar -xp -C "${pkgdir}"
  find "${pkgdir}" -type d -exec chmod 755 {} +
  find "${pkgdir}/opt" -name chrome-sandbox -exec chmod 4755 {} +
}

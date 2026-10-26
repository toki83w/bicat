# Maintainer: toki83w
#
# Builds the latest commit of the master branch. Run `makepkg -si` in this
# directory (it is in a subdirectory because makepkg would otherwise use the
# `src/` directory of the repository as its own).

pkgname=bicat-git
_pkgname=bicat
pkgver=0.1.0.r0.g0000000
pkgrel=1
pkgdesc='Show images via the Kitty image protocol (Vifm, Tmux)'
arch=('x86_64' 'aarch64')
url='https://github.com/toki83w/bicat'
license=('MIT')
depends=('gcc-libs' 'glibc')
makedepends=('cargo' 'git')
provides=("$_pkgname")
conflicts=("$_pkgname")
source=("$_pkgname::git+$url.git#branch=master")
sha256sums=('SKIP')

pkgver() {
  cd "$_pkgname"
  local _version
  _version=$(sed -n 's/^version = "\(.*\)"/\1/p' Cargo.toml | head -n1)
  printf '%s.r%s.g%s' "$_version" \
    "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
  cd "$_pkgname"
  export RUSTUP_TOOLCHAIN=stable
  cargo fetch --locked --target "$(rustc -vV | sed -n 's/host: //p')"
}

build() {
  cd "$_pkgname"
  export RUSTUP_TOOLCHAIN=stable
  export CARGO_TARGET_DIR=target
  cargo build --frozen --release
}

check() {
  cd "$_pkgname"
  export RUSTUP_TOOLCHAIN=stable
  export CARGO_TARGET_DIR=target
  cargo test --frozen --release
}

package() {
  cd "$_pkgname"
  install -Dm755 "target/release/$_pkgname" "$pkgdir/usr/bin/$_pkgname"
  install -Dm644 README.md "$pkgdir/usr/share/doc/$_pkgname/README.md"
  install -Dm644 LICENSE.md "$pkgdir/usr/share/licenses/$pkgname/LICENSE.md"
}

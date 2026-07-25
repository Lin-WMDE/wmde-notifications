# Maintainer: WMDE <https://wmde.fun>
# Contributor: System76 <info@system76.com> (original cosmic-notifications)
#
# Builds our fork Lin-WMDE/wmde-notifications (branch wmde). Standalone WMDE
# component: installs alongside cosmic-notifications (own binary wmde-notifications
# + own config namespace fun.wmde.Notifications), so NO conflicts/replaces cosmic-notifications.
# The daemon owns the standard org.freedesktop.Notifications name at runtime and
# ships no data files (binary only).
pkgname=wmde-notifications
pkgver=0.1.0
pkgrel=4
pkgdesc="WMDE notifications daemon (fork of cosmic-notifications) - serves org.freedesktop.Notifications"
arch=('x86_64')
url="https://wmde.fun"
license=('GPL-3.0-only')
# depends: wayland client (layer-shell via smithay-client-toolkit through libcosmic);
# rendering/image codecs are static Rust crates. Verify with namcap after first build.
depends=('glibc' 'gcc-libs' 'wayland' 'libxkbcommon')
makedepends=('rust' 'cargo' 'just' 'git' 'wayland' 'libxkbcommon' 'clang' 'lld' 'pkgconf')
# NOTE: Cargo.toml uses path deps to sibling checkouts (../libcosmic,
# ../wmde-panel/cosmic-panel-config). The build harness arranges them next to
# $srcdir; a standalone makepkg run without that layout fails dependency resolution.
source=("$pkgname::git+https://github.com/Lin-WMDE/wmde-notifications.git#branch=wmde")
sha256sums=('SKIP')

pkgver() {
  cd "$srcdir/$pkgname"
  # WMDE unified version: 1.4 (libcosmic base) . <commits since nearest tag> . g<short>.
  local desc
  desc=$(git describe --long --tags --abbrev=7 2>/dev/null || true)
  if [ -n "$desc" ]; then
    printf '1.4.%s.g%s' "$(printf '%s' "$desc" | sed -E 's/.*-([0-9]+)-g[0-9a-f]+$/\1/')" "$(git rev-parse --short=7 HEAD)"
  else
    printf '1.4.%s.g%s' "$(git rev-list --count HEAD)" "$(git rev-parse --short=7 HEAD)"
  fi
}

build() {
  cd "$srcdir/$pkgname"
  # x86-64-v3 (AVX2/BMI2) baseline for the WMDE repo; runs on Haswell+ (and the VM).
  export RUSTFLAGS="${RUSTFLAGS:+$RUSTFLAGS }-C target-cpu=x86-64-v3"
  just build-release
}

package() {
  cd "$srcdir/$pkgname"
  # installs /usr/bin/wmde-notifications (binary only, no data files)
  just rootdir="$pkgdir" prefix=/usr install
  install -Dm644 LICENSE.md "$pkgdir/usr/share/licenses/$pkgname/LICENSE.md"
}

# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://git.archlinux.org/svntogit/packages.git/tree/trunk?h=packages/pacman
#						Maintainer: Dan McGee <dan@archlinux.org>
# 						Maintainer: Dave Reisner <dreisner@archlinux.org>

pkgname=pacman
pkgver=5.1.0
pkgrel=3
pkgdesc="A library-based package manager with dependency support"
arch=('x86_64')
url="http://www.archlinux.org/pacman/"
license=('GPL')
groups=('base' 'base-devel')
depends=('bash' 'glibc' 'libarchive' 'curl' 'pacman-contrib'
         'gpgme' 'pacman-mirrorlist' 'archlinux-keyring')
makedepends=('asciidoc')
checkdepends=('python2' 'fakechroot')
backup=(etc/pacman.conf etc/makepkg.conf)
options=('strip' 'debug')
source=(https://sources.archlinux.org/other/pacman/$pkgname-$pkgver.tar.gz
        pacman.conf
        makepkg.conf
        repo_add_empty_database.patch)
sha256sums=('9f5993fc8923530713742f15df284677f297b3eca15ed7a24758c98ac7399bd3'
            '2c06507d7c98685ab47ce1f0809718d7dc9df75aef4784dc1661571b65233ad0'
            'c3a5f3ce8154032626c459862cef316a3fa49ae398c5222677bac05c4fb65a39'
            '927fb4c9deb3360391156a1b1a9c27fd1f0e27c6deb199da5d3dbd2b3d8ad01e')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal

prepare() {
	cd "$pkgname-$pkgver"
	
	patch -Np1 -i "${srcdir}/repo_add_empty_database.patch"
}

build() {
  cd "$pkgname-$pkgver"

  ./configure --prefix=/usr --sysconfdir=/etc \
    --localstatedir=/var --enable-doc \
    --with-scriptlet-shell=/usr/bin/bash \
    --with-ldconfig=/usr/bin/ldconfig
  make V=1
}

check() {
  make -C "$pkgname-$pkgver" check
}

package() {
  cd "$pkgname-$pkgver"

  make DESTDIR="$pkgdir" install

  # install Arch specific stuff
  install -dm755 "$pkgdir/etc"
  install -m644 "$srcdir/pacman.conf" "$pkgdir/etc"
  install -m644 "$srcdir/makepkg.conf" "$pkgdir/etc"

  # put bash_completion in the right location
  install -dm755 "$pkgdir/usr/share/bash-completion/completions"
  mv "$pkgdir/etc/bash_completion.d/pacman" "$pkgdir/usr/share/bash-completion/completions"
  rmdir "$pkgdir/etc/bash_completion.d"

  for f in makepkg pacman-key; do
    ln -s pacman "$pkgdir/usr/share/bash-completion/completions/$f"
  done
}

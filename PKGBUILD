# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Contributor: Allan McRae <allan@archlinux.org>
pkgname=gdb-git
pkgver=107474.acedf59370a
pkgrel=1
pkgdesc="The GNU Debugger"
arch=('i686' 'x86_64')
url="http://www.gnu.org/software/gdb/"
license=('GPL3')
depends=('expat' 'guile' 'ncurses' 'python' 'xz')
makedepends=('git')
provides=('gdb' 'gdb-common')
conflicts=('gdb' 'gdb-common')
backup=('etc/gdb/gdbinit')
options=('!libtool')
install=gdb.install
source=('gdb::git+git://sourceware.org/git/binutils-gdb.git')
md5sums=('SKIP')

pkgver() {
  cd "$srcdir/gdb"
  echo $(git rev-list --count master).$(git rev-parse --short master)
}

prepare() {
  cd "$srcdir/gdb"

  # fixes build, copied from the gdb PKGBUILD
  # hack! - libiberty configure tests for header files using "$CPP $CPPFLAGS"
  sed -i "/ac_cpp=/s/\$CPPFLAGS/\$CPPFLAGS -O2/" libiberty/configure
}

build() {
  cd "$srcdir/gdb"

  ./configure --prefix=/usr --disable-nls \
              --disable-werror \
              --with-system-readline \
              --with-python=/usr/bin/python \
              --with-system-gdbinit=/etc/gdb/gdbinit

  make
}

package() {
  cd "$srcdir/gdb"

  # install only gdb, otherwise it would install files already provided by binutils
  make DESTDIR="$pkgdir/" install-gdb

  # install "custom" system gdbinit
  install -dm755 $pkgdir/etc/gdb
  touch $pkgdir/etc/gdb/gdbinit
}
